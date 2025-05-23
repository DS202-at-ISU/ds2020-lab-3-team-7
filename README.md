
<!-- README.md is generated from README.Rmd. Please edit the README.Rmd file -->

# Lab report \#3 - instructions

Follow the instructions posted at
<https://ds202-at-isu.github.io/labs.html> for the lab assignment. The
work is meant to be finished during the lab time, but you have time
until Monday evening to polish things.

Include your answers in this document (Rmd file). Make sure that it
knits properly (into the md file). Upload both the Rmd and the md file
to your repository.

All submissions to the github repo will be automatically uploaded for
grading once the due date is passed. Submit a link to your repository on
Canvas (only one submission per team) to signal to the instructors that
you are done with your submission.

# Lab 3: Avenger’s Peril

## As a team

Extract from the data below two data sets in long form `deaths` and
`returns`

``` r
av <- read.csv("https://raw.githubusercontent.com/fivethirtyeight/data/master/avengers/avengers.csv", stringsAsFactors = FALSE)
head(av)
```

    ##                                                       URL
    ## 1           http://marvel.wikia.com/Henry_Pym_(Earth-616)
    ## 2      http://marvel.wikia.com/Janet_van_Dyne_(Earth-616)
    ## 3       http://marvel.wikia.com/Anthony_Stark_(Earth-616)
    ## 4 http://marvel.wikia.com/Robert_Bruce_Banner_(Earth-616)
    ## 5        http://marvel.wikia.com/Thor_Odinson_(Earth-616)
    ## 6       http://marvel.wikia.com/Richard_Jones_(Earth-616)
    ##                    Name.Alias Appearances Current. Gender Probationary.Introl
    ## 1   Henry Jonathan "Hank" Pym        1269      YES   MALE                    
    ## 2              Janet van Dyne        1165      YES FEMALE                    
    ## 3 Anthony Edward "Tony" Stark        3068      YES   MALE                    
    ## 4         Robert Bruce Banner        2089      YES   MALE                    
    ## 5                Thor Odinson        2402      YES   MALE                    
    ## 6      Richard Milhouse Jones         612      YES   MALE                    
    ##   Full.Reserve.Avengers.Intro Year Years.since.joining Honorary Death1 Return1
    ## 1                      Sep-63 1963                  52     Full    YES      NO
    ## 2                      Sep-63 1963                  52     Full    YES     YES
    ## 3                      Sep-63 1963                  52     Full    YES     YES
    ## 4                      Sep-63 1963                  52     Full    YES     YES
    ## 5                      Sep-63 1963                  52     Full    YES     YES
    ## 6                      Sep-63 1963                  52 Honorary     NO        
    ##   Death2 Return2 Death3 Return3 Death4 Return4 Death5 Return5
    ## 1                                                            
    ## 2                                                            
    ## 3                                                            
    ## 4                                                            
    ## 5    YES      NO                                             
    ## 6                                                            
    ##                                                                                                                                                                              Notes
    ## 1                                                                                                                Merged with Ultron in Rage of Ultron Vol. 1. A funeral was held. 
    ## 2                                                                                                  Dies in Secret Invasion V1:I8. Actually was sent tto Microverse later recovered
    ## 3 Death: "Later while under the influence of Immortus Stark committed a number of horrible acts and was killed.'  This set up young Tony. Franklin Richards later brought him back
    ## 4                                                                               Dies in Ghosts of the Future arc. However "he had actually used a hidden Pantheon base to survive"
    ## 5                                                      Dies in Fear Itself brought back because that's kind of the whole point. Second death in Time Runs Out has not yet returned
    ## 6                                                                                                                                                                             <NA>

Get the data into a format where the five columns for Death\[1-5\] are
replaced by two columns: Time, and Death. Time should be a number
between 1 and 5 (look into the function `parse_number`); Death is a
categorical variables with values “yes”, “no” and ““. Call the resulting
data set `deaths`.

``` r
library(tidyverse)

av %>% 
  select(
    Name.Alias,
    starts_with("Death")
  ) %>% 
  head()
```

    ##                    Name.Alias Death1 Death2 Death3 Death4 Death5
    ## 1   Henry Jonathan "Hank" Pym    YES                            
    ## 2              Janet van Dyne    YES                            
    ## 3 Anthony Edward "Tony" Stark    YES                            
    ## 4         Robert Bruce Banner    YES                            
    ## 5                Thor Odinson    YES    YES                     
    ## 6      Richard Milhouse Jones     NO

