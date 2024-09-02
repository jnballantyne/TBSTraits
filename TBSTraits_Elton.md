---
title: "TBSFrugivoreTraits"
author: "J Ballantyne"
date: "2024-09-02"
output: 
  html_document: 
    keep_md: yes
---



#1 EltonTraits - Mammals
#2 EltonTraits - Birds
#3 Pantheria test and missing data added - Mammals
#4 AVOnet - Birds

#1 EltonTraits - Mammals (My try for the mammals with elton_mammals in the package "traitdata")
## Get packages

```r
if(!require(dplyr))install.packages("dplyr", dependencies = TRUE)
```

```
## Loading required package: dplyr
```

```
## 
## Attaching package: 'dplyr'
```

```
## The following objects are masked from 'package:stats':
## 
##     filter, lag
```

```
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
```

```r
if(!require(devtools))install.packages("devtools", dependencies = TRUE)
```

```
## Loading required package: devtools
```

```
## Loading required package: usethis
```

```r
if(!require(traitdata))install.packages("traitdata", dependencies = TRUE)
```

```
## Loading required package: traitdata
```

```r
if(!require(tidyr))install.packages("tidyr", dependencies = TRUE)
```

```
## Loading required package: tidyr
```
## Load packages

```r
library(dplyr)
library(devtools)
library(traitdata)
library(tidyr)
```
## Check out elton_mammals

```r
##traitdata::elton_mammals
```
## Load Tiputini species list data - file is 082924_animals2.csv

```r
TBS_animals1 <- read.csv("/Users/jacquelineballantyne/Desktop/R_2024/TCTS_data/082924_animals2.csv", header=T, as.is=T)
```
## Remove volant mammals (for now)

```r
TBS_mams <- TBS_animals1[TBS_animals1$Class == "Mammalia", ]

TBS_nonvolmams <- TBS_mams[TBS_mams$Order != "Chiroptera", ]
```
## Get rid of silly cols in Tiputini data

```r
keeplist <- c("Scientific_name", "Common_name", "IUCN" #,"Detected"
              )
TBS_nonvolmams1 <- TBS_nonvolmams[,keeplist]
```
## Load elton_mammals database

```r
data("elton_mammals")
elton_mams1 <- elton_mammals
elton_mams1$Scientific_name <- paste(elton_mams1$Genus, elton_mams1$Species, sep = " ")
```
## Get traits and headers

```r
names(elton_mams1)
```

```
##  [1] "MSW3_ID"              "Genus"                "Species"             
##  [4] "Family"               "Diet.Inv"             "Diet.Vend"           
##  [7] "Diet.Vect"            "Diet.Vfish"           "Diet.Vunk"           
## [10] "Diet.Scav"            "Diet.Fruit"           "Diet.Nect"           
## [13] "Diet.Seed"            "Diet.PlantO"          "Diet.Source"         
## [16] "Diet.Certainty"       "ForStrat.Value"       "ForStrat.Certainty"  
## [19] "ForStrat.Comment"     "Activity.Nocturnal"   "Activity.Crepuscular"
## [22] "Activity.Diurnal"     "Activity.Source"      "Activity.Certainty"  
## [25] "BodyMass.Value"       "BodyMass.Source"      "BodyMass.SpecLevel"  
## [28] "Full.Reference"       "scientificNameStd"    "Scientific_name"
```

```r
names(TBS_nonvolmams1)
```

```
## [1] "Scientific_name" "Common_name"     "IUCN"
```

## Remove undetected mammals (do this step when you get the final list)

```r
###TBS_nonvolmams_detected <- TBS_animals2[TBS_animals2$Detected == "Yes", ]
```

## elton_mams species with traits (do this once I have selected the traits to keep)

```r
#keeplist <- c("Scientific_name", "AdultBodyMass_g") ## I think I should just skip this step and see if it works then go back and just keep ones that are complete for my list or nearly so

#elton_mams2 <- elton_mams1[,keeplist]
```
## merging my list with the elton_mammals list to get traits for mine only

```r
head(elton_mams1)
```

