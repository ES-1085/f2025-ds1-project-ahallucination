Project proposal
================
AI Hallunication Cases - Yuka, Brynn, & Sasha

``` r
library(tidyverse)
library(broom)
ai_data <- read_csv("../data/Charlotin-hallucination_cases.csv")
#view(ai_data)
```

## 1. Introduction

We found our data set on Kaggle, and it was contributed by a man named
Damien Charlotin. He is a senior research fellow, specifically on
artificial intelligence and the law. He is also a lecturer and
independent practitioner in Paris. The AI hallucinations dataset
contains cases from across the world from 2023 to 2025. The dataset
keeps a record of legal cases in which AI was used and found to be
wrong. Generative AI produced incorrect citations as well as other fake
arguments. The dataset contains the case, the type of hallucination, the
type of AI, the outcome of the case, the monetary penalty, the state,
the time, etc. Our data is set up nicely, where each observation
directly translates to a specific court case that is listed by name. A
LLM refers to a “large language model”. This represents the type of AI
used whether it was chat GPT, Gemini, etc. A hallucination is when one
of these LLMs basically produces an incorrect output or provides the
user with something nonsensical. Our data also includes a variable,
monetary penalty, which lists how much each “error” in a specific LLM is
penalized. The study in which the data was collected is relatively new.
There are currently 426 cases where generative AI has been used,
however, this dataset is still expanding as more come to light. Note
that this study is a work in progress and doesn’t contain all court
cases in which AI was used. It is noted on Kaggle that the data will be
updated annually with new findings. We were mainly interested in when
these cases started emerging, what forms of AI were being used the most,
and how some of our variables like party and hallucination type, affect
monetary penalty.Some of our research questions we want to examine
further are:

1.  Which states come up the most with AI hallucinations?
2.  What LLM is being used the most?
3.  What year did cases start to spike?
4.  What AI hallucinations have a higher monetary penalty?
5.  What effect did the party type have on the outcome and monetary
    penalty within each case?

## 2. Data

**Renaming Variables**

``` r
ai_data <- as_tibble(ai_data) |>
  rename(
`case_name` = `Case Name`,
`court` = `Court`,
`state` = `State(s)`,
`date` = `Date`,
`party` = `Party(ies)`,
`ai_tool` = `AI Tool`,
`hallucination` = `Hallucination`,
`outcome` = `Outcome`,
`monetary_penalty` = `Monetary Penalty`,
`professional_sanction` = `Professional Sanction`,
`key_principle` = `Key Principle`,
`pointer` = `Pointer`,
`source` = `Source`,
`details` = `Details`
)
```

**Data Cleanup**

Since we have very little numerical data, we need to first clean up our
dataset before we visualize. We focused specifically on cleaning the
variables we are analyzing: court, location, ai_tool, hallucination,
outcome, monetary_penalty.

First, we removed all extra spaces, standardized the capitalization, and
converted all empty observations to NA for all of our variables of
interest. Next, we did the same focusing specifically on ai_tool,
splitting rows based on common separators such as slashes, colons, and
commas. After tidying these data, we accounted for synonyms, and finally
did a wordcount for each of the tools mentioned. Then, we focused
specifically on monetary_penalty, and removed all non-numeric
characters, replaced any text indicative of a missing value with NA, and
turned the numbers into an actual number type. Finally, for the
variables hallucination, outcome, and professional_sanction, we used
keywords to group text (ignoring capitalization) and replace it with a
standard word or phrase.

``` r
ai_data <- ai_data |>
  mutate(
    across(
      c(court, state, hallucination, outcome, monetary_penalty, professional_sanction), ~ .x |>
        str_squish() |>
        str_to_title() |>
        na_if("")
    ),
monetary_penalty = monetary_penalty |>
      str_replace_all("[\\$,]", "") |>
      str_replace_all("(?i)none|n/a|na", NA_character_) |>
      as.numeric()
)
```

    ## Warning: There was 1 warning in `mutate()`.
    ## ℹ In argument: `monetary_penalty = as.numeric(...)`.
    ## Caused by warning:
    ## ! NAs introduced by coercion

