How many women take part in Gran Fondos?
================
Sarah Hosking
2018-03-21

``` r
library(tidyverse)
```

    ## Warning: package 'tidyverse' was built under R version 3.3.2

    ## Loading tidyverse: ggplot2
    ## Loading tidyverse: tibble
    ## Loading tidyverse: tidyr
    ## Loading tidyverse: readr
    ## Loading tidyverse: purrr
    ## Loading tidyverse: dplyr

    ## Warning: package 'ggplot2' was built under R version 3.3.2

    ## Warning: package 'tidyr' was built under R version 3.3.2

    ## Warning: package 'purrr' was built under R version 3.3.2

    ## Warning: package 'dplyr' was built under R version 3.3.2

    ## Conflicts with tidy packages ----------------------------------------------

    ## filter(): dplyr, stats
    ## lag():    dplyr, stats

``` r
library(treemapify)
```

    ## Warning: package 'treemapify' was built under R version 3.3.2

R Markdown
----------

This is an R Markdown document. Markdown is a simple formatting syntax for authoring HTML, PDF, and MS Word documents. For more details on using R Markdown see <http://rmarkdown.rstudio.com>.

When you click the **Knit** button a document will be generated that includes both content as well as the output of any embedded R code chunks within the document. You can embed an R code chunk like this:

``` r
path <- "../../../downloads/whistler_gf_2017.CSV"

whistler_gf <- read.csv(path, header = FALSE)
```

Add gender
----------

``` r
head(whistler_gf)
```

    ##   V1 V2              V3     V4                           V5
    ## 1  1  1 Sebastian Salas M30-34                      Jukebox
    ## 2  1  2      Mark Bonar M19-24                      Jukebox
    ## 3  1  3        Jon Bula M40-44 Pender Racing p/b Bicicletta
    ## 4  1  4       Adam Ward M25-29        Bullhearts Cycle Club
    ## 5  1  5    Matt Shandro M45-49                          TNA
    ## 6  1  6     Thomas Haas M45-49                          TNA
    ##                V6      V7   V8 V9 V10 V11  V12     V13 V14  V15 V16
    ## 1       Vancouver 3:21:40 5349  1 277   1 2258 0:04:30  42 36.3  NA
    ## 2       Vancouver 3:24:36 6140  1  45   2 2258 0:04:33  52 35.8  NA
    ## 3 North Vancouver 3:24:37 3354  1 298   3 2258 0:04:33  53 35.8  NA
    ## 4        Whistler 3:24:38 5832  1 166   4 2258          NA 35.8  NA
    ## 5       Vancouver 3:24:39 5408  1 354   5 2258 0:04:30  46 35.8  NA
    ## 6 North Vancouver 3:24:40 4024  2 354   6 2258 0:04:32  49 35.8  NA

Extract gender from age group var

verify

``` r
head(whistler_gf)
```

    ##   V1 V2              V3     V4                           V5
    ## 1  1  1 Sebastian Salas M30-34                      Jukebox
    ## 2  1  2      Mark Bonar M19-24                      Jukebox
    ## 3  1  3        Jon Bula M40-44 Pender Racing p/b Bicicletta
    ## 4  1  4       Adam Ward M25-29        Bullhearts Cycle Club
    ## 5  1  5    Matt Shandro M45-49                          TNA
    ## 6  1  6     Thomas Haas M45-49                          TNA
    ##                V6      V7   V8 V9 V10 V11  V12     V13 V14  V15 V16 gender
    ## 1       Vancouver 3:21:40 5349  1 277   1 2258 0:04:30  42 36.3  NA      M
    ## 2       Vancouver 3:24:36 6140  1  45   2 2258 0:04:33  52 35.8  NA      M
    ## 3 North Vancouver 3:24:37 3354  1 298   3 2258 0:04:33  53 35.8  NA      M
    ## 4        Whistler 3:24:38 5832  1 166   4 2258          NA 35.8  NA      M
    ## 5       Vancouver 3:24:39 5408  1 354   5 2258 0:04:30  46 35.8  NA      M
    ## 6 North Vancouver 3:24:40 4024  2 354   6 2258 0:04:32  49 35.8  NA      M