```
##   MSW3_ID           Genus       Species            Family Diet.Inv Diet.Vend
## 1       1    Tachyglossus     aculeatus    Tachyglossidae      100         0
## 2       2       Zaglossus attenboroughi    Tachyglossidae      100         0
## 3       3       Zaglossus       bartoni    Tachyglossidae      100         0
## 4       4       Zaglossus       bruijni    Tachyglossidae      100         0
## 5       5 Ornithorhynchus      anatinus Ornithorhynchidae       80         0
## 6       6       Caluromys     philander       Didelphidae       20         0
##   Diet.Vect Diet.Vfish Diet.Vunk Diet.Scav Diet.Fruit Diet.Nect Diet.Seed
## 1         0          0         0         0          0         0         0
## 2         0          0         0         0          0         0         0
## 3         0          0         0         0          0         0         0
## 4         0          0         0         0          0         0         0
## 5         0         20         0         0          0         0         0
## 6         0          0        10         0         20         0        10
##   Diet.PlantO Diet.Source Diet.Certainty ForStrat.Value ForStrat.Certainty
## 1           0       Ref_1            ABC              G                  A
## 2           0      Ref_65            ABC              G                  A
## 3           0       Ref_2             D1              G                  A
## 4           0       Ref_1            ABC              G                  A
## 5           0       Ref_1            ABC              G                  A
## 6          40       Ref_1            ABC             Ar                  A
##   ForStrat.Comment Activity.Nocturnal Activity.Crepuscular Activity.Diurnal
## 1                                   1                    1                0
## 2                                   1                    0                0
## 3                                   1                    0                0
## 4                                   1                    0                0
## 5                                   1                    1                1
## 6                                   1                    1                0
##   Activity.Source Activity.Certainty BodyMass.Value BodyMass.Source
## 1           Ref_1                ABC        3025.00         Ref_117
## 2           Ref_1                ABC        8532.39    Ref_2, Ref_3
## 3           Ref_1                ABC        7180.00         Ref_131
## 4           Ref_1                ABC       10139.50         Ref_117
## 5           Ref_1                ABC        1484.25         Ref_117
## 6           Ref_1                ABC         229.25         Ref_117
##   BodyMass.SpecLevel
## 1                  1
## 2                  0
## 3                  1
## 4                  1
## 5                  1
## 6                  1
##                                                                                                                                                                                                                                                    Full.Reference
## 1                                                                                                                                    Nowak R.M. (1999). Walker's mammals of the world. Sixth edition edn. The Johns Hopkins University Press, Baltimore, Maryland
## 2 Leary, T., Seri, L., Flannery, T., Wright, D., Hamilton, S., Helgen, K., Singadan, R., Menzies, J., Allison, A., James, R., Aplin, K., Salas, L. & Dickman, C. 2008.�Zaglossus attenboroughi. In: IUCN 2010. IUCN Red List of Threatened Species. Version 2010.
## 3                                                                                                                                                                                                                                                   Genus Average
## 4                                                                                                                                    Nowak R.M. (1999). Walker's mammals of the world. Sixth edition edn. The Johns Hopkins University Press, Baltimore, Maryland
## 5                                                                                                                                    Nowak R.M. (1999). Walker's mammals of the world. Sixth edition edn. The Johns Hopkins University Press, Baltimore, Maryland
## 6                                                                                                                                    Nowak R.M. (1999). Walker's mammals of the world. Sixth edition edn. The Johns Hopkins University Press, Baltimore, Maryland
##          scientificNameStd          Scientific_name
## 1   Tachyglossus aculeatus   Tachyglossus aculeatus
## 2  Zaglossus attenboroughi  Zaglossus attenboroughi
## 3        Zaglossus bartoni        Zaglossus bartoni
## 4        Zaglossus bruijni        Zaglossus bruijni
## 5 Ornithorhynchus anatinus Ornithorhynchus anatinus
## 6      Caluromys philander      Caluromys philander
```

```r
TBS_nonvolmams_elton <- merge(TBS_nonvolmams, elton_mams1, by.x = "Scientific_name", by.y = "Scientific_name", all.x = T, all.y = F)
head(TBS_nonvolmams_elton) 
```