``` r
ai_tool_tidy <- ai_data |>
  mutate(ai_tool = str_replace_all(ai_tool, " and | & ", ",")) |>
  separate_rows(ai_tool, sep = ",|/|;") |>
  mutate(ai_tool = str_squish(ai_tool),
         ai_tool = na_if(ai_tool, ""),
         ai_tool = if_else(str_to_lower(ai_tool) %in% c("n/a","na","none"), NA_character_, ai_tool)) |>
  drop_na(ai_tool)
```

``` r
synonyms <- c(
  "ChatGPT" = "OpenAI ChatGPT",
  "GPT-4" = "OpenAI ChatGPT",
  "GPT4" = "OpenAI ChatGPT",
  "Bard" = "Google Bard / Gemini",
  "Gemini" = "Google Bard / Gemini",
  "Claude" = "Anthropic Claude"
)
ai_tool_tidy <- ai_tool_tidy |>
  mutate(ai_tool = coalesce(synonyms[ai_tool], ai_tool))
```

``` r
word_counts <- ai_tool_tidy |>
  count(ai_tool, name = "freq", sort = TRUE)
```

``` r
ai_data <- ai_data |>
  mutate(
    hallucination = case_when(
      str_detect(coalesce(hallucination, ""), regex("citation", ignore_case = TRUE)) ~ "Fake Citation",
      str_detect(coalesce(hallucination, ""), regex("argument",  ignore_case = TRUE)) ~ "Fake Argument",
      str_detect(coalesce(hallucination, ""), regex("quote|source", ignore_case = TRUE)) ~ "Fake Source/Quote",
      TRUE ~ hallucination
    )
  )
ai_data <- ai_data |>
  mutate(
    hallucination = case_when(
      str_detect(hallucination, regex("citation", ignore_case = TRUE)) ~ "Fake Citation",
      str_detect(hallucination, regex("argument", ignore_case = TRUE)) ~ "Fake Argument",
      str_detect(hallucination, regex("quote|source", ignore_case = TRUE)) ~ "Fake Source/Quote",
      TRUE ~ hallucination
    )
  )
ai_data <- ai_data |>
  mutate(
    outcome = case_when(
      str_detect(outcome, regex("dismissed|thrown out", ignore_case = TRUE)) ~ "Dismissed",
      str_detect(outcome, regex("pending|ongoing", ignore_case = TRUE)) ~ "Pending",
      str_detect(outcome, regex("fined|sanction|penalty", ignore_case = TRUE)) ~ "Penalized",
      TRUE ~ outcome
    )
  )
ai_data <- ai_data |>
  mutate(
    professional_sanction = case_when(
      str_detect(professional_sanction, regex("yes|y", ignore_case = TRUE)) ~ "Yes",
      str_detect(professional_sanction, regex("no|n", ignore_case = TRUE)) ~ "No",
      TRUE ~ professional_sanction
    )
  )
```

``` r
print(dim(ai_data))
```

    ## [1] 426  14

