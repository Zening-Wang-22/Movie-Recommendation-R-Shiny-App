# 🎬 Movie Recommendation App

An interactive R Shiny app for finding movies based on a favorite title or a combination of genre, actor, and director preferences. The app uses TF-IDF representations of movie metadata and cosine similarity to generate recommendations.

**Built with:** R · Shiny · text2vec  
**Methods:** Text preprocessing · TF-IDF · Cosine similarity

<!-- Add a screenshot of the app showing recommendation results here. -->

## Features

- **Find similar movies:** Select a movie to receive five recommendations based on its metadata.
- **Explore your preferences:** Combine a genre, actor, and director to find matching movies.
- **Browse movie details:** Click a recommendation to view its synopsis, cast, director, and release information.
- **Discover something unexpected:** Use the Lucky Draw button to explore a randomly selected movie.

The interface also includes selectable background music and links for exploring movies by cast, director, or genre.

## Run locally

The current application is intended to run from RStudio.

1. Download or clone this repository.
2. Open `project.Rproj` in RStudio.
3. Install the packages used by the application and recommendation script:

```r
install.packages(c(
  "shiny", "shinyWidgets", "shinythemes",
  "superml", "dplyr", "stringr", "proxy",
  "stringdist", "tm", "ggplot2", "tidyverse",
  "tidyr", "purrr", "stringi", "clue", "text2vec"
))
```

4. Open `shiny.R` and click **Run App**.

Keep the working directory at the repository root so the app can find `movies.csv` and `movie_recommend_model.R`.

The model is built when the app starts, so the initial launch may take some time. Some visual assets are loaded from external websites and require an internet connection.

## How it works

### Movie representation

The recommendation pipeline selects a subset of 1,000 movies using the ranking function in `movie_recommend_model.R`. It combines five metadata fields:

- Genres
- Keywords
- Tagline
- Cast
- Director

The text is cleaned and converted into TF-IDF vectors, which weight terms according to their frequency within a movie's metadata and across the collection.

### Recommendations by movie

The app compares the selected movie with other titles using cosine similarity between their TF-IDF vectors. It ranks the results and returns five recommendations, excluding the selected title.

### Recommendations by preferences

The app combines the selected genre, actor, and director into a text query. In the current implementation, it fits TF-IDF to the movie collection together with this query, then ranks movies by their similarity to the query.

These preferences contribute to the similarity score rather than acting as strict filters.

## Repository structure

```text
.
├── shiny.R                  # Shiny interface and server logic
├── movie_recommend_model.R  # Data preparation and recommendation functions
├── movies.csv               # Movie metadata
├── project.Rproj            # RStudio project
├── www/                     # Local audio assets
└── README.md
```

## Limitations and next steps

The recommendations reflect similarities in movie metadata rather than predictions of individual user ratings. Results are limited to the selected movie collection and depend on the completeness of its metadata.

Potential improvements include:

- Evaluating recommendation quality against a simple baseline.
- Reusing fitted text representations to reduce repeated computation.
- Removing fixed dataset-size assumptions from the recommendation functions.
- Improving handling of missing metadata and ambiguous movie titles.
- Reducing reliance on externally hosted images.

Some movie homepage links may also be outdated.
