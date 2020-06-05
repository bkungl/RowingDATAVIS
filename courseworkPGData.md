---
id: litvis
narrative-schemas:
  - ../narrative-schemas/courseworkPG.yml

elm:
  dependencies:
    gicentre/elm-vegalite: latest
    gicentre/tidy: latest
---

@import "../css/datavis.less"

```elm {l=hidden}
import Tidy exposing (..)
import VegaLite exposing (..)
```

This sheet contains a variety of data. For this project, due to the COVID-19 situation I tried to make my workload easier by doing that majority of the data filtering beforehand instead of by using Tidy etc. I orgininally was collecting data by downloading PDF files and using an online tool to convert these files into excel format. I found later on that it was easier to manually transcribe data from WorldRowing.org instead of trying to use scripts/tools. Here is my most commonly used data:

```elm {l=hidden}
data2 =
    dataFromUrl "data/twoyears.csv"
```

```elm {l=hidden}
data3 =
    dataFromUrl "data/500m.csv"
```

```elm {l=hidden}
data4 =
    dataFromUrl "data/countries.csv"
```

```elm {l=hidden}
goldenRatio : Float
goldenRatio =
    1.618
```

```elm {l=hidden}
entryDataTable : Table
entryDataTable =
    """year,country,place,worldrowingname,weight
2019,ITA,1,ITA,Lightweight
2019,HUN,2,HUN,Lightweight
2019,AUS,3,AUS,Lightweight
2019,MEX,4,MEX,Lightweight
2019,GBR,5,GBR,Lightweight
2019,CAN,6,CAN,Lightweight
2019,AUT,7,AUT,Lightweight
2019,POL,8,POL,Lightweight
2019,USA,9,USA,Lightweight
2019,IRL,10,IRL,Lightweight
2019,CHE,11,SUI,Lightweight
2019,TUR,12,TUR,Lightweight
2019,SVN,13,SLO,Lightweight
2019,DEU,14,GER,Lightweight
2019,BRA,15,BRA,Lightweight
2019,SRB,16,SRB,Lightweight
2019,HRV,17,CRO,Lightweight
2019,JPN,18,JPN,Lightweight
2019,NZL,19,NZL,Lightweight
2019,GRC,20,GRE,Lightweight
2019,CZE,21,CZE,Lightweight
2019,NOR,22,NOR,Lightweight
2019,CHN,24,CHN,Lightweight
2019,KOR,25,KOR,Lightweight
2019,SVK,26,SVK,Lightweight
2019,ESP,27,ESP,Lightweight
2019,BEL,28,BEL,Lightweight
2019,SWE,29,SWE,Lightweight
2019,URY,30,URU,Lightweight
2019,IND,31,IND,Lightweight
2019,PRT,32,POR,Lightweight
2019,UZB,33,UZB,Lightweight
2018,DEU,1,GER,Lightweight
2018,CHE,2,SUI,Lightweight
2018,USA,3,USA,Lightweight
2018,CAN,4,CAN,Lightweight
2018,CHN,5,CHN,Lightweight
2018,HUN,6,HUN,Lightweight
2018,ITA,7,ITA,Lightweight
2018,GBR,8,GBR,Lightweight
2018,AUS,9,AUS,Lightweight
2018,SVN,10,SLO,Lightweight
2018,MEX,11,MEX,Lightweight
2018,FRA,12,FRA,Lightweight
2018,AUT,13,AUT,Lightweight
2018,SRB,15,SRB,Lightweight
2018,JPN,16,JPN,Lightweight
2018,FIN,17,FIN,Lightweight
2018,BGR,18,BUL,Lightweight
2018,BHS,19,BAH,Lightweight
2017,IRL,1,IRL,Lightweight
2017,NZL,2,NZL,Lightweight
2017,NOR,3,NOR,Lightweight
2017,CHE,4,SUI,Lightweight
2017,DEU,5,GER,Lightweight
2017,BRA,6,BRA,Lightweight
2017,MEX,7,MEX,Lightweight
2017,SVK,8,SVK,Lightweight
2017,HRV,9,CRO,Lightweight
2017,USA,10,USA,Lightweight
2017,HUN,11,HUN,Lightweight
2017,POL,12,POL,Lightweight
2017,SVN,13,SLO,Lightweight
2017,CAN,14,CAN,Lightweight
2017,ITA,15,ITA,Lightweight
2017,CZE,16,CZE,Lightweight
2017,UZB,17,UZB,Lightweight
2017,TUR,18,TUR,Lightweight
2017,CUB,19,CUB,Lightweight
2017,KOR,20,KOR,Lightweight
2017,GRC,21,GRE,Lightweight
2017,THA,22,THA,Lightweight
2017,GTM,23,GUA,Lightweight
2017,ECU,24,ECU,Lightweight
2017,NGA,25,NGR,Lightweight
2016,IRL,1,IRL,Lightweight
2016,HUN,2,HUN,Lightweight
2016,SVK,3,SVK,Lightweight
2016,SVN,4,SLO,Lightweight
2016,DEU,5,GER,Lightweight
2016,SRB,6,SRB,Lightweight
2016,ESP,7,ESP,Lightweight
2016,JPN,8,JPN,Lightweight
2016,ITA,9,ITA,Lightweight
2016,USA,10,USA,Lightweight
2016,HRV,11,CRO,Lightweight
2016,CHE,12,SUI,Lightweight
2016,POL,13,POL,Lightweight
2016,CHN,14,CHN,Lightweight
2016,CZE,15,CZE,Lightweight
2016,NLD,16,NED,Lightweight
2016,AUT,17,AUT,Lightweight
2016,PER,18,PER,Lightweight
2016,FIN,19,FIN,Lightweight
2016,RUS,20,RUS,Lightweight
2016,SWE,21,SWE,Lightweight
2016,THA,22,THA,Lightweight
2016,COL,23,COL,Lightweight
2016,LBY,24,LBA,Lightweight
2015,NZL,1,NZL,Lightweight
2015,SVN,2,SLO,Lightweight
2015,SRB,3,SRB,Lightweight
2015,DEU,4,GER,Lightweight
2015,USA,5,USA,Lightweight
2015,GBR,6,GBR,Lightweight
2015,BEL,7,BEL,Lightweight
2015,HUN,8,HUN,Lightweight
2015,POL,9,POL,Lightweight
2015,PER,10,PER,Lightweight
2015,HRV,11,CRO,Lightweight
2015,ITA,12,ITA,Lightweight
2015,GRC,13,GRE,Lightweight
2015,JPN,14,JPN,Lightweight
2015,BGR,15,BUL,Lightweight
2015,CHN,16,CHN,Lightweight
2015,TUR,17,TUR,Lightweight
2015,DZA,18,ALG,Lightweight
2015,UKR,19,UKR,Lightweight
2015,PRI,20,PUR,Lightweight
2015,PRT,21,POR,Lightweight
2015,TUN,22,TUN,Lightweight
2015,AUS,23,AUS,Lightweight
2015,AZE,24,AZE,Lightweight
2015,IRN,25,IRI,Lightweight
2015,IRQ,26,IRQ,Lightweight
2015,URY,27,URU,Lightweight
2015,GEO,28,GEO,Lightweight
2015,KAZ,29,KAZ,Lightweight
2015,LBY,30,LBA,Lightweight
2015,MDA,31,MDA,Lightweight
2015,MAR,32,MAR,Lightweight
2014,ITA,1,ITA,Lightweight
2014,DEU,2,GER,Lightweight
2014,CHE,3,SUI,Lightweight
2014,IRL,4,IRL,Lightweight
2014,AUS,5,AUS,Lightweight
2014,PRT,6,POR,Lightweight
2014,CHN,7,CHN,Lightweight
2014,DNK,8,DEN,Lightweight
2014,SVK,9,SVK,Lightweight
2014,GBR,10,GBR,Lightweight
2014,HUN,11,HUN,Lightweight
2014,JPN,12,JPN,Lightweight
2014,USA,13,USA,Lightweight
2014,ESP,14,ESP,Lightweight
2014,RUS,15,RUS,Lightweight
2014,FRA,16,FRA,Lightweight
2014,SVN,17,SLO,Lightweight
2014,ARG,18,ARG,Lightweight
2014,AZE,19,AZE,Lightweight
2014,BGR,20,BUL,Lightweight
2014,TUN,21,TUN,Lightweight
2014,DZA,22,ALG,Lightweight
2014,PER,23,PER,Lightweight
2014,SRB,24,SRB,Lightweight
2014,VUT,25,VAN,Lightweight
2014,ARM,26,ARM,Lightweight
2014,QAT,27,QAT,Lightweight
2013,DNK,1,DEN,Lightweight
2013,FRA,2,FRA,Lightweight
2013,HUN,3,HUN,Lightweight
2013,PRT,4,POR,Lightweight
2013,DEU,5,GER,Lightweight
2013,CHE,6,SUI,Lightweight
2013,USA,7,USA,Lightweight
2013,GBR,8,GBR,Lightweight
2013,CAN,9,CAN,Lightweight
2013,MEX,10,MEX,Lightweight
2013,BGR,11,BUL,Lightweight
2013,ITA,12,ITA,Lightweight
2013,NZL,13,NZL,Lightweight
2013,GRC,14,GRE,Lightweight
2013,SVN,15,SLO,Lightweight
2013,GTM,16,GUA,Lightweight
2013,TUN,17,TUN,Lightweight
2013,JPN,18,JPN,Lightweight
2013,KOR,19,KOR,Lightweight
2013,IDN,20,INA,Lightweight
2013,PHL,21,PHI,Lightweight
2013,MYS,22,MAS,Lightweight
2013,UGA,24,UGA,Lightweight
2013,LBY,25,LBA,Lightweight
2013,VUT,26,VAN,Lightweight
2013,KEN,27,KEN,Lightweight
2013,ZMB,28,ZAM,Lightweight
2012,DNK,1,DEN,Lightweight
2012,HUN,2,HUN,Lightweight
2012,USA,3,USA,Lightweight
2012,ITA,4,ITA,Lightweight
2012,AUT,5,AUT,Lightweight
2012,CHE,6,SUI,Lightweight
2012,FRA,7,FRA,Lightweight
2012,POL,8,POL,Lightweight
2012,BGR,9,BUL,Lightweight
2012,SVK,10,SVK,Lightweight
2012,MEX,11,MEX,Lightweight
2012,SVN,12,SLO,Lightweight
2012,DEU,13,GER,Lightweight
2012,BRA,14,BRA,Lightweight
2012,IRL,15,IRL,Lightweight
2012,PER,17,PER,Lightweight
2012,JPN,18,JPN,Lightweight
2012,TUN,19,TUN,Lightweight
2012,KOR,20,KOR,Lightweight
2012,LTU,21,LTU,Lightweight
2012,AUS,22,AUS,Lightweight
2012,ZMB,23,ZAM,Lightweight
2011,DNK,1,DEN,Lightweight
2011,ITA,2,ITA,Lightweight
2011,NZL,3,NZL,Lightweight
2011,USA,4,USA,Lightweight
2011,FRA,5,FRA,Lightweight
2011,GBR,6,GBR,Lightweight
2011,NOR,7,NOR,Lightweight
2011,DEU,8,GER,Lightweight
2011,TUR,10,TUR,Lightweight
2011,EGY,11,EGY,Lightweight
2011,SVN,12,SLO,Lightweight
2011,PRT,13,POR,Lightweight
2011,AZE,14,AZE,Lightweight
2011,DZA,15,ALG,Lightweight
2011,AGO,16,ANG,Lightweight
2011,THA,17,THA,Lightweight
2011,ARM,18,ARM,Lightweight
2010,ITA,1,ITA,Lightweight
2010,SVK,2,SVK,Lightweight
2010,HUN,3,HUN,Lightweight
2010,DNK,4,DEN,Lightweight
2010,JPN,5,JPN,Lightweight
2010,GBR,6,GBR,Lightweight
2010,NZL,7,NZL,Lightweight
2010,DEU,8,GER,Lightweight
2010,NLD,9,NED,Lightweight
2010,USA,10,USA,Lightweight
2010,AUT,11,AUT,Lightweight
2010,BRA,12,BRA,Lightweight
2010,PER,13,PER,Lightweight
2019,DEU,1,GER,Heavyweight
2019,DNK,2,DEN,Heavyweight
2019,NOR,3,NOR,Heavyweight
2019,LTU,4,LTU,Heavyweight
2019,NLD,5,NED,Heavyweight
2019,CZE,6,CZE,Heavyweight
2019,NZL,7,NZL,Heavyweight
2019,HRV,8,CRO,Heavyweight
2019,ITA,9,ITA,Heavyweight
2019,POL,10,POL,Heavyweight
2019,GRC,11,GRE,Heavyweight
2019,AZE,12,AZE,Heavyweight
2019,BGR,13,BUL,Heavyweight
2019,GBR,14,GBR,Heavyweight
2019,ISR,15,ISR,Heavyweight
2019,SRB,16,SRB,Heavyweight
2019,MEX,17,MEX,Heavyweight
2019,FRA,18,FRA,Heavyweight
2019,CHE,19,SUI,Heavyweight
2019,TUR,20,TUR,Heavyweight
2019,USA,21,USA,Heavyweight
2019,SWE,22,SWE,Heavyweight
2019,ROU,23,ROU,Heavyweight
2019,EGY,24,EGY,Heavyweight
2019,CUB,25,CUB,Heavyweight
2019,ESP,26,ESP,Heavyweight
2019,AUT,27,AUT,Heavyweight
2019,KAZ,28,KAZ,Heavyweight
2019,FIN,29,FIN,Heavyweight
2019,UZB,30,UZB,Heavyweight
2019,KOR,31,KOR,Heavyweight
2019,THA,32,THA,Heavyweight
2019,PRI,34,PUR,Heavyweight
2019,PRY,35,PAR,Heavyweight
2019,SVK,36,SVK,Heavyweight
2019,IND,37,IND,Heavyweight
2019,BMU,38,BER,Heavyweight
2019,BEN,39,BEN,Heavyweight
2019,EST,40,EST,Heavyweight
2019,VUT,41,VAN,Heavyweight
2019,ARE,42,UAE,Heavyweight
2018,NOR,1,NOR,Heavyweight
2018,CZE,2,CZE,Heavyweight
2018,LTU,3,LTU,Heavyweight
2018,GBR,4,GBR,Heavyweight
2018,NZL,5,NZL,Heavyweight
2018,DEU,6,GER,Heavyweight
2018,DNK,7,DEN,Heavyweight
2018,POL,8,POL,Heavyweight
2018,CHE,9,SUI,Heavyweight
2018,BLR,10,BLR,Heavyweight
2018,SRB,11,SRB,Heavyweight
2018,HUN,12,HUN,Heavyweight
2018,ISR,13,ISR,Heavyweight
2018,BEL,14,BEL,Heavyweight
2018,FIN,15,FIN,Heavyweight
2018,FRA,16,FRA,Heavyweight
2018,ITA,17,ITA,Heavyweight
2018,TUR,18,TUR,Heavyweight
2018,AUS,19,AUS,Heavyweight
2018,USA,20,USA,Heavyweight
2018,AZE,21,AZE,Heavyweight
2018,ZAF,22,RSA,Heavyweight
2018,SWE,24,SWE,Heavyweight
2018,EGY,25,EGY,Heavyweight
2018,EST,26,EST,Heavyweight
2018,UKR,27,UKR,Heavyweight
2018,PRY,28,PAR,Heavyweight
2018,THA,29,THA,Heavyweight
2018,BEN,30,BEN,Heavyweight
2018,VUT,31,VAN,Heavyweight
2017,CZE,1,CZE,Heavyweight
2017,CUB,2,CUB,Heavyweight
2017,GBR,3,GBR,Heavyweight
2017,HRV,4,CRO,Heavyweight
2017,NZL,5,NZL,Heavyweight
2017,DEU,6,GER,Heavyweight
2017,POL,7,POL,Heavyweight
2017,SRB,8,SRB,Heavyweight
2017,CHE,9,SUI,Heavyweight
2017,RUS,10,RUS,Heavyweight
2017,NLD,11,NED,Heavyweight
2017,DNK,12,DEN,Heavyweight
2017,FRA,13,FRA,Heavyweight
2017,BLR,14,BLR,Heavyweight
2017,ZAF,15,RSA,Heavyweight
2017,FIN,16,FIN,Heavyweight
2017,ARG,17,ARG,Heavyweight
2017,JPN,18,JPN,Heavyweight
2017,USA,19,USA,Heavyweight
2017,MEX,20,MEX,Heavyweight
2017,KOR,21,KOR,Heavyweight
2017,SWE,22,SWE,Heavyweight
2017,BRA,24,BRA,Heavyweight
2017,ISR,25,ISR,Heavyweight
2017,ITA,26,ITA,Heavyweight
2017,ESP,27,ESP,Heavyweight
2017,UKR,28,UKR,Heavyweight
2017,ZWE,29,ZIM,Heavyweight
2017,PRY,30,PAR,Heavyweight
2017,PRI,31,PUR,Heavyweight
2017,CHN,32,CHN,Heavyweight
2017,TUN,33,TUN,Heavyweight
2017,BEN,34,BEN,Heavyweight
2017,SLV,35,ESA,Heavyweight
2017,AZE,36,AZE,Heavyweight
2017,UZB,37,UZB,Heavyweight
2017,VNM,39,VIN,Heavyweight
2017,BHS,40,BAH,Heavyweight
2016,NZL,1,NZL,Heavyweight
2016,HRV,2,CRO,Heavyweight
2016,CZE,3,CZE,Heavyweight
2016,BEL,4,BEL,Heavyweight
2016,BLR,5,BLR,Heavyweight
2016,CUB,6,CUB,Heavyweight
2016,POL,7,POL,Heavyweight
2016,MEX,8,MEX,Heavyweight
2016,AUS,9,AUS,Heavyweight
2016,EGY,10,EGY,Heavyweight
2016,NOR,11,NOR,Heavyweight
2016,GBR,12,GBR,Heavyweight
2016,IND,13,IND,Heavyweight
2016,HUN,14,HUN,Heavyweight
2016,ARG,15,ARG,Heavyweight
2016,IDN,16,INA,Heavyweight
2016,KOR,17,KOR,Heavyweight
2016,URY,18,URU,Heavyweight
2016,LTU,19,LTU,Heavyweight
2016,PER,20,PER,Heavyweight
2016,IRQ,21,IRQ,Heavyweight
2016,UZB,22,UZB,Heavyweight
2016,DZA,23,ALG,Heavyweight
2016,PRY,24,PAR,Heavyweight
2016,ZWE,25,ZIM,Heavyweight
2016,THA,26,THA,Heavyweight
2016,TUN,27,TUN,Heavyweight
2016,ECU,28,ECU,Heavyweight
2016,VEN,29,VEN,Heavyweight
2016,VUT,30,VAN,Heavyweight
2016,KAZ,31,KAZ,Heavyweight
2016,LBY,32,LBA,Heavyweight
2015,CZE,1,CZE,Heavyweight
2015,NZL,2,NZL,Heavyweight
2015,LTU,3,LTU,Heavyweight
2015,NOR,4,NOR,Heavyweight
2015,HRV,5,CRO,Heavyweight
2015,CUB,6,CUB,Heavyweight
2015,BLR,7,BLR,Heavyweight
2015,GBR,8,GBR,Heavyweight
2015,POL,9,POL,Heavyweight
2015,ISR,10,ISR,Heavyweight
2015,DNK,11,DEN,Heavyweight
2015,BEL,12,BEL,Heavyweight
2015,MEX,13,MEX,Heavyweight
2015,DEU,14,GER,Heavyweight
2015,FIN,15,FIN,Heavyweight
2015,CAN,16,CAN,Heavyweight
2015,CHN,17,CHN,Heavyweight
2015,BRA,18,BRA,Heavyweight
2015,UKR,19,UKR,Heavyweight
2015,CHE,20,SUI,Heavyweight
2015,USA,21,USA,Heavyweight
2015,ARG,22,ARG,Heavyweight
2015,SRB,23,SRB,Heavyweight
2015,ITA,25,ITA,Heavyweight
2015,ROU,26,ROU,Heavyweight
2015,KOR,27,KOR,Heavyweight
2015,KAZ,28,KAZ,Heavyweight
2015,EGY,29,EGY,Heavyweight
2015,IDN,30,INA,Heavyweight
2015,ZWE,31,ZIM,Heavyweight
2015,TUN,32,TUN,Heavyweight
2015,UZB,33,UZB,Heavyweight
2015,URY,34,URU,Heavyweight
2015,BEN,35,BEN,Heavyweight
2015,IRQ,36,IRQ,Heavyweight
2015,SWE,37,SWE,Heavyweight
2015,VUT,38,VAN,Heavyweight
2015,LBY,39,LBA,Heavyweight
2015,PRI,40,PUR,Heavyweight
2015,CIV,41,CIV,Heavyweight
2014,CZE,1,CZE,Heavyweight
2014,NZL,2,NZL,Heavyweight
2014,CUB,3,CUB,Heavyweight
2014,LTU,4,LTU,Heavyweight
2014,DEU,5,GER,Heavyweight
2014,AZE,6,AZE,Heavyweight
2014,BLR,7,BLR,Heavyweight
2014,BEL,8,BEL,Heavyweight
2014,NLD,9,NED,Heavyweight
2014,CAN,10,CAN,Heavyweight
2014,ITA,11,ITA,Heavyweight
2014,CHE,12,SUI,Heavyweight
2014,AUS,13,AUS,Heavyweight
2014,NOR,14,NOR,Heavyweight
2014,FIN,15,FIN,Heavyweight
2014,HRV,16,CRO,Heavyweight
2014,DNK,17,DEN,Heavyweight
2014,MEX,18,MEX,Heavyweight
2014,CHN,19,CHN,Heavyweight
2014,ESP,20,ESP,Heavyweight
2014,FRA,21,FRA,Heavyweight
2014,ARG,22,ARG,Heavyweight
2014,ROU,23,ROU,Heavyweight
2014,SVN,24,SLO,Heavyweight
2014,USA,25,USA,Heavyweight
2014,PER,26,PER,Heavyweight
2014,UKR,27,UKR,Heavyweight
2014,ZWE,28,ZIM,Heavyweight
2014,PRY,29,PAR,Heavyweight
2014,PRI,30,PUR,Heavyweight
2014,QAT,31,QAT,Heavyweight
2013,CZE,1,CZE,Heavyweight
2013,CUB,2,CUB,Heavyweight
2013,DEU,3,GER,Heavyweight
2013,GBR,4,GBR,Heavyweight
2013,NLD,5,NED,Heavyweight
2013,LTU,6,LTU,Heavyweight
2013,AZE,7,AZE,Heavyweight
2013,BGR,8,BUL,Heavyweight
2013,ISR,9,ISR,Heavyweight
2013,RUS,10,RUS,Heavyweight
2013,CHE,11,SUI,Heavyweight
2013,IND,12,IND,Heavyweight
2013,USA,13,USA,Heavyweight
2013,AUS,14,AUS,Heavyweight
2013,ROU,15,ROU,Heavyweight
2013,KOR,16,KOR,Heavyweight
2013,HUN,17,HUN,Heavyweight
2013,UZB,18,UZB,Heavyweight
2013,KAZ,19,KAZ,Heavyweight
2013,UKR,20,UKR,Heavyweight
2013,IRQ,21,IRQ,Heavyweight
2013,VEN,23,VEN,Heavyweight
2013,MDA,24,MDA,Heavyweight
2013,IDN,25,INA,Heavyweight
2013,SLV,26,ESA,Heavyweight
2013,NAM,27,NAM,Heavyweight
2013,ZMB,28,ZAM,Heavyweight
2013,MYS,29,MAS,Heavyweight
2013,QAT,30,QAT,Heavyweight
2012,NZL,1,NZL,Heavyweight
2012,CZE,2,CZE,Heavyweight
2012,GBR,3,GBR,Heavyweight
2012,SWE,4,SWE,Heavyweight
2012,AZE,5,AZE,Heavyweight
2012,DEU,6,GER,Heavyweight
2012,CUB,7,CUB,Heavyweight
2012,LTU,8,LTU,Heavyweight
2012,NOR,9,NOR,Heavyweight
2012,ARG,10,ARG,Heavyweight
2012,CHN,11,CHN,Heavyweight
2012,BEL,12,BEL,Heavyweight
2012,DNK,13,DEN,Heavyweight
2012,MEX,14,MEX,Heavyweight
2012,HRV,15,CRO,Heavyweight
2012,IND,16,IND,Heavyweight
2012,POL,17,POL,Heavyweight
2012,BRA,19,BRA,Heavyweight
2012,EGY,20,EGY,Heavyweight
2012,KOR,21,KOR,Heavyweight
2012,IRN,22,IRI,Heavyweight
2012,CHL,23,CHI,Heavyweight
2012,USA,24,USA,Heavyweight
2012,PER,27,PER,Heavyweight
2012,KAZ,28,KAZ,Heavyweight
2012,SLV,29,ESA,Heavyweight
2012,ZWE,30,ZIM,Heavyweight
2012,TUN,31,TUN,Heavyweight
2012,CMR,32,CMR,Heavyweight
2012,NER,33,NIG,Heavyweight
2011,NZL,1,NZL,Heavyweight
2011,CZE,2,CZE,Heavyweight
2011,GBR,3,GBR,Heavyweight
2011,DEU,4,GER,Heavyweight
2011,SWE,5,SWE,Heavyweight
2011,NOR,6,NOR,Heavyweight
2011,LTU,7,LTU,Heavyweight
2011,CUB,8,CUB,Heavyweight
2011,AZE,9,AZE,Heavyweight
2011,CHN,10,CHN,Heavyweight
2011,USA,11,USA,Heavyweight
2011,BGR,12,BUL,Heavyweight
2011,BEL,13,BEL,Heavyweight
2011,CYP,14,CYP,Heavyweight
2011,MEX,15,MEX,Heavyweight
2011,HRV,16,CRO,Heavyweight
2011,IND,17,IND,Heavyweight
2011,AUS,18,AUS,Heavyweight
2011,SVK,19,SVK,Heavyweight
2011,IRN,20,IRI,Heavyweight
2011,CHL,21,CHI,Heavyweight
2011,ISR,22,ISR,Heavyweight
2011,CHE,23,SUI,Heavyweight
2011,ITA,24,ITA,Heavyweight
2011,SVN,25,SLO,Heavyweight
2011,EST,26,EST,Heavyweight
2011,IRQ,27,IRQ,Heavyweight
2011,PRY,28,PAR,Heavyweight
2011,MDA,29,MDA,Heavyweight
2011,SLV,30,ESA,Heavyweight
2011,COL,31,COL,Heavyweight
2011,CMR,32,CMR,Heavyweight
2011,GEO,33,GEO,Heavyweight
2011,KEN,34,KEN,Heavyweight
2010,CZE,1,CZE,Heavyweight
2010,NZL,2,NZL,Heavyweight
2010,GBR,3,GBR,Heavyweight
2010,NOR,4,NOR,Heavyweight
2010,CHN,5,CHN,Heavyweight
2010,SVN,6,SLO,Heavyweight
2010,SWE,7,SWE,Heavyweight
2010,LTU,8,LTU,Heavyweight
2010,EST,9,EST,Heavyweight
2010,DEU,10,GER,Heavyweight
2010,BEL,11,BEL,Heavyweight
2010,USA,12,USA,Heavyweight
2010,NLD,13,NED,Heavyweight
2010,AUS,14,AUS,Heavyweight
2010,RUS,15,RUS,Heavyweight
2010,HRV,16,CRO,Heavyweight
2010,CAN,17,CAN,Heavyweight
2010,SVK,18,SVK,Heavyweight
2010,CHE,19,SUI,Heavyweight
2010,FIN,20,FIN,Heavyweight
2010,FRA,21,FRA,Heavyweight"""
        |> fromCSV
```

