Read_dataset
================
2025-12-03

``` r
analytic = read_csv("./analytic.csv")
```

    ## Warning: One or more parsing issues, call `problems()` on your data
    ## frame for details, e.g.:
    ##   dat <- vroom(...)
    ##   problems(dat)

    ## Rows: 6216 Columns: 328
    ## ── Column specification ────────────────────────────────────
    ## Delimiter: ","
    ## chr   (5): bpaoarm, slq300, slq310, slq320, slq330
    ## dbl (310): seqn, ridageyr, sddsrvyr, ridstatr, riagendr, ridreth1, ridreth3,...
    ## lgl  (13): coffee_habit_binary, ridagemn, ridexagm, dmdhrgnd, dmdhragz, dmdh...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
view(analytic)
```

## Blood_Pressure_Pulse

``` r
pressure <- haven::read_xpt("./dataset/Blood_Pressure_Pulse.xpt")
view(pressure)
```

## Sleep

``` r
sleep <- haven::read_xpt("./dataset/Sleep_Disorders.xpt")
view(sleep)
```

## Diabetes

``` r
glyco <- haven::read_xpt("./dataset/Diabetes_Glycohemoglobin.xpt")
view(glyco)
```

``` r
insulin <- haven::read_xpt("./dataset/Diabetes_insulin.xpt")
view(insulin)
```

``` r
glucose <- haven::read_xpt("./dataset/Diabetes_Plasma Fasting Glucose.xpt")
view(glucose)
```

``` r
Dia_Q <- haven::read_xpt("./dataset/Diabetes_Q.xpt")
view(Dia_Q)
```

## CVD

``` r
HDL <- haven::read_xpt("./dataset/CVD_Cholesterol_HDL.xpt")
view(HDL)
```

``` r
LDL <- haven::read_xpt("./dataset/CVD_Cholesterol_LDL.xpt")
view(LDL)
```

``` r
Total <- haven::read_xpt("./dataset/CVD_Cholesterol_total.xpt")
view(Total)
```
