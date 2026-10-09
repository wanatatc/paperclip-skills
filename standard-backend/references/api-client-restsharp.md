---
title: Backend - External API Client (RestSharp)
topic: Backend
source: 00_Inbox/SIAM SMILE Web Spec 2.0.1 - Backend.md
---

# Backend: External API Client (RestSharp)

**สรุปสำหรับ AI:** เรียก API ภายนอกด้วย **RestSharp** (ไม่ใช้ HttpClient ตรง ๆ). ไว้ใน `Clients/`. ตั้งชื่อ class ลงท้าย `Client`, setting ลงท้าย `Setting`. Register เป็น Singleton ใน `ProjectSetup.cs`

## โครง Client พื้นฐาน
```csharp
public class RestSharpClient
{
    private readonly RestClient _client;
    private const string _getEndpoint = "/path1";

    public RestSharpClient(IOptions<RestSharpSetting> options)
    {
        var restOptions = new RestClientOptions(options.Value.BaseApiUrl);
        var jsonSetting = new JsonSerializerSettings
        {
            ReferenceLoopHandling = ReferenceLoopHandling.Ignore,
            NullValueHandling = NullValueHandling.Ignore
        };
        _client = new RestClient(restOptions, configureSerialization: s => s.UseNewtonsoftJson(jsonSetting));
        _client.AddDefaultHeader("Authorization", "Bearer " + options.Value.Token);
    }

    public async Task<ResponseDto> GetResponseAsync()
    {
        var request = new RestRequest(_getEndpoint);
        request.AddQueryParameter("page", "1");
        return await _client.GetAsync<ResponseDto>(request);
    }
}
```
- Endpoint เป็น `const` ใน class
- Body → `request.AddJsonBody(dto)`, Form → `AddParameter`, Upload → `AddFile`
- Download → `_client.DownloadAsync(request)` แล้วเขียน `Stream` ลงไฟล์

## แยก Client ต่อ resource
```csharp
public class UserClient : RestSharpClient { /* endpoint /user */ }
```
Register: `services.AddSingleton<RestSharpClient>(); services.AddSingleton<UserClient>();` (ProjectSetup.cs) → inject ใน service/controller

## Authentication
| แบบ | วิธี |
|---|---|
| Basic | `Authenticator = new HttpBasicAuthenticator(user, pass)` |
| JWT | `Authenticator = new JwtAuthenticator(token)` |
| Custom (login/refresh) | สร้าง class ที่ extend `AuthenticatorBase` override `GetAuthenticationParameter` |

**Custom Authenticator (มาตรฐาน SSD)**:
- ใน `GetAuthenticationParameter` → ถ้าไม่มี token หรือใกล้หมดอายุ (เผื่อ 5 นาที) → เรียก login
- ใช้ `SemaphoreSlim(1,1)` ป้องกันยิง login ซ้ำพร้อมกัน
- Login ด้วย client แยก (ไม่ใส่ base url) → เก็บ token + `DateTime.UtcNow.AddSeconds(expiresIn)`
- Return `HeaderParameter(KnownHeaders.Authorization, Token)`
- ตัวอย่างอยู่ใน `CxOneApiAuthenticator` / `CxOneApiClient`

## ข้อควรรู้
- `GetAsync<T>` / `PostAsync<T>` deserialize JSON ให้อัตโนมัติ และ **throw exception** เมื่อ error → ต้อง try/catch
- `ExecuteAsync` **ไม่ throw** → ต้องเช็ค `response.IsSuccessful` / `StatusCode` เอง
- RestSharp มี HttpClient pool ให้ตาม BaseUrl (ไม่ต้องจัดการ HttpClient เอง)
- ⚠ ตัวอย่างต้นฉบับเขียน `Options<RestSharpSetting>` และอ้าง `_setting` ที่ไม่ได้ประกาศ — ให้ใช้ `IOptions<T>` ตามรูปแบบด้านบน
