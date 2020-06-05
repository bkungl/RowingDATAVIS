---
follows: courseworkPGData.md
id: litvis
---

@import "../css/datavis.less"

# Postgraduate Coursework Template

{(questions|}

- **[RQ1]** Has the ideal race strategy for winning the World Rowing Championship as a lightweight male single sculler changed in the past decade? Has the speed of athletes improved?
- **[RQ2]** How does geographic location factor into race attendence from countries of lower [Gross Domestic Products](https://worldpopulationreview.com/countries/countries-by-gdp/#worldCountries) ? Has the event itself effectively produced more geographic inclusion due to all athletes being weight restricted?

{|questions)}

{(visualization|}

### [RQ1] Analysis of Lightweight Single Sculler World Champion Race Performance by Year

```elm {v interactive }
{--
TODO

- On click open 500m splits and Placement (maybe something from session 6 interactive legend)
- Clean up, style (circle size, etc), wanted to remove the 00:0 part of the times.
- Line for average time
--}


raceTimes : Spec
raceTimes =
    let
        data =
            dataFromUrl "data/500m.csv" [ parse [ ( "total", foDate "%M:%S.%L" ) ] ]

        enc =
            encoding
                << position X
                    [ pName "year"
                    , pNominal
                    , pTitle "Year"
                    ]
                << position Y
                    [ pName "total"
                    , pOrdinal
                    , pTimeUnit minutesSeconds
                    , pSort [ soByChannel chY, soDescending ]
                    , pTitle "Total Race Time"
                    ]
                << tooltips
                    [ [ tName "total"
                      , tNominal
                      , tTitle "Total Time"
                      , tTimeUnit minutesSeconds

                      --, tFormat "%M:%S.%L"
                      ]
                    , [ tName "country"
                      , tNominal
                      , tTitle "Country"
                      ]
                    ]

        cfg =
            configure
                << configuration (coAxis [ axcoLabelAngle 0 ])
    in
    toVegaLite
        [ height 175
        , width (1000 / goldenRatio)
        , data
        , enc []
        , circle []
        , cfg []
        , title "Total Race Duration (2001-2019)" []
        ]
```

---

```elm {interactive l=hidden v}
quickTest : Spec
quickTest =
    let
        sel =
            selection
                << select "mySelection" seInterval [ seBindScales, seEncodings [ chY ] ]

        enc =
            encoding
                << position X
                    [ pName "dist"
                    , pQuant
                    , pTitle "Distance"

                    --this doesnt work as well as it could but if i cant improve, oh well
                    , pScale [ scDomain (doNums [ 100, 1970 ]) ]
                    ]
                << position Y
                    [ pName "speed"
                    , pQuant
                    , pTitle "Speed (m/s)"
                    , pScale [ scDomain (doNums [ 4.2, 5.2 ]) ]
                    ]
                << color
                    [ mName "sr"
                    , mNominal --need to find way to make a key show
                    , mTitle "Strokes/Minute"
                    , mScale [ scScheme "reds" [] ]
                    ]
                --this is the hover feature if this map is interactive
                << tooltips [ [ tName "sr", tNominal, tTitle "Stroke Rate" ], [ tName "dist", tNominal, tTitle "Race Distance (m)" ] ]

        --fix axis/legend
        res =
            resolve
                --try to set the axes: (label each x axis)
                << resolution (reAxis [ ( chX, reIndependent ), ( chY, reIndependent ) ])
                --this is supposed to set the scale (dont start at 0)
                --but this one gives the legend on each graph
                << resolution (reScale [ ( chColor, reIndependent ) ])
                --set the legend
                << resolution (reLegend [ ( chColor, reIndependent ) ])
    in
    toVegaLite
        [ data2 []
        , enc []
        , columns 2
        , res []
        , facetFlow [ fName "year", fNominal, fHeader [ hdTitle "Comparison of Race Performance of LM1X World Champion (By Year)" ] ]
        , specification (asSpec [ width (450 / goldenRatio), height 120, bar [], enc [], sel [] ])
        ]
```

---

### [RQ2] Comparion of Geographic Inclusion between the Lightweight and Heavyweight Single Event

```elm {l=hidden v interactive}
{--
TODO For Map

- filter by placement range (gold medal, any medal, > c final, etc)
- show all by default until selector is touched
- make 1 2 and 3 get gold, silver, bronze color, rest scale?
- i wanted to be able to zoom into places like europe but I think this functionality doesn't work on maps
--}


regionMap2 : Spec
regionMap2 =
    let
        --get data
        geoData =
            dataFromUrl "https://gicentre.github.io/data/geoTutorials/world-110m.json"
                [ topojsonFeature "countries1" ]

        --more data stuff:
        entryData =
            dataFromColumns []
                --real data is called entryDataTable
                --test data is called testTable
                --can also access slowTable,medalistTable
                << dataColumn "Year" (numColumn "year" entryDataTable |> nums)
                << dataColumn "Country" (strColumn "country" entryDataTable |> strs)
                << dataColumn "Place" (numColumn "place" entryDataTable |> nums)
                << dataColumn "Weight" (strColumn "weight" entryDataTable |> strs)

        --Projections
        proj =
            projection [ prType equirectangular ]

        --encodings
        enc =
            encoding
                --most recent working
                << shape [ mName "geo", mMType GeoFeature ]
                {--<< color
                    [ mLegend [ leTitle "Country8594358293047342058783457" ]
                    , mSelectionCondition (selectionName "mySelection") [] [ mStr "teal" ]
                    ]
                    --}
                << color
                    [ mName "Place"
                    , mMType Quantitative
                    , mScale
                        [ scScheme "lightorange" [ 0.8, 0 ]

                        --33 is the record high entires.. was trying to find a way to give 1 2 and 3 their medal colors then do the scale, but had no luck
                        , scDomain (doNums [ 1, 35 ])
                        ]

                    {--
                    , mScale
                        (categoricalDomainMap
                            [ ( "1", "gold" ) --gold
                            , ( "2", "silver" ) --silver
                            , ( "3", "brown" ) --bronze
                            , ( "4", "hsl(0,70%,50%)" )
                            ]
                        )--}
                    , mLegend [ leTitle "Placement Result" ]
                    ]
                << tooltips
                    [ [ tName "Country", tNominal, tTitle "Country Code" ]
                    , [ tName "Year", tNominal, tTitle "Year" ]
                    , [ tName "Place", tNominal, tTitle "Result" ]
                    ]

        sel =
            selection
                << select "mySelection"
                    seSingle
                    [ seBind
                        [ iRange "yearSlider"
                            [ inName "Year", inMin 2010, inMax 2019, inStep 1 ]
                        ]
                    , seInit [ ( "yearSlider", num 2016 ) ]
                    ]
                << select "myWeightSelection"
                    seSingle
                    [ seFields [ "Weight" ]
                    , seInit [ ( "Weight", str "Heavyweight" ) ]
                    , seBind [ iRadio "Weight" [ inName "Lightweight or Heavyweight: ", inOptions [ "Lightweight", "Heavyweight" ] ] ]
                    ]

        -- TODO:
        -- figure out how to label numbers ("medalists" instead of 3)
        -- clear selection to avoid selection filters and show all data
        {--i decided to remove this because it doesnt really have to do with inclusion but the color shading still helps indicate placement by country
                << select "myPlacementSelection"
                    seSingle
                    [ seFields [ "Place" ]
                    , seInit [ ( "Place", str "50" ) ]
                    , seBind [ iSelect "Place" [ inName "Placement   ", inOptions [ "1", "3", "6", "50" ] ] ]
                    ]
                    --}
        --transformations
        --i had to remove HKG because its inside china
        trans =
            transform
                --use something similar to this for multiple slectors
                << filter (fiExpr "datum.Year == mySelection_yearSlider && datum.Weight == myWeightSelection_Weight")
                --<< filter (fiExpr "datum.Year == mySelection_yearSlider")
                << lookupAs "Country" geoData "id" "geo"

        --specs
        backgroundSpec =
            asSpec [ geoData, geoshape [ maFill "white", maStroke "black" ] ]

        entrySpec =
            asSpec [ geoshape [], enc [], trans [], sel [] ]

        cfg =
            configure
                << configuration (coView [ vicoStroke Nothing ])
                << configuration (coLegend [ lecoOrient loBottomRight, lecoOffset 1 ])

        --publish to vega
    in
    toVegaLite
        [ height 500
        , width (1300 / goldenRatio)
        , proj
        , entryData []
        , cfg []
        , layer [ backgroundSpec, entrySpec ]
        , title "Heavyweight 1x vs Lightweight 1x Geographic Inclusion" []
        ]
```

---

```elm {v }
{--
TODO

- Set the legend to tell that lightweight is red and heavyweight is blue
--}


aggregateEntries : Spec
aggregateEntries =
    let
        entryDataLight =
            dataFromColumns []
                << dataColumn "Year" (numColumn "year" lightweightEntryNumsTable |> nums)
                << dataColumn "Entries" (numColumn "entries" lightweightEntryNumsTable |> nums)

        lightweightColor =
            "red"

        entryDataHeavy =
            dataFromColumns []
                << dataColumn "Year" (numColumn "year" heavyweightEntryNumsTable |> nums)
                << dataColumn "Entries" (numColumn "entries" heavyweightEntryNumsTable |> nums)

        encLight =
            encoding
                << position X
                    [ pName "Year"
                    , pNominal
                    ]
                << position Y
                    [ pName "Entries"
                    , pQuant
                    ]

        encHeavy =
            encoding
                << position X
                    [ pName "Year"
                    , pNominal
                    ]
                << position Y
                    [ pName "Entries"
                    , pQuant
                    ]

        cfg =
            configure
                << configuration (coAxis [ axcoLabelAngle 0 ])

        lightSpec =
            asSpec [ line [ maFillOpacity 0.8, maColor lightweightColor ], entryDataLight [], width (600 / goldenRatio), encLight [], title "Lightweight (red) vs Heavyweight (blue) 1x Entries Per Year" [] ]

        heavySpec =
            asSpec [ line [ maFillOpacity 0.8 ], entryDataHeavy [], width (600 / goldenRatio), encHeavy [] ]
    in
    toVegaLite
        [ height 175
        , columns 2
        , cfg []
        , layer [ lightSpec, heavySpec ]
        ]
```

{|visualization)}

{(insights|}

##### RQ1 Race Strategy Over Time

The visualizations I chose for the implementation of solving my first research question were helpful when put together to get a better understanding of a race performance for a typical lightweight male sculler. I think it’s important to note that there are a lot of factors that influence the total race duration for a world champion in any given year. These factors include, but are not limited to: race timing, air temperature, UV index, water temperature, conditions and currents, humidity, weather conditions, wind speeds and directions, and recovery time in between progression races. My first visualization, the scatter plot, showed that although there is a general average best time, which from research on previous years racing conditions, indicate the average speed required to win a world championship is somewhere around the 6:52-6:59 range. I think the important things to note are that most elite race courses are constructed in a way to minimize the impact of external factors. As a result, almost all race courses are on protected lakes that are protected from wind and run in the same direction of the prevailing winds of the area. This results in a faster race time as winds are pushing competitors boats, allowing them to achieve higher speeds and utilize higher stroke rates.

These factors are helped by faceted bar chart visualization. This facet shows the race profile of each year’s World Rowing Championship lightweight single sculls winner. I think the facet was the most effective way to analyze this info as conditions vary by year and so races can’t be directly compared to each other. I couldn’t imagine a better way to put all 10 years worth of race data next to each other and make accurate comparisons. Each individual chart is zoomable and scalable so although there is a predetermined zoom ranger that isn’t perfect from every year, it is adjustable to help get a better picture about race performance for lightweight men. The visualization helps to show how in reality the race plan of a lightweight man is a bluff: racers go off the line at significantly higher speeds and stroke rates than they are able to maintain for the race. While a more even race plan (see 2016) is arguably more efficient, there comes a significant mental advantage of racing from the front instead of behind, especially when the nature of your sport has you face behind you. Winners get to have the added information of where opponents are, while losers only see what is behind them, unless they want to sacrifice their speed to turn around to check. Usually, races involve a sprint at the start that utilizes rate and power, a slower middle section which is more of a “maintain” phase, and a sprint at the end, which mostly utilizes only power. Rates don’t go up as much at the end due to the exhaustion of racers. This excludes the Kiwi racer in 2016 who did not front end focus his race and instead started more modestly and closer to his target race speed which effectively helped conserve more energy for his final sprint, where he found his fastest speeds of the whole race.
Concluding, these visualizations helped to show that although rowing technology, training methodologies, scientific information, etc has improved and evolved in the past decade significantly, the race strategy to find World Champion-level success remains the same: A superhuman effort off the first 100 meters, a gradual shift into racing pace, and then a final sprint starting around the 1750 meter mark.

##### RQ2 Geographic Inclusion from Lightweight Sculling

For my second research question, I wanted to analyze the existing debate for the value of lightweight rowing, being that lightweight rowing allows for poorer nations with traditionally smaller people to have a fair chance at competing at the highest level of rowing. Rowing is unique in that it is the only non-combat sport in the Olympics to utilize weights classes (however, this is quickly changing: Rio 2016 was the last year for the lightweight men’s coxless four, and the now postponed Tokyo 2020 was rumored to be the last year for lightweight rowing at the Olympics in general). I wanted to analyze the difference between the difference in countries entering world championships in lightweight and heavyweight events.

I used an interactive map to color the countries that entered racing each year. A great example of comparison is 2016. There is a lightweight/heavyweight toggle button and a slider to view different years. I found through this visualization that actually the opposite is true. Most successful lightweight athletes come from the richest countries, while the heavyweight event had much more geographic diversity, especially in Olympic years. While rowing is a predominantly European sport with significant interest from North America as well, the heavyweight event saw more competitors from Asia, Africa, and South American then the lightweight event could produce.

Finally, I produced a simple line graph showing the difference between entries of lightweight against heavyweight single scullers at World Championship regattas. Every year, there were more heavyweight men than lightweights. So my visualizations found that not only are there more countries interested in racing the heavyweight event, but also those countries are located in more continents.

{|insights)}

{(designJustification|}

I thought I used a collection of simple, yet effective visualizations in order to provide helpful insight towards my research questions. I think my scatter plot isn’t very flashy but quite easily gets the point across about overall race duration in the lightweight single: first, it's completely dependent on weather. There is a huge spread of race times (6:43 through 7:32), and there is no clear trend of improvement or setback. Since this visualization was meant to be simple, I tried to keep the design as simple as possible. I was not able to get the times to exclude the 00:0 portion of the format, but I utilized information gained from Tufte to help make the graph more pleasing, such as rotating the years to make them more readable and utilizing his golden rule. I choose not to use a legend for this graph as I don’t think it added any valuable information.

The facet view of all 10 races displayed in 50m splits really shows how similar each year is from 2010 until 2019. I think the sample size is sadly too small, but World Rowing only utilized GPS tracking technology starting in 2010. I think implementing an interactive scaling feature really saves this visualization. Since each year has such a variety of racing conditions and total time duration, there isn’t really a great way to view both all data as well as change over the course of the race. Zooming too far out made it look all the same and zooming too far in cut out data on some graphs. As a result, zooming really helped give each graph the ability to be easily analyzed. Also, I thought it was important to let each graph have their own legend and color scale because in years where the weather was bad and racers could achieve stroke rates in the upper 50s each minute off the start, it was still clear to see that no matter the race conditions strokes/minute scores are significantly higher at the start and end of each race.

I thought visualizing geographic inclusion was difficult but a map is the best way to do it with options of changing the filtering. In years like 2016 it was really easy to see how heavyweight sculling outshined lightweight. There was significantly more inclusion from Africa and South America. This was further bolstered by the simple line graph included after the map. I think this was a simple but powerful addition to answering my second research question. I unfortunately could not figure out how to implement one legend that uses two different datasets so the line graph doesn’t have a legend which absolutely detracts from the visualization. I also thought keeping lightweights red was helpful as it was the same color as the speed charts in RQ1.

{|designJustification)}

{(validation|}

Before I include the limitations and shortfallings of my visualizations, I thought I found success in visualizing solutions to my research questions. I think my data, especially the facet view of the 50 meter splits, really helped my understand the data on a level I could not understand on its own. The map also really helped, as looking at three letter country codes doesn't put into perspective how much lightweight rowing does or does not impact geographic inclusion. I really enoyed the topic I picked. While it wasn't very academic or important, it comes at great personal interest to me and made wokring through the challenge of learning Elm-VegaLite mostly remotely worthwhile and exciting. I can definitely say that lightweight racing hasn't really changed in the past decade, and can objectively argue that it produces more inclusion (the heavyweight counterpart event clearly outshone it).

I’m hoping to categorize my thoughts related to my visualizations into two parts: lack of programming knowledge due to COVID-19 and legitimate limitations of my visualizations. I thought it was important to include my “TODO”s into my submitted project in order to give a better indication of what kind of things I would have liked to accomplish.

I was really hoping to evolve the first visualization for RQ1, the circle chart, to have an interactive “onClick” feature that would open the race performance showing the racer’s placement at each 500m mark and each 500m split time. While I wasn’t able to implement this, I hope it did not severely impact the information presented in RQ1 visualizations.

Furthermore, I wanted to ability to zoom into the map, which is not currently a feature available in elm-vegalite, include a more informative legend for the line graphs in my final line graph, have special color distinctions for medalists on the map, be able to filter specific results on the map, such as seeing the six slowest competitors by year, showing all countries for all years my default until a selection is provided.

In regards to limitations of my visualizations, I think I made a few mistakes that if I had the time to start over I would try to change. To begin with, I realized too late that the heavyweight single is an Olympic Class event, while the lightweight event is not. The lightweight single is used more as a development boat than the Olympic class lightweight men’s double sculls event. This does not mean that in non-Olympic years the best lightweights don’t race this field, but may impact the data of the heavyweight single scullers. Due to the fact that the heavyweight single races for Olympic medals, and the relatively cheap (for the sport of rowing) cost of paying to produce a highly competitive single than, for say, a men’s eight, many less wealthy nations are able to support one athlete to compete for them in the Olympics for rowing. Countries like this include Cuba, Greece, Azerbaijan, Bulgaria, Serbia, Armenia, Vanuatu, etc.

Also, World Rowing does not easily provide the race conditions for analysis. If I wanted to make a more fair comparison of races between years, I would have needed more data in order to make educated assumptions and more direct analysis. It would have been interesting in determining if race speeds have actually increased but due to poor race conditions do not reflect this. In order to do this currently, I would have had to try to look up weather data from 10 years ago in 10 different places, as well as determine the direction of the wind, the direction of the race course, found formulas that help determine the exact impact of wind speeds on rowing, etc. This made this task borderline impossible with the presently available data.

{|validation)}

{(references|}

**Haroz, S., and Whitney, D.** (2012) How capacity limits of attention influence information visualization effectiveness. _IEEE Transactions on Visualization and Computer Graphics_, 18(12) pp.2402-2410.

this is for color stuff: https://vega.github.io/vega/docs/schemes/

{|references)}
