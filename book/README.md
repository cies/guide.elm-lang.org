# Elm: Elegant browser programs

**Elm is a programming language that compiles to JavaScript (the main language understood by web browsers).**

Contrary to some other programming languages (like C, C++, Java or Rust), Elm is not "general purpose", it is designed specifically to create web applications.

What makes Elm unique:

  - Easy to get started, as it...
    - ...is a "small" language (not much to learn),
    - ...has a complete and well integrated set of tools, and
    - ...gives beginner friendly errors!
  - Elm makes writing correct programs easy (by virtue of something called *strong type safety*).
  - All Elm applications follow *The Elm Architecture* which makes reading other's apps easy.
  

## The deep dive

Before we go into the details of the Elm programming language, lets first have a look at the code for simple Elm application. This little app that lets you increment and decrement a number:

```elm
module Main exposing (main)

import Browser
import Html exposing (Html, button, div, text)
import Html.Events as E

type alias Model = { count : Int }

type Msg = Increment | Decrement

update : Msg -> Model -> Model
update msg model = case msg of
    Increment -> { model | count = model.count + 1 }
    Decrement -> { model | count = model.count - 1 }

view : Model -> Html Msg
view model =
    div []
        [ button [ E.onClick Increment ] [ text "+1" ]
        , div [] [ text <| String.fromInt model.count ]
        , button [ E.onClick Decrement ] [ text "-1" ]
        ]

main : Program () Model Msg
main = Browser.sandbox { init = { count = 0 }, view = view, update = update }
```

Next we look a bit deeper into each section of this code.

It start with a *module definition*, giving it a name (`Main`) and exposing only one function (`main`):

```elm
module Main exposing (main)
```

Then you find the `import` statements, this makes functionality of other Elm modules ([`Browser`](https://package.elm-lang.org/packages/elm/browser/latest/Browser), [`Html`](https://package.elm-lang.org/packages/elm/html/latest/Html) and [`Html.Events`](https://package.elm-lang.org/packages/elm/html/latest/Html-Events)) available to the code in this module:

```elm
import Browser
import Html exposing (Html, button, div, text)
import Html.Events exposing (onClick)
```

To find the documentation of these modules you can follow the links in the text above, use a search engine, or install a [code editor that understands Elm] (these usually show documentation by mouse clicking the code).

The following two lines define data types, the `Model` which "keeps the count" and the `Msg` which specifies all possible "actions" this app support:

```elm
type alias Model = { count : Int }

type Msg = Increment | Decrement
```

This block of code defines the `update` function, which transforms a value of type `Msg` and a value of type `Model` into a value of type `Model` (it updates a `Model` with a `Msg`):

```elm
update : Msg -> Model -> Model
update msg model = case msg of
    Increment -> { model | count = model.count + 1 }
    Decrement -> { model | count = model.count - 1 }
```

Further on in this guide we will fully explain how to read code like this.

[HTML] and [CSS] are most commonly used to create interfaces for web browsers. The interface of this simple app only uses HTML. It helps to be familiar with HTML and CSS when starting with Elm, but you can also pick it up along the way. The next block of code defines how to render a `Model` in a web browser, for which it uses functions from the `Html` module we included earlier. Some of these functions, i.e. `div` and `button` correspond to HTML tags.

The `view` function transforms a value of type `Model` into a value of type `Html Msg` (it creates an HTML representation of a model):

```elm
view : Model -> Html Msg
view model =
    div []
        [ button [ onClick Increment ] [ text "+1" ]
        , div [] [ text (String.fromInt model.count) ]
        , button [ onClick Decrement ] [ text "-1" ]
        ]
```

The last two lines describe the main function which combines the `view` and `update` functions into a `Program`, a runnable Elm application.

```elm
main : Program () Model Msg
main = Browser.sandbox { init = { count = 0 }, view = view, update = update }
```

You may [try out this app in Ellie](https://ellie-app.com/new).