``` r
path2 <- "../../../downloads/sb_gf.CSV"
strade <- read.csv(path2)
tail(strade)
```

    ##      POS_ASSOLUTA PETTORALE    COGNOME      NOME
    ## 1852         1852       494    MORETTI     FABIO
    ## 1853         1853       886     MAZZEO  RICCARDO
    ## 1854         1854      2352    LUCENTE FRANCESCO
    ## 1855         1855      3580      RIZZA FRANCESCO
    ## 1856         1856      3667 CRAPANZANO   LEANDRO
    ## 1857         1857       811      CONTE      LUCA
    ##                                             TEAM NAZIONALITA CATEGORIA
    ## 1852                           ASD PEDALE FINESE         ita        M5
    ## 1853                                 PPR CYCLING         ita        M5
    ## 1854         SC CASAMASSIMA-LABICICLETTASTORE.IT         ita        M3
    ## 1855 asd dilettantisticaciclistica centro sicula         ita        M4
    ## 1856                         SPECIAL BIKERS TEAM         ita        M4
    ## 1857                                 PPR CYCLING         ita        M3
    ##      POS_CAT POS_SESSO TEMPO_UFFICIALE DISTACCO
    ## 1852     352      1794         7:26:46 +3:39:17
    ## 1853     353      1795         7:28:37 +3:41:09
    ## 1854     328      1796         7:29:00 +3:41:32
    ## 1855     404      1797         7:31:24 +3:43:56
    ## 1856     405      1798         7:31:24 +3:43:56
    ## 1857     329      1799         7:31:38 +3:44:09

