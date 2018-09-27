#### PART 2?

For now close the browser, **delete** all stuff that is in the Main.elm file and close the elm reactor by running **CTRL**+**D**

#### Starting an App to display JSON in the browser

#### Create a project folder

To create an client folder and and server folder run:

```mkdir -p movies/{client,server}```

Instead of using an real API we will mock an JSON Rest API with node.js by using

[https://github.com/typicode/json-server](https://github.com/typicode/json-server)

Install it by running:

``` npm install -g json-server ```

Inside the server folder create a JSON file named **movies.json** and put the following into it:

```json
{
    "movies": [{
      "title" : "The Terminator",
      "year" : 1984,
      "characters" : ["Terminator", "Kyle Reese", "Sarah Connor", "Lieutenant Ed Traxler", "Detective Hal Vukovich"],
      "director" : "James Cameron"
    },
  
    {
      "title" : "Terminator 2 - Judgement Day",
      "year" : 1991,
      "characters" : ["Terminator", "Sarah Connor", "John Connor", "T-1000", "Myles Dyson"],
      "director" : "James Cameron"
    },
  
    {
      "title" : "Terminator 3 - Rise of the Machines",
      "year": 2003,
      "characters" : ["Terminator", "John Connor", "Kate Brewster", "T-X", "Robert Brewster"],
      "director" : "Jonathan Mostow"
    },
  
    {
      "title" : "Terminator - Salvation",
      "year": 2009,
      "characters" : ["John Connor", "Marcus Wright", "Blair Williams", "Dr. Serena Kogan", "Kyle Reese"],
      "director" : "McG"
    },
  
    {
      "title" : "Terminator - Genisys",
      "year":2015,
      "characters" : ["Guardian", "Jon Connor", "Sarah Connor", "Kyle Reese", "O'Brien"],
      "director" : "Alan Taylor"
    },
  
    {
      "title" : "Terminator 6 - Terminator Reboot",
      "year":2019,
      "characters" : ["The Terminator", "Grace", "Sarah Connor", "Terminator", "Dani Ramos"],
      "director" : "Tim Miller"
    }]
  
  }
```

#### Install missing dependencies

In order to run the Elm app we gonna built we need to install missing dependencies as we need to write an program to encode and decode JSON. 

If you navigate to [https://package.elm-lang.org/](https://package.elm-lang.org/) you will see all current packages provided for elm 0.19.

To decode JSON we need the package **elm/json**:

``` elm install elm/json ```

Another prompt will show up:

```

I found it in your elm.json file, but in the "indirect" dependencies.
Should I move it into "direct" dependencies for more general use? [Y/n]: 

```

Answer again with yes so you can use elm/json directly in the client app.

Now you should have a the following folders and files present in your client folder:


```
├── client
│   ├── elm.json
│   ├── Main.elm
│   └── src
└── server
    └── movies.json
```


#### Why do I need an JSON decoder?

We need to write an encode function and an decode function in order to represent the JSON data in our Elm app.

Let's just assume we have the following JSON:

```"title" : "The Terminator"```

Elm itself isn't related to JavaScript or its Objects, Elm itself represents Data as 

In order to represent this JSON data in Elm we first need to
import the elm.json.decoder from elm/json:

```elm

module Movie exposing (Movie, encode, decoder)

import Json.Decode exposing (Decoder, field, string)

titleDecoder : Decoder String
titleDecoder =

field "title" String



```

Then we need to create a Model which is a representation of the JSON Object in Elm:

```elm

type alias Movies =

{
movies : String
, title : String
, year : Int
, main_characters : String
, director : String 
}

```

Then we need the write an encode function which lets us represent the data inside the view:

```elm

encode : Movie -> E.Value
encode movie =
E.object

```

After this we go on and write the decoder function so we can represent the data in Elm:

```elm

decoder : D.Decoder Movie
decoder =
    D.map4 Movie
    (D.field "title" D.string)
    (D.field "year" D.Int)
    (D.field "main_characters" D.string)
    (D.field "director" D.string)

```

#### PROTOTYPE

```elm

module Movie exposing (Movie, encode, decoder)

import Json.Decode as D
import Json.Encode as E

type alias Movies =

{
movie : String
, title : String
, year : Int
, characters : String
, director : String
}

encode : Movie -> E.Value
encode movie =
E.object

decoder : D.Decoder Movie
decoder =
    D.map4 Movie
    (D.field "title" D.string)
    (D.field "year" D.Int)
    (D.field "characters" D.string)
    (D.field "director" D.string)

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

```json-server --watch movies.json --port 5050```

Wait a second and json-server will serves our JSON file as resources under:

[http://localhost:5050/movies](http://localhost:5050/movies) 

Open this link and you should see our JSON file with values in the browser.

Now open another terminal and run elm reactor again inside the client folder:

[http://localhost:8000/Main.elm](http://localhost:8000/Main.elm)
 
Open the link and you should see your Elm app displaying the JSON file with values inside your elm-based client app.  Congrats!


#### Conclusion

This blog post gave you an short introduction to Elm and how to decode JSON with it.

If you're hooked right now I highly recommend reading:

[The Official Elm Introduction](https://guide.elm-lang.org/) 

and code through all examples in

[The Elm Architecture](https://guide.elm-lang.org/architecture/)

If you  want to know more about JSON decoders and Evan Czaplicki's vision of Elm's of data interchange in Elm you can read more about it here:

[https://gist.github.com/evancz/1c5f2cf34939336ecb79b97bb89d9da6](https://gist.github.com/evancz/1c5f2cf34939336ecb79b97bb89d9da6)


I hope you enjoyed reading it if so feel free to follow me on [GitHub](https://github.com/nfuhs) or [Twitter](https://twitter.com/NorbertFuhs)

If you have any questions left just post an issue in this repository:

[https://github.com/nfuhs/get-started-elm](https://github.com/nfuhs/get-started-elm)