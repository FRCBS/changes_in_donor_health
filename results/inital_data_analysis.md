Executive summary
=================

In this file we carry out inital data analysis to decide which
explenatory variables to exclude based on correlations between the
explenatory variables, shapes of variables distributions and counts of
missing data. Then the chosen variables are coded as change between the
2 questionnaires.

Load the data
=============

We load the data and assign groups as they were assigned in the first
part of the study.

    #Load questionnaire data
    load(file = "../data/final_data.rdata") 
    data <- data %>% ungroup()
    data %>% group_by(questionnaire) %>% count()

    ## # A tibble: 2 x 2
    ## # Groups:   questionnaire [2]
    ##   questionnaire     n
    ##   <chr>         <int>
    ## 1 First          1481
    ## 2 Second         1481

    data %>% group_by(questionnaire,group)  %>%  count()

    ## # A tibble: 8 x 3
    ## # Groups:   questionnaire, group [8]
    ##   questionnaire group                       n
    ##   <chr>         <fct>                   <int>
    ## 1 First         Men                       598
    ## 2 First         Post_menopause_women      362
    ## 3 First         Pre_menopause_women       483
    ## 4 First         Women_pre_menop_no_mens    38
    ## 5 Second        Men                       598
    ## 6 Second        Post_menopause_women      362
    ## 7 Second        Pre_menopause_women       483
    ## 8 Second        Women_pre_menop_no_mens    38

    data <- data %>% filter(group != 'Women_pre_menop_no_mens')
    data %>% group_by(questionnaire) %>% count()

    ## # A tibble: 2 x 2
    ## # Groups:   questionnaire [2]
    ##   questionnaire     n
    ##   <chr>         <int>
    ## 1 First          1443
    ## 2 Second         1443

Here are the questions we are processing. See further details from
<https://onlinelibrary.wiley.com/action/downloadSupplement?doi=10.1111%2Fvox.12856&file=vox12856-sup-0002-SupInfo2.pdf>

