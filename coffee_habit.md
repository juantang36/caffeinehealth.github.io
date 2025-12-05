coffee_habit
================
2025-12-03

#### Filter USDA food code for coffee

``` r
foodcode = read_xpt("dataset/FoodCode.xpt") |>
  clean_names()
# filter food codes that include "coffee"
coffee_foods = foodcode |>
  mutate(desc = tolower(drxfcld)) |>
  filter(str_detect(desc, "coffee"))
# USDA food code for coffee
coffee_code = coffee_foods |>
  pull(drxfdcd) |>
  unique()
```

#### Clean and tidy diet interview dataset

``` r
# import dataset
individual_foods_1 = read_xpt("dataset/Individual_Foods_1.xpt") |>
  clean_names()
individual_foods_2 = read_xpt("dataset/Individual_Foods_2.xpt") |>
  clean_names()
```

#### Coffee drinking habits

``` r
# participants that consume coffee on both day 1 and day 2
coffee_day_1 = individual_foods_1 |>
  # dr1ifdcd is the variable name for USDA food code
  filter(dr1ifdcd %in% coffee_code) |> 
  # coffee drinker on day 1
  distinct(seqn) |>
  pull(seqn)

coffee_day_2 = individual_foods_2 |>
  # dr2ifdcd is the variable name for USDA food code
  filter(dr2ifdcd %in% coffee_code) |> 
  # coffee drinker on day 2
  distinct(seqn) |>
  pull(seqn)
  
# coffee_habit: consume coffee on both days
ids_both_days = intersect(coffee_day_1, coffee_day_2)

# consume coffee for at least one day
ids_any_day = union(coffee_day_1, coffee_day_2)

# moderate coffee consumption: consume coffee only on one day
ids_one_day_only = setdiff(ids_any_day, ids_both_days)

# all participants
all_ids = union(individual_foods_1$seqn, 
                individual_foods_2$seqn)

# no_coffee_habit: do not consume coffee on both days
ids_never = setdiff(all_ids, ids_any_day)
length(ids_never)
```

    ## [1] 3373

``` r
length(ids_both_days)
```

    ## [1] 2416

``` r
length(ids_one_day_only)
```

    ## [1] 963

``` r
length(ids_never)
```

    ## [1] 3373

``` r
length(all_ids)
```

    ## [1] 6752

#### Create coffee_habit variable in main dataset

``` r
analytic = read.csv("analytic.csv") |>
  mutate(coffee_habit_binary = case_when(
    seqn %in% ids_both_days ~ TRUE, # drink coffee on both days = have coffee habit
    seqn %in% ids_never ~ FALSE, # do not drink coffee on either day = no coffee habit
    TRUE ~ NA
  ))
table(analytic$coffee_habit_binary, useNA = "ifany")
```

    ## 
    ## FALSE  TRUE  <NA> 
    ##  1336  1775  3105

``` r
analytic = analytic |>
  select(seqn, coffee_habit_binary, everything())
```

# Filter sequence by age and pregnancy status

``` r
analytic = analytic |>
  filter(
    ridageyr >= 20,
    ridageyr <= 70,
    ridexprg != 1 | is.na(ridexprg)   # only remove pregnanted group
  ) |>
  relocate(ridageyr, .after = coffee_habit_binary)
nrow(analytic)
```

    ## [1] 6216

``` r
table(analytic$coffee_habit_binary, useNA = "ifany")
```

    ## 
    ## FALSE  TRUE  <NA> 
    ##  1336  1775  3105

``` r
# Save the updated file so git can track the change
write.csv(analytic, "analytic.csv", row.names = FALSE)
```