``` r
is_w <- grep("W", strade$CATEGORIA)
strade[is_w,]
```

    ##      POS_ASSOLUTA PETTORALE              COGNOME            NOME
    ## 42             42       253              PARENTE          SIMONA
    ## 165           165       265                MORRI          DEBORA
    ## 262           262       468              MENEGON         MICHELA
    ## 276           276       592             COLLINGE           EMMIE
    ## 393           393       238                PRATI  MARIA CRISTINA
    ## 587           587      4079             SCARDOVI          FULVIA
    ## 591           591      3355          VAN LEEUWEN        MARIANNE
    ## 677           677      4671                RÖSCH          ANDREA
    ## 695           695      3170           PASQUALINI           NADIA
    ## 717           717       382           SCHIEVENIN        FEDERICA
    ## 776           776       347               FLUHME           LIDIA
    ## 834           834      5006            WOODFIELD             RIA
    ## 853           853       650               COMANI        EMANUELA
    ## 909           909      4391                WULFF SIREN BREIDALEN
    ## 919           919      1679            POLLICINO MIRELLA ROSARIA
    ## 940           940      2454              PEDRINI        ISABELLA
    ## 978           978       383               CIBIEN       FRANCESCA
    ## 980           980       216               BASINI         MELISSA
    ## 1018         1018      3234          GOMEZ PARLA    MARIA NIEVES
    ## 1024         1024      2451                 GORI           MIRKO
    ## 1060         1060      2699                BELLI          ILARIA
    ## 1079         1079      2011                GIANI       FRANCESCA
    ## 1110         1110      4265             GUGGIARI        CATERINA
    ## 1113         1113      4064            CONNERNEY          RACHEL
    ## 1124         1124      2491           GREENHALGH           LAURA
    ## 1201         1201      1719              BONOTTI       RAFFAELLA
    ## 1226         1226      2017              PASTORE           PAOLA
    ## 1251         1251      2232              CASADEI        CRISTINA
    ## 1254         1254      2562             DE PIERI           SOFIA
    ## 1348         1348      1508              STIVARI          FLAVIA
    ## 1358         1358      1623                DIARA           ERICA
    ## 1404         1404      2872              BIAGINI         ROBERTA
    ## 1429         1429      3135           BELLINGERI          CHIARA
    ## 1461         1461        79             LAMPAERT         AURÉLIE
    ## 1468         1468      4181                LOPES        STEFANIA
    ## 1487         1487      2561           MARIGHETTO           ELENA
    ## 1495         1495      2005                JONES             ROS
    ## 1502         1502       997 MARTINENGO CESARESCO           LAURA
    ## 1508         1508      4723               BERTOK      ELISABETTA
    ## 1511         1511      3240       GRO ABRAHAMSEN            AINO
    ## 1513         1513      2875              FALSINI        STEFANIA
    ## 1520         1520      4259             DELARBRE        PATRICIA
    ## 1521         1521      4202             TASTAVIN          OFÉLIE
    ## 1598         1598       431              VALLERA           ELISA
    ## 1641         1641      4464                MOYER         COLLEEN
    ## 1644         1644      5070           DI GIORGIO         VIVIANA
    ## 1668         1668      3494              GRASSIA         VALERIA
    ## 1691         1691       700              PETRINI           LUCIA
    ## 1694         1694      2014               GUELPA        ELEONORA
    ## 1696         1696      1219   DAL POZZO D'ANNONE      GUENDALINA
    ## 1701         1701       978               VECCHI         EUGENIA
    ## 1702         1702       977                CURTI           LIVIA
    ## 1722         1722      2876             VALBUENA   ELENA BEATRIZ
    ## 1754         1754      3615           CECCARELLI          LORENA
    ## 1765         1765      2419                 CECI       DONATELLA
    ## 1795         1795      3110              HOSKING           SARAH
    ## 1799         1799      4303           MALENCHINI      CLEMENTINA
    ## 1837         1837      5025              LINDELL            ANNA
    ##                                    TEAM NAZIONALITA CATEGORIA POS_CAT
    ## 42                        TEAM ISOLMANT         ita        W2      NA
    ## 165            TEAM DEL CAPITANO A.S.D.         ita        W2      NA
    ## 262      GRUPPO CICLISTICO VAL DI MERSE         ita        W2      NA
    ## 276     SCV SQUADRA CORSE VALTELLINAASD         gbr        W1       1
    ## 393            TEAM DEL CAPITANO A.S.D.         ita        W2       1
    ## 587         asd specialissima bike team         ita        W2       2
    ## 591                                             ned        W2       3
    ## 677                 TESSERA GIORNALIERA         sui        W1       2
    ## 695                      TEAM CINGOLANI         ita        W1       3
    ## 717              S.S.D. PEDALE FELTRINO         ita        W1       4
    ## 776                 Gran Fondo New York         usa        W1       5
    ## 834                 TESSERA GIORNALIERA         gbr       EWS       1
    ## 853            A.S.D. ONLYOFF DUE RUOTE         ita        W2       4
    ## 909                    Kristiansands CK         nor        W2       5
    ## 919                 TESSERA GIORNALIERA         ita        W2       6
    ## 940              A.S.D. ALL BIKES MANTA         ita        W1       6
    ## 978              S.S.D. PEDALE FELTRINO         ita        W1       7
    ## 980                        ARS ET ROBUR         ita        W1       8
    ## 1018       BUSTURIA TXITTINDULARI TALDE         esp        W2       7
    ## 1024                TESSERA GIORNALIERA         ita        W2       8
    ## 1060                     HIGH LEVEL 2.0         ita        W2       9
    ## 1079                      BIKE DIVISION         ita        W1       9
    ## 1110                      bike division         ita        W2      10
    ## 1113                          team glow         gbr        W1      10
    ## 1124                  LES FILLES QUEEN          gbr        W1      11
    ## 1201                   TEAM BIKE JO.ER.         ita        W2      11
    ## 1226                      CASTELLETTESE         ita        W1      12
    ## 1251              TEAM AQUILOTTI CERVIA         ita        W2      12
    ## 1254                TESSERA GIORNALIERA         ita       EWS       2
    ## 1348              TEAM PASSION FAENTINA         ita        W1      13
    ## 1358            ASD TEAM LABRONICA BIKE         ita        W2      13
    ## 1404                      A.S.D. IMPERO         ita        W2      14
    ## 1429                                            ita        W2      15
    ## 1461                TESSERA GIORNALIERA         bel        W1      14
    ## 1468  A.S.D. SQUADRA CORSE CUSSIGH BIKE         ita        W2      16
    ## 1487                TESSERA GIORNALIERA         ita        W2      17
    ## 1495                TESSERA GIORNALIERA         gbr        W2      18
    ## 1502          RACING ROSOLA BIKE A.S.D.         ita        W2      19
    ## 1508       ASD NOSURRENDER CYCLING TEAM         ita        W2      20
    ## 1511                                            den        W2      21
    ## 1513  GRUPPO SPORTIVO TERRALBA ARENZANO         ita        W2      22
    ## 1520                           Delarbre         fra        W2      23
    ## 1521                VELO CLUB SALINDRES         fra       EWS       3
    ## 1598                        PPR CYCLING         ita        W2      24
    ## 1641                TESSERA GIORNALIERA         usa        W2      25
    ## 1644               TEAM DE ROSA SANTINI         ita        W2      26
    ## 1668                GS TEAM PERINI BIKE         ita        W1      15
    ## 1691                CICLI NERI TEAM ASD         ita        W1      16
    ## 1694        CICOBIKES- DSB-NONSOLOFANGO         ita        W1      17
    ## 1696                   ROLLING DREAMERS         ita        W1      18
    ## 1701 VELO CLUB OLMO LA BICICLISSIMA ASD         ita        W2      27
    ## 1702 VELO CLUB OLMO LA BICICLISSIMA ASD         ita        W2      28
    ## 1722                                            ita        W1      19
    ## 1754    FREE BIKERS PEDALE FOLLONICHESE         ita        W2      29
    ## 1765                     ASD SBUBBIKERS         ita        W2      30
    ## 1795                TESSERA GIORNALIERA         fra        W2      31
    ## 1799              ASD TURBOLENTO MILANO         ita        W2      32
    ## 1837                TESSERA GIORNALIERA         swe        W1      20
    ##      POS_SESSO TEMPO_UFFICIALE DISTACCO
    ## 42           1         4:09:08   +21:39
    ## 165          2         4:31:03   +43:35
    ## 262          3         4:39:27   +51:58
    ## 276          4         4:41:17   +53:49
    ## 393          5         4:48:48 +1:01:19
    ## 587          6         5:04:55 +1:17:26
    ## 591          7         5:05:04 +1:17:36
    ## 677          8         5:11:51 +1:24:22
    ## 695          9         5:12:19 +1:24:50
    ## 717         10         5:13:58 +1:26:30
    ## 776         11         5:18:49 +1:31:21
    ## 834         12         5:23:02 +1:35:34
    ## 853         13         5:24:29 +1:37:01
    ## 909         14         5:29:34 +1:42:06
    ## 919         15         5:30:07 +1:42:39
    ## 940         16         5:31:34 +1:44:05
    ## 978         17         5:33:58 +1:46:30
    ## 980         18         5:34:03 +1:46:34
    ## 1018        19         5:36:46 +1:49:18
    ## 1024        20         5:37:42 +1:50:14
    ## 1060        21         5:40:40 +1:53:11
    ## 1079        22         5:42:02 +1:54:33
    ## 1110        23         5:44:13 +1:56:44
    ## 1113        24         5:44:19 +1:56:51
    ## 1124        25         5:44:39 +1:57:10
    ## 1201        26         5:52:15 +2:04:47
    ## 1226        27         5:54:23 +2:06:55
    ## 1251        28         5:55:23 +2:07:55
    ## 1254        29         5:55:39 +2:08:10
    ## 1348        30         6:02:38 +2:15:09
    ## 1358        31         6:02:55 +2:15:26
    ## 1404        32         6:08:34 +2:21:06
    ## 1429        33         6:10:34 +2:23:05
    ## 1461        34         6:13:17 +2:25:48
    ## 1468        35         6:13:44 +2:26:16
    ## 1487        36         6:16:12 +2:28:44
    ## 1495        37         6:16:58 +2:29:30
    ## 1502        38         6:17:55 +2:30:27
    ## 1508        39         6:18:29 +2:31:00
    ## 1511        40         6:18:54 +2:31:25
    ## 1513        41         6:19:11 +2:31:42
    ## 1520        42         6:20:35 +2:33:06
    ## 1521        43         6:20:35 +2:33:06
    ## 1598        44         6:31:28 +2:44:00
    ## 1641        45         6:36:47 +2:49:19
    ## 1644        46         6:37:21 +2:49:53
    ## 1668        47         6:42:10 +2:54:42
    ## 1691        48         6:44:29 +2:57:01
    ## 1694        49         6:44:42 +2:57:13
    ## 1696        50         6:45:08 +2:57:40
    ## 1701        51         6:45:52 +2:58:23
    ## 1702        52         6:46:02 +2:58:34
    ## 1722        53         6:49:56 +3:02:28
    ## 1754        54         6:56:37 +3:09:09
    ## 1765        55         6:58:58 +3:11:29
    ## 1795        56         7:04:01 +3:16:33
    ## 1799        57         7:04:46 +3:17:18
    ## 1837        58         7:16:01 +3:28:33