```elm {l=hidden}
entryNumsTable : Table
entryNumsTable =
    """year,weightclass,entries
2019,Lightweight,33
2018,Lightweight,19
2017,Lightweight,25
2016,Lightweight,24
2015,Lightweight,32
2014,Lightweight,27
2013,Lightweight,28
2012,Lightweight,23
2011,Lightweight,18
2010,Lightweight,14
2019,Heavyweight,42
2018,Heavyweight,31
2017,Heavyweight,40
2016,Heavyweight,32
2015,Heavyweight,41
2014,Heavyweight,31
2013,Heavyweight,30
2012,Heavyweight,33
2011,Heavyweight,34
2010,Heavyweight,21"""
        |> fromCSV
```

```elm {l=hidden}
lightweightEntryNumsTable : Table
lightweightEntryNumsTable =
    """year,entries
2019,33
2018,19
2017,25
2016,24
2015,32
2014,27
2013,28
2012,23
2011,18
2010,14"""
        |> fromCSV
```

```elm {l=hidden}
heavyweightEntryNumsTable : Table
heavyweightEntryNumsTable =
    """year,entries
2019,42
2018,31
2017,40
2016,32
2015,41
2014,31
2013,30
2012,33
2011,34
2010,21"""
        |> fromCSV
```

```elm {l=hidden}
winnerTable : Table
winnerTable =
    entryDataTable
        |> filterRows "place" (\v -> v == "1")
        |> filterColumns (\c -> c == "year" || c == "country" || c == "place")
```

