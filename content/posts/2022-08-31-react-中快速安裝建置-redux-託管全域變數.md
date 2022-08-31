---
title: React 中快速安裝建置 Redux 託管全域變數
date: 2022-08-31T05:35:19.352Z
author: Boison
slug: React-Redux-101
tags:
  - React
  - Redux
draft: false
---
Redux 就是用來託管全域變數，好處是當你的專案有很多層的 Component 時不用上下層把資料傳來傳去，即使現在簡單的專案我們會使用其他更小型簡單的套件或 hook（如 useContext）去實現這個需求，但其概念大多雷同。

> **流程就是用 reducer 託管資料，並用 action 向 reducer 發出要修改託管的資料的值**

* Reducer 負責託管資料

* Store 負責統一把不同 Reducer 託管的資料集中取出

* action 負責向 Reducer 發出要修改資料的指令

---

## 1. 安裝套件

* npm install redux 

* npm redux-react-hook / npm react-redux

---

## 2. 建立資料結構

* src 中建立 reducer 資料夾

* reducer 資料夾中建立 weather.js（名稱可改）的 reducer 保管資料

```javascript
const initState = {
    currentPage: 'WeatherCard',
}

const todoReducer = (state = initState, action) => {
    switch (action.type) {
      default:
        return state;
    }
}
// 每一個 Reducer 都會有兩個參數
// 第一個參數會將初始的資料狀態 initState 交由 Reducer 保管
// 第二個參數會傳入現在 Reducer 要對 State 做什麼動作的指令及額外的參數



export default todoReducer
```

* 在 src 下建立 store 資料夾中建立 index.js 負責整合所有的 Reducer

```javascript
import { createStore } from 'redux'
import weatherReducer from '@/redux/weather'

const store = createStore(weatherReducer)

export default store
```

如果有多個 Reducer 的情況，就得使用 `combileReducer` 先將 Reducer 給組合起來

```javascript
import { createStore, combineReducers } from 'redux';
import waetherReducer from '@/redux/weather'
import convertReducer from '@/redux/convert'

const rootReducer = combineReducers({
  waetherReducer,
  convertReducer,
})

const store = createStore(rootReducer)
```

---

## 3. 用 Provider 接收 Reducer 託管的資料

* 在 index.js 中用 Provider component 接收 store 資料

**法一：用 react-redux**

```javascript
import React from 'react'
import ReactDOM from 'react-dom/client'
import '@/index.css'
import App from './App'
import { Provider } from 'redux-react'
import store from '@/redux/store/index'

const root = ReactDOM.createRoot(document.getElementById('root') as HTMLElement);
root.render(
  <React.StrictMode>
    <Provider store={store}>
      <App />
    </Provider>
  </React.StrictMode>
)

// Provider 是 react-redux 中的組件，它會接收上方創建的 Store
// Provider 在 Component 的最上層用 Store 管理所有 Reducer 
// Reducer 則是管理所有 State
```

**法二：用 redux-react-hook**

```javascript
import React from 'react'
import ReactDOM from 'react-dom/client'
import '@/index.css'
import App from './App'
import { StoreContext } from 'redux-react-hook'
import store from '@/redux/store/index'

const root = ReactDOM.createRoot(document.getElementById('root') as HTMLElement);
root.render(
  <React.StrictMode>
    <StoreContext.Provider value={store}>
      <App />
    </StoreContext.Provider>
  </React.StrictMode>
)

// Provider 是 react-redux 中的組件，它會接收上方創建的 Store
// Provider 在 Component 的最上層用 Store 管理所有 Reducer 
// Reducer 則是管理所有 State
```

---

## 4. 在 Component 取得 Reducer 託管的資料的值

* 在 Component 中取得 state 

  * **法一：在 redux-react-hook 中**

    * 提供了 StoreContext、useDispatch 和 useMappedState 來操作 redux 中的 store

  * **法二：在 react-redux 7.1 的 hooks 版**

    * 提供了 useSelector()、useDispatch()、useStore() 這 3 個主要方法

    * useSelector 中除了能從 store 中拿到 state 以外，還支援深度比較的功能，如果相應的 state 前後沒有改變，就不會去重新的計算

> **法一：在 redux-react-hook** **中使用 useMappedState**

```javascript
import React from "react"
import "./Converter.css"
import { useMappedState, useDispatch } from "redux-react-hook"

...

export default function SpeedConverter() {
  const inputValue = useMappedState(state => state.inputValue)
  const newInputValue = useMappedState(state => state.newInputValue)
  
  return (
    ...
  )
}
```

> **法二：在 react-redux 7.1 的 hooks 版中使用 useSelector**

```javascript
import React, { useEffect } from 'react'
import '@/components/Weather/WeatherThemeSwitch.css'
import { switchTheme } from '@/action/weather'
import { useSelector, useDispatch } from 'react-redux'
interface RootState {
    currentTheme: string
}

const WeatherThemeSwitch = () => {
    const dispatch = useDispatch()
    const currentTheme = useSelector((state:RootState) => state.currentTheme)
    
    ...

    return (
        <label className="container" >
            <input className='toggle-checkbox' type='checkbox' id="toggle-checkbox" onClick={handleTheme}></input>
            <div className='toggle-slot' >
                <div className='toggle-button'></div>
            </div>
        </label>
    )
}
  
export default WeatherThemeSwitch
```

---

## 5. 在 Component 中修改 Reducer 託管的資料得值

* 在 Component 中修改 state 

  * **法一：在 redux-react-hook 中**

    * 提供了 StoreContext、useDispatch 和 useMappedState 來操作 redux 中的 store

  * **法二：在 react-redux 7.1 的 hooks 版**

    * 提供了 useSelector()、useDispatch()、useStore() 這 3 個主要方法

    * useSelector 中除了能從 store 中拿到 state 以外，還支援深度比較的功能，如果相應的 state 前後沒有改變，就不會去重新的計算

