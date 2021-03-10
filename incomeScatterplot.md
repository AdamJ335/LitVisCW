---
follows: incomeRoot
---

```elm{v interactive}
cfg =
    let
        font =
            "Roboto Condensed"
    in
    configure
        << configuration (coAxis [ axcoTitleFont font, axcoLabelFont font, axcoGrid False ])
        << configuration (coView [ vicoStroke Nothing, vicoContinuousWidth 500, vicoContinuousHeight 500 ])
        << configuration (coText [ maAlign haRight, maFontSize 7, maAngle 20, maDx -4 ])
enc =
    encoding
        << position X
            [ pName "5pcIncome"
            , pQuant
            , pAxis [ axTitle "Poorest 5% (£)" ]
            , pScale [ scZero True ]
            ]
        << position Y
            [ pName "95pcIncome"
            , pQuant
            , pAxis [ axTitle "Richest 5% (£)" ]
            , pScale [ scZero True ]
            ]
        << order [ oName "Year", oTemporal ]
labelEnc =
    encoding
        << position X [ pName "5pcIncome", pQuant ]
        << position Y [ pName "95pcIncome", pQuant ]
        << text [ tName "PMLabel" ]
scatter : Spec
scatter =
    let
        lineSpec =
            asSpec
                [ enc []
                , line
                    [ maInterpolate miMonotone
                    , maPoint (pmMarker [ maFill "black", maStroke "white", maStrokeWidth 1.5 ])
                    ]
                ]

        labelSpec =
            asSpec [ labelEnc [], sel [], textMark [] ]

        sel =
            selection << select "view" seInterval [ seBindScales ]
    in
    toVegaLite [ cfg [], data, layer [ lineSpec, labelSpec ] ]

```