``` r
deaths <- av %>% 
  pivot_longer(
    starts_with("Death"),
    names_to = "Time",
    values_to = "Died"
  ) %>% 
  select(
    URL, Name.Alias, Time, Died
  )
head(deaths)
```

    ## # A tibble: 6 × 4
    ##   URL                                                Name.Alias      Time  Died 
    ##   <chr>                                              <chr>           <chr> <chr>
    ## 1 http://marvel.wikia.com/Henry_Pym_(Earth-616)      "Henry Jonatha… Deat… "YES"
    ## 2 http://marvel.wikia.com/Henry_Pym_(Earth-616)      "Henry Jonatha… Deat… ""   
    ## 3 http://marvel.wikia.com/Henry_Pym_(Earth-616)      "Henry Jonatha… Deat… ""   
    ## 4 http://marvel.wikia.com/Henry_Pym_(Earth-616)      "Henry Jonatha… Deat… ""   
    ## 5 http://marvel.wikia.com/Henry_Pym_(Earth-616)      "Henry Jonatha… Deat… ""   
    ## 6 http://marvel.wikia.com/Janet_van_Dyne_(Earth-616) "Janet van Dyn… Deat… "YES"

``` r
View(deaths)
```

Similarly, deal with the returns of characters.

``` r
av <- read.csv("https://raw.githubusercontent.com/fivethirtyeight/data/master/avengers/avengers.csv", stringsAsFactors = FALSE)

av %>% 
  select(
    Name.Alias,
    starts_with("Return")
  ) %>% 
  head()
```

    ##                    Name.Alias Return1 Return2 Return3 Return4 Return5
    ## 1   Henry Jonathan "Hank" Pym      NO                                
    ## 2              Janet van Dyne     YES                                
    ## 3 Anthony Edward "Tony" Stark     YES                                
    ## 4         Robert Bruce Banner     YES                                
    ## 5                Thor Odinson     YES      NO                        
    ## 6      Richard Milhouse Jones

``` r
returns <- av %>% 
  pivot_longer(
    starts_with("Return"),
    names_to = "Time",
    values_to = "Returned"
  ) %>% 
  select(
    URL, Name.Alias, Time, Returned
  )
head(returns)
```

    ## # A tibble: 6 × 4
    ##   URL                                                Name.Alias   Time  Returned
    ##   <chr>                                              <chr>        <chr> <chr>   
    ## 1 http://marvel.wikia.com/Henry_Pym_(Earth-616)      "Henry Jona… Retu… "NO"    
    ## 2 http://marvel.wikia.com/Henry_Pym_(Earth-616)      "Henry Jona… Retu… ""      
    ## 3 http://marvel.wikia.com/Henry_Pym_(Earth-616)      "Henry Jona… Retu… ""      
    ## 4 http://marvel.wikia.com/Henry_Pym_(Earth-616)      "Henry Jona… Retu… ""      
    ## 5 http://marvel.wikia.com/Henry_Pym_(Earth-616)      "Henry Jona… Retu… ""      
    ## 6 http://marvel.wikia.com/Janet_van_Dyne_(Earth-616) "Janet van … Retu… "YES"

``` r
View(returns)
```

Based on these datasets calculate the average number of deaths an
Avenger suffers.

``` r
numdeaths <- deaths %>%
  filter(Died == "YES") %>%
  count(URL) %>% summarise(averagedeaths = sum(n)/nrow(av))
  #had to use URL because some of the names were null and then it made one row with 7 deaths for all null names

#average number of deaths per avenger is .5144509
numdeaths$averagedeaths
```

    ## [1] 0.5144509

Average number of deaths for an avenger is .5144509

## Individually

For each team member, copy this part of the report.

