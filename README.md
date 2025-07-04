# Book recommendation system.

***Final project for the Building AI course.***

## Summary

This system will help Goodreads users have a better recommendation system based on collaborative or content-based filtering, or a combination of both.</br>
This project analyses user interests and suggests books that might interest them. The goal is to help many people around the world who love reading discover new books in an easy and personalized way.</br>


## Concept
My idea solves problems when recommending other books or reading material to Goodreads users.</br>
This problem is common on the app today, in 2025, because sometimes users can't find reading material similar to their current or previous reading material.</br>
My personal motivation is to try to help users get better service in recommending future reading material.</br>
This topic is important and interesting because it helps many people know what their next book to read will be.</br>

Various Problems:</br>
-Lack of an efficient recommendation service.</br>
-Users find it difficult to find recommended reading similar to what they are reading or have already read.</br>
-Does not improve aesthetics.</br>
-Recommendations can sometimes be ignored by the user if they don't like what is shown, by selecting the "Not interested" option.</br>

### What recommendation features does Goodreads currently have?</br>
On Goodreads, we can currently ask the platform to suggest books based on our favourite genres from the books already on our shelves.
You can also use the book lists created by other users. You can find lists on any topic, which are also a good starting point for discovering new books in your favourite genres. </br>
In the browser menu, you have two options and a few others, such as the Explorer, which allows you to discover books and authors and then add them to your library. </br>
The biggest drawback is that it's a platform aimed at English-speaking audiences, especially as a native speaker, rather than Spanish speakers, for example. </br>
To search for recommended books in your native language, you must follow a series of steps indicated on the page. </br>
To check these recommendations, I used [my Goodreads account.](https://www.goodreads.com/emece_reading)</br>

These steps can be found on the platform's help page.
[Help Goodreads.](https://help.goodreads.com/s/article/How-can-I-get-recommendations-for-books-in-my-language)</br>

## How do I use it?
The solution for users to get recommendations based on their current or previous readings is to have a service, using the Goodreads algorithm or its data, where this data can be used as a source for future recommendations and where a new book can be recommended to a reader who needs the service at some point. The idea of ​​this solution is to save readers time searching for a new book to read in any environment, be it their PC, tablet, smartphone app, etc. The users to consider will always be readers with a Goodreads profile, where they have their read, in-read, and to-do lists, as well as other personalized shelves. Based on the data in the **.csv** files I show below, we can recommend books similar to those the reader has already read or is currently reading.

<img src="https://upload.wikimedia.org/wikipedia/commons/1/1a/Goodreads_logo.svg" width="200" height="100">
</br>
<img src="https://dical.es/modules/ph_simpleblog/featured/78.jpg" height="400" width="600">

## Steps.
**General steps.**</br>
1.Load the data.</br>
    1.1.Goodreads IDs.</br>
    1.2.Data to load.</br>
2.Perform a preliminary analysis.</br>
3.Create a book recommendation system based on collaborative and/or content-based filtering.</br>
    3.1.Pros and cons of a content-based recommendation system.</br>
    3.2.Pros and cons of a user-based recommendation system.</br>

***Let's go step by step.***

**1. Load the data.**</br>

First you need to make sure you have the right dataset downloaded from Kaggle: [Kaggle Dataset.](https://www.kaggle.com/datasets/zygmunt/goodbooks-10k)</br>

Or take a third-party example like this one, where we find six million reviews of the ten thousand most popular books with the highest number of reviews, which also includes:</br>
**Books marked for reading by users**.</br>
**Book metadata: author, year, etc**.</br>
**Tags, bookshelves, genres, etc**.</br>
[Zygmuntz's README.MD on GitHub.](https://github.com/zygmuntz/goodbooks-10k)</br>

We must also consider the necessary libraries installed, such as panda, numpy, scikit-learn, matplotlib, seaborn, etc.</br>
If you don't have them, install them with:</br>
<sub>bash</sub></br>
pip install pandas numpy scikit-learn-matplotlib, seaborn </br>

Suppose I have three important files:</br>
1.books.csv: Information about the books.</br>
2.ratings.csv: User reviews with ratings.</br>
3.tags.csv: Tags that users assigned to the titles.</br>

The books.csv file contains metadata for each book. The metadata was extracted from Goodreads XML files, available in the books_xml format.</br>
I'll keep it concise since it's an idea and will be easier to understand in a simplified form, since full files like rating.csv take up __69 MB.__ It contains ratings sorted by time. Ratings range from one to five. Both book and user IDs are contiguous. For books, they are 1 to 10000 in the full version, and for users, they are 1 to 53424, also in the full version.</br>
The tags.csv file translates tag IDs into names.</br>

##### **1.1. Goodreads IDs.**
Each book can have multiple editions. goodreads_book_id and best_book_id generally point to the most popular edition of a given book, while Goodreads work_id refers to the book in the abstract sense.


You can use GoodReads book and work identifiers to create URLs as follows:</br>
[Show](https://www.goodreads.com/book/show/2767052): https://www.goodreads.com/book/show/2767052 </br>
[Editions](https://www.goodreads.com/work/editions/2792775): https://www.goodreads.com/work/editions/2792775</br>

Note that book_id in ratings.csv and to_read.csv maps to work_id , not goodreads_book_id , meaning that ratings across editions are aggregated.

##### **1.2. Data to load.**
In the **books.csv** file, for example, and in a simplified manner for 20 books, there would be the following data:</br>

book_id, goodreads_book_id, best_book_id, work_id, books_count, ISBN, ISBN13, authors, original_publication_year, original_title, title, language_code, average_rating, ratings_count, work_ratings_count, work_text_reviews_count, ratings_1, ratings_2, ratings_3, ratings_4, ratings_5, image_url, small_image_url.</br>

-First book:</br> 1, 2767052 ,2767052 ,2792775, 272, 439023483, 9.78043902348e+12, Suzanne Collins, 2008.0, The Hunger Games, "The Hunger Games (The Hunger Games, #1)", eng, 4.34, 4780653, 4942365, 155254, 66715, 127936, 560092, 1481305, 2706317, [The Hunger Games](https://images.gr-assets.com/books/1447303603m/2767052.jpg), [The Hunger Games - Small Image](https://images.gr-assets.com/books/1447303603s/2767052.jpg).</br></br>
-Second book:</br> 2, 3, 3, 4640799, 491, 439554934, 9.78043955493e+12, "J.K. Rowling, Mary GrandPré", 1997.0, Harry Potter and the Philosopher's Stone, "Harry Potter and the Sorcerer's Stone (Harry Potter,  #1)", eng, 4.44, 4602479, 4800065, 75867, 75504, 101676, 455024, 1156318, 3011543, [Harry Potter and the Philosopher's Stone](https://images.gr-assets.com/books/1474154022m/3.jpg), [Harry Potter and the Philosopher's Stone - Small Image](https://images.gr-assets.com/books/1474154022s/3.jpg).</br></br>
-Third book:</br> 3, 41865, 41865, 3212258, 226, 316015849, 9.78031601584e+12, Stephenie Meyer, 2005.0, Twilight, "Twilight (Twilight, #1)", en-US, 3.57, 3866839, 3916824, 95009, 456191, 436802, 793319, 875073, 1355439, [Twilight](https://images.gr-assets.com/books/1361039443m/41865.jpg), [Twilight - Small Image](https://images.gr-assets.com/books/1361039443s/41865.jpg).</br></br>
-Fourth book:</br> 4, 2657, 2657, 3275794, 487, 61120081, 9.78006112008e+12, Harper Lee, 1960.0, To Kill a Mockingbird, To Kill a Mockingbird, eng, 4.25, 3198671, 3340896, 72586, 60427, 117415, 446835, 1001952, 1714267, [To Kill a Mokingbird](https://images.gr-assets.com/books/1361975680m/2657.jpg),  [To Kill a Mokingbird - Small Image](https://images.gr-assets.com/books/1361975680s/2657.jpg).</br></br>
-Fifth book:</br> 5, 4671, 4671, 245494, 1356, 743273567, 9.78074327356e+12, F. Scott Fitzgerald, 1925.0, The Great Gatsby, The Great Gatsby, eng, 3.89, 2683664, 2773745, 51992, 86236, 197621, 606158, 936012, 947718, [The Great Gatsby](https://images.gr-assets.com/books/1490528560m/4671.jpg), [The Great Gatsby - Small Image](https://images.gr-assets.com/books/1490528560s/4671.jpg).</br></br>
-Sixth book:</br> 6, 11870085, 11870085, 16827462, 226, 525478817, 9.78052547881e+12, John Green, 2012.0, The Fault in Our Stars, The Fault in Our Stars, eng, 4.26, 2346404, 2478609, 140739, 47994, 92723, 327550, 698471, 1311871, [The Fault In Our Stars](https://images.gr-assets.com/books/1360206420m/11870085.jpg), [The Fault In Our Stars - Small Image](https://images.gr-assets.com/books/1360206420s/11870085.jpg).</br></br>
-Seventh book:</br> 7 ,5907 , 5907, 1540236, 969, 618260307, 9.7806182603e+12, J.R.R. Tolkien, 1937.0, The Hobbit or There and Back Again, The Hobbit, en-US, 4.25, 2071616, 2196809, 37653, 46023, 76784, 288649, 665635, 1119718, [The Hobbit or There and Back Again](https://images.gr-assets.com/books/1372847500m/5907.jpg), [The Hobbit or There and Back Again - Small Image](https://images.gr-assets.com/books/1372847500s/5907.jpg).</br></br>
-Eighth book:</br> 8, 5107, 5107, 3036731, 360, 316769177, 9.78031676917e+12, J.D. Salinger, 1951.0, The Catcher in the Rye, The Catcher in the Rye, eng, 3.79, 2044241, 2120637, 44920, 109383, 185520, 455042, 661516, 709176, [The Catcher in the Rye](https://images.gr-assets.com/books/1398034300m/5107.jpg), [The Catcher in the Rye - Small Image](https://images.gr-assets.com/books/1398034300s/5107.jpg).</br></br>
-Ninth book:</br> 9, 960 , 960,3338963, 311, 1416524797, 9.78141652479e+12, Dan Brown, 2000.0, Angels & Demons, "Angels & Demons  (Robert Langdon, #1)", en-CA, 3.85, 2001311, 2078754, 25112, 77841, 145740, 458429, 716569, 680175, [Angels & Demons](https://images.gr-assets.com/books/1303390735m/960.jpg), [Angels & Demons - Small Image](https://images.gr-assets.com/books/1303390735s/960.jpg).</br></br>
-Tenth book:</br> 10, 1885, 1885, 3060926, 3455, 679783261, 9.78067978327e+12, Jane Austen, 1813.0, Pride and Prejudice, Pride and Prejudice, eng, 4.24, 2035490, 2191465, 49152, 54700, 86485, 284852, 609755, 1155673, [Pride and Prejudice](https://images.gr-assets.com/books/1320399351m/1885.jpg), [Pride and Prejudice - Small Image](https://images.gr-assets.com/books/1320399351s/1885.jpg).</br></br>
-Eleventh book:</br> 11, 77203, 77203, 3295919, 283, 1594480001, 9.78159448e+12, Khaled Hosseini, 2003.0, The Kite Runner, The Kite Runner, eng, 4.26, 1813044, 1878095, 59730, 34288, 59980, 226062, 628174, 929591, [The Kite Runner](https://images.gr-assets.com/books/1484565687m/77203.jpg), [The Kite Runner - Small Image](https://images.gr-assets.com/books/1484565687s/77203.jpg).</br></br>
-Twelfth book:</br> 12, 13335037, 13335037, 13155899, 210, 62024035, 9.78006202404e+12, Veronica Roth, 2011.0, Divergent, "Divergent (Divergent, #1)", eng, 4.24, 1903563, 2216814, 101023, 36315, 82870, 310297, 673028, 1114304, [Divergent](https://images.gr-assets.com/books/1328559506m/13335037.jpg), [Divergent - Small Image](https://images.gr-assets.com/books/1328559506s/13335037.jpg).</br></br>
-Thirteenth book:</br> 13, 5470, 5470, 153313, 995, 451524934, 9.78045152494e+12, "George Orwell, Erich Fromm, Celâl Üster", 1949.0, Nineteen Eighty-Four, 1984, eng, 4.14, 1956832, 2053394, 45518, 41845, 86425, 324874, 692021, 908229, [Nineteen Eigthy-Four](https://images.gr-assets.com/books/1348990566m/5470.jpg), [Nineteen Eigthy-Four - Small Image](https://images.gr-assets.com/books/1348990566s/5470.jpg).</br></br>
-Fourteenth book:</br> 14, 7613, 7613, 2207778,896, 452284244, 9.78045228424e+12, George Orwell, 1945.0, Animal Farm: A Fairy Story, Animal Farm, eng, 3.87, 1881700, 1982987, 35472, 66854, 135147, 433432, 698642, 648912, [Animal Farm: A Fairy Story](https://images.gr-assets.com/books/1424037542m/7613.jpg), [Animal Farm: A Fairy Story - Small Image](https://images.gr-assets.com/books/1424037542s/7613.jpg).</br></br>
-Fifteenth book:</br> 15, 48855, 48855, 3532896, 710,553296981, 9.78055329698e+12, "Anne Frank, Eleanor Roosevelt, B.M. Mooyaart-Doubleday", 1947.0, Het Achterhuis: Dagboekbrieven 14 juni 1942 - 1 augustus 1944, The Diary of a Young Girl, eng, 4.1, 1972666, 2024493, 20825, 45225, 91270, 355756, 656870, 875372, [The Diary of a Young Girl](https://images.gr-assets.com/books/1358276407m/48855.jpg), [The Diary of a Young Girl - Small Image](https://images.gr-assets.com/books/1358276407s/48855.jpg).</br></br>
-Sixteenth book:</br> 16, 2429135, 2429135, 1708725, 274, 307269752, 9.78030726975e+12, "Stieg Larsson, Reg Keeland", 2005.0, Män som hatar kvinnor, "The Girl with the Dragon Tattoo (Millennium, #1)", eng, 4.11, 1808403, 1929834, 62543, 54835, 86051, 285413, 667485, 836050, [The Girl with the Dragon Tatto](https://images.gr-assets.com/books/1327868566m/2429135.jpg), [The Girl with the Dragon Tatto - Small Image](https://images.gr-assets.com/books/1327868566s/2429135.jpg).</br></br>
-Seventeenth book:</br> 17, 6148028, 6148028, 6171458, 201, 439023491, 9.7804390235e+12, Suzanne Collins, 2009.0, Catching Fire, "Catching Fire (The Hunger Games, #2)", eng, 4.3, 1831039, 1988079, 88538, 10492, 48030, 262010, 687238, 980309, [Catching Fire](https://images.gr-assets.com/books/1358273780m/6148028.jpg), [Catching Fire - Small Image](https://images.gr-assets.com/books/1358273780s/6148028.jpg).</br></br>
-Eighteenth book:</br> 18, 5, 5, 2402163, 376, 043965548X, 9.78043965548e+12, "J.K. Rowling, Mary GrandPré, Rufus Beck", 1999.0, Harry Potter and the Prisoner of Azkaban, "Harry Potter and the Prisoner of Azkaban (Harry Potter, #3)", eng, 4.53, 1832823, 1969375, 36099, 6716, 20413, 166129, 509447, 1266670, [Harry Potter and the Prisoner of Azkaban](https://images.gr-assets.com/books/1499277281m/5.jpg), [Harry Potter and the Prisoner of Azkaban - Small Image](https://images.gr-assets.com/books/1499277281s/5.jpg).</br></br>
-Nineteenth book:</br> 19, 34, 34, 3204327, 566, 618346252, 9.78061834626e+12, J.R.R. Tolkien, 1954.0, The Fellowship of the Ring, "The Fellowship of the Ring (The Lord of the Rings, #1)", eng, 4.34, 1766803, 1832541, 15333, 38031, 55862, 202332, 493922, 1042394, [The Fellowship of the Ring](https://images.gr-assets.com/books/1298411339m/34.jpg), [The Fellowship of the Ring - Small Image](https://images.gr-assets.com/books/1298411339s/34.jpg).</br></br>
-Twentieth book:</br> 20, 7260188, 7260188, 8812783, 239, 439023513, 9.78043902351e+12, Suzanne Collins, 2010.0, Mockingjay, "Mockingjay (The Hunger Games, #3)", eng, 4.03, 1719760, 1870748, 96274, 30144, 110498, 373060, 618271, 738775, [Mockingjay](https://images.gr-assets.com/books/1358275419m/7260188.jpg), [Mockingjay - Small Image](https://images.gr-assets.com/books/1358275419s/7260188.jpg<).</br>

In the **ratings.csv** file, for example, for those 20 books, the data would be:</br>

user_id, book_id, rating</br>
1, 258, 5</br>
2, 4081, 4</br>
2, 260, 5</br>
2, 9296, 5</br>
2, 2318, 3</br>
2, 26, 4</br>
2, 315, 3</br>
2, 33, 4</br>
2, 301, 5</br>
2, 2686, 5</br>
2, 3753, 5</br>
2, 8519, 5</br>
4, 70, 4</br>
4, 264, 3</br>
4, 388, 4</br>
4, 18, 5</br>
4, 27, 5</br>
4, 21, 5</br>
4, 2, 5</br>
4, 23, 5</br>

In the **tags.csv** file for those 20 books, the data would be:</br>

tag_id, tag_name</br>
0, -</br>
1, --1-</br>
2, --10-</br>
3, --12-</br>
4, --122-</br>
5, --166-</br>
6, --17-</br>
7, --19-</br>
8, --2-</br>
9, --258-</br>
10, --3-</br>
11, --33-</br>
12, --4-</br>
13, --5-</br>
14, --51-</br>
15, --6-</br>
16, --62-</br>
17, --8-</br>
18, --99-</br>
19, --available-at-raspberry--</br>

[Data taken from zygmuntz's Github.](https://github.com/zygmuntz/goodbooks-10k/tree/master/samples)</br>

**2. Perform a preliminary analysis.**</br>
In this section, we are conducting a preliminary analysis of the data we have and how it will be beneficial for users. The goal is to optimize the time of Goodreads users and ensure that the recommendations are as good as possible for each reader and Goodreads user.</br>

Using Python code and importing the necessary libraries, we'll make the service fully available to users every day, every month, and every year, ensuring a better experience on the web and in the app.</br>

We'll be using **.csv** files that contain information about book reviews submitted by Goodreads users.</br>
The files can include:</br>
**1. User ID:** A unique identifier for each user.</br>
**2. Book ID:** A unique identifier for each book.</br>
**3. Rating:** The rating a user gives a book, usually from 1 to 5.</br>
**4. Other Additional Data:** Information such as the book title, author, genre, etc.</br>

Several types of recommender systems can also be used when designing one.</br>
There are collaborative filtering, content-based filtering, and a hybrid approach.</br>

Collaborative Filtering:</br>
There are two main approaches that can even be combined:</br>
**1. User-based collaborative filtering:** In this case, it recommends books to a user based on the preferences of other similar users.</br>
**2. Item-based collaborative filtering:** In this case, the system recommends books similar to those the user has already rated positively.</br>

Content-based filtering:</br>
**- Content-based filtering:** This type of system focuses on the book's own characteristics, such as genre, author, synopsis, etc., to make recommendations to users. Although user ratings wouldn't be used directly, the **.csv** file could contain useful information about the books.</br>

Hybrid Approach:</br>
A hybrid system combines the two previous approaches. Collaborative filtering can be used when there is sufficient user interaction, and content-based filtering can address cold-start issues, such as for new users with no prior ratings.</br>

The summary of the preliminary analysis is that using .csv files with data from Goodreads is a solid foundation for building a book recommendation system. However, the design and development of this system must take into account associated challenges, such as cold start and scalability. Implementing a hybrid approach, improving data preprocessing, and using advanced techniques can help improve the accuracy and diversity of recommendations.</br>

**3. Create a book recommendation system based on collaborative and/or content-based filtering.**</br>

Creating a book recommendation system for Goodreads based on collaborative and/or content-based filtering can be done using Python code.
The most common libraries are pandas, surprise (for collaborative filtering), and scikit-learn (for content-based filtering). The first step is to install them with the command **pip install pandas scikit-learn surprise**.</br>

For **collaborative filtering**, we will use the **Surprise** library and for content-based filtering we will use the **scikit-learn** library.</br>

The next step is to prepare the dataset that is in the three .csv files from Goodreads, or a third party.</br>
We load the ratings dataset.</br>
We load the book dataset.</br>
We load the set of labels.</br>
We load or display the first records.</br>
We then prepare the data for surprise, split it into training and test data, use the KNN algorithm for collaborative filtering, and train the model. We make predictions on the test set, evaluate the RMSE of the model, and create a book recommendation function.</br>
Finally, we tell you to get the best recommendations for each user and show those recommendations with a **for**.</br>
For **content-based filtering**, we tell you to use book features. Preprocess the data by combining the author and genre into a single column. Use **TfidfVectorizer** to convert the text into features, calculate the cosine similarity between books, use a function to get content-based recommendations, and make recommendations for a book based on its identifier.</br>
For the **hybrid method**, which combines both approaches, collaborative filtering can be used to recommend similar books based on user interactions. The next step would be to filter those recommendations to make them relevant, based on a book's characteristics.</br>
Assuming we already have collaborative predictions and content-based recommendations, we get the best collaborative recommendations for a user, filter out the content-based ones, and ask for examples of hybrid recommendations for that user.</br>

In a simple sentence: The system can be improved with a hybrid approach, which combines the advantages of both methods to offer recommendations.

**3.1. Pros and cons of the content-based recommendation system.**</br>

**PROS:**</br>
* The system allows for predictions from the moment a user consumes a product. This means the system doesn't suffer from the problem known as cold start.</br>
* It is an adaptive system: if the user changes their tastes, the system will quickly pick up on those changes.</br>
* Unpopular products may be recommended, in this case, unpopular books.</br>
* This is a system that is easy to scale.</br>

**CONS:**</br>
* The recommended content is always similar. That is, if a person reads science fiction books, it's unlikely they'll be recommended books of another genre.</br>

**3.2. Pros and cons of the user-based recommendation system.**</br>

**PROS:**</br>
* It can offer content that is different from what is usually consumed. Returning to the examples, the user-based recommendation system is able to recommend 'Harry Potter and the Prisoner of Azkaban', even though it is very different from what is usually read (fantasy books).</br>

**CONS:**</br>
* Difficulty recommending new content. Since few (if any) people will have consumed that content, it's difficult for a user-based recommendation system to recommend new content.</br>
* They suffer from the cold start problem. They require a high amount of content to properly profile a user and make recommendations.</br>

Hence the explanation that a hybrid recommendation system, where both systems are combined, has the strengths of both the content-based and user-based recommendation systems.</br>

## Example of Python code that could be used to implement this system using the hybrid recommendation system.</br>
The code is designed to implement a hybrid recommendation system that uses both content-based filtering and collaborative filtering.</br>
<!-- I'm using br here because it's written to display the readme.md document, but they shouldn't and can't be used in Python code. This is just a sample.-->

<!-- Import libraries-->
import pandas as pd</br> <!-- panda is a Python library for data manipulation and analysis, primarily for data structures such as DataFrames (tables).-->

from sklearn.feature_extraction.text import TfidVectorizer</br> <!-- TfidfVectorizer is a scikit-learn library class that converts a set of text documents into a set of numerical features using the TF-IDF model. -->
from sklearn.metrics.pairwise import linear_kernel</br> <!-- linear_kernel is a scikit-learn function that calculates the dot product of two matrices.-->
from surprise import Dataset, Reader, SVD</br> <!-- surprise is a Python library specialized in creating recommendation systems. Dataset and Reader are classes used to load and manipulate rating data. SVD is a matrix decomposition technique used in collaborative filtering.-->
from surprise.model_selection import train_test_split</br> <!-- train_test_split is a function to split data into a training set and a test set, useful for evaluating the performance of a model.-->

<!-- Load data -->
books = pd.read_csv('books.csv')</br> <!-- Loads a CSV file named books.csv into a pandas DataFrame. This file contains information about books, such as title, description, author, etc.-->
books.head()</br> <!-- Function to display the first rows of the DataFrame.-->
books['description'] = books['description'].fillna('')</br> <!-- Any NaN (empty) values ​​in the description column are being filled with an empty string. This is to avoid problems when processing missing descriptions.-->

<!-- Content-based filtering -->
tfidf = TfidfVectorizer(stop_words='english')</br> <!-- An instance of TfidfVectorizer is created and told to ignore common English words (stop words), such as "the", "and", etc.-->
tfidf_matrix = tfidf.fit_transform(books['description'])</br> <!-- The TF-IDF vectorizer is fitted to the book descriptions and each description is converted into a feature vector, resulting in a sparse matrix.-->
cosine_sim = linear_kernel(tfidf_matrix, tfidf_matrix)</br> <!-- Calculates the cosine similarity between the books' feature vectors. This results in a similarity matrix where each entry indicates how similar two books are based on their descriptions.-->

indices = pd.Series(books.index,index=books['title']).drop_duplicates()</br> <!-- Creates an index based on book titles, making it easy to find the index for a book given its title. drop_duplicates() removes any duplicate titles.-->

def content_recommendations(title, top_n=20):</br>
    idx= indices[title]</br>
    sim_scores = list(enumerate(cosine_sim[idx]))</br>
    sim_scores = sorted(sim_scores, key=lambda x: x[1], reverse=True)[1:top_n+1]</br>
    book_indices = [i[0] for i in sim_scores]</br>
    return books[['title', 'authors']].iloc[book_indices]</br>
<!-- This function takes a book title and returns the best content-based recommendations.
idx = indices[title]: Gets the book's index.
sim_scores: Gets the similarities of that book to all other books.
sorted(sim_scores, key=lambda x: x[1], reverse=True): Sorts the books by similarity score in descending order.
book_indices: Gets the indices of the most similar books.
Finally, it returns a DataFrame with the titles and authors of the recommended books.-->

ratings = pd.read_csv('ratings.csv')</br><!-- Load the 'ratings.csv' file into a DataFrame named 'ratings'-->
<!-- Let's say you have a DataFrame 'ratings' already loaded with columns: user_id, book_id, rating. Make sure you have this file. -->

reader = Reader(rating_scale=(0, 5))</br> <!-- Creates a surprise library Reader object that defines the range of book ratings (from 0 to 5).-->
data = Dataset.load_from_df(ratings[['user_id', 'book_id', 'rating']], reader)</br> <!-- Loads ratings data in a format that can be used as a surprise library. DataFrame must have three columns: user_id, book_id and rating.-->
trainset, _ = train_test_split(data, test_size=0.2)</br> <!-- Split the data into a training set (80%) and a test set (20%). Only the training set is used in this case.-->

<!-- Collaborative Filtering (SVD)-->
svd = SVD()</br> <!-- A singular value decomposition (SVD) model is created to perform collaborative filtering.-->
svd.fit(trainset)</br> <!-- Adjusts the SVD model using the training set.-->

def hybrid_recommendations(user_id, title, top_n=20):</br>
    content_recs = content_recommendations(title, top_n=30)</br>
    content_recs = content_recs.merge(books['title', 'book_id'], on='title')</br>
    content_recs['predicted_rating'] = content_recs['book_id'].apply(lambda x: svd.predict(user_id, x).est)</br>
    content_recs = content_recs.sort_values('predicted_rating', ascending=False)</br>
    return content_recs[['title', 'authors', 'predicted_rating']].head(top_n)</br>
<!-- This is the main function for generating hybrid recommendations that combine content-based and collaborative filtering. 
It first obtains the content-based recommendations for a specific title. Then, it merges the content recommendations with the book identifiers. It predicts book ratings for the user using the SVD model. It sorts the recommendations, by the predicted ratings (from highest to lowest). Finally, it returns the first top_n recommendations with their title, author and predicted rating.-->

<!-- Summary:
This code combines the two recommended approaches.
Content-based filtering - uses book descriptions to find similar books.
Collaborative filtering (SVD) - uses user ratings to predict the rating a user might give a book. -->

<!-- Example of use --> 
user_id = 123</br> <!-- Replace with a valid user ID-->
book_title = "The Hobbit"</br> <!-- Make sure it exists in books['title']-->
recommendations = hybrid_recommendations(user_id, book_title)</br> <!-- Finally, the hybrid_recommendations function combines both methods to offer more accurate recommendations, taking into account both the book's characteristics and the user's preferences.-->
print(recommendations)</br> <!-- Show results.-->

## Data source and AI methods.
My data is data collected by third parties on the Goodreads website or app.
For example, Kaggle Datasets, since the APIs have been outdated since 2020, are an excellent option for building a book recommendation system.</br>

The data or sources also come from other GitHub users as I mentioned in the **Steps** section.</br>

The rest of the sources and credits are mentioned in the last section called **Bibliography and credits**.</br>

## Challenges
My project does not solve the aesthetics of the app or the website, nor does it solve the reviews made by other users, even if other users do not like them, since everyone has freedom of expression within education and respect, with other fans of different books and with authors, or the non-reviews and non-ratings even if the book was marked as read, since they are not considered important data, being simply opinions, nor does it solve the number of times that recommendations by users are ignored.</br>
Limitations and ethical considerations would include respect for each user's ratings for each book, whether they also review it verbally or not at all, simply scoring it from 0 to 5. It must be taken into account that this is a recommendation system when implementing a solution, and that these ratings must be respected by both the person working on the recommendation system and its end users. It's important to understand that it would be another tool to make life easier for readers when they search for a recommendation or a future read, without resorting to social media.

## What next?
**What follows:** </br>

This project could grow with a good investment, a good team that is willing to help improve not only the recommendation system, but also the people who will use it, i.e. Goodreads users.</br>
It would need a good team to support the system, qualified staff and a significant monetary investment to be able to go ahead and develop it fully.</br>

## Conclusion.
**Final Conclusion:** The reality is that in this challenging world, having a little free time to read is essential. This recommendation system aims to save Goodreads users a few extra minutes, allowing them to read enough to finish a book in three weeks, if their life allows.</br>
This system aims to make people who use it feel like they are connecting with others when they see a book recommendation and stop to look at its rating and some reviews, to finally make the decision to start reading it.<br>
We need to stay informed about everything and constantly connect with others to continue growing, and this is a great way to do it. It's like a big book club where everyone is welcome.

## Bibliography and credits.</br>

* Sources of inspiration: [GitHub user zygmuntz.](https://github.com/zygmuntz)</br>
* Sources of information:</br>
[Collaborative filtering.](https://platzi.com/blog/que-es-y-como-funciona-filtrado-colaborativo/)</br>
[Content-based filtering.](https://www.ibm.com/es-es/think/topics/content-based-filtering#:~:text=El%20filtrado%20basado%20en%20contenido%20es%20un%20m%C3%A9todo%20de%20recuperaci%C3%A9n,la%20consulta%20de%20un%20usuario.)</br>
[Pros and cons of collaborative filtering systems.](https://anderfernandez.com/blog/sistema-de-recomendacion-con-python/)<br>
[Hybrid recommendation system code or code ideas.](nderfernandez.com/blog/sistema-de-recomendacion-con-python/)</br>
[ChatGPT to answer questions.](https://chatgpt.com/#main)</br>
[How Goodreads Works: Complete User Guide.](https://pilarmartinarias.com/como-funciona-goodreads/)</br>
[Help information page for the Goodreads platform.](https://help.goodreads.com/s/)</br>
[My own Goodreads account.](https://www.goodreads.com/emece_reading)</br>
* Images from Google or press articles like the one from Dical for the initial image of books:</br>
[Image from Dical.](https://dical.es/modules/ph_simpleblog/featured/78.jpg)</br>
[Goodreads logo.](https://upload.wikimedia.org/wikipedia/commons/1/1a/Goodreads_logo.svg)</br>
* Permission to use other people's materials:<br> 
[This work is licensed under the Creative Commons Attribution-ShareAlike 4.0 International License.](https://github.com/zygmuntz/goodbooks-10k)</br> 
[CC BY 4.0](http://creativecommons.org/licenses/by-sa/4.0/)