```elm {l=hidden}
showWinnerTable : List String
showWinnerTable =
    tableSummary 10 winnerTable
```

```elm {l=hidden}
medalistTable : Table
medalistTable =
    entryDataTable
        |> filterRows "place" (\v -> v == "1" || v == "2" || v == "3")
        --|> filterRows "year" (\v -> v == "2019")
        |> filterColumns (\c -> c == "year" || c == "country" || c == "place")
```

```elm {l=hidden}
showMedalistTable : List String
showMedalistTable =
    tableSummary 10 medalistTable
```

```elm {l=hidden}
slowTable : Table
slowTable =
    entryDataTable
        |> filterRows "place" (\v -> v >= "19")
        |> filterColumns (\v -> v == "year" || v == "country" || v == "place")
```

```elm {l=hidden}
showSlowTable : List String
showSlowTable =
    tableSummary 10 slowTable
```

```elm {l=hidden}
testTable : Table
testTable =
    """year,country,place,worldrowingname,weight
2019,ITA,1,ITA,Lightweight
2019,HUN,2,HUN,Lightweight
2019,AUS,3,AUS,Lightweight
2019,MEX,4,MEX,Lightweight
2019,GBR,5,GBR,Lightweight
2019,CAN,6,CAN,Heavyweight
2019,AUS,3,AUS,Heavyweight
2019,MEX,4,MEX,Heavyweight
2019,GBR,5,GBR,Heavyweight
2019,CAN,6,CAN,Heavyweight
"""
        |> fromCSV
```
