# Model Deployment Netflix Project

This project aims to build and deploy a content-based recommendation system for movies and TV shows available on Netflix. This project was created by Group 4:

2702223084 - Jonathan Christopher Gani

2702274495 - Kevin Gabriel Wiharja

2702258333 - Hartono Yaputra

## 1. Data Cleaning & Exploratory Data Analysis (EDA) 🧹

The initial phase of this project involves data cleaning and analysis to ensure the data used is of high quality and ready for modeling. This process includes several steps:

Loading Data: The dataset used is netflix_titles.csv, which contains information about movies and TV shows on Netflix.

Data Format Conversion: The date_added column's format was converted to a datetime data type for easier processing.

Removing Identifier Column: The show_id column was removed as it is irrelevant for the modeling process and can complicate the detection of duplicate data.

Checking and Fixing Column Values: Several inconsistent values in the rating column (e.g., '74 min', '84 min') were identified and removed from the dataset.

Removing Unused Columns: Columns such as director, country, date_added, and duration were removed because this recommendation system focuses on content attributes like cast, listed_in, and description.

Handling Missing Values: Rows with empty values in the cast and rating columns were deleted because they are difficult to impute and could introduce bias into the model.

Data Distribution Analysis: A distribution analysis was performed on numerical (release_year) and categorical (type, rating) columns to understand the data's characteristics.

After the cleaning process, the dataset is ready for the next stage: building the recommendation system.

## 2. Content-Based Recommendation System ⚙️

This recommendation system is built using a Content-Based Filtering approach, which recommends items based on the similarity of their attributes. The steps are as follows:

Feature Selection: Relevant features such as listed_in (genre), title, rating, and description are combined into a single text string for each item.

Text Vectorization with TF-IDF: A TfidfVectorizer is used to convert the combined text data into a numerical representation (vector). This process also ignores common English words (stop words) that do not provide significant information.

Cosine Similarity Calculation: cosine_similarity is calculated from the TF-IDF matrix to measure how similar one movie/TV show is to another. This similarity score ranges from 0 (not similar) to 1 (very similar).

## 3. Recommendation System Implementation 🧑‍💻

A function named recommend_system was created to generate recommendations. This function takes the title of a movie/TV show as input and returns a list of the 5 most similar items based on their cosine similarity scores.

Usage example:

```bash
# Get recommendations for "InuYasha the Movie 4: Fire on the Mystic Island"
recommend_system('InuYasha the Movie 4: Fire on the Mystic Island')

# Get recommendations for "Naruto Shippuden the Movie: Blood Prison"
recommend_system('Naruto Shippuden the Movie: Blood Prison')
```

The output of this function is a DataFrame containing the title, type, cast, rating, genre, and similarity score of the recommended items.

## 4. Model Deployment Preparation ⚒️

To prepare the model for use in other applications (e.g., a web app), all essential components of the recommendation system are saved into a file named netflix_recommender.pkl using the pickle library. The saved components include:

tfidf: The fitted TfidfVectorizer object.

cosine_sim: The cosine similarity matrix.

dataset: The cleaned DataFrame.

indices: The mapping between titles and their corresponding indices in the dataset.

By saving these components, the model can be reloaded and used to provide recommendations without needing to be retrained from scratch.

## 5. Deploying the Model

After we finish the deployment preparation step we can finally move to the deployment step. In this project we're going to deploy the program to Streamlit. first, we have to upload all the codes and requirements for easier deployment to github then we can open streamlit and upload our repository so Streamlit can download the requirements we set earlier. The python version we're using in this deployment step is Python 3.9, after all the requirements are met we can use the program. You can access the link here: movie-recommendation-md.streamlit.app
