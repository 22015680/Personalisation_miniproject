# Content-Based Book Recommendation System

## Introduction
The popularity of the Internet has brought a huge amount of information to users, but the vast increase in the volume has made it difficult for users to access information that is truly useful to them. Recommendation systems are a very promising solution to this problem.These systems significantly enhance user experience by providing personalized suggestions. 

This project aims to develop a content-based recommendation system for books using a dataset of 7,000 books. The dataset provides close to seven thousand books containing identifiers, title, subtitle, authors, categories, thumbnail url, description, published year, average rating, and number of ratings. The dataset is provided as comma-delimited CSV. The recommendation system leverages this data to recommend the top 10 most similar books to a user based on their preference.

## Methodology
### Selection of the dataset
Content-based recommendation algorithms recommend items based on the features of the data, so there are a number of factors to consider. First is the size of the dataset. A larger dataset will usually provide more accurate and diverse recommendations. Second is the quality of the data, which should be accurate and relevant. Third is feature richness, which for books might include title, author, genre, overview, or even user reviews. Fourth is data diversity, which should include as many books as possible across a variety of genres and features. Finally, the data should not have too many nulls or missing values.

During the selection process, I found that many datasets on the Internet were not suitable. They either lacked book descriptions, were mixed with multiple languages or had many null values. After long selecting, I finally settled on the 7k books dataset. This dataset provides ISBN, title, subtitle, authors, genres, thumbnail, overview, year of publication, average rating, and rating counts. These are all useful features when constructing a content-based recommendation system. Besides, the dataset has a low number of null values, with the exception of subtitle which has 65% vacancies. All other data remains at 95% availability.

![7k books dataset preview](/images/image_01.png)
<p align="center">7k books dataset preview</p>

### Feature extraction
First of all, the data needs to be preprocessed and the features of the data need to be extracted. The feature extraction technique used in this project is the Term Frequency-Inverse Document Frequency (TF-IDF). TF-IDF is a fundamental technique used in machine learning feature extraction and can reflect how important a word is to a corpus of documents. In the context of a content-based recommendation system, TF-IDF can be used to generate a matrix of document features to calculate the similarity between different items, and then provide recommendations.

In this project, all the English stop words such as 'the' and 'a' were removed using a TF-IDF Vectorizer Object. This is because stop words for information retrieval and natural language processing are usually common words that do not contribute to the meaning of the sentence. Since the dataset also contains null values, it is necessary to replace NaN with an empty string. After that, duplicate data needs to be cleaned up. Duplicate entries based on 'title' were removed to ensure uniqueness of books. The dataset was then indexed by the book titles.

Next, features being extracted were then combined into a single column labeled 'content'. The TF-IDF Vectorizer Object was then used to transform the 'content' column into a TF-IDF matrix. Different features can be extracted here. 

![Code for feature extraction in the project](/images/image_02.png)
<p align="center">Code for feature extraction in the project</p>

### Similarity computation
Second, the similarity between books needed to be computed. The cosine similarity was adopted to compute the similarity between books. The cosine similarity is a measure of similarity between two non-zero vectors of an inner product space. Compared to using K-nearest neighbors (KNN), where the function is only approximated locally and all computation is deferred until function evaluation, using cosine similarity directly can be simpler and just as effective in this project. It is because that this project used a relevantly small dataset, where the similarity between all pairs of books can be precomputed, while KNN will just get the K most similar documents. In this project, a cosine similarity matrix was computed from the TF-IDF matrix.

![Code for similarity computation in the project](/images/image_03.png)
<p align="center">Code for similarity computation in the project</p>

### Recommendation system
The final step was to develop a function that returns the top 10 most similar books to a given book. The function takes a book title as input and uses the cosine similarity matrix to find the most similar books. It gets the index of the book matching the title from a reverse map of indices and book titles. Then, it retrieves the pairwise similarity scores of all books with the input book.

The function then sorts the books based on their similarity scores. It retrieves the scores of the 10 most similar books and gets their indices. Finally, it returns the top 10 most similar books along with their average ratings.

The average ratings of each book had been considered. Instead of extracting it as a feature, I ended up ranking the 10 most similar books returned by the function in terms of average rating.

![Code for the function that returns the top 10 most similar books and the results sorted by average rating](/images/image_04.png)
<p align="center">Code for the function that returns the top 10 most similar books and the results sorted by average rating</p>

## Results
The recommendation system was tested with the book The Great Gatsby. At first, 'title', 'authors', 'categories', and 'description' were extracted as features. Book title provides a concise representation of the book's subject and can often capture key information about the content. Users often have preferences for specific authors, and recommending books by the same authors can be relevant and personalized. Categories allow for a high-level classification of books, and enables recommendations based on user preferences for specific genres. The description provides a summary of the content. Therefore, I extracted them as features.

The results sorted by average rating are displayed in the chart below.

![Results for 10 most similar books to given book](/images/image_05.png)
<p align="center">Results for 10 most similar books to given book</p>

This revealed that the system had succeeded in recommending the 10 most similar books. I then looked closely at the main content of each book and found that indeed each was similar in some way to the given book, indicating that the recommendation system was successful.

## Discussion
The content-based recommendation system's effectiveness can the similarity of the composite content features used in the model. A life in letters, for example, is Fitzgerald's biography and autobiography, and is similar to The Great Gatsby in terms of its author and subject matter. Like Water for Chocolate, as another example, is more similar to The Great Gatsby in terms of theme, genre and story outline. This suggests that it is a valid system.

Because this system is a content-based recommendation system, it has both the advantages and disadvantages of a content-based recommendation system. The advantage of a content-based recommendation system is that it can obtain recommendations without relying on user information or interaction behaviour, so the system is beneficial for readers looking for new books with similar content to what they already like. On the other hand, the disadvantage of the system is that user-specific preferences are not taken into account, only the content of the book is. In this way, the current system design may not cater for users who are looking for variety, or those whose preferences are unable to be fully captured through content similarity alone.

## Reflections and Future Work
To overcome these limitations, future versions of the system could adopt a hybrid recommendation strategy, combining the advantages of content-based and collaborative filtering methods. In this manner, in addition to the content of the book, the user's behavior will be taken into account too. Furthermore, other features such as book popularity (reflected in "ratings count") can also be integrated into the recommendation algorithm to improve its performance.

In summary, the content-based book recommendation system developed in this project demonstrates great potential in helping readers discover new books that match their preferences. It uses text analysis and similarity scoring to generate relevant recommendations. While there is room for improvement and extension, the system provides a solid foundation for the development of more advanced personalised book recommendation systems.
