# Elm

Elm code can be tested in the [sandbox app](http://elm-lang.org/examples/hello-html) without installation.   

# Install  
```bash
sudo npm install -g elm # Normal install
sudo npm install --unsafe-perm -g elm # If npm gives you permission errors.
```

`elm-package install elm-lang/PACKAGE_NAME` - Install dependencies.  

#### Alternative
Install yarn
```bash
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
sudo apt-get update && sudo apt-get install yarn
```

Install Elm
```bash
export PATH="$PATH:`yarn global bin`" # Add in order to use Elm commands.
yarn global add elm
```
# Elm "Hello World!"
Create the `Main.elm` file. This, along with all the dependecies will transpile into Javascript in step 3.

```elm
import Html exposing (text)

main =
  text "Hello, World!"
```
Create the `index.html` file, which will load the javascript file created in step 3.

```html
<html>
    <head></head>
    <body>
        <div id="app"></div>
        <script src="main.js"></script>
        <script>
            var app = Elm.Main.embed(document.getElementById("app"));
        </script>
    </body>
</html>
```

Compile/transpile the elm code into javascript.
```bash
elm-make Main.elm --output=main.js # Locations are relative.
```

The result from this is the creation of:
- `elm-stuff` - Holds the dependecies, like `node_modules`.  
- `elm-package.json` - Lists the dependecies, like `package.json`.  
- `main.js` - The transpiled elm code, to be used in `index.html`.

# Build/Compile
`elm-make Main.elm --output=../public/main.js` - Build

# App Build Order  

1. Connect modules / Navigation
2. Login
3. Saving Token
4. Logout
5. Lock/Control Access to Pages
6. Individual Pages (Modules)  

# The Elm Architecture  

![TEA](../pics/elm/TEA.png)

There are 3 main wiring parts between modules. They happen in the `Model`, `Update` and `View`.

![TEA](../pics/elm/elm_arch.png)

# ChildModule.elm

```elm
module ChildModule exposing(..)
```

**Note:** In order to import a module in `Main`, the source folder (where all the modules are) needs to be added in the `elm-package.json` file under `source-directories` like so:  

```json
"source-directories": [
        ".",
        "src"
    ]
```

# Main.elm

## 1. Model

The `Main` module doesn't need to know the details of `ChildModule`'s model. It just needs to know that the `ChildModule` has a model, which is exposed and can be referenced by using `ChildModule.Model`. The initial value can be referenced as well with `ChildModule.initModel`.

So we just need to create a `childModule` field of type `ChildModule.Model`.

```elm  
import ChildModule

type alias Model =
    { page : Page,
    , childModule : ChildModule.Model
    }

initModel : Model
initModel =
    { page = ChildModulePage
    , childModule = ChildModule.initModel
    }
```

## 2. Update  

When the message is of type `ChildModuleMsg` i.e. originating from an outside module, we pull out the included message and put it into the identifier `cmMsg`.  

The `Main` module doesn't know how to handle this message directly, but the `ChildModule`'s **update function** does (for any message in that module), so we just call it to deal with the `ChildModule`'s **model**.  

To do this, we update the model by setting the `childModule` field to the value returned by calling the `ChildModule`'s **update function** and passing it the two parameters, the **message** `cmMsg` and the **current model** for the child module `model.childModule`

```elm  
type Msg
    = ChangePage Page
    | ChildModuleMsg ChildModule.messages

update : Msg -> Model -> Model
update msg model =
    case msg of
        ChangePage page ->
            { model | page = page }

        ChildModuleMsg cmMsg ->
            { model | childModule = ChildModule.update cmMsg model.childModule }
```
## 3. View  

This is a **GOTCHA** part due to the different `Html Msg` types. We need to transform the html capable of generating `ChildModule` messages into `Html` capable of generating `Main` messages by using `map`.  

The first parameter for the `map` function is a transformation function, which is the `ChildModuleMsg`'s type constructor that acts as a function.

The second parameter is the `ChildModule`'s view function with the **current model** as an argument, hence the tuple.  

**In other words, we are transforming any message coming from the child view, into main messages by using map.**  

```elm
view : Model -> Html Msg
view model =
    let
        page =
            case model.page of
                ChildModulePage ->
                    Html.map ChildModuleMsg (ChildModule.view model.childModule)
    in
        div [] [ page ]

```

# Coonection and Navigation

After creating a `ChildModule`, exposing it and adding it to `elm-package.json`'s `source-directories`, we need to connect the module and navigate between them.  

The connection is explained in **The Elm Architecture** above.  

To use the navigation, we need to install the package with `elm-package install elm-lang/navigation` along with th dependencies and import the package with `include Navigation`.  

After that, in the `main` function, we change the `Html.program` function call into `Navigation.program location`.  

```elm
main : Program Never Model Msg
main =
    -- Html.program
    Navigation.program locationToMsg
    { init = init
    , update = update
    , view = view
    , subscriptions = subscriptions
    }
```

`Navigation.program` takes a function as a parameter, which takes a location as a parameter.  

The `locationToMsg` is called every time the URL changes. The function takes a `Navigation.Location` and returns a `Msg`.  

This function needs to:
1. Transform the URL hash into a `Page`.
2. Return the page as part of a `Msg`.
3. The message is fed back into the `update` function.