``` r
is_m <- grep("M", strade$CATEGORIA)
strade$gender <- ""
```

``` r
strade$gender[is_m] = "M"
```

``` r
strade$gender[is_w] = "F"
```

``` r
levels(strade$gender) <- c(levels(strade$gender), "N")
strade$gender[strade$gender==""] <- "N"
```

``` r
strade$gender <- as.factor(strade$gender)
str(strade)
```

    ## 'data.frame':    1857 obs. of  12 variables:
    ##  $ POS_ASSOLUTA   : int  1 2 3 4 5 6 7 8 9 10 ...
    ##  $ PETTORALE      : int  248 263 31 278 337 252 355 256 266 35 ...
    ##  $ COGNOME        : Factor w/ 1688 levels "AARRESTAD","ABMA",..: 545 1215 632 1248 1094 708 368 1054 357 1065 ...
    ##  $ NOME           : Factor w/ 592 levels "ADAM","ADAMO",..: 548 570 132 189 247 116 345 366 487 76 ...
    ##  $ TEAM           : Factor w/ 841 levels ""," ASD BICI&BIKE",..: 268 729 257 140 140 738 276 343 729 694 ...
    ##  $ NAZIONALITA    : Factor w/ 25 levels "aus","aut","bel",..: 14 14 14 14 14 14 14 14 14 14 ...
    ##  $ CATEGORIA      : Factor w/ 13 levels "","ELMT","EWS",..: 4 5 2 2 4 2 2 4 4 5 ...
    ##  $ POS_CAT        : int  NA NA NA 1 1 2 3 2 3 1 ...
    ##  $ POS_SESSO      : int  1 2 3 4 5 6 7 8 9 10 ...
    ##  $ TEMPO_UFFICIALE: Factor w/ 1521 levels "3:47:29","3:47:40",..: 1 1 2 3 4 5 6 7 8 9 ...
    ##  $ DISTACCO       : Factor w/ 1519 levels "","+1","+1:00:06",..: 1 2 662 1261 1312 1313 1314 1315 1316 1402 ...
    ##  $ gender         : Factor w/ 3 levels "F","M","N": 2 2 2 2 2 2 2 2 2 2 ...

