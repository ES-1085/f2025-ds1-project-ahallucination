---
editor_options: 
  markdown: 
    wrap: 72
---

# AI Hallucination Cases

by Brynn, Sasha, & Yuka

## Summary

**Background**

We found our data set on Kaggle, and it was contributed by a man named Damien Charlotin. He is a senior research fellow, specifically on artificial intelligence and the law. He is also a lecturer and independent practitioner in Paris. The AI hallucinations dataset contains cases from across the world from 2023 to 2025. The dataset keeps a record of legal cases in which AI was used and found to be wrong. Generative AI produced incorrect citations as well as other fake arguments. The dataset contains the case, the type of hallucination, the type of AI, the outcome of the case, the monetary penalty, the state, the time, etc. Our data is set up nicely, where each observation directly translates to a specific court case that is listed by name. A LLM refers to a “large language model”. This represents the type of AI used whether it was chat GPT, Gemini, etc. A hallucination is when one of these LLMs basically produces an incorrect output or provides the user with something nonsensical. Our data also includes a variable, monetary penalty, which lists how much each “error” in a specific LLM is penalized. The study in which the data was collected is relatively new. There are currently 426 cases where generative AI has been used, however, this dataset is still expanding as more come to light. Note that this study is a work in progress and doesn’t contain all court cases in which AI was used. It is noted on Kaggle that the data will be updated annually with new findings. We were mainly interested in when these cases started emerging, what forms of AI were being used the most, and how some of our variables like party and hallucination type, affect monetary penalty.


**Research Questions**

1.  Which states come up the most with AI hallucinations?
2.  What LLM is being used the most?
3.  What year did cases start to spike?
4.  What effect did the type of AI hallucinations have on the outcome?


**Methods / Approaches**

The dataset of 426 observations and 15 variables was imported from Kaggle.com into the PositCloud platform. To prepare the data for analysis, variable names were standardized, inconsistent text formatting was corrected, and entries that were empty or unspecified in critical variables (court location, date, hallucination type, outcome, and AI tool) were removed. The following libraries were used: tidyverse, broom, lubridate, scales, dplyr, tidyr, ggplot2, maps, scatterpie, stringr, and forcats. For each plot, additional data processing steps were carried out to address specific analytical objectives. For the ‘Trends in cases by LLM type’ plot, the dataset was filtered for the most frequent LLMs, naming synonyms were consolidated, and data were aggregated by month. Construction of the ‘Outcome by state’ map included standardizing court location names to capitalized U.S. state names, filtering for the four primary outcome groups, and calculating total case counts per state to generate proportional scatterpie charts. For analysis of the ‘Hallucination type and outcome’ plot, hallucination types were categorized, the top five most frequent types were identified, and their distributions across outcomes were examined. All visualizations used colorblind-accessible palettes, labels and axes were reordered and spaced for readability, minimal themes were applied, and alternative texts were included.


**Findings & Takeaways**

From our analysis, we can see several clear patterns in how AI hallucinations are appearing in U.S. court cases. The states with the highest number of reported incidents are New York, California, Georgia, South Carolina, and Virginia. Within these states, except for California, the most common outcome was a warning. In California, the majority of the cases were Penalized. Across all years in the dataset, ChatGPT remains the most commonly used LLM, dominating filings through 2023 and well into 2024, even as tools like MS CoPilot and Gemini begin appearing in later cases. Although the earliest documented AI hallucination cases for this dataset occur in May 2023, the data shows a huge spike in late 2024 when references to AI generated content increase dramatically. We also find that the type of hallucination influences the outcome: fake citations, the common form, typically resulted in warnings; fake sources or quotes were usually associated with dismissals; false legal judgements tended to be dismissed or met with warnings; and fake legal arguments often resulted in penalties or warnings. Altogether, the data suggests that while AI usage in legal contexts is rapidly rising, the severity and nature of the hallucination play a major role in determining how courts respond. 


**Limitations**

The biggest limitation in our dataset was that the United States appeared as a strong outlier in the number of cases recorded per country. Nothing in our dataset explained why this occurred. Although the dataset included global observations, the small frequencies in other countries made it difficult to draw meaningful conclusions about AI hallucination trends in foreign countries. On the other hand, the high volume of cases in the United States allowed us to focus more closely on that region and visualize what time period cases spiked. Another limitation was the substantial amount of qualitative information in our dataset. This made it extremely difficult to create visualizations due to the lack of numerical data. We often resorted to more basic frequency plots. Over time we were able to find new plots, like the bubble charts and map based pie charts, that showcased our data and findings in a meaningful way. However, getting to that point definitely took a lot of trial and error. 


**Future Ideas** 

If we had more time to expand on our project, we would focus on gathering additional data on Chat GPT’s overall popularity. Our data set was skewed by the large number of cases involving Chat GPT, and locating a corresponding dataset on its usage trends could help us explain why it appeared as such an outlier. We ultimately concentrated on cases from the United States, as most of our observations fell there. However, this is an ongoing study and it would be interesting to see what the dataset would look like in the near future. We are curious to see if as the dataset grows, the majority of cases will remain in the United States or disperse more. With more comparative datasets and observations, we would have been able to draw more substantial conclusions about the spike in Chat GPT and the disproportionate number of cases in the United States. Additionally, there were a lot of unidentified and implied entries for our AI tool variable. Although court documents were available for many of these cases, we did not have time to review each one by keywords that could identify the primary AI tool being misused. Doing this would have allowed us to clean up and strengthen this variable. Due to overall time constraints, we focused on the top identifiable tools.  


**Conclusion**

Overall, our analysis highlights how quickly AI hallucinations have become a recognizable issue in the legal system and how unevenly they are distributed across states, tools, and types of errors. Although the dataset is still developing, clear patterns already emerge. We can see that most cases originate in the United States, and ChatGPT overwhelmingly dominates as the LLM most frequently involved in hallucinations. We also found that there is an apparent relationship between the type of hallucination and the punishment. Fake citations or sources often lead to warnings or dismissals, while fabricated legal arguments are more likely to result in penalties. These trends suggest that courts are beginning to differentiate between types of AI misuse, responding more seriously when the hallucination alters substantive legal reasoning rather than simply referencing inaccurate supporting material.
The rapid increase in cases throughout 2024 signals that the legal profession is encountering generative AI issues at a growing pace, and the courts are still adapting to them. As models continue to evolve and adoption increases, the patterns we observed may shift, especially as new tools enter the market and court systems develop clearer standards for AI-generated materials. Our project highlights the risks of LLMs in legal contexts and the  importance of continued monitoring as the dataset expands. 


## Handout

Our handout can be found [here](handout/handout.pdf).

## Memo

A link to the code and how we created our graphics in our memo can be
found [here](memo/memo.html).

## Data

Charlotin, D. (2025). AI Hallucination Cases Data 2025 [Data set]. Kaggle. https://www.kaggle.com/datasets/umerhaddii/ai-hallucination-cases-data-2025

## References

Reference 1: https://www.kaggle.com/datasets/umerhaddii/ai-hallucination-cases-data-2025

Reference 2: https://www.nber.org/system/files/working_papers/w34255/w34255.pdf




