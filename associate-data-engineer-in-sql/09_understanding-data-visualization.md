## [Understanding Data Visualization](https://app.datacamp.com/learn/courses/understanding-data-visualization)


### Three ways of getting insights
```
- Calculating summary statistics    -> mean, median, standard deviation
- Running models                    -> linear and logistic regression 
- Drawing plots                     -> scatter, bar, histogram
```

### Continuous and categorical variables
```    
Continuous      -> usually number   -> heights, temperature, revenues
Categorical     -> usually text     -> eye colors, countries, Industry

Can be either   -> age is continuous, but age group is categorical
                 time is continuous, month of year is categorical
```

### Histogram
``` 
- If you have a single continuous variables
- You want to ask questions about the shape of its distribution 

bins = intervals

Modality: how many peaks?
Unimodal 
Bimodal
Trimodal

Skewness: Is is symmetric?
lef-skewed -> full on the right, outlier on the left
symmetric
right-skewed -> full on the left, outlier on the right

Kurtosis: how many extreme values?
leptokurtic     -> narrow peak, lots of extreme values
mesokurtic      -> normal distribution
platykurtic     -> broad peak, few extreme value
```

### Box plot
```
- When you have a continuous variable, split by a categorical variable
- When you want to compare the distributions of the continuous variable for each category

line in the middle      -> median
line below the middle   -> lower quartile (one quarter of the values are below it)
line upper the middle   -> upper quartile (three quarters of the values are below it)
low whisker             -> minimum value 
upper whisker           -> maximum value
```

### Visualizing two variables
```
Scatter plot:
- You have two continuous variable
- You want to answer question about the relationship between the two variables
- Correlation: How close are you being able to fit a straight line through points?

line to the upper right     -> positive correlation 
line to the upper left      -> negative correlation 
spreading to the left       -> weak negative
spreading to the right      -> weak positive
gathered in the center      -> no correlation

Adding smooth trend lines or curve
```

### Line plot
```
- You have two continuous variables
- You want to answer questions about their relationship
- Consecutive observation are connected somehow
```

### Bar plot
```
- You have a categorical variable
- You want to counts or percentage for each category 
Ocassionaly:
- You want another numeric score for each category, and need to include zero in the plot
```

### Dots plot
```
- You have a categorical variable
- You want to display numeric scores for each category on a log scale
- You want to display multiple numeric scores for each category
```

### The color and the shape
```
Higher dimensions:
- 3D scatter plot

x and y are not the only dimensions:
Points also have these dimensions
- color
- size
- transparency
- shape

Others dimensions for line plots:
- color
- thickness
- transparency
- line type (solid, dashes, dots)
```

### Using color
```
Colorspaces:
- Red-Green-Blue
- Cyan-Magenta-Yellow-Black
- Hue- Chroma-Luminance

Hue (color of the rainbow)
Chroma (intensity of the color)
Luminance (brightness of the color)
```

### Choosing a plotting palette
```
- Usually each color should stand out as much as other colors
- The perceptual distance from one color in the next should be constant

Three types of color scale: qualitative
qualitative     -> distinguish unordered categories -> hue
sequential      -> show ordering                    -> chroma or luminance
diverging       -> show above or below a midpoint   -> chroma or luminance, with 2 hues , example: agree, neutral, disagree

viridis => for color blind people
```

### Plotting many variables at once
```
Pair plot:
- You have up to ten variables (either continuous, categorical, mix)
- You want to see the distribution for each variable
- You want to see the relationship between each pair of variables

When sould you use a correlation heatmap
- You have lots of continuos variables
- You want to simple overview of how each pair of variables is related

When you should use a parallel coordinates plot:
- You have lots of continuous variables
- You want to find patterns accross these variables
- You want to see how each observation is related to each other
```

### Polar coordinates
```
Bar plot + polar coordinates = pie plot

When you should use a polar coordinates:
- Almost never
- If you have a variable that is naturally circular (time, compass direction)

Histogram + polar coordinates = rose plot

When you should use a rose plot:
- You have a continuous variable that is naturally circular
- You want to see the distribution of this variable
```

### Axes of evil
```
- Nonsense bar length
- y axis does not start at zero
- y axis does not have a consistent scale
- dual axes are misleading
- Better to use multiple panels
```

### Sensory overload
```
Measures of a good visualization:
- How many interesting insights can your reader get from the plot?
- How quickly can they get thise insights?
```

### Chartjunk
```
Any element of the plot that distracts from reader getting insight:
- pictures
- skueomorphism: reflection ,shadows, etc
- extra dimensions
- ostentatious (norak) colors or lines
```