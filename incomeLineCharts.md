---
follows: incomeRoot
---

```elm {v siding}
richest : Spec
richest =
    let
        enc =
            encoding
                << position X [ pName "Year", pTemporal, pAxis [ axFormat "%Y" ] ]
                << position Y [ pName "95pcIncome", pQuant, pTitle "Richest 5% (£)" ]
    in
    toVegaLite [ width 400, data, enc [], line [ maStroke "darkBlue" ] ]
```

```elm {v siding}
poorest : Spec
poorest =
    let
        enc =
            encoding
                << position X [ pName "Year", pTemporal, pAxis [ axFormat "%Y" ] ]
                << position Y [ pName "5pcIncome", pQuant, pTitle "Poorest 5% (£)" ]
    in
    toVegaLite [ width 400, data, enc [], line [ maStroke "darkRed" ] ]
```

```elm {v siding}
combinedLinechart : Spec
combinedLinechart =
    let
        enc5pc =
            encoding
                << position X [ pName "Year", pTemporal, pAxis [ axFormat "%Y" ] ]
                << position Y [ pName "5pcIncome", pQuant, pTitle "Poorest 5% household income (£)" ]

        enc95pc =
            encoding
                << position X [ pName "Year", pTemporal, pAxis [ axFormat "%Y" ] ]
                << position Y [ pName "95pcIncome", pQuant, pTitle "Richest 5% household income (£)" ]

        res =
            resolve
                << resolution (reAxis [ ( chY, reIndependent ) ])
                << resolution (reScale [ ( chY, reIndependent ) ])
    in
    toVegaLite
        [ width 400
        , data
        , res []
        , layer
            [ asSpec [ enc5pc [], line [ maStroke "darkred" ] ]
            , asSpec [ enc95pc [], line [ maStroke "darkblue" ] ]
            ]
        ]
```
