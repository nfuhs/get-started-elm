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

To create an client folder and and server folder run:

```mkdir -p movies/{client,server}```

Instead of using an real API we will mock an JSON Rest API with node.js by using

[https://github.com/typicode/json-server](https://github.com/typicode/json-server)

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

The following prompt will show that informs you that all Elm projects start
with an Elm.json file and asks you to create one:


```Knowing all that, would you like me to create an elm.json file now? [Y/n]:```

Answer with Yes and proceed by creating an elm.json file.

Now move on and create inside the client folder the file Main.elm:

``` touch Main.elm ```

#### Install missing dependencies

In order to run the Elm example app we're gonna built we need to install missing dependencies as we need to write an program to encode and decode JSON. 

If you navigate to [https://package.elm-lang.org/](https://package.elm-lang.org/) you will see all current packages provided for elm 0.19.

To decode JSON we need the package **elm/json**:

``` elm install elm/json ```

Another prompt will show up asking you to move the elm.json file to direct dependencies:

```

Hello! Elm projects always start with an elm.json file. I can create them!

Now you may be wondering, what will be in this file? How do I add Elm files to
my project? How do I see it in the browser? How will my code grow? Do I need
more directories? What about tests? Etc.

Check out <https://elm-lang.org/0.19.0/init> for all the answers!

Knowing all that, would you like me to create an elm.json file now? [Y/n]:

```

Answer again with Yes and move on.

#### Why do I need an JSON decoder?

We neeed to write an encode function and an decode function in order to represent the JSON data in our Elm app.

Lets just asume we have following JSON:

```{"Movie": "Terminator"}```

In order to represent this JSON Oject in Elm we first need to import the elm.json.decoder
from elm/json:

```elm

module Movie exposing (Movie, encode, decoder)

import Json.Decode as I
import Json.Encode as J

```

Then we need to create a Model which is a representation of the JSON  Object in Elm:

```elm

type alias Movies =

{
movie : String
, quote : String
, actor : String
}

```

Then we need the  write an Encode function which lets us represent the data inside the view:

```elm

encode : Movie -> E.Value
encode movie =
E.object

```

After this we go on and write the decoder function so we can represent the data in Elm:

```elm

decoder : D.Decoder Movie
decoder =
    D.map3 Movie
    (D.field "movie" D.string)
    (D.field "quote" D.string)
    (D.field "actor" D.string)

```

#### PROTOTYPE

```elm

module Movie exposing (Movie, encode, decoder)

import Json.Decode as I
import Json.Encode as J

type alias Movies =

{
movie : String
, quote : String
, actor : String
}

encode : Movie -> E.Value
encode movie =
E.object

decoder : D.Decoder Movie
decoder =
    D.map3 Movie
    (D.field "movie" D.string)
    (D.field "quote" D.string)
    (D.field "actor" D.string)

```

#### TODO Finished Elm App

``` elm

module Data.JsonValue exposing (JsonValue(..), decoder)

import movie exposing (movie)
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

If you're hooked right now I highly recommend reading:

[The Official Elm Introduction](https://guide.elm-lang.org/) 

and code through all examples in

[The Elm Architecture](https://guide.elm-lang.org/architecture/)

If you  want to know more about JSON decoders and Evan Czaplicki's vision of Elm's of data interchange in Elm you can read more about it here:

[https://gist.github.com/evancz/1c5f2cf34939336ecb79b97bb89d9da6](https://gist.github.com/evancz/1c5f2cf34939336ecb79b97bb89d9da6)


I hope you enjoyed reading it if so feel free to follow me on GitHub or Twitter:

If you have any questions left just post an issue in the blogpost repo:

[https://github.com/nfuhs/get-started-elm](https://github.com/nfuhs/get-started-elm)