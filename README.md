### Get started with Elm

This blopost is a short introduction to Elm and will teach you something about Elm weird parts for beginners, decoding JSON.
who are familar with JavaScript and node.js in order to consume data via JSON REST responses from a third party API.  

#### An short introduction to Elm 

Elm was created by Evan Czaplicki as his [thesis](https://www.seas.harvard.edu/sites/default/files/files/archived/Czaplicki.pdf) in 2012, Elm is a purely functional programming language for building web applications.

In my view one the most interesting points is the Elm Architecture:

![Model-Update-View](img/Model-Update-View.png)

This **MUV** pattern is always the same across all Elm apps once you understand this things like writing an SPA with elm will become very ease with Elm. 

This is really a straightfoward concept in Elm and pretty similar to other reactive concepts like Facebook's [React](https://reactjs.org/) for example.

#### Install Elm and setup

This tutorial asumes you have Elm and all it tools setup so if you haven't you first need install Elm,
please follow the [Official Elm Introduction](https://guide.elm-lang.org/install.html) to do so.

#### Check Elm is intalled correctly

When you type:

```elm```

And it prints:

``` Hi, thank you for trying out Elm 0.19.0. I hope you like it! ```

You're good to go. Enough talk,  lets get started to write some Elm.

#### Create the folder of the node server

First lets create the folder of the node based server:

``` mkdir mockserver ```

Instead of using an real API we will mock an JSON Rest API with node.js by using [https://github.com/typicode/json-server](https://github.com/typicode/json-server)

Install it by running:

``` npm install -g json-server ```

Inside the mcokserver folder create a named **db.json** and put the following JSON to it:
[]()

#### Create a folder for the App

Then let's create an folder for our App in move in it:

``` mkdir elm-marvel```

Then init the Elm project with:

``` elm init ```

#### Install missing dependencies
In order to run the Elm example app we're gonna built we need to install missing dependencies we need to write an program to decode JSON. 

If you navigate to [https://package.elm-lang.org/](https://package.elm-lang.org/) you will see all current
packages provided for elm 0.19.

In order to decode JSON we need the package elm/json:

``` elm package install elm/json ```

#### Why do I need an JSON decoder?

A decoder is a function than can take a pice of JSON decode it into an Elm value, with a type that matches a type Elm knows about.

For example if we have this JSON:

```{"Movie": "Terminator"} ```

#### Finished Elm App

``` elm
module Data.JsonValue exposing (JsonValue(..), decoder)

import Dict exposing (Dict)
import elm/json.decoder as Decode
    exposing
        (Decoder
        , dict
        , string
        , int
        , float
        , list
        , null
        , oneOf
        , lazy
        , map
        , bool
        )

type JsonValue
    = JsonString string
    | JsonInt Int
    | JsonFloat float
    | JsonArray (List JsonValue)
    | JsonObject (Dict String JsonValue)
    | JsonNull 

decoder : Decoder JsonValue
decoder =
    oneOf
    [ map JsonStringstring string
    , map JsonInt int 
    , map JsonFloat float
    , map JsonBoolean bool
    , list (lazy (\_ -> decoder)) |> map JsonArray
    , dict (lazy (\_ -> decoder)) |> map JsonObject
    , null JsonNull
    ]
```

#### Conclusion

You learned how decode JSON with Elm and created an little app to consume API calls
with it.

If you're hooked right now I highly recommend reading
[Official Elm Introduction](https://guide.elm-lang.org/) and code trough all examples in [The Elm Architecture](The Elm Architecture).



