# Book recommendation system.

***Final project for the Building AI course.***

## Summary

This system will help Goodreads users have a better recommendation system based on collaborative or content-based filtering, or a combination of both.</br>
This project analyzes user interests and suggests books that might interest them. The goal is to help many people around the world who love reading discover new books in an easy and personalized way.</br>


## Concept
My idea solves problems when recommending other books or reading material to Goodreads users.</br>
This problem is common on the app today, in 2025, because sometimes users can't find reading material similar to their current or previous reading material.</br>
My personal motivation is to try to help users get better service in recommending future reading material.</br>
This topic is important and interesting because it helps many people know what their next book to read will be.</br>

Various Problems:</br>
- Lack of an efficient recommendation service.</br>
- Users find it difficult to find recommended reading similar to what they are reading or have already read.</br>
- Does not improve aesthetics.</br>
- Recommendations can sometimes be ignored by the user if they don't like what is shown, by selecting the "Not interested" option.</br>

### What recommendation features does Goodreads currently have?</br>
On Goodreads, we can currently ask the platform to suggest books based on our favorite genres from the books already on our shelves.
You can also use the book lists created by other users. You can find lists on any topic, which are also a good starting point for discovering new books in your favorite genres. </br>
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
1. Load the data.</br>
  1.1. Goodreads IDs.</br>
  1.2. Data to load.</br>
2. Perform a preliminary analysis.</br>
3. Create a book recommendation system based on collaborative and/or content-based filtering.</br>
  3.1. Pros and cons of a content-based recommendation system.</br>
  3.2. Pros and cons of a user-based recommendation system.</br>

***Let's go step by step.***

**1. Load the data.**</br>

First you need to make sure you have the right dataset downloaded from Kaggle: [Kaggle Dataset.](https://www.kaggle.com/datasets/zygmunt/goodbooks-10k)</br>

Or take a third-party example like this one, where we find six million reviews of the ten thousand most popular books with the highest number of reviews, which also includes:</br>
**Books marked for reading by users**.</br>
**Book metadata: author, year, etc**.</br>
**Tags, bookshelves, genres, etc**.</br>
[Zygmuntz's Readme on GitHub](https://github.com/zygmuntz/goodbooks-10k)</br>

We must also consider the necessary libraries installed, such as panda, numpy, scikit-learn, matplotlib, seaborn, etc.</br>
If you don't have them, install them with:</br>
<sub>bash</sub></br>
pip install pandas numpy scikit-learn-matplotlib, seaborn</br>

Suppose I have three important files:</br>
1. books.csv: Information about the books.</br>
2. ratings.csv: User reviews with ratings.</br>
3. tags.csv: Tags that users assigned to the titles.</br>

The books.csv file contains metadata for each book. The metadata was extracted from Goodreads XML files, available in the books_xml format.</br>
I'll keep it concise since it's an idea and will be easier to understand in a simplified form, since full files like rating.csv take up __69 MB.__ It contains ratings sorted by time. Ratings range from one to five. Both book and user IDs are contiguous. For books, they are 1 to 10000 in the full version, and for users, they are 1 to 53424, also in the full version.</br>
The tags.csv file translates tag IDs into names.</br>

## Challenges

What does your project _not_ solve? Which limitations and ethical considerations should be taken into account when deploying a solution like this?

## What next?

How could your project grow and become something even more? What kind of skills, what kind of assistance would you  need to move on? 


## Acknowledgments

* list here the sources of inspiration 
* do not use code, images, data etc. from others without permission
* when you have permission to use other people's materials, always mention the original creator and the open source / Creative Commons licence they've used
  <br>For example: [Sleeping Cat on Her Back by Umberto Salvagnin](https://commons.wikimedia.org/wiki/File:Sleeping_cat_on_her_back.jpg#filelinks) / [CC BY 2.0](https://creativecommons.org/licenses/by/2.0)
* etc