> **法一：在 redux-react-hook** 

* **redux 中新增 convert.ts (名字自訂)**

```javascript
import { createStore } from 'redux'

// reducer
const valReducer = (state:any, action:any) => {
    switch (action.type) {
        case 'CONVERT_TEMP':
            return {inputValue: Number(action.payload.inputValue),  
                newInputValue: Number(action.payload.inputValue)/8}
        default:
            return state
    }
}

// store
let store = createStore(valReducer, {
    inputValue: 0, 
    newInputValue:0
})

export default store
```

* **在 Component 中使用 dispatch**

```javascript
import React from "react"
import "./Converter.css"
import { useMappedState, useDispatch } from "redux-react-hook"

...

export default function SpeedConverter() {
  const inputValue = useMappedState(state => state.inputValue)
  const newInputValue = useMappedState(state => state.newInputValue)
  
  return (
    <>
      <div className="container">
        <div className="card-header">Network Speed Converter </div>
        <div className="card-body">
            <UnitControl />
          <div className="converter">
            <div className="flex-1">
              <div className="converter-title">Set</div>
              <input
                type="number"
                className="input-number"
                min="0"
                onChange = {(e: React.ChangeEvent<HTMLInputElement>) => {
                  dispatch({type: 'CONVERT_TEMP', payload: {inputValue: e.target.value}})
                }} 
                value={inputValue}
              />
            </div>
            ...
      </div>
    </>
  );
}
```

> **法二：在 react-redux 7.1 的 hooks 版中使用 dispatch**

* 在 src 資料夾下建立 action 資料夾中建立 weather.ts

  * 在 Reducer 內會根據 `action.type` 來判斷要做什麼事情

  * 假設我們要做新增待辦事項的功能，就得先為這個事件創建指令

  * 而創建的指令會需要一個新的目錄 action 來管理

  * 只要在 Component 中呼叫函式傳入參數，就能夠回傳觸發 Reducer 的物件

```javascript
// 宣告與指令同名的常數變數
// 將指令 export，其他地方有需要使用到就從這裡 import
// 確保 action 指令的來源只有一個
export const EDIT_PAGE = 'EDIT_PAGE'
export const SWITCH_THEME = 'SWITCH_THEME'
export const EDIT_LOCATION  = 'EDIT_LOCATION'


// 建立傳入 Reducer 的物件
// Reducer 是靠第二個參數 action 的 type 去判斷要做什麼事情
// 因此我們得把上面的 EDIT_PAGE 等個別放到一個物件的 type 屬性中
// 物件裡要送給 Reducer 執行事件時的參數都會放在 payload 屬性中
// 建立函式包裝，讓 Component 可以傳入參數
export const editPage = (page:string) => ({
  type: EDIT_PAGE,
  payload: {
    page,
  },
})

export const editLocation = (location:Object) => ({
  type: EDIT_LOCATION,
  payload: {
    location,
  },
})

export const switchTheme = (theme:string) => ({
  type: SWITCH_THEME,
  payload: {
    theme,
  },
})
```

* 在 reducer 資料夾中的 weather.ts(名稱自訂) 撰寫 reducer 收到參數 (payload) 後的處理

  * 根據 action 給的名稱決定要觸發哪個功能

```javascript
import * as actions from '@/action/weather'
import { findLocation } from '@/utils/utils'

const initState = {
    currentPage: 'WeatherCard',
    currentTheme: 'light',
    currentLocation: findLocation(localStorage.getItem('city') || '臺北市')
}

const weatherReducer = (state:any = initState, action:any) => {
    switch (action.type) {
        case actions.EDIT_PAGE:
            return {
                ...state,
                currentPage: action.payload.page,
            }
        case actions.EDIT_LOCATION:
            return {
                ...state,
                currentLocation: action.payload.location,
            }
        case actions.SWITCH_THEME:
            return {
                ...state,
                currentTheme: action.payload.theme,
            }
        default:
            return state
    }
}

export default weatherReducer
```

* 在 Component 中呼叫 action 修改交給 reducer 保管的資料的值

```javascript
import React, { useEffect } from 'react'
import '@/components/Weather/WeatherThemeSwitch.css'
import { switchTheme } from '@/action/weather'
import { useSelector, useDispatch } from 'react-redux'
interface RootState {
    currentTheme: string
}

const WeatherThemeSwitch = () => {
    const dispatch = useDispatch()
    const currentTheme = useSelector((state:RootState) => state.currentTheme)
    const handleTheme = () => {
        dispatch(switchTheme(currentTheme === 'light' ? 'dark' : 'light'))
    }
    useEffect(() => {
        let domToggleCheckbox = (document.getElementById('toggle-checkbox') as HTMLInputElement)
        domToggleCheckbox.checked = currentTheme === 'dark' ? true : false
    }, [currentTheme])

    return (
        <label className="container" >
            <input className='toggle-checkbox' type='checkbox' id="toggle-checkbox" onClick={handleTheme}></input>
            <div className='toggle-slot' >
                <div className='toggle-button'></div>
            </div>
        </label>
    )
}
  
export default WeatherThemeSwitch
```

以上程式碼皆放在 [GitHub](https://pse.is/BS-Weather) 供查看

---

> **參考資料**
>
> 1. [React 的快樂小夥伴 - Redux 資料管理篇](https://ithelp.ithome.com.tw/articles/10219962)
>
> 2. [React 的快樂小夥伴 - Redux 事件處理篇](https://ithelp.ithome.com.tw/articles/10220700)