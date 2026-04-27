```
module Main exposing (main) -- Main - file/module name, exposing (main) - outside access like API or external function

import Browser -- import all module Browser
import Html exposing (Html, button, div, text) -- getting direct functions by the "exposing" operator
import Html.Events exposing (onClick)

-- MODEL
-- App state
type alias Model =
  Int -- type alias Model = Int - just another name for Int

-- initial state
init : Model
init =
  0


-- UPDATE
-- actions, type - create type, | - or, Msg = Increment or Decrement
type Msg
  = Increment
  | Decrement

-- function
update : Msg -> Model -> Model
update msg model = -- msg and model - argumtns
  case msg of -- pattern matching
    Increment ->
      model + 1 -- returning new model with model + 1

    Decrement ->
      model - 1


-- VIEW
-- function
view: Model -> Html Msg -- Html - UI, Msg - which actions can generate
view model =
  div [] -- div - function, [] - argument list, [button ..., div ..., button ...] - children list
    [ button [ onClick Decrement ] [ text "-" ] -- button - function, [ onClick Decrement ] - attributes, [ text "+" ] - children
    , div [] [ text (String.fromInt model) ] -- model - Int, String.fromInt - convert, text - Html text
    , button [ onClick Increment ] [ text "+" ] -- onClick Increment - function, when press -> send Msg = Increment
    ]


-- MAIN

main =
  Browser.sandbox -- run app, pass record
    { init = init
    , update = update
    , view = view
    }

{-
init -> Model (state)
     \/
view -> UI
     \/
click -> Msg (actions)
     \/
update (logic) -> new Model
     \/
view -> UI
-}
```


# 🧩 1. module

module Main exposing (main)

## 📌 Что происходит

- `module Main` → имя файла/модуля
    
- `exposing (main)` → что доступно снаружи
    

👉 только `main` — точка входа

---

# 🧩 2. import

import Browser  
import Html exposing (Html, button, div, text)  
import Html.Events exposing (onClick)

## 📌 Что это

- подключение модулей
    
- `exposing (...)` → какие функции берём напрямую
    

👉 без exposing:

Html.div

👉 с exposing:

div

---

# 🧩 3. Model

type alias Model =  
    Int

## 📌 Что это

👉 состояние приложения

---

## 🔍 `type alias`

type alias Model = Int

👉 это:

Model = просто другое имя для Int

---

# 🧩 4. init

init : Model  
init =  
    0

## 📌 Что это

👉 начальное состояние

---

# 🧩 5. Msg

type Msg  
    = Increment  
    | Decrement

## 📌 Что это

👉 события (actions)

---

## 🔥 Синтаксис

- `type` → создаём тип
    
- `|` → “или”
    

👉 читается:

Msg = Increment ИЛИ Decrement

---

# 🧩 6. update

update : Msg -> Model -> Model

## 📌 Тип

👉 функция:

Msg → Model → Model

---

## 🔍 Реализация

update msg model =

👉 аргументы:

- `msg`
    
- `model`
    

---

### 🔥 case

case msg of

👉 pattern matching

---

Increment ->  
    model + 1

## 📌 Что происходит

- `model + 1` → оператор `+`
    
- возвращаем новый Model
    

---

⚠️ важно:

мы НЕ меняем model  
мы создаём новый

---

# 🧩 7. view

view : Model -> Html Msg

## 📌 Тип

👉 функция:

Model → Html Msg

---

## 🔍 Html Msg

- `Html` → UI
    
- `Msg` → какие события может генерировать
    

---

## 🔧 Разбор

div []

👉

- `div` — функция
    
- `[]` — список атрибутов
    

---

[ button ..., div ..., button ... ]

👉 список детей

---

## 🔹 button

button [ onClick Increment ] [ text "+" ]

### 📌 разбор

- `button` = функция
    
- `[ onClick Increment ]` = атрибуты
    
- `[ text "+" ]` = дети
    

---

### 🔥 onClick

onClick Increment

👉 функция:

при клике → отправь Msg = Increment

---

## 🔹 text

text (String.fromInt model)

👉

- `model` → Int
    
- `String.fromInt` → конвертация
    
- `text` → HTML текст
    

---

# 🧩 8. main

main =  
    Browser.sandbox

## 📌 Что это

👉 запускает приложение

---

## 🔧 sandbox

Browser.sandbox  
    { init = init  
    , update = update  
    , view = view  
    }

👉 передаём record

---

## 🔥 это record

{ init = init  
, update = update  
, view = view  
}

---

# 🧠 Как работает всё вместе

init → Model  
      ↓  
view → UI  
      ↓  
click → Msg  
      ↓  
update → новый Model  
      ↓  
view → UI

---
# 🔥 Супер-коротко

- Model = состояние
- Msg = события
- update = логика
- view = UI