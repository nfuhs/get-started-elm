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

After this you can check that Elm is installed correctly:

```elm```

And it prints:

``` Hi, thank you for trying out Elm 0.19.0. I hope you like it! ```

You're good to go. Enough talk,  lets get started to write some Elm.

#### Create a project folder

Run

```mkdir -p movies/{client,server}```

to create an client folder and and server folder

Instead of using an real API we will mock an JSON Rest API with node.js by using [https://github.com/typicode/json-server](https://github.com/typicode/json-server)

Install it by running:

``` npm install -g json-server ```

Inside the server folder create a JSON file named **db.json** and put the following into:

```json
{
    "movie": [
      {"id": 1, "title": "Terminator 1", "actor": "Arnold Schwarzenegger"},
      {"id": 2, "title": "Terminator 2", "actor": "Arnold Schwarzenegger"}
    ],
    "quote": [
      {"id": 1, "body": "I'll be back"},
      {"id": 2, "body": "Hasta la vista , baby"}
    ],
    "actor": { "name": "Arnold Schwarzenegger"}
}
```

#### Begin the Elm App

Move to the client folder and then init the Elm project with:

``` elm init ```

The following prompt will show up:

```sh
Hello! Elm projects always start with an elm.json file. I can create them!

Now you may be wondering, what will be in this file? How do I add Elm files to
my project? How do I see it in the browser? How will my code grow? Do I need
more directories? What about tests? Etc.

Check out <https://elm-lang.org/0.19.0/init> for all the answers!

Knowing all that, would you like me to create an elm.json file now? [Y/n]:
```

Answer with Yes and proceed by creating an elm.json file.

#### Install missing dependencies

In order to run the Elm example app we're gonna built we need to install missing dependencies we need to write an program to decode JSON. 

If you navigate to [https://package.elm-lang.org/](https://package.elm-lang.org/) you will see all current
packages provided for elm 0.19.

To decode JSON we need the package elm/json:

``` elm install elm/json ```

Another prompt will show up asking you to move the elm.json file to direct dependencies:

```bash
I found it in your elm.json file, but in the "indirect" dependencies.
Should I move it into "direct" dependencies for more general use? [Y/n]:
```

Answer again with Yes and move on.

#### Why do I need an JSON decoder?

A decoder is a function than can take a pice of JSON decode it into an Elm value, with a type that matches a type Elm knows about.

For example if we have this JSON:

```{"Movie": "Terminator"} ```

Now move on and create inside the client folder a file named Main.elm:

``` touch Main.elm ```

Inside the client directory create the 

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

#### Run the JSON Server and the Elm App:

Inside the server folder run the following command to start the JSON server:

```json-server --watch db.json --port 5050```

After this open another terminal and run inside the client folder:

``` elm reactor ```

After this open [http://localhost:8000/Main.elm](http://localhost:8000/Main.elm)
 
Open the link and you should see your Elm app diplaying the JSON values inside your browser Congrats!

### Turning Elm into an normal Single Page Application

Inside the client folder create 

#### Conclusion

This blogpost showed you how to decode JSON with Elm and created an little app to consume API calls with it.

I hope you enjoyed reading it if so feel free to follow me on Twitter:

If you're hooked right now I highly recommend reading:

[The Official Elm Introduction](https://guide.elm-lang.org/) 

and code through all examples in

[The Elm Architecture](https://guide.elm-lang.org/architecture/)