Each team member picks one of the statements in the FiveThirtyEight
[analysis](https://fivethirtyeight.com/features/avengers-death-comics-age-of-ultron/)
and fact checks it based on the data. Use dplyr functionality whenever
possible.

### FiveThirtyEight Statement

> Quote the statement you are planning to fact-check.

### Include the code

Make sure to include the code to derive the (numeric) fact for the
statement

### Include your answer

Include at least one sentence discussing the result of your
fact-checking endeavor.

Upload your changes to the repository. Discuss and refine answers as a
team.

### - Kaitlyn Hoyme -

> “But you can only tempt death so many times. There’s a 2-in-3 chance
> that a member of the Avengers returned from their first stint in the
> afterlife, but only a 50 percent chance they recovered from a second
> or third death.” - fivethirtyeight.com

``` r
deadavengers <- av %>%
  filter(Death1 == "YES") %>%
  count(URL) 

returnedavengers <- av %>%
  filter(Death1 == "YES", Return1 == "YES") %>%
  count(URL) 

#count(returnedavengers)$n
#46 avengers died and came back once
#count(deadavengers)$n
#69 avengers died once

firstdeathreturn <- count(returnedavengers)$n / count(deadavengers)$n
firstdeathreturn
```

    ## [1] 0.6666667

``` r
#This is 2/3, so the first part of the statement is correct, as we can see after their first death, 2/3 of the avengers came back.

diedtwice <- av %>% 
  filter(Death2=="YES") %>% count(URL)
#count(diedtwice)$n
#16 avengers died twice

diedthrice <- av %>% 
  filter(Death3=="YES") %>% count(URL)
#count(diedthrice)$n
#2 avengers died 3 times

returnedafter2 <- av %>%
   filter(Return2 == "YES") %>% count(URL)
#returnedafter2

returnedafter3 <- av %>%
    filter(Return3 == "YES") %>% count(URL)
#returnedafter3

#count(returnedafter2)$n
#8 avengers returned after their second death


#return after dying twice
count(returnedafter2)$n / count(diedtwice)$n
```

    ## [1] 0.5

``` r
#return after dying thrice
count(returnedafter3)$n / count(diedthrice)$n
```

    ## [1] 0.5

``` r
#both of these = .5, so we see the statement is true.
```

As we can confirm by looking at the code, the statement was correct in
saying that after the first death there is about a 2/3 chance of
returning, and after the 2nd or 3rd there is a 50% chance.

### - Cassidy Berghoff -

> “The MVP of the Earth-616 Marvel Universe Avengers has to be Jocasta —
> an android based on Janet van Dyne and built by Ultron — who has been
> destroyed five times and then recovered five times.” -
> fivethirtyeight.com

``` r
numDeath = deaths %>% filter(Name.Alias == "Jocasta", Died == "YES") %>% nrow()
numReturn = returns %>% filter(Name.Alias == "Jocasta", Returned == "YES") %>% nrow()

sprintf("The number of times Jocasta has died is: %d. The number of times Jocasta has returned is: %d.", numDeath, numReturn)
```

    ## [1] "The number of times Jocasta has died is: 5. The number of times Jocasta has returned is: 5."

As seen above, the statement is correct. Jocasta specifically has died 5
times and has returned all 5 times.

### - Zach Malo -

> “Given the Avengers’ 53 years in operation and overall mortality rate,
> fans of the comics can expect one current or former member to die
> every seven months or so, with a permanent death occurring once every
> 20 months.”-fivethirtyeight.com

``` r
cat(sum(deaths$Died=='YES'),"deaths have occurred\n")
```

    ## 89 deaths have occurred

``` r
cat("A death happens once every",53*12/sum(deaths$Died=='YES'),"months!\n")
```

    ## A death happens once every 7.146067 months!

``` r
cat(sum(returns$Returned=='NO'),"permanent deaths have occurred\n") 
```

    ## 32 permanent deaths have occurred

``` r
cat("A permanent death happens once every",53*12/sum(returns$Returned=='NO'),"months!\n")
```

    ## A permanent death happens once every 19.875 months!

I have found that a current or former member of the avengers dies about
every 7 months which I believe fairly falls within “or so” range. But
the unqualified statement that a permanent death happens every 20 months
is slightly off with the actual number being once every 19.875 months.

### - Tanish Visanagiri -
> “Eight out of 10 Avengers who died at some point 
> have come back to life.” – fivethirtyeight.com

```{r}
died_at_least_once <- av %>% 
  filter(Death1 == "YES" | Death2 == "YES" | Death3 == "YES" | Death4 == "YES" | Death5 == "YES") %>% 
  nrow()
  
returned_at_least_once <- av %>% 
  filter(
    (Death1 == "YES" | Death2 == "YES" | Death3 == "YES" | Death4 == "YES" | Death5 == "YES") &
      (Return1 == "YES" | Return2 == "YES" | Return3 == "YES" | Return4 == "YES" | Return5 == "YES")
  ) %>%
  nrow()

ratio_returned <- returned_at_least_once / died_at_least_once

sprintf(
  "Out of %d Avengers who died at least once, %d eventually returned. That's %.2f%%, or about %.1f out of 10.",
  died_at_least_once,
  returned_at_least_once,
  ratio_returned * 100,
  ratio_returned * 10
)
```
