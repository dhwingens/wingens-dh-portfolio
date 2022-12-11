---
layout: page
title: Text Processing with R
description: This lesson uses R to analyze State of the Union Addresses
---

## Source: 
[Jeri Wieringa, “Basic Text Processing in R,” (2017),
https://doi.org/10.46430/phen0061.](https://programminghistorian.org/en/lessons/basic-text-processing-in-r)

``` r
library(tidyverse)
```

    ## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.1 ──

    ## ✓ ggplot2 3.3.5     ✓ purrr   0.3.4
    ## ✓ tibble  3.1.0     ✓ dplyr   1.0.7
    ## ✓ tidyr   1.1.3     ✓ stringr 1.4.0
    ## ✓ readr   1.4.0     ✓ forcats 0.5.1

    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
library(tokenizers)
```

``` r
text <- paste("Now, I understand that because it's an election season",
          "expectations for what we will achieve this year are low.",
          "But, Mister Speaker, I appreciate the constructive approach",
          "that you and other leaders took at the end of last year",
          "to pass a budget and make tax cuts permanent for working",
          "families. So I hope we can work together this year on some",
          "bipartisan priorities like criminal justice reform and",
          "helping people who are battling prescription drug abuse",
          "and heroin abuse. So, who knows, we might surprise the",
          "cynics again")

text
```

    ## [1] "Now, I understand that because it's an election season expectations for what we will achieve this year are low. But, Mister Speaker, I appreciate the constructive approach that you and other leaders took at the end of last year to pass a budget and make tax cuts permanent for working families. So I hope we can work together this year on some bipartisan priorities like criminal justice reform and helping people who are battling prescription drug abuse and heroin abuse. So, who knows, we might surprise the cynics again"

``` r
words <- tokenize_words(text)

words
```

    ## [[1]]
    ##  [1] "now"          "i"            "understand"   "that"         "because"     
    ##  [6] "it's"         "an"           "election"     "season"       "expectations"
    ## [11] "for"          "what"         "we"           "will"         "achieve"     
    ## [16] "this"         "year"         "are"          "low"          "but"         
    ## [21] "mister"       "speaker"      "i"            "appreciate"   "the"         
    ## [26] "constructive" "approach"     "that"         "you"          "and"         
    ## [31] "other"        "leaders"      "took"         "at"           "the"         
    ## [36] "end"          "of"           "last"         "year"         "to"          
    ## [41] "pass"         "a"            "budget"       "and"          "make"        
    ## [46] "tax"          "cuts"         "permanent"    "for"          "working"     
    ## [51] "families"     "so"           "i"            "hope"         "we"          
    ## [56] "can"          "work"         "together"     "this"         "year"        
    ## [61] "on"           "some"         "bipartisan"   "priorities"   "like"        
    ## [66] "criminal"     "justice"      "reform"       "and"          "helping"     
    ## [71] "people"       "who"          "are"          "battling"     "prescription"
    ## [76] "drug"         "abuse"        "and"          "heroin"       "abuse"       
    ## [81] "so"           "who"          "knows"        "we"           "might"       
    ## [86] "surprise"     "the"          "cynics"       "again"

``` r
length(words)
```

    ## [1] 1

``` r
length(words[[1]])
```

    ## [1] 89

``` r
tab <- table(words[[1]])
tab <- data_frame(word = names(tab), count = as.numeric(tab))
```

    ## Warning: `data_frame()` was deprecated in tibble 1.1.0.
    ## Please use `tibble()` instead.

``` r
tab
```

    ## # A tibble: 71 x 2
    ##    word       count
    ##    <chr>      <dbl>
    ##  1 a              1
    ##  2 abuse          2
    ##  3 achieve        1
    ##  4 again          1
    ##  5 an             1
    ##  6 and            4
    ##  7 appreciate     1
    ##  8 approach       1
    ##  9 are            2
    ## 10 at             1
    ## # … with 61 more rows

``` r
arrange(tab, desc(count))
```

    ## # A tibble: 71 x 2
    ##    word  count
    ##    <chr> <dbl>
    ##  1 and       4
    ##  2 i         3
    ##  3 the       3
    ##  4 we        3
    ##  5 year      3
    ##  6 abuse     2
    ##  7 are       2
    ##  8 for       2
    ##  9 so        2
    ## 10 that      2
    ## # … with 61 more rows

Detecting Sentence Boundaries
-----------------------------

``` r
sentences <- tokenize_sentences(text)
sentences
```

    ## [[1]]
    ## [1] "Now, I understand that because it's an election season expectations for what we will achieve this year are low."                                                                       
    ## [2] "But, Mister Speaker, I appreciate the constructive approach that you and other leaders took at the end of last year to pass a budget and make tax cuts permanent for working families."
    ## [3] "So I hope we can work together this year on some bipartisan priorities like criminal justice reform and helping people who are battling prescription drug abuse and heroin abuse."     
    ## [4] "So, who knows, we might surprise the cynics again"

``` r
sentence_words <- tokenize_words(sentences[[1]])
sentence_words
```

    ## [[1]]
    ##  [1] "now"          "i"            "understand"   "that"         "because"     
    ##  [6] "it's"         "an"           "election"     "season"       "expectations"
    ## [11] "for"          "what"         "we"           "will"         "achieve"     
    ## [16] "this"         "year"         "are"          "low"         
    ## 
    ## [[2]]
    ##  [1] "but"          "mister"       "speaker"      "i"            "appreciate"  
    ##  [6] "the"          "constructive" "approach"     "that"         "you"         
    ## [11] "and"          "other"        "leaders"      "took"         "at"          
    ## [16] "the"          "end"          "of"           "last"         "year"        
    ## [21] "to"           "pass"         "a"            "budget"       "and"         
    ## [26] "make"         "tax"          "cuts"         "permanent"    "for"         
    ## [31] "working"      "families"    
    ## 
    ## [[3]]
    ##  [1] "so"           "i"            "hope"         "we"           "can"         
    ##  [6] "work"         "together"     "this"         "year"         "on"          
    ## [11] "some"         "bipartisan"   "priorities"   "like"         "criminal"    
    ## [16] "justice"      "reform"       "and"          "helping"      "people"      
    ## [21] "who"          "are"          "battling"     "prescription" "drug"        
    ## [26] "abuse"        "and"          "heroin"       "abuse"       
    ## 
    ## [[4]]
    ## [1] "so"       "who"      "knows"    "we"       "might"    "surprise" "the"     
    ## [8] "cynics"   "again"

``` r
length(sentence_words[[1]])
```

    ## [1] 19

``` r
length(sentence_words[[2]])
```

    ## [1] 32

``` r
length(sentence_words[[3]])
```

    ## [1] 29

``` r
length(sentence_words[[4]])
```

    ## [1] 9

``` r
#or another way...
sapply(sentence_words, length)
```

    ## [1] 19 32 29  9

Analyzing Barack Obama’s 2016 State of the Union
------------------------------------------------

``` r
base_url <- "https://raw.githubusercontent.com/programminghistorian/jekyll/gh-pages/assets/basic-text-processing-in-r/"
url <- sprintf("%s/sotu_text/236.txt", base_url)
text <- paste(readLines(url), collapse = "\n")
```

``` r
words <- tokenize_words(text)
length(words[[1]])
```

    ## [1] 6113

``` r
tab <- table(words[[1]])
tab <- data_frame(word = names(tab), count = as.numeric(tab))
tab <- arrange(tab, desc(count))
tab
```

    ## # A tibble: 1,590 x 2
    ##    word  count
    ##    <chr> <dbl>
    ##  1 the     281
    ##  2 to      209
    ##  3 and     189
    ##  4 of      148
    ##  5 that    125
    ##  6 we      124
    ##  7 a       120
    ##  8 in      105
    ##  9 our      96
    ## 10 is       72
    ## # … with 1,580 more rows

``` r
wf <- read_csv(sprintf("%s/%s", base_url, "word_frequency.csv"))
```

    ## 
    ## ── Column specification ────────────────────────────────────────────────────────
    ## cols(
    ##   language = col_character(),
    ##   word = col_character(),
    ##   frequency = col_double()
    ## )

``` r
wf
```

    ## # A tibble: 150,000 x 3
    ##    language word  frequency
    ##    <chr>    <chr>     <dbl>
    ##  1 en       the       3.93 
    ##  2 en       of        2.24 
    ##  3 en       and       2.21 
    ##  4 en       to        2.06 
    ##  5 en       a         1.54 
    ##  6 en       in        1.44 
    ##  7 en       for       1.01 
    ##  8 en       is        0.800
    ##  9 en       on        0.638
    ## 10 en       that      0.578
    ## # … with 149,990 more rows

``` r
tab <- inner_join(tab, wf)
```

    ## Joining, by = "word"

``` r
tab
```

    ## # A tibble: 1,527 x 4
    ##    word  count language frequency
    ##    <chr> <dbl> <chr>        <dbl>
    ##  1 the     281 en           3.93 
    ##  2 to      209 en           2.06 
    ##  3 and     189 en           2.21 
    ##  4 of      148 en           2.24 
    ##  5 that    125 en           0.578
    ##  6 we      124 en           0.236
    ##  7 a       120 en           1.54 
    ##  8 in      105 en           1.44 
    ##  9 our      96 en           0.170
    ## 10 is       72 en           0.800
    ## # … with 1,517 more rows

``` r
filter(tab, frequency < 0.1)
```

    ## # A tibble: 1,457 x 4
    ##    word     count language frequency
    ##    <chr>    <dbl> <chr>        <dbl>
    ##  1 america     28 en          0.0232
    ##  2 people      27 en          0.0817
    ##  3 just        25 en          0.0787
    ##  4 world       23 en          0.0734
    ##  5 american    22 en          0.0387
    ##  6 work        22 en          0.0713
    ##  7 make        20 en          0.0689
    ##  8 want        19 en          0.0440
    ##  9 change      18 en          0.0358
    ## 10 years       18 en          0.0574
    ## # … with 1,447 more rows

``` r
print(filter(tab, frequency < 0.002), n = 15)
```

    ## # A tibble: 463 x 4
    ##    word        count language frequency
    ##    <chr>       <dbl> <chr>        <dbl>
    ##  1 laughter       11 en        0.000643
    ##  2 voices          8 en        0.00189 
    ##  3 allies          4 en        0.000844
    ##  4 harder          4 en        0.00152 
    ##  5 qaida           4 en        0.000183
    ##  6 terrorists      4 en        0.00122 
    ##  7 bipartisan      3 en        0.000145
    ##  8 generations     3 en        0.00123 
    ##  9 stamp           3 en        0.00166 
    ## 10 strongest       3 en        0.000591
    ## 11 syria           3 en        0.00136 
    ## 12 terrorist       3 en        0.00181 
    ## 13 tougher         3 en        0.000247
    ## 14 weaken          3 en        0.000181
    ## 15 accelerate      2 en        0.000544
    ## # … with 448 more rows

\#\#Document Summarization

``` r
metadata <- read_csv(sprintf("%s/%s", base_url, "metadata.csv"))
```

    ## 
    ## ── Column specification ────────────────────────────────────────────────────────
    ## cols(
    ##   president = col_character(),
    ##   year = col_double(),
    ##   party = col_character(),
    ##   sotu_type = col_character()
    ## )

``` r
metadata
```

    ## # A tibble: 236 x 4
    ##    president          year party       sotu_type
    ##    <chr>             <dbl> <chr>       <chr>    
    ##  1 George Washington  1790 Nonpartisan speech   
    ##  2 George Washington  1790 Nonpartisan speech   
    ##  3 George Washington  1791 Nonpartisan speech   
    ##  4 George Washington  1792 Nonpartisan speech   
    ##  5 George Washington  1793 Nonpartisan speech   
    ##  6 George Washington  1794 Nonpartisan speech   
    ##  7 George Washington  1795 Nonpartisan speech   
    ##  8 George Washington  1796 Nonpartisan speech   
    ##  9 John Adams         1797 Federalist  speech   
    ## 10 John Adams         1798 Federalist  speech   
    ## # … with 226 more rows

``` r
tab <- filter(tab, frequency < 0.002)
result <- c(metadata$president[236], metadata$year[236], tab$word[1:5])
paste(result, collapse = "; ")
```

    ## [1] "Barack Obama; 2016; laughter; voices; allies; harder; qaida"

Analyzing States of the Union 1790-2016
---------------------------------------

``` r
files <- sprintf("%s/sotu_text/%03d.txt", base_url, 1:236)
text <- c()
for (f in files) {
  text <- c(text, paste(readLines(f), collapse = "\n"))
}
```

``` r
words <- tokenize_words(text)
sapply(words, length)
```

    ##   [1]  1091  1404  2305  2100  1969  2922  1988  2879  2062  2220  1507  1374
    ##  [13]  3228  2203  2270  2100  2930  2863  2389  2676  1832  2448  2273  3248
    ##  [25]  3259  2115  3145  3368  4423  4378  4709  3447  5831  4733  6383  8416
    ##  [37]  9027  7745  6996  7329 10547 15109  7200  7889  7918 13473 10840 12386
    ##  [49] 11471 11521 13463  9011  8254  8436  8049  9331 16151 18251 16447 21368
    ##  [61]  7637  8340 13273  9951  9613 10152 11626 10512 13685 16396 12378 14085
    ##  [73]  6998  8410  6132  5975  9258  7155 12032  9886  7720  8772  6480 10131
    ##  [85] 10058  9844 12238  6816 10755  7909 11670 13405 13382 10315  8414  8956
    ##  [97] 19828 15196  5305 13276 13038 11559 16357 13718 12360 15972 14710 15568
    ## [109] 12157 20301 22907 19228 19708  9825 15017 17517 25147 23680 27519 19515
    ## [121] 13947 27696 23838 25275  3566  4550  7735  2125  3931  5482  4765  2714
    ## [133]  5621  5774  6715  6978 10871 10333  8800  8083 11049  4560  5705  4232
    ## [145]  2240  3545  3849  2745  4736  3815  3244  3354  3555  4649  3876  3162
    ## [157]  8202 27942  6090  5130  3418  5154  4000  5387  6997  9737  6027  7295
    ## [169]  1090  8339  4166  4954  4995  5693  5300  6260  6637  5429  3233  4453
    ## [181]  5582  7221  4941  4137  4482  4532  3997 17383  5199 22419  4153  4999
    ## [193]  4747  4580 12155  3273 21668  3475 33643  4470 33921  5182  5582  4968
    ## [205]  4235  3493  3843  4851  4824  3788  3964  5117  7033  7440  9216  6366
    ## [217]  6787  7335  7535  9155  4386  3845  5396  5201  5075  5342  5575  5729
    ## [229]  6092  7263  6909  7066  6872  7087  6819  6113

``` r
qplot(metadata$year, sapply(words, length))
```

![](text_processing_files/figure-markdown_github/unnamed-chunk-20-1.png)
<img src="text_processing_files/figure-markdown_github/unnamed-chunk-20-1.png" class="img-responsive" alt=""> </div>


``` r
qplot(metadata$year, sapply(words, length),
      color = metadata$sotu_type)
```

![](text_processing_files/figure-markdown_github/unnamed-chunk-21-1.png)

``` r
sentences <- tokenize_sentences(text)
```

``` r
sentence_words <- sapply(sentences, tokenize_words)
```

``` r
sentence_length <- list()
for (i in 1:nrow(metadata)) {
  sentence_length[[i]] <- sapply(sentence_words[[i]], length)
}
```

``` r
sentence_length_median <- sapply(sentence_length, median)
```

``` r
qplot(metadata$year, sentence_length_median)
```

![](text_processing_files/figure-markdown_github/unnamed-chunk-26-1.png)

``` r
qplot(metadata$year, sentence_length_median) +
  geom_smooth()
```

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

![](text_processing_files/figure-markdown_github/unnamed-chunk-27-1.png)

``` r
description <- c()
for (i in 1:length(words)) {
  tab <- table(words[[i]])
  tab <- data_frame(word = names(tab), count = as.numeric(tab))
  tab <- arrange(tab, desc(count))
  tab <- inner_join(tab, wf)
  tab <- filter(tab, frequency < 0.002)

  result <- c(metadata$president[i], metadata$year[i], tab$word[1:5])
  description <- c(description, paste(result, collapse = "; "))
}
```


``` r
cat(description, sep = "\n")
```

    ## George Washington; 1790; ought; render; afford; blessings; derive
    ## George Washington; 1790; militia; requisite; abundant; belongs; consuls
    ## George Washington; 1791; indians; treaties; equally; gentlemen; naturally
    ## George Washington; 1792; tribes; frontier; hitherto; hostility; legislature
    ## George Washington; 1793; ought; militia; commissioners; dignity; fulfillment
    ## George Washington; 1794; militia; insurrection; inspector; ought; gentlemen
    ## George Washington; 1795; indians; tranquillity; gentlemen; treaty; blessings
    ## George Washington; 1796; treaty; commissioners; ought; majesty; proportion
    ## John Adams; 1797; treaty; commissioners; vessels; gentlemen; sums
    ## John Adams; 1798; commissioners; gentlemen; treaty; croix; demarcation
    ## John Adams; 1799; gentlemen; commissioners; satisfactory; treaty; amity
    ## John Adams; 1800; gentlemen; happiness; considerable; ought; treaty
    ## Thomas Jefferson; 1801; legislature; requisite; respecting; vessels; expedient
    ## Thomas Jefferson; 1802; vessels; discharge; legislature; naval; extraordinary
    ## Thomas Jefferson; 1803; friendship; treasury; vessels; neutral; ours
    ## Thomas Jefferson; 1804; discharge; dispositions; establishing; friendship; intercourse
    ## Thomas Jefferson; 1805; vessels; belligerent; competent; desirable; admit
    ## Thomas Jefferson; 1806; negotiations; clarke; inhabitants; mediterranean; messrs
    ## Thomas Jefferson; 1807; harbors; intercourse; legislature; militia; threatened
    ## Thomas Jefferson; 1808; belligerent; decrees; embargo; manifested; militia
    ## James Madison; 1809; arrangement; favorable; engagements; ratification; treasury
    ## James Madison; 1810; neutral; blockades; decrees; contemplated; danish
    ## James Madison; 1811; repeal; communicated; councils; decrees; friendship
    ## James Madison; 1812; savages; militia; favorable; armistice; considerable
    ## James Madison; 1813; prisoners; honorable; militia; savages; commander
    ## James Madison; 1814; militia; warfare; civilized; commanded; frontier
    ## James Madison; 1815; treasury; tribes; afford; attained; branches
    ## James Madison; 1816; treasury; favorable; intercourse; militia; prosperity
    ## James Monroe; 1817; colonies; tribes; arrangement; treasury; treaty
    ## James Monroe; 1818; savages; tribes; adventurers; indians; respecting
    ## James Monroe; 1819; treaty; ratified; majesty; ratification; explanations
    ## James Monroe; 1820; considerable; vessels; colonies; blessings; favorable
    ## James Monroe; 1821; vessels; manufactures; treaty; colonies; provinces
    ## James Monroe; 1822; treasury; likewise; vessels; respecting; communicated
    ## James Monroe; 1823; appropriated; expenditures; treasury; treaty; vessels
    ## James Monroe; 1824; treaty; tribes; negotiation; likewise; appropriation
    ## John Quincy Adams; 1825; treasury; accomplished; commissioners; deliberations; naval
    ## John Quincy Adams; 1826; colonies; vessels; discriminating; intercourse; receipts
    ## John Quincy Adams; 1827; intercourse; receipts; treaty; negotiation; vessels
    ## John Quincy Adams; 1828; treaties; commenced; intercourse; colonies; enumeration
    ## Andrew Jackson; 1829; treasury; discharge; disposition; injurious; feelings
    ## Andrew Jackson; 1830; vessels; treaty; appropriations; treasury; indians
    ## Andrew Jackson; 1831; treaty; indemnity; majesty; tribes; assurances
    ## Andrew Jackson; 1832; treasury; intercourse; heretofore; hoped; indians
    ## Andrew Jackson; 1833; treasury; treaty; chambers; intercourse; vessels
    ## Andrew Jackson; 1834; treaty; appropriation; treasury; chambers; appropriations
    ## Andrew Jackson; 1835; treaty; chambers; constitutional; intercourse; treasury
    ## Andrew Jackson; 1836; surplus; treasury; appropriations; ration; treaty
    ## Martin Van Buren; 1837; treasury; disposition; satisfactory; vessels; indians
    ## Martin Van Buren; 1838; treasury; indians; tribes; moneys; appropriations
    ## Martin Van Buren; 1839; treasury; specie; prosperity; afford; necessity
    ## Martin Van Buren; 1840; expenditures; indians; treasury; treaty; appropriations
    ## John Tyler; 1841; treasury; circulation; discharge; necessity; distant
    ## John Tyler; 1842; treasury; circulation; exchequer; treaty; specie
    ## John Tyler; 1843; treasury; regarded; greatly; heretofore; consequence
    ## John Tyler; 1844; treaty; annexation; treasury; happiness; negotiation
    ## James K. Polk; 1845; treasury; treaty; vessels; imposed; negotiation
    ## James K. Polk; 1846; treaty; paredes; redress; annexation; grande
    ## James K. Polk; 1847; treaty; treasury; indemnity; steamers; cession
    ## James K. Polk; 1848; treasury; expenditures; constitutional; veto; tariff
    ## Zachary Taylor; 1849; treaty; treasury; vessels; correspondence; intercourse
    ## Millard Fillmore; 1850; expenditures; treasury; hoped; respectfully; treaty
    ## Millard Fillmore; 1851; treasury; appropriations; expenditures; exports; expedition
    ## Millard Fillmore; 1852; indians; prosperity; blessings; naval; treasury
    ## Franklin Pierce; 1853; intercourse; treasury; regarded; appropriations; arrangement
    ## Franklin Pierce; 1854; treaty; naval; treasury; proposition; favorable
    ## Franklin Pierce; 1855; treaty; constitutional; expenditures; nicaragua; pretensions
    ## Franklin Pierce; 1856; treaty; constitutional; panama; repeal; territories
    ## James Buchanan; 1857; treaty; slavery; honduras; circulation; whilst
    ## James Buchanan; 1858; treaty; treasury; whilst; nicaragua; ought
    ## James Buchanan; 1859; treasury; treaty; slaves; expenditures; ought
    ## James Buchanan; 1860; constitutional; treaty; slave; slavery; sovereign
    ## Abraham Lincoln; 1861; insurrection; insurgents; hired; judges; appropriation
    ## Abraham Lincoln; 1862; emancipation; slavery; slave; colored; treasury
    ## Abraham Lincoln; 1863; naval; proclamation; emancipation; receipts; disbursements
    ## Abraham Lincoln; 1864; treasury; naval; pensioners; receipts; aggregate
    ## Andrew Johnson; 1865; preservation; freedmen; treason; circulation; electors
    ## Andrew Johnson; 1866; expenditures; constitutional; declared; emperor; expedition
    ## Andrew Johnson; 1867; treasury; circulation; inclusive; coin; expenditures
    ## Andrew Johnson; 1868; expenditures; treasury; treaty; circulation; republics
    ## Ulysses S. Grant; 1869; expenditures; treaty; receipts; treasury; largely
    ## Ulysses S. Grant; 1870; treaty; vessels; domingo; disposed; appropriation
    ## Ulysses S. Grant; 1871; treaty; desirable; appropriation; herewith; hoped
    ## Ulysses S. Grant; 1872; treaty; appropriation; majesty; steamship; disposed
    ## Ulysses S. Grant; 1873; correspondence; herewith; specie; expenditures; treasury
    ## Ulysses S. Grant; 1874; prosperity; herewith; hoped; naturalization; specie
    ## Ulysses S. Grant; 1875; shores; treasury; satisfactory; treaty; herewith
    ## Ulysses S. Grant; 1876; treaty; appropriations; naturalization; diplomatic; expenditures
    ## Rutherford B. Hayes; 1877; coinage; coin; tender; treaty; expenditures
    ## Rutherford B. Hayes; 1878; indians; expenditures; appropriations; commissioners; receipts
    ## Rutherford B. Hayes; 1879; appropriation; appropriations; examinations; expenditures; treasury
    ## Rutherford B. Hayes; 1880; appropriations; appropriation; treasury; necessity; treaty
    ## Chester A. Arthur; 1881; imports; indians; exports; treasury; territories
    ## Chester A. Arthur; 1882; expenditures; appointments; treasury; appropriations; lieutenant
    ## Chester A. Arthur; 1883; treaty; circulation; customs; treasury; vessels
    ## Chester A. Arthur; 1884; vessels; treaty; intercourse; treaties; expenditures
    ## Grover Cleveland; 1885; treaty; indians; coinage; vessels; citizenship
    ## Grover Cleveland; 1886; treaty; surplus; treasury; pensions; worthy
    ## Grover Cleveland; 1887; tariff; treasury; taxation; wool; surplus
    ## Grover Cleveland; 1888; expenditures; treaty; pensions; surplus; citizenship
    ## Benjamin Harrison; 1889; treasury; circulation; indians; surplus; appropriation
    ## Benjamin Harrison; 1890; tariff; largely; receipts; treaty; expenditures
    ## Benjamin Harrison; 1891; naval; electors; treasury; appropriation; tariff
    ## Benjamin Harrison; 1892; tons; wages; vessels; imports; exports
    ## Grover Cleveland; 1893; preceding; vessels; treaty; amounted; tariff
    ## Grover Cleveland; 1894; amounted; circulation; treasury; vessels; expenditures
    ## Grover Cleveland; 1895; treasury; circulation; arbitration; coinage; consular
    ## Grover Cleveland; 1896; preceding; expenditures; amounted; treasury; vessels
    ## William McKinley; 1897; cuban; tribes; civilized; negotiations; indians
    ## William McKinley; 1898; naval; santiago; admiral; treasury; vessels
    ## William McKinley; 1899; treaty; manila; treasury; exposition; archipelago
    ## William McKinley; 1900; treaty; imperial; philippine; peking; expenditures
    ## Theodore Roosevelt; 1901; merely; cannot; arid; irrigation; streams
    ## Theodore Roosevelt; 1902; tariff; corporations; evils; prosperity; canal
    ## Theodore Roosevelt; 1903; treaty; panama; isthmus; canal; colombia
    ## Theodore Roosevelt; 1904; naturalization; corporations; interstate; civilized; citizenship
    ## Theodore Roosevelt; 1905; cannot; interstate; railroad; corporations; supervision
    ## Theodore Roosevelt; 1906; merely; colored; interstate; seals; corporations
    ## Theodore Roosevelt; 1907; interstate; corporations; railroads; supervision; tariff
    ## Theodore Roosevelt; 1908; corporations; judges; interstate; forests; fisheries
    ## William Howard Taft; 1909; tariff; deficit; naval; expenditures; treaty
    ## William Howard Taft; 1910; canal; tariff; naval; treasury; ought
    ## William Howard Taft; 1911; statute; canal; wool; tariff; corporations
    ## William Howard Taft; 1912; canal; panama; ought; tariff; naval
    ## Woodrow Wilson; 1913; ought; constitutional; nominees; administer; beg
    ## Woodrow Wilson; 1914; ought; profitable; constructive; friendship; jealous
    ## Woodrow Wilson; 1915; cruisers; submarines; treasury; fifteen; cannot
    ## Woodrow Wilson; 1916; interstate; railroads; acted; arbitration; concerted
    ## Woodrow Wilson; 1917; peoples; enemies; wrongs; alien; everywhere
    ## Woodrow Wilson; 1918; railways; railroads; billions; armies; empires
    ## Woodrow Wilson; 1919; unrest; exports; interstate; remedy; disputes
    ## Woodrow Wilson; 1920; treasury; expenditures; appropriations; floating; necessity
    ## Warren G. Harding; 1921; tariff; ought; necessity; reclamation; imports
    ## Warren G. Harding; 1922; railway; tile; ought; tribunal; freight
    ## Calvin Coolidge; 1923; ought; coal; burden; greatly; humanity
    ## Calvin Coolidge; 1924; nitrogen; prosperity; railways; taxation; burden
    ## Calvin Coolidge; 1925; ought; expenditures; judges; prosperity; tile
    ## Calvin Coolidge; 1926; ought; tariff; imports; cooperative; greatly
    ## Calvin Coolidge; 1927; farmer; surplus; treasury; appropriations; considerable
    ## Calvin Coolidge; 1928; prosperity; expenditures; surplus; encouragement; irrigation
    ## Herbert Hoover; 1929; tariff; appropriations; necessity; annum; desirable
    ## Herbert Hoover; 1930; distress; commodities; reorganization; unemployment; expenditure
    ## Herbert Hoover; 1931; unemployment; emergencies; deposits; expenditures; naval
    ## Herbert Hoover; 1932; reconstruction; expenditures; greatly; failures; accomplished
    ## Franklin D. Roosevelt; 1934; cannot; civilization; exploitation; mere; readjustment
    ## Franklin D. Roosevelt; 1935; cannot; undertaken; unemployed; livelihood; definite
    ## Franklin D. Roosevelt; 1936; autocracy; neighbor; peaceful; rulers; americas
    ## Franklin D. Roosevelt; 1937; speculation; unemployment; civilization; constitutional; continuously
    ## Franklin D. Roosevelt; 1938; abuses; expenditures; wages; cannot; balanced
    ## Franklin D. Roosevelt; 1939; aggression; eighty; idle; cannot; sixty
    ## Franklin D. Roosevelt; 1940; cannot; everywhere; centuries; civilization; compelled
    ## Franklin D. Roosevelt; 1941; everywhere; cannot; dictators; hemisphere; unity
    ## Franklin D. Roosevelt; 1942; conquest; hitler; enemies; planes; nazis
    ## Franklin D. Roosevelt; 1943; cannot; planes; conquest; criticism; decent
    ## Franklin D. Roosevelt; 1944; allies; cannot; enemies; sailors; selfish
    ## Franklin D. Roosevelt; 1945; peoples; allies; nurses; liberated; cannot
    ## Franklin D. Roosevelt; 1945; nurses; allies; cannot; enemies; peoples
    ## Harry S Truman; 1946; expenditures; appropriations; receipts; authorizations; peacetime
    ## Harry S Truman; 1947; disputes; strikes; bargaining; atomic; jurisdictional
    ## Harry S Truman; 1948; inflation; decade; incomes; restore; afford
    ## Harry S Truman; 1949; prosperity; cannot; authorize; afford; boom
    ## Harry S Truman; 1950; peoples; expenditures; ideals; cannot; mankind
    ## Harry S Truman; 1951; soviet; aggression; defend; ideals; allies
    ## Harry S Truman; 1952; communist; aggression; allies; cannot; soviet
    ## Dwight D. Eisenhower; 1953; inflation; deficits; promptly; burden; communist
    ## Harry S Truman; 1953; soviet; communist; atomic; rulers; cannot
    ## Dwight D. Eisenhower; 1954; allies; communist; propose; reductions; mobilization
    ## Dwight D. Eisenhower; 1955; vigorous; strengthen; urge; civilian; communist
    ## Dwight D. Eisenhower; 1956; expanding; allies; disarmament; intend; peacetime
    ## Dwight D. Eisenhower; 1956; prosperity; strengthen; atomic; communist; accomplished
    ## Dwight D. Eisenhower; 1957; prosperity; peoples; cannot; productive; atomic
    ## Dwight D. Eisenhower; 1958; missiles; missile; soviet; soviets; ballistic
    ## Dwight D. Eisenhower; 1959; expenditures; peoples; cannot; inflation; missile
    ## Dwight D. Eisenhower; 1960; peoples; cannot; expenditures; inflation; soviet
    ## John F. Kennedy; 1961; deficit; allies; domination; hopes; soviet
    ## Dwight D. Eisenhower; 1961; communist; strengthened; greatly; strengthening; unemployment
    ## John F. Kennedy; 1962; cannot; urge; allies; farms; compete
    ## John F. Kennedy; 1963; cannot; communist; afford; strengthen; communism
    ## Lyndon B. Johnson; 1964; hopes; cannot; unemployment; decent; afford
    ## Lyndon B. Johnson; 1965; propose; unity; communist; soviet; enrich
    ## Lyndon B. Johnson; 1966; propose; aggression; pursuit; sacrifice; strengthen
    ## Lyndon B. Johnson; 1967; soviet; succeed; allies; cannot; communist
    ## Lyndon B. Johnson; 1968; abundance; expenditures; propose; treaty; appropriations
    ## Lyndon B. Johnson; 1969; commitments; prosperity; treaty; presidents; surtax
    ## Richard M. Nixon; 1970; decade; seventies; propose; balanced; blame
    ## Richard M. Nixon; 1971; propose; localities; inflation; prosperity; peacetime
    ## Richard M. Nixon; 1972; burden; decades; bipartisan; commitments; compete
    ## Richard M. Nixon; 1972; manpower; inflation; propose; cannot; prosperity
    ## Richard M. Nixon; 1974; prosperity; afford; cannot; colleagues; lasting
    ## Richard M. Nixon; 1974; urge; inflation; bicentennial; prosperity; unemployment
    ## Gerald R. Ford; 1975; propose; coal; barrels; crude; inflation
    ## Gerald R. Ford; 1976; cannot; propose; inflation; generations; adversaries
    ## Gerald R. Ford; 1977; productive; branches; cannot; deter; recession
    ## Jimmy Carter; 1978; inflation; cannot; unemployment; canal; deficit
    ## Jimmy Carter; 1978; strengthen; propose; passage; inflation; allies
    ## Jimmy Carter; 1979; inflation; cannot; soviet; allies; congressional
    ## Jimmy Carter; 1979; inflation; propose; fy; congressional; exports
    ## Jimmy Carter; 1980; soviet; preserve; aggression; inflation; persian
    ## Jimmy Carter; 1980; soviet; inflation; urge; strengthen; propose
    ## Ronald Reagan; 1981; inflation; reductions; subsidies; taxpayers; expenditures
    ## Jimmy Carter; 1981; soviet; coal; allies; urge; assure
    ## Ronald Reagan; 1982; inflation; deficits; unemployment; needy; soviet
    ## Ronald Reagan; 1983; deficits; inflation; soviet; allies; bipartisan
    ## Ronald Reagan; 1984; bipartisan; inflation; courage; eighties; restore
    ## Ronald Reagan; 1985; stronger; barriers; propose; spreading; abortion
    ## Ronald Reagan; 1986; cannot; soviet; courage; decade; deficit
    ## Ronald Reagan; 1987; soviet; competitiveness; hemisphere; honorable; nicaragua
    ## Ronald Reagan; 1988; laughter; deficits; nicaragua; prosperity; reforms
    ## George Bush; 1989; propose; cannot; deficit; priorities; freeze
    ## George Bush; 1990; panama; soviet; anchor; announcing; bless
    ## George Bush; 1991; struggle; aggression; soviet; blessings; persian
    ## George Bush; 1992; laughter; missiles; burden; cannot; gains
    ## William J. Clinton; 1993; deficit; propose; incomes; invest; decade
    ## William J. Clinton; 1994; deficit; renew; ought; brady; cannot
    ## William J. Clinton; 1995; ought; covenant; deficit; bureaucracy; voted
    ## William J. Clinton; 1996; bipartisan; gangs; medicare; deficit; harder
    ## William J. Clinton; 1997; bipartisan; cannot; balanced; nato; immigrants
    ## William J. Clinton; 1998; bipartisan; deficit; propose; bosnia; millennium
    ## William J. Clinton; 1999; medicare; propose; surplus; balanced; bipartisan
    ## William J. Clinton; 2000; propose; laughter; medicare; bipartisan; prosperity
    ## George W. Bush; 2001; medicare; courage; surplus; josefina; laughter
    ## George W. Bush; 2002; terrorist; terrorists; allies; camps; homeland
    ## George W. Bush; 2003; hussein; saddam; inspectors; qaida; terrorists
    ## George W. Bush; 2004; terrorists; propose; medicare; seniors; killers
    ## George W. Bush; 2005; terrorists; iraqis; reforms; decades; generations
    ## George W. Bush; 2006; hopeful; offensive; retreat; terrorists; terrorist
    ## George W. Bush; 2007; terrorists; qaida; extremists; struggle; baghdad
    ## George W. Bush; 2008; terrorists; empower; qaida; extremists; deny
    ## Barack Obama; 2009; deficit; afford; cannot; lending; invest
    ## Barack Obama; 2010; deficit; laughter; afford; decade; decades
    ## Barack Obama; 2011; deficit; republicans; democrats; laughter; afghan
    ## Barack Obama; 2012; afford; deficit; tuition; cannot; doubling
    ## Barack Obama; 2013; deficit; deserve; stronger; bipartisan; medicare
    ## Barack Obama; 2014; cory; laughter; decades; diplomacy; invest
    ## Barack Obama; 2015; laughter; childcare; democrats; rebekah; republicans
    ## Barack Obama; 2016; laughter; voices; allies; harder; qaida
