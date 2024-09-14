# Data_Structures_And_Algorithms
My repository on Data Structures and Algorithms

```{r}
# Source: http://www.htmlwidgets.org/showcase_plotly.html
library(plotly)
p <- ggplot(data = diamonds, aes(x = cut, fill = clarity)) +
            geom_bar(position = "dodge")
ggplotly(p)
```