``` r
table(strade$gender)
```

    ## 
    ##    F    M    N 
    ##   58 1797    2

``` r
library(lubridate)
```

    ## 
    ## Attaching package: 'lubridate'

    ## The following object is masked from 'package:base':
    ## 
    ##     date

``` r
res <- hms(strade$TEMPO_UFFICIALE)        # format to 'hours:minutes:seconds'
time_in_min <- hour(res)*60 + minute(res)
strade$time_in_min <- time_in_min
```

``` r
library(ggplot2)
```

``` r
res <- hms(whistler_gf$V7)        # format to 'hours:minutes:seconds'
time_in_min <- hour(res)*60 + minute(res)
whistler_gf$time_in_min <- time_in_min
```

``` r
f <- as.character(length(is_w))
percent = round((length(is_w)/nrow(strade)) * 100, 0)
sub <-  paste(as.character(nrow(strade)), "riders,", f," of them women (", as.character(percent), "%)")

p <- ggplot(strade, aes(time_in_min, fill = gender)) +
  geom_histogram(binwidth = 15, position = 'identity', alpha = 0.5) 

p + labs(title = "Stade Bianche 2018", subtitle = sub)
```

![](gran_fondos_files/figure-markdown_github/hist%20by%20gender-1.png)

``` r
by(strade$time_in_min, strade$gender, summary)
```

    ## strade$gender: F
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##   249.0   330.2   358.5   357.0   388.2   436.0 
    ## -------------------------------------------------------- 
    ## strade$gender: M
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##   227.0   294.0   329.0   332.1   365.0   451.0 
    ## -------------------------------------------------------- 
    ## strade$gender: N
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##     278     306     334     334     362     390

