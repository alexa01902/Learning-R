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




