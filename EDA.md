# EDA

## Visualing data using ggplot2
ggplot2 is one of the core packages int the tidyverse package. So to use ggplot2, the tidyverse packages has to be loaded by running

```r
library(tidyverse)
```
### Ways to view the data
- Type the name of the data frame in the console
- glimpse()

### Creating a ggplot

Begin a plot with the function ggplot(), defining a plot object that you then add alyers to. The first argument of ggplot() is the dataset
to use in the graph. Next, we need to provide information about how to visualize the data. The mapping function of the ggplot() function defines
how variables in your dataset are mapped to visual propertiese (aesthetics) of your plot. The maping argument is always defined int the aes() function, 
and the x and y argumetns of aes() specify which variables to map to the x and y axes. Now, we need to define a geom: the geometrical object that a plot uses to present data.
Example of functions are : geom_bar(), geom_line(), geom_boxplot(), geom_point().

```r
ggplot(
  data = penguins,
  mapping = aes(x = flipper_length_mm, y = body_mass_g)
) +
  geom_point()
```
The function labs() adds labels.

### Visualizing numerical data
Histogram is a good way to visualize data, using the geom_histogram() function.

```r
ggplot(penguins, aes(x = body_mass_g)) +
  geom_histogram(binwidth = 200)
```
For a smoothed-out version of a histogram, the geom_desnity() can be used.

### Visualizing relationships
#### A numerical and a cateegorical variable
To visualize the relationship between a numerical and a categorical variable we can use side-by-side box plots.
```r
ggplot(penguins, aes(x = species, y = body_mass_g)) +
  geom_boxplot()
```
Alternatively, we can make density plots with geom_density().
```r
ggplot(penguins, aes(x = body_mass_g, color = species)) +
  geom_density(linewidth = 0.75)
```

#### Two catergorical
Either boxplot with frequencies
```r
ggplot(penguins, aes(x = island, fill = species)) +
  geom_bar()
```
or relative frequency

```r
ggplot(penguins, aes(x = island, fill = species)) +
  geom_bar(position = "fill")
```
#### Two numerical variables
Scatterplots is the most commonly used plots for visualizing the relationship between two numerical variables. 
```r
ggplot(penguins, aes(x = flipper_length_mm, y = body_mass_g)) +
  geom_point()
```
#### Three or more variables
As mentioned ealier, we can incorporate more variables into a plot by mapping them  to additional aesthetics.
```r
ggplot(penguins, aes(x = flipper_length_mm, y = body_mass_g)) +
  geom_point(aes(color = species, shape = island))
```
However adding too many aesthetic mappings to a plot makes it cluttered and difficult to make sense of. Another way, which is particularly useful for categorical variables, is to split your plot into facets, subplots that each display one subset of the data.

To facet your plot by a single variable, use facet_wrap(). The first argument of facet_wrap() is a formula3, which you create with ~ followed by a variable name. The variable that you pass to facet_wrap() should be categorical.
```r
ggplot(penguins, aes(x = flipper_length_mm, y = body_mass_g)) +
  geom_point(aes(color = species, shape = species)) +
  facet_wrap(~island)
```

### Saving your plot
Use ggsave()
```r
ggplot(penguins, aes(x = flipper_length_mm, y = body_mass_g)) +
  geom_point()
ggsave(filename = "penguin-plot.png")
```

## Data transformation

### dplyr
We are going to explore the dplyr package for data transformation. 
dplyr’s verbs are organized into four groups based on what they operate on: rows, columns, groups, or tables. In the following sections you’ll learn the most important verbs for rows, columns, and groups.

#### Rows

##### filter()
filter() allows you to keep rows based on the values of the columns1. The first argument is the data frame. The second and subsequent arguments are the conditions that must be true to keep the row. For example, we could find all flights that departed more than 120 minutes (two hours) late:

```r
flights |> 
  filter(dep_delay > 120)

flights |> 
  filter(month %in% c(1, 2))
```
When you run filter() dplyr executes the filtering operation, creating a new data frame, and then prints it. It doesn’t modify the existing flights dataset because dplyr functions never modify their inputs. To save the result, you need to use the assignment operator, <-.

##### arrange()
arrange() changes the order of the rows based on the value of the columns. It takes a data frame and a set of column names (or more complicated expressions) to order by. If you provide more than one column name, each additional column will be used to break ties in the values of preceding columns. For example, the following code sorts by the departure time, which is spread over four columns. We get the earliest years first, then within a year the earliest months, etc.

```r
flights |> 
  arrange(year, month, day, dep_time)
```
You can use desc() on a column inside of arrange() to re-order the data frame based on that column in descending (big-to-small) order. For example, this code orders flights from most to least delayed:

```r
flights |> 
  arrange(desc(dep_delay))
```
##### distinct()
distinct() finds all the unique rows in a dataset. Most of the time, however, you’ll want the distinct combination of some variables, so you can also optionally supply column names:

```r
# Remove duplicate rows, if any
flights |> 
  distinct()

# Find all unique origin and destination pairs
flights |> 
  distinct(origin, dest)
```
##### Columns

###### mutate()
The job of mutate() is to add new columns that are calculated from the existing columns.
```r
flights |> 
  mutate(
    gain = dep_delay - arr_delay,
    speed = distance / air_time * 60
  )
```

##### select()

It’s not uncommon to get datasets with hundreds or even thousands of variables. In this situation, the first challenge is often just focusing on the variables you’re interested in. select() allows you to rapidly zoom in on a useful subset using operations based on the names of the variables:

```r
# Select columns by name
flights |> 
  select(year, month, day)

# Select all columns between year and day (inclusive):
flights |> 
  select(year:day)

# Select all columns except those from year to day (inclusive):
flights |> 
  select(!year:day)

# Select all columns that are characters:
flights |> 
  select(where(is.character))
```

#### Groups
##### group_by()
Use group_by() to divide your dataset into groups meaningful for your analysis:
```r
flights |> 
  group_by(month)
```
group_by() doesn’t change the data but, if you look closely at the output, you’ll notice that the output indicates that it is “grouped by” month (Groups: month [12]). This means subsequent operations will now work “by month”. group_by() adds this grouped feature (referred to as class) to the data frame, which changes the behavior of the subsequent verbs applied to the data.

##### summarize()
The most important grouped operation is a summary, which, if being used to calculate a single summary statistic, reduces the data frame to have a single row for each group. You can create any number of summaries in a single call to summarize(). 
```r
flights |> 
  group_by(month) |> 
  summarize(
    avg_delay = mean(dep_delay, na.rm = TRUE), 
    n = n()
  )
```




