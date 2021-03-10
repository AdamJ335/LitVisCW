---
elm:
  dependencies:
    gicentre/elm-vegalite: latest
---

```elm {l=hidden}
import VegaLite exposing (..)
```

Top 5 programming languages according to the [TIOBE index](https://www.tiobe.com/tiobe-index).

```elm {l=hidden}
data =
    dataFromUrl "https://gicentre.github.io/data/tiobeIndexMay2018.csv"
```

```elm {v l siding}
helloLitvis : Spec
helloLitvis =
    let
        enc =
            encoding
                << position X
                    [ pName "language"
                    , pSort [ soByField "rating" opMean, soDescending ]
                    ]
                << position Y [ pName "rating", pQuant ]
    in
    toVegaLite [ data [], enc [], bar [] ]
```

Below is the same data displayed differently

```elm {v siding}
helloLitvis : Spec
helloLitvis =
    let
        enc =
            encoding
                << position Y [ pName "language" ]
                << position X [ pName "rating", pQuant ]
    in
    toVegaLite [ data [], enc [], bar [] ]
```