<table>
<colgroup>
<col width="6%" />
<col width="12%" />
<col width="81%" />
</colgroup>
<thead>
<tr class="header">
<th>Number</th>
<th>Abbreviation</th>
<th>Question text</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>1.1</td>
<td>HBP</td>
<td>Have you ever been diagnosed with any of the following conditions? Elevated blood pressure</td>
</tr>
<tr class="even">
<td>1.2</td>
<td>cholesterol</td>
<td>Have you ever been diagnosed with any of the following conditions? Elevated cholesterol levels</td>
</tr>
<tr class="odd">
<td>1.3</td>
<td>blood_sugar</td>
<td>Have you ever been diagnosed with any of the following conditions? Elevated blood sugar levels</td>
</tr>
<tr class="even">
<td>2</td>
<td>low_hb_hist</td>
<td>Have you ever been diagnosed with a low haemoglobin (Hb) count?</td>
</tr>
<tr class="odd">
<td>2.1</td>
<td>anemia_hist</td>
<td>If yes, have you ever been diagnosed with anaemia?</td>
</tr>
<tr class="even">
<td>3</td>
<td>high_hb_hist</td>
<td>Have you ever undergone medical tests because of a high haemoglobin count?</td>
</tr>
<tr class="odd">
<td>4</td>
<td>anti_inf</td>
<td>During the past four weeks, have you taken anti-inflammatory pain medication (such as aspirin, ibuprofen or ketoprofen)?</td>
</tr>
<tr class="even">
<td>5</td>
<td>health</td>
<td>How would you rate your recent health in general?</td>
</tr>
<tr class="odd">
<td>6</td>
<td>health_c</td>
<td>How would you rate your health compared with other people of similar age?</td>
</tr>
<tr class="even">
<td>7.1</td>
<td>act_light</td>
<td>Based on your health, how would you rate your ability to carry out the following daily activities? A. Light chores, such as moving a table, vacuuming or cycling.</td>
</tr>
<tr class="odd">
<td>7.2</td>
<td>act_heavy</td>
<td>Based on your health, how would you rate your ability to carry out the following daily activities? B. Climbing several flights of stairs</td>
</tr>
<tr class="even">
<td>8</td>
<td>phys_inf</td>
<td>During the past four weeks, have your physical symptoms (such as pain) or health in general interfered with your work or other daily activities?</td>
</tr>
<tr class="odd">
<td>9</td>
<td>ment_inf</td>
<td>During the past four weeks, has your mental health interfered with your work or other daily activities?</td>
</tr>
<tr class="even">
<td>10</td>
<td>accomplish</td>
<td>Have you felt that you were able to accomplish less than you would have liked to?</td>
</tr>
<tr class="odd">
<td>11.A</td>
<td>freq_calm</td>
<td>During the past four weeks, how often have you felt: calm and relaxed?</td>
</tr>
<tr class="even">
<td>11.B</td>
<td>freq_energy</td>
<td>During the past four weeks, how often have you felt: full of energy?</td>
</tr>
<tr class="odd">
<td>11.C</td>
<td>freq_sad</td>
<td>During the past four weeks, how often have you felt: sad?</td>
</tr>
<tr class="even">
<td>12</td>
<td>health_soc</td>
<td>During the past four weeks, how often has your physical or emotional health interfered with your social activities (such as visiting friends or relatives)?</td>
</tr>
<tr class="odd">
<td>13.A</td>
<td>sleep_night</td>
<td>How many hours do you sleep on average A. per night (round to the nearest half hour)?</td>
</tr>
<tr class="even">
<td>13.B</td>
<td>sleep_24h</td>
<td>How many hours do you sleep on average B. over a 24-hour period, including nights and daytime naps (round to the nearest half hour)?</td>
</tr>
<tr class="odd">
<td>14.A</td>
<td>sleep_enough</td>
<td>Sleep habits A. Do you think you get enough sleep?</td>
</tr>
<tr class="even">
<td>14.B</td>
<td>sleep_tired</td>
<td>Sleep habits B. Do you feel tired during the day?</td>
</tr>
<tr class="odd">
<td>15.A</td>
<td>vita_multi</td>
<td>During the past four weeks, have you taken A. multivitamin tablets/supplements?</td>
</tr>
<tr class="even">
<td>15.B</td>
<td>vita_C</td>
<td>During the past four weeks, have you taken B. vitamin C tablets/supplements?</td>
</tr>
<tr class="odd">
<td>15.C</td>
<td>vita_iron</td>
<td>During the past four weeks, have you taken C. iron tablets/supplements?</td>
</tr>
<tr class="even">
<td>16</td>
<td>iron_supp</td>
<td>Were you given iron tablets when you last donated blood?</td>
</tr>
<tr class="odd">
<td>16.1</td>
<td>iron_supp_detailed</td>
<td>If yes, how many iron tablets did you take?</td>
</tr>
<tr class="even">
<td>17</td>
<td>diet</td>
<td>Do you follow a special diet?</td>
</tr>
<tr class="odd">
<td>18.A</td>
<td>freq_red_meat</td>
<td>How often do you have A. red meat (beef, pork, lamb, game) as the main course?</td>
</tr>
<tr class="even">
<td>18.B</td>
<td>freq_cutlets</td>
<td>How often do you have B. cutlets?</td>
</tr>
<tr class="odd">
<td>18.C</td>
<td>freq_fish</td>
<td>How often do you have C. fish?</td>
</tr>
<tr class="even">
<td>18.D</td>
<td>freq_eggs</td>
<td>How often do you have D. eggs?</td>
</tr>
<tr class="odd">
<td>18.E</td>
<td>freq_fruits</td>
<td>How often do you have E. fruit and berries?</td>
</tr>
<tr class="even">
<td>18.F</td>
<td>freq_vege</td>
<td>How often do you have F. salad and vegetables?</td>
</tr>
<tr class="odd">
<td>18.G</td>
<td>freq_juices</td>
<td>How often do you have G. fruit juices?</td>
</tr>
<tr class="even">
<td>18.H</td>
<td>freq_wholem</td>
<td>How often do you have H. wholemeal products (such as porridge, bread, muesli)?</td>
</tr>
<tr class="odd">
<td>19.A</td>
<td>freq_milk</td>
<td>How often do you have A. milk or other dairy products?</td>
</tr>
<tr class="even">
<td>19.B</td>
<td>freq_cof</td>
<td>How often do you have B. coffee?</td>
</tr>
<tr class="odd">
<td>19.C</td>
<td>freq_tea</td>
<td>How often do you have C. tea?</td>
</tr>
<tr class="even">
<td>19.D</td>
<td>freq_beer</td>
<td>How often do you have D. beer?</td>
</tr>
<tr class="odd">
<td>19.E</td>
<td>freq_wine</td>
<td>How often do you have E. wine?</td>
</tr>
<tr class="even">
<td>19.F</td>
<td>freq_spirits</td>
<td>How often do you have F. spirits?</td>
</tr>
<tr class="odd">
<td>20</td>
<td>smoking</td>
<td>Do you smoke?</td>
</tr>
<tr class="even">
<td>21</td>
<td>phys_cond</td>
<td>How would you rate your current physical condition?</td>
</tr>
<tr class="odd">
<td>22</td>
<td>day_act</td>
<td>Which of the following statements best describes your typical day (work, studies, leisure)?</td>
</tr>
<tr class="even">
<td>23</td>
<td>h_act_light</td>
<td>On average, how much time do you spend each day doing light everyday physical activity, such as cycling, walking or rollerblading? Tick the alternative that best describes the average time…</td>
</tr>
<tr class="odd">
<td>24</td>
<td>freq_sports</td>
<td>How often do you exercise/do sports in your free time?</td>
</tr>
<tr class="even">
<td>26</td>
<td>h_act_none</td>
<td>On average, how long do you spend each day in your free time on activities that are sedentary (such as doing handicrafts, reading, watching television, playing computer games, surfing the …</td>
</tr>
<tr class="odd">
<td>S1</td>
<td>life_qual</td>
<td>How would you rate your quality of life?</td>
</tr>
<tr class="even">
<td>S2</td>
<td>education</td>
<td>What is the highest level of education you have attained?</td>
</tr>
<tr class="odd">
<td>S3</td>
<td>employment</td>
<td>Employment status</td>
</tr>
</tbody>
</table>

Remove end point variable
=========================

In the inital data analysis stage we do not want to look at our primary
endpoint question 5 "How would you rate your recent health in general?"
i.e. "health" to avoid fishing for correlations.

    temp <- data %>% dplyr::select(donor,health) %>% filter(is.na(health))
    data <- data %>% filter(!donor %in% unique(temp$donor))  %>% dplyr::select(-health)
    data %>% group_by(questionnaire) %>% count()

    ## # A tibble: 2 x 2
    ## # Groups:   questionnaire [2]
    ##   questionnaire     n
    ##   <chr>         <int>
    ## 1 First          1438
    ## 2 Second         1438

### Remove donors with extreme physiological measures

As decided previously, we remove data according the following criteria:

-   CRP &gt;= 30
-   Ferritin &gt;= 400

<!-- -->

    #Rename because next file will also bring an object called data
    qdata <-data
    #load measurement data
    load(file = "../data/quest_2_donor_data.rdata")

    data <- data %>% 
      rename(Ferritin_beginning = Ferritin,
             CRP_beginning = CRP) %>% dplyr::select(donor,Ferritin_beginning,CRP_beginning) %>% inner_join(qdata)
    cat(nrow(data)/2,"\n")

    ## 1438

    data <- data %>%   filter(Ferritin_beginning < 400)
    cat(nrow(data)/2,"after high ferritin removal\n")

    ## 1435 after high ferritin removal

    data <- data %>%   filter(CRP_beginning < 30)
    cat(nrow(data)/2,"after high CRP removal\n")

    ## 1431 after high CRP removal

    data <- data %>% ungroup()

    data <- data %>% dplyr::select(-Ferritin_beginning,-CRP_beginning)
    data %>% group_by(questionnaire) %>% count()

    ## # A tibble: 2 x 2
    ## # Groups:   questionnaire [2]
    ##   questionnaire     n
    ##   <chr>         <int>
    ## 1 First          1431
    ## 2 Second         1431

Preprocessing recodings
=======================

Questions with 3 possible answers : "no", "yes", "don't know", are
recoded as an ordered factor with 3 levels: "yes"&lt; "don't know" &lt;
"no".

    #Have you felt that you were able to accomplish less than you would have liked to?
    temp <- as.character(data$accomplish)
    temp[is.na(temp)] <- "don't know"
    data$accomplish = ordered(temp,
                    levels =  c("yes","don't know", "no"))
    #During the past four weeks, has your mental health interfered with your work or other daily activities?
    ##Later in explorative_analysis.Rmd we find out that some variables cannot be fitted. We remove them now not to loose data over them
    # temp <- as.character(data$ment_inf)
    # temp[is.na(temp)] <- "don't know"
    # data$ment_inf = ordered(temp,
    #                 levels =  c("yes","don't know", "no"))

    #During the past four weeks, have your physical symptoms (such as pain) or health in general interfered with your work or other daily activities?
    temp <- as.character(data$phys_inf)
    temp[is.na(temp)] <- "don't know"
    data$phys_inf = ordered(temp,
                    levels =  c("yes","don't know", "no"))

How much data do we have
========================

    #Save questions only asked in the second questionaire to be added back later.
    additionalq <- data %>% filter(questionnaire == "Second") %>% dplyr::select(donor, life_qual, education, employment)
    #drop out variables that, 
    # we know from our previous work are too messy: iron_supp_detailed
    # were only asked in second questionaire: life_qual, education, employment
    # are conveniency variables: AgeGroup, SixMonthsFromStartCount_FB, TwoYearsFromStartCount_FB
    temp <- data %>% dplyr::select(-iron_supp_detailed, -life_qual, -education, -employment,-AgeGroup, -SixMonthsFromStartCount_FB,-TwoYearsFromStartCount_FB)

    #

    temp <- na.omit(temp)
    table(table(temp$donor))

    ## 
    ##   1   2 
    ## 492 817

817 donors with complete data in both questionaires.

    temp %>% group_by(questionnaire) %>%  count()

    ## # A tibble: 2 x 2
    ## # Groups:   questionnaire [2]
    ##   questionnaire     n
    ##   <chr>         <int>
    ## 1 First          1038
    ## 2 Second         1088

As we will loose almost half of our data if we study only complete data
we have try to make decisions on which questions to exclude prior to
analyses or which data to impute.

    temp <- data %>% dplyr::select(-iron_supp_detailed, -life_qual, -education, -employment,-AgeGroup, -SixMonthsFromStartCount_FB,-TwoYearsFromStartCount_FB,-donor)
    temp <- temp %>% group_by(questionnaire) %>%  summarise_all(funs(sum(is.na(.))))
    temp <- as_tibble(t(temp), rownames = "Variable") %>% filter(Variable != "questionnaire")
    colnames(temp)[2] <- "First"
    colnames(temp)[3] <- "Second"
    temp <- temp %>%  gather(key="questionnaire",value="NAs",-Variable) %>% mutate(NAs = as.numeric(gsub(" ","",NAs))) %>% filter(NAs != 0)

    ggplot(temp) + geom_col(aes(x=Variable,y=NAs,fill=questionnaire),pos="dodge") +  coord_flip() 

![](../results/inital_data_analysis_files/figure-markdown_strict/unnamed-chunk-9-1.png)

    temp <- temp %>% arrange(NAs) %>% tail(10)

    ggplot(temp) + geom_col(aes(x=Variable,y=NAs,fill=questionnaire),pos="dodge") +  coord_flip() 

![](../results/inital_data_analysis_files/figure-markdown_strict/unnamed-chunk-10-1.png)

People cannot remember if they have taken vitamine supplements
containing iron or vitamine C. Let's first see if some of these will
drop out because they correlate heavily with some other explenatory
variable.

Correlations between variables
==============================

    temp <- data %>% dplyr::select(-iron_supp_detailed, -life_qual, -education, -employment,-AgeGroup, -SixMonthsFromStartCount_FB,-TwoYearsFromStartCount_FB, -donor,-questionnaire,-sex,-group)
    temp$date <- as.numeric(temp$date)

    #Change the numeric to factors to enable similar correlation calculation for all
    temp <- temp %>% mutate_if(is.numeric, ~cut(.,5))

    #https://datascience.stackexchange.com/questions/893/how-to-get-correlation-between-two-categorical-variable-and-a-categorical-variab
    cormat <- matrix(NA,ncol = ncol(temp),nrow=ncol(temp))
    rownames(cormat) <-colnames(temp)
    colnames(cormat) <-colnames(temp)
    for (i in rownames(cormat)) {
      for (j in colnames(cormat)) {
        d <- table(as.factor(temp[[i]]),as.factor(temp[[j]]))
    #    print(d)
        #drop any all zero rows
        d <- d[apply(X=d,MARGIN = 1,FUN = function(x){!all(x == 0 )}),]
        d <- d[,apply(X=d,MARGIN = 2,FUN = function(x){!all(x == 0 )})]
        #print(d)
        t <- chisq.test(d,correct = F)
        cramersV <- sqrt(t$statistic / sum(d))
        if (cramersV > 1) {
          #sometimes it fails in perfect correlation
          cramersV <- 1
        }
        cormat[i,j] <- cramersV
      }
    }

Visualise correlation disribution

    temp <- data.frame(correlation=cormat[lower.tri(cormat)])
    ggplot(temp) +  geom_histogram(aes(x=correlation),binwidth = 0.1)

![](../results/inital_data_analysis_files/figure-markdown_strict/unnamed-chunk-12-1.png)

    #and create a fake significance matrix of arbitary correlation of 0.5
    p.mat <- matrix(1, nrow=nrow(cormat),ncol=ncol(cormat))
    p.mat[as.vector(cormat > 0.5)] <- 0.01 
    diag(p.mat) <- 1

    corrplot(cormat,type="lower",order="hclust",p.mat=p.mat,sig.level = 0.05,insig = "label_sig",pch.col = 'red',pch.cex = 1)

![](../results/inital_data_analysis_files/figure-markdown_strict/unnamed-chunk-13-1.png)

Extract those with correlation above 0.5.

    #take anything with at least 0.5 correlation
    any_above <- function(x){any(x > 0.5) }
    temp <-cormat
    #empty the diagonal
    diag(temp) <- 0
    #filter fake significance matrix
    temp2 <- p.mat[apply(temp,1, any_above),apply(temp,2, any_above)]
    #filter correlation matrix
    temp <- temp[apply(temp,1, any_above),apply(temp,2, any_above)]




    corrplot(temp,type="lower",order="hclust",p.mat=temp2,sig.level = 0.05,insig = "label_sig",pch.col = 'red',pch.cex = 1)

![](../results/inital_data_analysis_files/figure-markdown_strict/unnamed-chunk-14-1.png)

Choose what to keep
===================

Between those that correlate above 0.5 choose which variable to keep to
present the correlaiting variables in the exploratory analysis.

act\_light, act\_heavy
----------------------

    ggplot(data) + geom_bar(aes(x=act_light))

![](../results/inital_data_analysis_files/figure-markdown_strict/unnamed-chunk-15-1.png)

    ggplot(data) + geom_bar(aes(x=act_heavy))

![](../results/inital_data_analysis_files/figure-markdown_strict/unnamed-chunk-16-1.png)

    table(is.na(data$act_heavy))

    ## 
    ## FALSE  TRUE 
    ##  2828    34

    table(is.na(data$act_light))

    ## 
    ## FALSE  TRUE 
    ##  2848    14

act\_heavy looses more data, but has much more spread pontentially
contaning more information.

Keep "act\_heavy".

health\_c vs phys\_cond
-----------------------

    ggplot(data) + geom_bar(aes(x=health_c))

![](../results/inital_data_analysis_files/figure-markdown_strict/unnamed-chunk-18-1.png)

    ggplot(data) + geom_bar(aes(x=phys_cond))

![](../results/inital_data_analysis_files/figure-markdown_strict/unnamed-chunk-19-1.png)

    #Notice that this has been already set from worst to best unlike in the original questionaire

    table(is.na(data$health_c))

    ## 
    ## FALSE  TRUE 
    ##  2829    33

    table(is.na(data$phys_cond))

    ## 
    ## FALSE  TRUE 
    ##  2859     3

phys\_cond looks better and looses less data.

vita\_c vs vita\_multi
----------------------

    ggplot(data) + geom_bar(aes(x=vita_C))

![](../results/inital_data_analysis_files/figure-markdown_strict/unnamed-chunk-21-1.png)

    ggplot(data) + geom_bar(aes(x=vita_multi))

![](../results/inital_data_analysis_files/figure-markdown_strict/unnamed-chunk-22-1.png)

    table(is.na(data$vita_C))

    ## 
    ## FALSE  TRUE 
    ##  2712   150

    table(is.na(data$vita_multi))

    ## 
    ## FALSE  TRUE 
    ##  2783    79

vita\_multi looses much less data and looks better so vita\_C can be
dropped.

sleep\_night vs sleep\_24h
--------------------------

    ggplot(data) + geom_histogram(aes(x=sleep_night),binwidth = 1)

![](../results/inital_data_analysis_files/figure-markdown_strict/unnamed-chunk-24-1.png)

    ggplot(data) + geom_histogram(aes(x=sleep_24h),binwidth = 1)

![](../results/inital_data_analysis_files/figure-markdown_strict/unnamed-chunk-25-1.png)

    table(is.na(data$sleep_night))

    ## 
    ## FALSE  TRUE 
    ##  2855     7

    table(is.na(data$sleep_24h))

    ## 
    ## FALSE  TRUE 
    ##  2850    12

Distributions look quite similar, but sleep\_night looses less data.

freq\_fruits vs freq\_vege
--------------------------

    ggplot(data) + geom_bar(aes(x=freq_fruits))

![](../results/inital_data_analysis_files/figure-markdown_strict/unnamed-chunk-27-1.png)

    ggplot(data) + geom_bar(aes(x=freq_vege))

![](../results/inital_data_analysis_files/figure-markdown_strict/unnamed-chunk-28-1.png)

    table(is.na(data$freq_fruits))

    ## 
    ## FALSE  TRUE 
    ##  2853     9

    table(is.na(data$freq_vege))

    ## 
    ## FALSE  TRUE 
    ##  2849    13

freq\_fruits look better and looses less data.

freq\_sports vs phys\_cond
--------------------------

    ggplot(data) + geom_bar(aes(x=freq_sports))

![](../results/inital_data_analysis_files/figure-markdown_strict/unnamed-chunk-30-1.png)

    ggplot(data) + geom_bar(aes(x=phys_cond))

![](../results/inital_data_analysis_files/figure-markdown_strict/unnamed-chunk-31-1.png)

    table(is.na(data$freq_sports))

    ## 
    ## FALSE  TRUE 
    ##  2839    23

    table(is.na(data$phys_cond))

    ## 
    ## FALSE  TRUE 
    ##  2859     3

Hard to say anything about the distributions, but physical condition
looses less data.

freq\_spirits, freq\_beer, freq\_wine
-------------------------------------

    ggplot(data) + geom_bar(aes(x=freq_spirits))

![](../results/inital_data_analysis_files/figure-markdown_strict/unnamed-chunk-33-1.png)

    ggplot(data) + geom_bar(aes(x=freq_beer))

![](../results/inital_data_analysis_files/figure-markdown_strict/unnamed-chunk-34-1.png)

    ggplot(data) + geom_bar(aes(x=freq_wine))

![](../results/inital_data_analysis_files/figure-markdown_strict/unnamed-chunk-35-1.png)

    table(is.na(data$freq_spirits))

    ## 
    ## FALSE  TRUE 
    ##  2847    15

    table(is.na(data$freq_beer))

    ## 
    ## FALSE  TRUE 
    ##  2857     5

    table(is.na(data$freq_wine))

    ## 
    ## FALSE  TRUE 
    ##  2851    11

Hard to say, but freq\_beer looses least data. But this does not cover
at all persons that drink mostly cider which could be in particular
found in pre-menopausal women.

To have have some kind of proxy over these incomplete but overlapping
questions we will create a new variable freq\_alc.

    #Combine the alcohol questions
    data$freq_alc <- as.ordered(apply(cbind(data$freq_beer,data$freq_wine,data$freq_spirits),1,max,na.rm=TRUE))
    #Make it in to a factor
    levels(data$freq_alc) <- levels(data$freq_beer)

freq\_cutlets, freq\_meat, diet, freq\_meat, freq\_fish, freq\_eggs
-------------------------------------------------------------------

    ggplot(data) + geom_bar(aes(x=freq_cutlets))

![](../results/inital_data_analysis_files/figure-markdown_strict/unnamed-chunk-38-1.png)

    ggplot(data) + geom_bar(aes(x=freq_red_meat))

![](../results/inital_data_analysis_files/figure-markdown_strict/unnamed-chunk-39-1.png)

    ggplot(data) + geom_bar(aes(x=diet))

![](../results/inital_data_analysis_files/figure-markdown_strict/unnamed-chunk-40-1.png)

    ggplot(data) + geom_bar(aes(x=freq_fish))

![](../results/inital_data_analysis_files/figure-markdown_strict/unnamed-chunk-41-1.png)

    ggplot(data) + geom_bar(aes(x=freq_eggs))

![](../results/inital_data_analysis_files/figure-markdown_strict/unnamed-chunk-42-1.png)

    table(is.na(data$freq_cutlets))

    ## 
    ## FALSE  TRUE 
    ##  2844    18

    table(is.na(data$freq_red_meat))

    ## 
    ## FALSE  TRUE 
    ##  2851    11

    table(is.na(data$diet))

    ## 
    ## FALSE  TRUE 
    ##  2846    16

    table(is.na(data$freq_fish))

    ## 
    ## FALSE  TRUE 
    ##  2841    21

    table(is.na(data$freq_eggs))

    ## 
    ## FALSE  TRUE 
    ##  2833    29

As already seen in the enrollment data paper freq\_red\_meats
distribution looks best also it looses leas data.

BMI, height and weigth
----------------------

    ggplot(data) + geom_histogram(aes(x=BMI),binwidth = 1)

![](../results/inital_data_analysis_files/figure-markdown_strict/unnamed-chunk-44-1.png)

    ggplot(data) + geom_histogram(aes(x=height),binwidth = 5)

![](../results/inital_data_analysis_files/figure-markdown_strict/unnamed-chunk-45-1.png)

    ggplot(data) + geom_histogram(aes(x=weight),binwidth = 5)

![](../results/inital_data_analysis_files/figure-markdown_strict/unnamed-chunk-46-1.png)

    table(is.na(data$BMI))

    ## 
    ## FALSE  TRUE 
    ##  2846    16

    table(is.na(data$height))

    ## 
    ## FALSE  TRUE 
    ##  2851    11

    table(is.na(data$weight))

    ## 
    ## FALSE  TRUE 
    ##  2847    15

Keep BMI as composite of both.

freq\_energy, freq\_sad, health\_soc, freq\_calm, accomplish
------------------------------------------------------------

    ggplot(data) + geom_bar(aes(x=freq_energy))

![](../results/inital_data_analysis_files/figure-markdown_strict/unnamed-chunk-48-1.png)

    ggplot(data) + geom_bar(aes(x=freq_sad))

![](../results/inital_data_analysis_files/figure-markdown_strict/unnamed-chunk-49-1.png)

    ggplot(data) + geom_bar(aes(x=health_soc))

![](../results/inital_data_analysis_files/figure-markdown_strict/unnamed-chunk-50-1.png)

    ggplot(data) + geom_bar(aes(x=freq_calm))

![](../results/inital_data_analysis_files/figure-markdown_strict/unnamed-chunk-51-1.png)

    ggplot(data) + geom_bar(aes(x=accomplish))

![](../results/inital_data_analysis_files/figure-markdown_strict/unnamed-chunk-52-1.png)

    table(is.na(data$freq_energy))

    ## 
    ## FALSE  TRUE 
    ##  2850    12

    table(is.na(data$freq_sad))

    ## 
    ## FALSE  TRUE 
    ##  2827    35

    table(is.na(data$health_soc))

    ## 
    ## FALSE  TRUE 
    ##  2826    36

    table(is.na(data$freq_calm))

    ## 
    ## FALSE  TRUE 
    ##  2854     8

    table(is.na(data$accomplish))

    ## 
    ## FALSE 
    ##  2862

accomplish has a tricky distribution.

freq\_energy correlates with phys\_cond and can be dropped for that.

health\_soc has a very skewed distiribution.

freq\_calm looses least data and has maybe the best looking
distribution.

h\_act\_light vs h\_act\_noen
-----------------------------

    ggplot(data) + geom_bar(aes(x=h_act_light))

![](../results/inital_data_analysis_files/figure-markdown_strict/unnamed-chunk-54-1.png)

    ggplot(data) + geom_bar(aes(x=h_act_none))

![](../results/inital_data_analysis_files/figure-markdown_strict/unnamed-chunk-55-1.png)

    table(is.na(data$h_act_light))

    ## 
    ## FALSE  TRUE 
    ##  2851    11

    table(is.na(data$h_act_none))

    ## 
    ## FALSE  TRUE 
    ##  2847    15

h\_act\_light looses less data.

Drop questions based on correlation analysis
============================================

Based on the above we have decided to drop: act\_light, health\_c,
vita\_C, sleep\_24h, freq\_vege, freq\_sports, freq\_spirits,
freq\_beer, freq\_wine, freq\_cutlets, diet, freq\_fish, freq\_eggs,
weight, height, accomplish, freq\_energy, health\_soc, freq\_sad,
h\_act\_none

    temp <- data %>% dplyr::select(-iron_supp_detailed, -life_qual, -education, -employment,-AgeGroup, -SixMonthsFromStartCount_FB,-TwoYearsFromStartCount_FB, -act_light, -health_c, -vita_C, -sleep_24h, -freq_vege, -freq_sports, -freq_spirits, -freq_wine, -freq_beer, -freq_cutlets, -diet, -freq_fish, -freq_eggs, -weight, -height, -accomplish, -freq_energy, -health_soc, -freq_sad,-h_act_none)

    #Later in explorative_analysis.Rmd we find out that some variables cannot be fitted. We remove them now not to loose data over them
    temp <- temp %>% dplyr::select(-sleep_night,
    -ment_inf,
    -vita_iron
    #education is removed later as it was only asked in the second questionaire
    #smoking_behavior # this is brought in and removed in
    )

    temp <- na.omit(temp)
    temp %>% group_by(questionnaire) %>% count()

    ## # A tibble: 2 x 2
    ## # Groups:   questionnaire [2]
    ##   questionnaire     n
    ##   <chr>         <int>
    ## 1 First          1186
    ## 2 Second         1245

From 33 questions.

    #But questionnaire, donor, group, age, sex and date are not procesed into change between questionnaires
    tx <- setdiff(colnames(temp),c("questionnaire", "donor", "group", "age", "sex","date" ))
    tx

    ##  [1] "act_heavy"     "phys_inf"      "freq_calm"     "sleep_enough" 
    ##  [5] "sleep_tired"   "freq_red_meat" "freq_fruits"   "freq_juices"  
    ##  [9] "freq_wholem"   "freq_milk"     "freq_cof"      "freq_tea"     
    ## [13] "smoking"       "phys_cond"     "day_act"       "h_act_light"  
    ## [17] "cholesterol"   "blood_sugar"   "HBP"           "low_hb_hist"  
    ## [21] "anemia_hist"   "high_hb_hist"  "anti_inf"      "vita_multi"   
    ## [25] "iron_supp"     "BMI"           "freq_alc"

    length(tx)

    ## [1] 27

How will the sex/menopause groups look

    temp %>% group_by(questionnaire,group) %>% filter(group != 'Women_pre_menop_no_mens') %>%  count()

    ## # A tibble: 6 x 3
    ## # Groups:   questionnaire, group [6]
    ##   questionnaire group                    n
    ##   <chr>         <fct>                <int>
    ## 1 First         Men                    492
    ## 2 First         Post_menopause_women   280
    ## 3 First         Pre_menopause_women    414
    ## 4 Second        Men                    509
    ## 5 Second        Post_menopause_women   307
    ## 6 Second        Pre_menopause_women    429

    table(table(temp$donor))

    ## 
    ##    1    2 
    ##  337 1047

1047 donors with complete data in both questionaires.

By excluding several questions that correlate with other questions and
have distributions that are skewed (e.g. diet, accomplish) or are
uniformative (e.g. act\_light) or are not clearly ordered factors (like
diet) we get complete data for about two hundred more persons. There
might still be too many explenatory variables for the smallest group
i.e. Post\_menopause\_women and we will still loose data when we look
for complete cases in both questionaires.

Recode answers as change between the questionnaires
===================================================

Exclude questions that are dealt with later in the regression data
processing: BMI, smoking health, date, age, group. "health" has been
already excluded. N.B. smoking is not part of the hypothesis testing. It
is just processed later.

    temp <- temp %>% dplyr::select(-BMI,-smoking,-date,-age,-group,-sex)
    #Extract complete data for both questinaires and separate questionaire answers into different columns for processing
    splitbyq <- split(temp,f=temp$questionnaire)
    first <- splitbyq[["First"]]
    second <- splitbyq[["Second"]]
    colnames(second) <- paste0(colnames(second),"_2")
    temp <- inner_join(first,second,by=c("donor"="donor_2"))
    cat(nrow(temp),"complete cases in both questionnaires\n")

    ## 1047 complete cases in both questionnaires

    classes <- sort(unlist(lapply(lapply(first,class),paste,collapse=" ")))
    classes <- as.tibble(cbind(names(classes),classes))
    no_answers <- first %>% summarise_all(~length(levels(.)))
    no_answers <- as_tibble(cbind(variable = names(no_answers), t(no_answers)))
    no_answers <- full_join(no_answers,classes,by=c("variable"="V1")) %>% rename(no_answers=V2)
    no_answers %>% group_by(no_answers)  %>% count() %>%  arrange(n)

    ## # A tibble: 5 x 2
    ## # Groups:   no_answers [5]
    ##   no_answers     n
    ##   <chr>      <int>
    ## 1 3              1
    ## 2 5              1
    ## 3 4              7
    ## 4 0              9
    ## 5 6              9

    no_answers %>%  filter(no_answers == 0)

    ## # A tibble: 9 x 3
    ##   variable      no_answers classes  
    ##   <chr>         <chr>      <chr>    
    ## 1 donor         0          character
    ## 2 cholesterol   0          logical  
    ## 3 blood_sugar   0          logical  
    ## 4 HBP           0          logical  
    ## 5 low_hb_hist   0          logical  
    ## 6 anemia_hist   0          logical  
    ## 7 high_hb_hist  0          logical  
    ## 8 iron_supp     0          logical  
    ## 9 questionnaire 0          character

no\_aswers == 0 are non-factoral variables. Change in cholesterol,
blood\_sugar, HBP, low\_hb\_hist, anemia\_hist, high\_hb\_hist is coded
as if the status changed during the study.

    temp <- temp %>% mutate(cholesterol_d = case_when(cholesterol == 0 & cholesterol_2 == 1 ~ 1,
                                              TRUE ~ 0
                                              )) %>% mutate(cholesterol_d = as.logical(cholesterol_d))

    temp <- temp %>% mutate(blood_sugar_d = case_when(blood_sugar == 0 & blood_sugar_2 == 1 ~ 1,
                                              TRUE ~ 0
                                              )) %>% mutate(blood_sugar_d = as.logical(blood_sugar_d))

    temp <- temp %>% mutate(HBP_d = case_when(HBP == 0 & HBP_2 == 1 ~ 1,
                                              TRUE ~ 0
                                              )) %>% mutate(HBP_d = as.logical(HBP_d))

    temp <- temp %>% mutate(low_hb_hist_d = case_when(low_hb_hist == 0 & low_hb_hist_2 == 1 ~ 1,
                                              TRUE ~ 0
                                              )) %>% mutate(low_hb_hist_d = as.logical(low_hb_hist_d))

    temp <- temp %>% mutate(anemia_hist_d = case_when(anemia_hist == 0 & anemia_hist_2 == 1 ~ 1,
                                              TRUE ~ 0
                                              )) %>% mutate(anemia_hist_d = as.logical(anemia_hist_d))

    temp <- temp %>% mutate(high_hb_hist_d = case_when(high_hb_hist == 0 & high_hb_hist_2 == 1 ~ 1,
                                              TRUE ~ 0
                                              )) %>% mutate(high_hb_hist_d = as.logical(high_hb_hist_d))

iron\_supp recoded as either reducing, stable or increasing.

    temp <- temp %>% mutate(iron_supp_d = case_when(iron_supp == FALSE & iron_supp_2 == TRUE ~ "Increased",
                                                    iron_supp == TRUE & iron_supp_2 == FALSE ~ "Reduced",
                                              TRUE ~ "Stable"
                                              )) %>% mutate( iron_supp_d= factor(iron_supp_d,levels = c("Reduced","Stable","Increased"),ordered = TRUE))

sleep\_night recoded as difference

    ##Later in explorative_analysis.Rmd we find out that some variables cannot be fitted. We remove them now not to loose data over them
    #temp <- temp %>% mutate(sleep_night_d = sleep_night_2 - sleep_night)

    no_answers %>%  filter(no_answers == 6)

    ## # A tibble: 9 x 3
    ##   variable      no_answers classes       
    ##   <chr>         <chr>      <chr>         
    ## 1 freq_calm     6          ordered factor
    ## 2 freq_red_meat 6          ordered factor
    ## 3 freq_fruits   6          ordered factor
    ## 4 freq_juices   6          ordered factor
    ## 5 freq_wholem   6          ordered factor
    ## 6 freq_milk     6          ordered factor
    ## 7 freq_cof      6          ordered factor
    ## 8 freq_tea      6          ordered factor
    ## 9 phys_cond     6          ordered factor

These are all ordered factors. They are recoded as either reducing,
stable or increasing.

    temp <- temp %>% mutate(freq_calm_d = as.numeric(freq_calm_2)  - as.numeric(freq_calm)) %>% 
      mutate(freq_calm_d = case_when(
        freq_calm_d > 0 ~ "Increased",
        freq_calm_d < 0 ~ "Reduced",
        TRUE ~ "Stable"
      )) %>% mutate( freq_calm_d= factor(freq_calm_d,levels = c("Reduced","Stable","Increased"),ordered = TRUE))

    temp <- temp %>% mutate(freq_red_meat_d = as.numeric(freq_red_meat_2)  - as.numeric(freq_red_meat)) %>% 
      mutate(freq_red_meat_d = case_when(
        freq_red_meat_d > 0 ~ "Increased",
        freq_red_meat_d < 0 ~ "Reduced",
        TRUE ~ "Stable"
      )) %>% mutate( freq_red_meat_d= factor(freq_red_meat_d,levels = c("Reduced","Stable","Increased"),ordered = TRUE))

    temp <- temp %>% mutate(freq_fruits_d = as.numeric(freq_fruits_2)  - as.numeric(freq_fruits)) %>% 
      mutate(freq_fruits_d = case_when(
        freq_fruits_d > 0 ~ "Increased",
        freq_fruits_d < 0 ~ "Reduced",
        TRUE ~ "Stable"
      )) %>% mutate( freq_fruits_d= factor(freq_fruits_d,levels = c("Reduced","Stable","Increased"),ordered = TRUE))

    temp <- temp %>% mutate(freq_juices_d = as.numeric(freq_juices_2)  - as.numeric(freq_juices)) %>% 
      mutate(freq_juices_d = case_when(
        freq_juices_d > 0 ~ "Increased",
        freq_juices_d < 0 ~ "Reduced",
        TRUE ~ "Stable"
      )) %>% mutate( freq_juices_d= factor(freq_juices_d,levels = c("Reduced","Stable","Increased"),ordered = TRUE))

    temp <- temp %>% mutate(freq_wholem_d = as.numeric(freq_wholem_2)  - as.numeric(freq_wholem)) %>% 
      mutate(freq_wholem_d = case_when(
        freq_wholem_d > 0 ~ "Increased",
        freq_wholem_d < 0 ~ "Reduced",
        TRUE ~ "Stable"
      )) %>% mutate( freq_wholem_d= factor(freq_wholem_d,levels = c("Reduced","Stable","Increased"),ordered = TRUE))

    temp <- temp %>% mutate(freq_milk_d = as.numeric(freq_milk_2)  - as.numeric(freq_milk)) %>% 
      mutate(freq_milk_d = case_when(
        freq_milk_d > 0 ~ "Increased",
        freq_milk_d < 0 ~ "Reduced",
        TRUE ~ "Stable"
      )) %>% mutate( freq_milk_d= factor(freq_milk_d,levels = c("Reduced","Stable","Increased"),ordered = TRUE))

    temp <- temp %>% mutate(freq_cof_d = as.numeric(freq_cof_2)  - as.numeric(freq_cof)) %>% 
      mutate(freq_cof_d = case_when(
        freq_cof_d > 0 ~ "Increased",
        freq_cof_d < 0 ~ "Reduced",
        TRUE ~ "Stable"
      )) %>% mutate( freq_cof_d= factor(freq_cof_d,levels = c("Reduced","Stable","Increased"),ordered = TRUE))

    temp <- temp %>% mutate(freq_tea_d = as.numeric(freq_tea_2)  - as.numeric(freq_tea)) %>% 
      mutate(freq_tea_d = case_when(
        freq_tea_d > 0 ~ "Increased",
        freq_tea_d < 0 ~ "Reduced",
        TRUE ~ "Stable"
      )) %>% mutate( freq_tea_d= factor(freq_tea_d,levels = c("Reduced","Stable","Increased"),ordered = TRUE))

    temp <- temp %>% mutate(phys_cond_d = as.numeric(phys_cond_2)  - as.numeric(phys_cond)) %>% 
      mutate(phys_cond_d = case_when(
        phys_cond_d > 0 ~ "Increased",
        phys_cond_d < 0 ~ "Reduced",
        TRUE ~ "Stable"
      )) %>% mutate( phys_cond_d= factor(phys_cond_d,levels = c("Reduced","Stable","Increased"),ordered = TRUE))

    no_answers %>%  filter(no_answers == 4)

    ## # A tibble: 7 x 3
    ##   variable     no_answers classes       
    ##   <chr>        <chr>      <chr>         
    ## 1 act_heavy    4          ordered factor
    ## 2 sleep_enough 4          ordered factor
    ## 3 sleep_tired  4          ordered factor
    ## 4 day_act      4          ordered factor
    ## 5 h_act_light  4          ordered factor
    ## 6 anti_inf     4          ordered factor
    ## 7 vita_multi   4          ordered factor

These are all ordered factors. They are recoded as either reducing,
stable or increasing.

    temp <- temp %>% mutate(act_heavy_d = as.numeric(act_heavy_2)  - as.numeric(act_heavy)) %>% 
      mutate(act_heavy_d = case_when(
        act_heavy_d > 0 ~ "Increased",
        act_heavy_d < 0 ~ "Reduced",
        TRUE ~ "Stable"
      )) %>% mutate( act_heavy_d= factor(act_heavy_d,levels = c("Reduced","Stable","Increased"),ordered = TRUE))

    #sleep_enough
    temp <- temp %>% mutate(sleep_enough_d = as.numeric(sleep_enough_2)  - as.numeric(sleep_enough)) %>% 
      mutate(sleep_enough_d = case_when(
        sleep_enough_d > 0 ~ "Increased",
        sleep_enough_d < 0 ~ "Reduced",
        TRUE ~ "Stable"
      )) %>% mutate( sleep_enough_d= factor(sleep_enough_d,levels = c("Reduced","Stable","Increased"),ordered = TRUE))

    #day_act
    temp <- temp %>% mutate(day_act_d = as.numeric(day_act_2)  - as.numeric(day_act)) %>% 
      mutate(day_act_d = case_when(
        day_act_d > 0 ~ "Increased",
        day_act_d < 0 ~ "Reduced",
        TRUE ~ "Stable"
      )) %>% mutate( day_act_d= factor(day_act_d,levels = c("Reduced","Stable","Increased"),ordered = TRUE))

    #h_act_light
    temp <- temp %>% mutate(h_act_light_d = as.numeric(h_act_light_2)  - as.numeric(h_act_light)) %>% 
      mutate(h_act_light_d = case_when(
        h_act_light_d > 0 ~ "Increased",
        h_act_light_d < 0 ~ "Reduced",
        TRUE ~ "Stable"
      )) %>% mutate( h_act_light_d= factor(h_act_light_d,levels = c("Reduced","Stable","Increased"),ordered = TRUE))

    #vita_multi
    temp <- temp %>% mutate(vita_multi_d = as.numeric(vita_multi_2)  - as.numeric(vita_multi)) %>% 
      mutate(vita_multi_d = case_when(
        vita_multi_d > 0 ~ "Increased",
        vita_multi_d < 0 ~ "Reduced",
        TRUE ~ "Stable"
      )) %>% mutate( vita_multi_d= factor(vita_multi_d,levels = c("Reduced","Stable","Increased"),ordered = TRUE))

    # Later in explorative_analysis.Rmd we find out that some variables cannot be fitted. We remove them now not to loose data over them
    #vita_iron
    # temp <- temp %>% mutate(vita_iron_d = as.numeric(vita_iron_2)  - as.numeric(vita_iron)) %>% 
    #   mutate(vita_iron_d = case_when(
    #     vita_iron_d > 0 ~ "Increased",
    #     vita_iron_d < 0 ~ "Reduced",
    #     TRUE ~ "Stable"
    #   )) %>% mutate( vita_iron_d= factor(vita_iron_d,levels = c("Reduced","Stable","Increased"),ordered = TRUE))

But these 2 are diffent order of time and they are turned around to have
same reducing, stable or increasing coding.

sleep\_tired Levels: always\_or\_almost &lt; mostly &lt; rarely &lt;
never i.e. is their tiredness reduced, stable or increasing

anti\_inf: Levels: daily &lt; not\_daily &lt; occasionally &lt; none

    temp <- temp %>% mutate(sleep_tired_d = as.numeric(sleep_tired)  - as.numeric(sleep_tired_2)) %>% 
      mutate(sleep_tired_d = case_when(
        sleep_tired_d > 0 ~ "Increased",
        sleep_tired_d < 0 ~ "Reduced",
        TRUE ~ "Stable"
      )) %>% mutate( sleep_tired_d= factor(sleep_tired_d,levels = c("Reduced","Stable","Increased"),ordered = TRUE))

    temp <- temp %>% mutate(anti_inf_d = as.numeric(anti_inf)  - as.numeric(anti_inf_2)) %>% 
      mutate(anti_inf_d = case_when(
        anti_inf_d > 0 ~ "Increased",
        anti_inf_d < 0 ~ "Reduced",
        TRUE ~ "Stable"
      )) %>% mutate( anti_inf_d= factor(anti_inf_d,levels = c("Reduced","Stable","Increased"),ordered = TRUE))

    no_answers %>%  filter(no_answers == 3)

    ## # A tibble: 1 x 3
    ##   variable no_answers classes       
    ##   <chr>    <chr>      <chr>         
    ## 1 phys_inf 3          ordered factor

phys\_inf Levels: yes &lt; don't know &lt; no -&gt; has your physical
health inferior? e.g. first : no, second: yes -&gt; physical health
inferring "Increased"

ment\_inf Levels: yes &lt; don't know &lt; no -&gt; has your mental
health inferred?

    temp <- temp %>% mutate(phys_inf_d = as.numeric(phys_inf)  - as.numeric(phys_inf_2)) %>% 
      mutate(phys_inf_d = case_when(
        phys_inf_d > 0 ~ "Increased",
        phys_inf_d < 0 ~ "Reduced",
        TRUE ~ "Stable"
      )) %>% mutate( phys_inf_d= factor(phys_inf_d,levels = c("Reduced","Stable","Increased"),ordered = TRUE))

    #Later in explorative_analysis.Rmd we find out that some variables cannot be fitted. We remove them now not to loose data over them
    # temp <- temp %>% mutate(ment_inf_d = as.numeric(ment_inf)  - as.numeric(ment_inf_2)) %>% 
    #   mutate(ment_inf_d = case_when(
    #     ment_inf_d > 0 ~ "Increased",
    #     ment_inf_d < 0 ~ "Reduced",
    #     TRUE ~ "Stable"
    #   )) %>% mutate( ment_inf_d= factor(ment_inf_d,levels = c("Reduced","Stable","Increased"),ordered = TRUE))

    no_answers %>%  filter(no_answers == 5)

    ## # A tibble: 1 x 3
    ##   variable no_answers classes       
    ##   <chr>    <chr>      <chr>         
    ## 1 freq_alc 5          ordered factor

freq\_alc Levels: daily\_or\_almost &lt; a\_few\_per\_week &lt;
a\_few\_per\_month &lt; very\_rarely &lt; never

    temp <- temp %>% mutate(freq_alc_d = as.numeric(freq_alc)  - as.numeric(freq_alc_2)) %>% 
      mutate(freq_alc_d = case_when(
        freq_alc_d > 0 ~ "Increased",
        freq_alc_d < 0 ~ "Reduced",
        TRUE ~ "Stable"
      )) %>% mutate( freq_alc_d= factor(freq_alc_d,levels = c("Reduced","Stable","Increased"),ordered = TRUE))

Select only the change variables and store.

    cols2keep<- grep("_d$",colnames(temp),value = TRUE)
    cols2keep

    ##  [1] "cholesterol_d"   "blood_sugar_d"   "HBP_d"          
    ##  [4] "low_hb_hist_d"   "anemia_hist_d"   "high_hb_hist_d" 
    ##  [7] "iron_supp_d"     "freq_calm_d"     "freq_red_meat_d"
    ## [10] "freq_fruits_d"   "freq_juices_d"   "freq_wholem_d"  
    ## [13] "freq_milk_d"     "freq_cof_d"      "freq_tea_d"     
    ## [16] "phys_cond_d"     "act_heavy_d"     "sleep_enough_d" 
    ## [19] "day_act_d"       "h_act_light_d"   "vita_multi_d"   
    ## [22] "sleep_tired_d"   "anti_inf_d"      "phys_inf_d"     
    ## [25] "freq_alc_d"

    cols2keep<- c("donor",cols2keep)
    changedata <- temp %>% dplyr::select(cols2keep)
    dim(changedata)

    ## [1] 1047   26

Checks
======

    setdiff(colnames(first),gsub("_d$","",colnames(changedata)))

    ## [1] "questionnaire"

    setdiff(gsub("_d$","",colnames(changedata)),colnames(first))

    ## character(0)

    classes <- sort(unlist(lapply(lapply(changedata,class),paste,collapse=" ")))
    classes <- as.tibble(cbind(names(classes),classes))
    levels <- unlist(lapply(lapply(changedata,levels),paste,collapse=" "))
    levels <- as.tibble(cbind(names(levels),levels))
    changedataclasses <- full_join(classes,levels,by=c("V1"="V1")) %>% rename(variable=V1)  
    kable(changedataclasses,caption="Classes and levels of variables only analysed in explorative analysis")

<table>
<caption>Classes and levels of variables only analysed in explorative analysis</caption>
<thead>
<tr class="header">
<th align="left">variable</th>
<th align="left">classes</th>
<th align="left">levels</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">donor</td>
<td align="left">character</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">cholesterol_d</td>
<td align="left">logical</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">blood_sugar_d</td>
<td align="left">logical</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">HBP_d</td>
<td align="left">logical</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">low_hb_hist_d</td>
<td align="left">logical</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">anemia_hist_d</td>
<td align="left">logical</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">high_hb_hist_d</td>
<td align="left">logical</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">iron_supp_d</td>
<td align="left">ordered factor</td>
<td align="left">Reduced Stable Increased</td>
</tr>
<tr class="odd">
<td align="left">freq_calm_d</td>
<td align="left">ordered factor</td>
<td align="left">Reduced Stable Increased</td>
</tr>
<tr class="even">
<td align="left">freq_red_meat_d</td>
<td align="left">ordered factor</td>
<td align="left">Reduced Stable Increased</td>
</tr>
<tr class="odd">
<td align="left">freq_fruits_d</td>
<td align="left">ordered factor</td>
<td align="left">Reduced Stable Increased</td>
</tr>
<tr class="even">
<td align="left">freq_juices_d</td>
<td align="left">ordered factor</td>
<td align="left">Reduced Stable Increased</td>
</tr>
<tr class="odd">
<td align="left">freq_wholem_d</td>
<td align="left">ordered factor</td>
<td align="left">Reduced Stable Increased</td>
</tr>
<tr class="even">
<td align="left">freq_milk_d</td>
<td align="left">ordered factor</td>
<td align="left">Reduced Stable Increased</td>
</tr>
<tr class="odd">
<td align="left">freq_cof_d</td>
<td align="left">ordered factor</td>
<td align="left">Reduced Stable Increased</td>
</tr>
<tr class="even">
<td align="left">freq_tea_d</td>
<td align="left">ordered factor</td>
<td align="left">Reduced Stable Increased</td>
</tr>
<tr class="odd">
<td align="left">phys_cond_d</td>
<td align="left">ordered factor</td>
<td align="left">Reduced Stable Increased</td>
</tr>
<tr class="even">
<td align="left">act_heavy_d</td>
<td align="left">ordered factor</td>
<td align="left">Reduced Stable Increased</td>
</tr>
<tr class="odd">
<td align="left">sleep_enough_d</td>
<td align="left">ordered factor</td>
<td align="left">Reduced Stable Increased</td>
</tr>
<tr class="even">
<td align="left">day_act_d</td>
<td align="left">ordered factor</td>
<td align="left">Reduced Stable Increased</td>
</tr>
<tr class="odd">
<td align="left">h_act_light_d</td>
<td align="left">ordered factor</td>
<td align="left">Reduced Stable Increased</td>
</tr>
<tr class="even">
<td align="left">vita_multi_d</td>
<td align="left">ordered factor</td>
<td align="left">Reduced Stable Increased</td>
</tr>
<tr class="odd">
<td align="left">sleep_tired_d</td>
<td align="left">ordered factor</td>
<td align="left">Reduced Stable Increased</td>
</tr>
<tr class="even">
<td align="left">anti_inf_d</td>
<td align="left">ordered factor</td>
<td align="left">Reduced Stable Increased</td>
</tr>
<tr class="odd">
<td align="left">phys_inf_d</td>
<td align="left">ordered factor</td>
<td align="left">Reduced Stable Increased</td>
</tr>
<tr class="even">
<td align="left">freq_alc_d</td>
<td align="left">ordered factor</td>
<td align="left">Reduced Stable Increased</td>
</tr>
</tbody>
</table>

Additional questions of the second questionaire
===============================================

Consider adding the second questionaire additional questions back and
save

    additionalq <- inner_join(changedata,additionalq,by=c("donor"="donor"))
    table(is.na(additionalq$life_qual))

    ## 
    ## FALSE  TRUE 
    ##  1045     2

    table(is.na(additionalq$employment))

    ## 
    ## FALSE  TRUE 
    ##  1045     2

    table(is.na(additionalq$education))

    ## 
    ## FALSE  TRUE 
    ##  1045     2

    ggplot(additionalq) + geom_bar(aes(x=life_qual))

![](../results/inital_data_analysis_files/figure-markdown_strict/unnamed-chunk-83-1.png)

Could excellent be combined very\_good and very\_poor and poor to
average?

    additionalq <- additionalq %>% mutate(life_qual = case_when(
      life_qual == "very_poor" ~ "average",
      life_qual == "poor" ~ "average",
      life_qual == "average" ~ "average",
      life_qual == "good" ~ "good",
      life_qual == "very_good" ~ "very_good",
      life_qual == "excellent" ~ "very_good"
    )) %>% mutate(life_qual = factor(life_qual))

    ggplot(additionalq) + geom_bar(aes(x=life_qual))

![](../results/inital_data_analysis_files/figure-markdown_strict/unnamed-chunk-85-1.png)

    ggplot(additionalq) + geom_bar(aes(x=employment))

![](../results/inital_data_analysis_files/figure-markdown_strict/unnamed-chunk-86-1.png)

This is not at all informative. Could it be fulltime or not?

    additionalq <- additionalq %>% mutate(employment = case_when(
      employment == "full-time" ~ TRUE,
      TRUE ~ FALSE
      ))

    ggplot(additionalq) + geom_bar(aes(x=education))

![](../results/inital_data_analysis_files/figure-markdown_strict/unnamed-chunk-88-1.png)

Also some grouping would be good in here.

    additionalq <- additionalq %>% mutate(education = case_when(
      education == "trade-school" |  education == "vocational_college" |  education == "graduate_degree" | education == "bachelor_degree"   ~ "high",
      education == "vocational_school" |  education == "high-school"  ~ "middle",
      education == "basic_education" | education == "middle-school"  ~ "lower"
      
      )) %>% mutate(education = factor(education))
    #Later in explorative_analysis.Rmd we find out that education cannot be fitted 
    additionalq <- additionalq %>% select(-education)

    cat("Down to:" ,nrow(na.omit(additionalq)),"from:",nrow(additionalq),"\n")

    ## Down to: 1045 from: 1047

    changedata <- na.omit(additionalq)


    #changedata <- inner_join(changedata,additionalq,by=c("donor"="donor"))
    file <- "../results/changedata.rdata"
    save(changedata,file=file)

Just to check:

    changedata %>% count()

    ## # A tibble: 1 x 1
    ##       n
    ##   <int>
    ## 1  1045

By dropping many questions we manage to save several hundred donors. In
addition, next in explorative\_analysis.Rmd even couple more variables
need to dropped out in order to fit the models. Hence, dropping related
variables is required.