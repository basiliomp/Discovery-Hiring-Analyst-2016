# Data Analysis Task

Task description and data for candidates applying to be a Data Analyst in the [Discovery department](https://www.mediawiki.org/wiki/Wikimedia_Discovery) at [Wikimedia Foundation](https://wikimediafoundation.org/wiki/Home).

## Background

Discovery (and other teams within the Foundation) rely on *event logging* (EL) to track a variety of performance and usage metrics to help us make decisions. Specifically, Discovery is interested in:

- *clickthrough rate*: the proportion of search sessions where the user clicked on one of the results displayed
- *zero results rate*: the proportion of searches that yielded 0 results

and other metrics outside the scope of this task. EL uses JavaScript to asynchronously send messages (events) to our servers when the user has performed specific actions. In this task, you will analyze a subset of our event logs.

## Task

You must create a **reproducible report**\* answering the following questions:  
```{r}
# Initial set-up #
#setwd("Data Analyst task")
  # Loading packages
  require(tidyr)
  # Loading data
  events <- read.csv("events_log.csv.gz", header = T, stringsAsFactors = F)
  # Checking data loaded
  str(events) # Show dimensions of the data frame and its variables.
  summary(events) # Get a hint of the data stored by each variable.
  head(events) # Print out a handful of observations to inspect them.
```

**1. What is our daily overall clickthrough rate?**

Transforming timestamp variable into a date-time formatted variable, stored as 'time'.
```{r}
events$time <- strptime(x = events$timestamp, format = "%Y%m%d%H%M%S")
```
Four cases were not properly transformed because the timestamp did not meet the formatting requirements. Here those 4 rogue cases are shown:
```{r}
events[which(is.na(events$time)),]

time_missing <- round(sum(is.na(events$time))/nrow(events) * 100, 6)
```

However, given that it is a minor issue affecting `r time_missing` % of cases, we will continue the analysis.
```{r}
CTR <- events %>%
            group_by()
            
```

  **How does it vary between the groups?**
```{r, cache=TRUE}

```

**2. Which results do people tend to try first?** 
```{r, cache=TRUE}

```

  **How does it change day-to-day?**
```{r, cache=TRUE}

```

**3. What is our daily overall zero results rate?** 
```{r, cache=TRUE}

```

  **How does it vary between the groups?**
```{r, cache=TRUE}

```

**4. Let *session length* be approximately the time between the first event and the last event in a session. Choose a variable from the dataset and describe its relationship to session length. Visualize the relationship.**
```{r, cache=TRUE}

```

**5. Summarize your findings in an *executive summary*.**
```{r, cache=TRUE}

```

\* Given dependencies and other instructions, we should be able to re-run your source code with the dataset in the same directory and obtain the same results and figures. Popular formats for this include RMarkdown and Jupyter Notebook (formerly IPython).

**Note**: if you submit your report as a Jupyter/IPython notebook on Greenhouse, please upload a copy to GitHub and include the link when you submit it on Greenhouse.

## Data

The dataset comes from a [tracking schema](https://meta.wikimedia.org/wiki/Schema:TestSearchSatisfaction2) that we use for assessing user satisfaction. Desktop users are randomly sampled to be anonymously tracked by this schema which uses a "I'm alive" pinging system that we can use to estimate how long our users stay on the pages they visit. The dataset contains just a little more than a week of EL data.

| Column          | Value   | Description                                                                       |
|:----------------|:--------|:----------------------------------------------------------------------------------|
| uuid            | string  | Universally unique identifier (UUID) for backend event handling.                  |
| timestamp       | integer | The date and time (UTC) of the event, formatted as YYYYMMDDhhmmss.                |
| session_id      | string  | A unique ID identifying individual sessions.                                      |
| group           | string  | A label ("a" or "b").                                     |
| action          | string  | Identifies in which the event was created. See below.                             |
| checkin         | integer | How many seconds the page has been open for.                                      |
| page_id         | string  | A unique identifier for correlating page visits and check-ins.                    |
| n_results       | integer | Number of hits returned to the user. Only shown for searchResultPage events.      |
| result_position | integer | The position of the visited page's link on the search engine results page (SERP). |

The following are possible values for an event's action field:

- **searchResultPage**: when a new search is performed and the user is shown a SERP.
- **visitPage**: when the user clicks a link in the results.
- **checkin**: when the user has remained on the page for a pre-specified amount of time.

### Example Session

|uuid                             |      timestamp|session_id       |group |action           | checkin|page_id          | n_results| result_position|
|:--------------------------------|:--------------|:----------------|:-----|:----------------|-------:|:----------------|---------:|---------------:|
|4f699f344515554a9371fe4ecb5b9ebc | 20160305195246|001e61b5477f5efc |b     |searchResultPage |      NA|1b341d0ab80eb77e |         7|              NA|
|759d1dc9966353c2a36846a61125f286 | 20160305195302|001e61b5477f5efc |b     |visitPage        |      NA|5a6a1f75124cbf03 |        NA|               1|
|77efd5a00a5053c4a713fbe5a48dbac4 | 20160305195312|001e61b5477f5efc |b     |checkin          |      10|5a6a1f75124cbf03 |        NA|               1|
|42420284ad895ec4bcb1f000b949dd5e | 20160305195322|001e61b5477f5efc |b     |checkin          |      20|5a6a1f75124cbf03 |        NA|               1|
|8ffd82c27a355a56882b5860993bd308 | 20160305195332|001e61b5477f5efc |b     |checkin          |      30|5a6a1f75124cbf03 |        NA|               1|
|2988d11968b25b29add3a851bec2fe02 | 20160305195342|001e61b5477f5efc |b     |checkin          |      40|5a6a1f75124cbf03 |        NA|               1|

This user's search query returned 7 results, they clicked on the first result, and stayed on the page between 40 and 50 seconds. (The next check-in would have happened at 50s.)
