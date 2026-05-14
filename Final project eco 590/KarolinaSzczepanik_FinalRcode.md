---
output:
  html_document: default
  pdf_document: default
---
FINAL R NOTEBOOK WITH CODE
Karolina Szczepanik


```r
library(dplyr)
library(tidyverse)
library(estimatr)
library(Rcpp)
library(stargazer)
library(ggplot2)
library(gridExtra)
```r

# import movie dataset into R
movies <- read.csv("movies_final.csv")

# graph 1:compares awards and IMDb ratings
g1 <- ggplot(movies, aes(Awards_Count, imdbRating)) +
  geom_point() +
  labs(title = "Awards vs IMDb Rating",
       x = "Awards",
       y = "IMDb Rating")

# graph 2: compares runtime and IMDb ratings
g2 <- ggplot(movies, aes(Runtime, imdbRating)) +
  geom_point() +
  labs(title = "Runtime vs IMDb Rating",
       x = "Runtime",
       y = "IMDb Rating")
       
#puts both graphs together
grid.arrange(g1, g2, ncol = 1)


# graph 3: shows where most IMDb ratings fall in the dataset
ggplot(movies, aes(imdbRating)) +
  geom_density(fill = "dark grey", color = "black", alpha = 0.7) +
  labs(
    title = "Distribution of IMDb Ratings",
    x = "IMDb Rating",
    y = "Frequency"
  )



# regression 1: full dataset with more than 3 variables
model1 <- lm(imdbRating ~ Runtime + Awards_Count + Genre_Count, data = movies)
summary(model1)


# regression 2: filtered datase`t, only movies longer than 120 minutes
long_movies <- movies[movies$Runtime > 120, ]

model2 <- lm(imdbRating ~ Awards_Count + Genre_Count, data = long_movies)
summary(model2)


# regression 3: preferred model
model3 <- lm(imdbRating ~ Runtime + Awards_Count + Genre_Count, data = movies)
summary(model3)


Regression Interpretation:

Regression 1 --> looks at the relationship between IMDb ratings, runtime, awards count, and genre count across the full movie dataset. 
Runtime had a small positive relationship with IMDb ratings, meaning longer movies were slightly associated with higher ratings.

Regression 2 --> only focuses on movies longer than 120 minutes. This was done to see if the relationships changed for longer films. 
In this model, awards count and genre count did not show a strong relationship with IMDb ratings.

Regression 3 --> This is my preferred model because it uses the full dataset and includes multiple movie characteristics together. 
Overall, the results suggest that runtime may have a stronger relationship with IMDb ratings than awards count or genre count in this dataset.

These regressions show correlations between variables, not causation.






