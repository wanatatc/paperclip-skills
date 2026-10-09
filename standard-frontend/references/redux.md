---
title: Frontend - Redux (Slice Pattern)
topic: Frontend
source: 00_Inbox/SIAM SMILE Web Spec 2.0.1 - Frontend.md
---

# Frontend: Redux (Slice Pattern)

**สรุปสำหรับ AI:** ใช้ Redux Toolkit. 1 module = 1 slice (`src/app/modules/<Module>/<module>Slice.ts`). ลงทะเบียนใน `src/redux/rootReducer.ts`. ใน component ใช้ `useAppSelector` / `useAppDispatch` จาก `src/redux/hook.ts` (ไม่ใช้ `useSelector`/`useDispatch` ตรง ๆ)

## กฎ
1. ประกาศ type ของ State + `initialState`
2. Payload หลายค่า → ประกาศ type object ชื่อ `<Action>Payload`
3. `createSlice({ name: "<module>", initialState, reducers })` — `name` = ชื่อ module
4. `export const { actions } = slice.actions`, `export default slice.reducer` (บังคับ)
5. Selector export ได้ (ไม่บังคับ)
6. เพิ่มใน `rootReducer.ts` — ถ้าต้องการ persist ใช้ `persistReducer(persistConfig, slice)`

## ตัวอย่างย่อ
```ts
export type SetShowCountPayload = { count: number; showCount: boolean };

const counterSlice = createSlice({
  name: "counter",
  initialState: { count: 0, settings: { showCount: true } } as counterState,
  reducers: {
    setCount: (state, action: PayloadAction<number>) => { state.count = action.payload; },
    setShowCount: (state, action: PayloadAction<SetShowCountPayload>) => {
      state.count = action.payload.count;
      state.settings.showCount = action.payload.showCount;
    },
  },
});
export const { setCount, setShowCount } = counterSlice.actions;
export default counterSlice.reducer;
```

```ts
// src/redux/rootReducer.ts
export const rootReducer = combineReducers({
  layout: persistReducer(persistConfig, layoutSlice),
  counter: counterSlice,
});
```

## ใช้ใน Component
```ts
const { count, showCount } = useAppSelector(countSelector);
const dispatch = useAppDispatch();
dispatch(setCount(count + 1));
```