```
##       Scientific_name Lastest_revision   Code    Class     Order     Family.x
## 1  Alouatta seniculus         30/04/23 ALOSEN Mammalia   Primata     Atelidae
## 2    Aotus vociferans         30/04/23 AOTVOC Mammalia   Primata      Aotidae
## 3    Ateles belzebuth         30/04/23 ATEBEL Mammalia   Primata     Atelidae
## 4 Atelocynus microtis         30/04/23 ATEMIC Mammalia Carnivora      Canidae
## 5  Bassaricyon alleni         10/05/23 BASALE Mammalia Carnivora  Procyonidae
## 6 Bradypus variegatus         30/04/23 BRAVAR Mammalia    Pilosa Bradypodidae
##       Genus.x Scientific_name_2024                 Common_name      Diet
## 1    Alouatta   Alouatta seniculus Columbian red howler monkey Herbivore
## 2       Aotus     Aotus vociferans         Spix's night monkey Herbivore
## 3      Ateles     Ateles belzebuth White-bellied spider monkey Herbivore
## 4  Atelocynus  Atelocynus microtis             Short-eared dog Herbivore
## 5 Bassaricyon   Bassaricyon alleni      Eastern lowland olingo  Omnivore
## 6    Bradypus  Bradypus variegatus        Brown-throated sloth Herbivore
##        Strata IUCN
## 1    Arboreal   LC
## 2    Arboreal   LC
## 3    Arboreal   EN
## 4 Terrestrial   NT
## 5       Mixed   LC
## 6    Arboreal   LC
##                                                               Note MSW3_ID
## 1                                                                      627
## 2                                                                      640
## 3 Wierdly the IUCN range is missing any info in Ecuador so strange     619
## 4                                                                     4924
## 5                       Added by me was not in here anywhere oddly    5063
## 6                                                                      440
##       Genus.y    Species     Family.y Diet.Inv Diet.Vend Diet.Vect Diet.Vfish
## 1    Alouatta  seniculus     Atelidae        0         0         0          0
## 2       Aotus vociferans      Aotidae       20         0         0          0
## 3      Ateles  belzebuth     Atelidae       10         0         0          0
## 4  Atelocynus   microtis      Canidae        0        80         0          0
## 5 Bassaricyon     alleni  Procyonidae       20        10         0          0
## 6    Bradypus variegatus Bradypodidae        0         0         0          0
##   Diet.Vunk Diet.Scav Diet.Fruit Diet.Nect Diet.Seed Diet.PlantO Diet.Source
## 1         0         0         40         0         0          60       Ref_1
## 2        10         0         20        10        20          20       Ref_1
## 3        10         0         60         0        10          10       Ref_1
## 4         0         0          0         0         0          20       Ref_1
## 5         0         0         70         0         0           0       Ref_1
## 6         0         0          0         0         0         100       Ref_1
##   Diet.Certainty ForStrat.Value ForStrat.Certainty ForStrat.Comment
## 1            ABC             Ar                  A                 
## 2            ABC             Ar                  A                 
## 3            ABC             Ar                  A                 
## 4            ABC              G                  A                 
## 5            ABC             Ar                  A                 
## 6            ABC             Ar                  A                 
##   Activity.Nocturnal Activity.Crepuscular Activity.Diurnal Activity.Source
## 1                  0                    0                1           Ref_1
## 2                  1                    0                0           Ref_1
## 3                  0                    0                1           Ref_1
## 4                  1                    0                0           Ref_1
## 5                  1                    1                0           Ref_1
## 6                  1                    1                1           Ref_1
##   Activity.Certainty BodyMass.Value BodyMass.Source BodyMass.SpecLevel
## 1                ABC        6145.54         Ref_117                  1
## 2                ABC         872.99         Ref_117                  1
## 3                ABC        5000.00         Ref_117                  1
## 4                ABC        7749.97         Ref_117                  1
## 5                ABC        1235.01         Ref_117                  1
## 6                ABC        4335.01         Ref_117                  1
##                                                                                                                 Full.Reference
## 1 Nowak R.M. (1999). Walker's mammals of the world. Sixth edition edn. The Johns Hopkins University Press, Baltimore, Maryland
## 2 Nowak R.M. (1999). Walker's mammals of the world. Sixth edition edn. The Johns Hopkins University Press, Baltimore, Maryland
## 3 Nowak R.M. (1999). Walker's mammals of the world. Sixth edition edn. The Johns Hopkins University Press, Baltimore, Maryland
## 4 Nowak R.M. (1999). Walker's mammals of the world. Sixth edition edn. The Johns Hopkins University Press, Baltimore, Maryland
## 5 Nowak R.M. (1999). Walker's mammals of the world. Sixth edition edn. The Johns Hopkins University Press, Baltimore, Maryland
## 6 Nowak R.M. (1999). Walker's mammals of the world. Sixth edition edn. The Johns Hopkins University Press, Baltimore, Maryland
##     scientificNameStd
## 1  Alouatta seniculus
## 2    Aotus vociferans
## 3    Ateles belzebuth
## 4 Atelocynus microtis
## 5  Bassaricyon alleni
## 6 Bradypus variegatus
```
## show the ones that dont match

```r
TBS_nonvolmams_elton_noNA <- na.omit(TBS_nonvolmams_elton)
```


```r
TBS_nonvolmams_nonmatch <- anti_join(TBS_nonvolmams_elton, TBS_nonvolmams_elton_noNA, by="Scientific_name")
head(TBS_nonvolmams_nonmatch)
```

```
##  [1] Scientific_name      Lastest_revision     Code                
##  [4] Class                Order                Family.x            
##  [7] Genus.x              Scientific_name_2024 Common_name         
## [10] Diet                 Strata               IUCN                
## [13] Note                 MSW3_ID              Genus.y             
## [16] Species              Family.y             Diet.Inv            
## [19] Diet.Vend            Diet.Vect            Diet.Vfish          
## [22] Diet.Vunk            Diet.Scav            Diet.Fruit          
## [25] Diet.Nect            Diet.Seed            Diet.PlantO         
## [28] Diet.Source          Diet.Certainty       ForStrat.Value      
## [31] ForStrat.Certainty   ForStrat.Comment     Activity.Nocturnal  
## [34] Activity.Crepuscular Activity.Diurnal     Activity.Source     
## [37] Activity.Certainty   BodyMass.Value       BodyMass.Source     
## [40] BodyMass.SpecLevel   Full.Reference       scientificNameStd   
## <0 rows> (or 0-length row.names)
```

### Notes from ^
#### I made two columns in my data set with the most up to date Scientific name and the old names used in elton_traits - I remember seeing a code somewhere that auto updated but I dont have time for that rn



```r
##summary(traitdata)
```

Note that the `echo = FALSE` parameter was added to the code chunk to prevent printing of the R code that generated the plot.