``` r
print(glimpse(ai_data))
```

    ## Rows: 426
    ## Columns: 14
    ## $ case_name             <chr> "The People v. Raziel Ruiz Alvarez", "In the Int…
    ## $ court                 <chr> "Ca California", "Ca Iowa", "Ca California", "D.…
    ## $ state                 <chr> "Usa", "Usa", "Usa", "Usa", "Usa", "Usa", "Singa…
    ## $ date                  <date> 2025-10-02, 2025-10-01, 2025-09-30, 2025-09-30,…
    ## $ party                 <chr> "Lawyer", "Lawyer", "Pro Se Litigant", "Expert",…
    ## $ ai_tool               <chr> "Unidentified", "Unidentified", "Implied", "Chat…
    ## $ hallucination         <chr> "Fake Citation", "Fake Citation", "Fake Citation…
    ## $ outcome               <chr> "Penalized", "Penalized", "Warning", "Dismissed"…
    ## $ monetary_penalty      <dbl> NA, NA, NA, NA, NA, NA, 800, NA, NA, NA, NA, NA,…
    ## $ professional_sanction <chr> "Yes", "Yes", "No", "No", "No", "No", "No", "No"…
    ## $ key_principle         <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
    ## $ pointer               <chr> "Volokh", NA, NA, NA, NA, NA, NA, NA, NA, NA, "V…
    ## $ source                <chr> "https://reason.com/volokh/2025/10/04/court-espe…
    ## $ details               <chr> "\"When criminal defense attorneys fail to compl…
    ## # A tibble: 426 × 14
    ##    case_name          court state date       party ai_tool hallucination outcome
    ##    <chr>              <chr> <chr> <date>     <chr> <chr>   <chr>         <chr>  
    ##  1 The People v. Raz… Ca C… Usa   2025-10-02 Lawy… Uniden… Fake Citation Penali…
    ##  2 In the Interest o… Ca I… Usa   2025-10-01 Lawy… Uniden… Fake Citation Penali…
    ##  3 In re the Marriag… Ca C… Usa   2025-09-30 Pro … Implied Fake Citation Warning
    ##  4 Khoury et al v. I… D. U… Usa   2025-09-30 Expe… ChatGPT Fake Evidence Dismis…
    ##  5 Tomlin v. State o… D. N… Usa   2025-09-30 Pro … Implied Fake Citation Warning
    ##  6 Munoz v. Lopez     Ca C… Usa   2025-09-29 Pro … Implied Fake Citation Warning
    ##  7 Tajudin bin Gulam… High… Sing… 2025-09-29 Lawy… Uniden… Fake Citation Costs …
    ##  8 Chapter Kris Jack… N.d.… Usa   2025-09-29 Pro … Implied Fake Citation Show C…
    ##  9 Jade Riley Burch … D. N… Usa   2025-09-26 Pro … Uniden… Fake Citation Warning
    ## 10 Reddy v Saroya     Ca A… Cana… 2025-09-26 Lawy… Implied Fake Citation <NA>   
    ## # ℹ 416 more rows
    ## # ℹ 6 more variables: monetary_penalty <dbl>, professional_sanction <chr>,
    ## #   key_principle <chr>, pointer <chr>, source <chr>, details <chr>

``` r
print(spec(ai_data))
```

    ## NULL

## 3. Data analysis plan

- What variables will you visualize to explore your research questions?

court -\> We will explore whether or not any patterns arise between the
specific court and the severity of the punishment, or rather if there
are certain courts that try more ai hallucination cases than others

location -\> We will observe if there is any relationship between
location and the frequency of cases

ai_tool -\> Do certain ai tools/softwares appear in these cases more
often? Are certain awarded larger penalties?

hallucination -\> What were the most common hallucinations in these
cases? Were there specific ai tools that were ore often responsible for
certain hallucinations?

outcome -\> What were the most common outcomes for these cases, and do
they vary by location, ai tool used, court tries in, or the specific
hallucination?

monetary_penatly -\> What was the range of monetary penalties for these
cases? What is considered on the high end and what hallucinations led to
these penalties?

professional_sanction -\> What is the relationship between monetary
penalty and whether or not professional disciplinary action was taken
against the party involved?

- Will there be any other data that you need to find to help with your
  research question?

As of now, we don’t think so. However, we might need to find other data
as we explore our research questions further.

## 3. Preliminary Visualizations

**Summary Statistics**

``` r
summary(ai_data$monetary_penalty)
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
    ##     1.0     1.0     1.0   255.3   400.5  1000.0     415

**Preliminary Visualization**

``` r
library(dplyr)
word_counts <- ai_data |>
  count(ai_tool, sort = TRUE)

ggplot(ai_data, aes(x = monetary_penalty)) +
  geom_histogram(bins = 30, fill = "red", color = "white") +
  labs(title = "Distribution of Monetary Penalties",
       x = "Penalty Amount (USD)", y = "Number of Cases") +
  theme_minimal()
```

    ## Warning: Removed 415 rows containing non-finite outside the scale range
    ## (`stat_bin()`).

![](proposal_files/figure-gfm/visualizations-1.png)<!-- -->

``` r
ai_data |>
  count(state, sort = TRUE) |>
  slice_max(n, n = 10) |>
  ggplot(aes(x = reorder(state, n), y = n)) +
  geom_col(fill = "pink") +
  coord_flip() +
  labs(title = "Top 10 States by Number of AI Hallucination Cases",
       x = "State", y = "Number of Cases") +
  theme_minimal()
```

![](proposal_files/figure-gfm/visualizations-2.png)<!-- -->
