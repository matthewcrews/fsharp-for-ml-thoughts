# F# for ML

## Questions

1. What you're using F# for today
2. What you're looking to use F# for in the future
3. Some specifics about "analytical" work you do
4. Some things in the language and/or ecosystem you need to address the second bullet point

## Thoughts

### Data Scientist Mindset

Data Scientists focus on two key workflows: Time to Insight and Time to Model. The tools they choose typically center on optimizing one or both of these activites. They can be broken down into these loops

#### Time to Insight

1. Import data (CSV/Excel/DB)
2. Shape the data (Joins, Grouping, Aggregates)
3. Compute features
4. Explore the data
    - Plot the data
    - Perform ANOVA
    - Correlation Analysis
5. If we have insight, stop. If not, return to 1

#### Time to Model

> **Note**: A pipeline is a series of Featurizers with a Model Trainer at the end

1. Import the data (CSV/Excel/DB)
2. Shape the data (Joins, Grouping, Aggregates)
3. Compute input features
4. Create Pipeline(s)
5. Train Pipeline(s)
6. Evaluate Pipeline(s)
7. If Pipeline is "good", stop. If not, return to 1
8. Export Pipeline: The trained Pipeline needs to be exported in a format that it can be reloaded and used for scoring in the future.

### Missing Pieces
1. DataFrame like type/library to work with
    - Fast for working in-memory but can also scale to larger data sets
    - R's `data.table` library is best in class for in-memory operations
    - Spark is the industry standard distributed DataFrame. Dask is a Python competitor. Scaling up the size of the data set you are working with should be as "seamless" as possible.

2. Vectorized operations/functions for Columns
    - As a Data Scientist, I am used to working with columns of data and performing operations along the entire column. `R` is a vector-based language so these types of operations are baked into the language. I am not sure if we can bake this into a `Column` type, but being able to project/broadcast a function along an entire row is quite powerful.
    - `R` is a vector-based language so these types of operations are baked into the language.
    - The `Numpy` package supports these kids of operations with the `NDArray` type
    - Julia does this by adding a dot (.). The documentation referers to this as [dot syntax](https://docs.julialang.org/en/v1/manual/functions/#man-vectorized-1)
    - I am not sure if we can bake this into a `Column` type, but being able to project/broadcast a function along an entire row is quite powerful.
    - Examples:
        - Column Addition: 
            - `df.ColC = df.ColA + df.ColB`
        - Vectorized Function Call: 
            - `df.ColC = Stats.statsFunc df.ColA df.ColB`
            - `StatsFunc: float -> float -> float`
        - Could we do something like: 
            - `df.ColC = Series.map funcA df.ColA df.ColB`
            - But without having to create `map2`, `map3`, `map2withParam`
            - Feels very un-F# :/

3. Full suite of Joins/Aligning data
    - Inner/Outer/Cross/Left Outer/Right Outer
    - AsOf/Rolling Joins: 
        - Last Observation Carried Forward (LOCF), LOCF within range
        - Nearest Preceding
        - Nearest Succeeding
        - Nearest
        - Within Window (Preceding Window, Succeeding Window, Preceding or Succeeding)
        - Nearest Observation, Nearest Observation within range
        - [Rolling Joins Example](https://r-norberg.blogspot.com/2016/06/understanding-datatable-rolling-joins.html)
        - [Overlaps Joins Example](https://www.rdocumentation.org/packages/data.table/versions/1.12.8/topics/foverlaps)
        - Best In Class: `data.table` or kdb+

4. Easy grouping of rows and performing analysis on the groups
5. Windowing of functions
6. Build ecosystem is fractured and overwhelming
    - Do I use Fake, msbuild, VSCode's tasks with Fake, VSCode's tasks with msbuild?
7. Package management is fractured and overwhelming
    - Paket vs. Nuget