``` r
by(strade$time_in_min, strade$gender, sd)
```

    ## strade$gender: F
    ## [1] 42.6094
    ## -------------------------------------------------------- 
    ## strade$gender: M
    ## [1] 47.90162
    ## -------------------------------------------------------- 
    ## strade$gender: N
    ## [1] 79.19596

``` r
pbox <- ggplot(strade, aes(gender, time_in_min, fill = gender)) +
  geom_boxplot(alpha = 0.2)

pbox
```

![](gran_fondos_files/figure-markdown_github/sb%20box%20plot-1.png)

``` r
pbox <- pbox %+% aes(CATEGORIA, time_in_min)
pbox <- pbox +
  geom_point(alpha = 0.2) +
  facet_wrap(~gender, ncol = 1, scales = 'free')

pbox
```

![](gran_fondos_files/figure-markdown_github/unnamed-chunk-16-1.png)

``` r
f <- table(whistler_gf$gender=='F')[1]
```

``` r
total = nrow(whistler_gf)
percent = round((f/total) * 100, 0)
subtitle <-  paste(as.character(total), "riders,", as.character(f),"of them women (", as.character(percent), "%)")

p <- p %+% whistler_gf
p + labs(title = 'Whistler GF 2017', subtitle = subtitle)
```

![](gran_fondos_files/figure-markdown_github/unnamed-chunk-17-1.png)

``` r
by(whistler_gf$time_in_min, whistler_gf$gender, summary)
```

    ## whistler_gf$gender: F
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##   216.0   311.8   360.0   360.2   407.0   525.0 
    ## -------------------------------------------------------- 
    ## whistler_gf$gender: M
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##   201.0   273.0   319.0   324.2   366.0   577.0 
    ## -------------------------------------------------------- 
    ## whistler_gf$gender: N
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##   261.0   297.2   333.5   333.5   369.8   406.0

``` r
by(whistler_gf$time_in_min, whistler_gf$gender, sd)
```

    ## whistler_gf$gender: F
    ## [1] 63.34341
    ## -------------------------------------------------------- 
    ## whistler_gf$gender: M
    ## [1] 68.71754
    ## -------------------------------------------------------- 
    ## whistler_gf$gender: N
    ## [1] 102.5305

``` r
# create df of counts for each gender
names = c('Gender', 'Count')
whistler_gender_counts <- table(whistler_gf$gender)
whistler_gender_counts <- as.data.frame(whistler_gender_counts)
names(whistler_gender_counts) = names

# plot
palette = c('pink', 'lightblue', 'yellow')

gender_tree <- ggplot(whistler_gender_counts, 
                      aes(area = Count, fill = Gender, label = Gender)) + 
  geom_treemap() +
  geom_treemap_text(colour = "white", place = "centre", grow = TRUE) +
  scale_fill_manual(values=palette)

gender_tree + 
  labs(title = "Whistler Gran Fondo 2017", subtitle = subtitle)
```

![](gran_fondos_files/figure-markdown_github/treeplot%20whistler-1.png)

``` r
strade_gender_tbl <- table(strade$gender) %>% 
  as.data.frame()

names(strade_gender_tbl) = names

gender_tree %+% strade_gender_tbl +
  labs(title = "Stade Bianche 2018", subtitle = sub)
```

![](gran_fondos_files/figure-markdown_github/strade%20gender%20treemap-1.png)

``` r
pbox <- pbox %+% whistler_gf + aes(V4, time_in_min)
pbox
```

![](gran_fondos_files/figure-markdown_github/unnamed-chunk-18-1.png)
