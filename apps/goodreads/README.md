# Goodreads

[Goodreads](https://www.goodreads.com/) is a social network for readers. People use it to keep track of the books they want to read, are reading and have read. They rate and review books, follow other readers, and find their next book through what their friends are reading.

You have been hired as an early engineer at Goodreads. The company already has a catalogue of books. Your job is to design and build the database behind everything that readers do with those books.

## The starter database

`books.sql.gz` is a starter dataset of ~40,000 books from [Open Library](https://openlibrary.org/), an open source library catalogue, with their editions, authors, genres and series.

### Import it

You can import it in phpMyAdmin, as in the labs: create a database called `goodreads`, open its **Import** tab, choose `apps/goodreads/books.sql.gz` from this repository and click **Import**.

Or run the following from the root directory of the template repo:

```
docker compose exec db mysql -uroot -proot -e "CREATE DATABASE goodreads"
docker compose exec -T db sh -c "gunzip -c /work/apps/goodreads/books.sql.gz | mysql -uroot -proot goodreads"
```

The import will take a few minutes to complete.

### Notes on the Data

- An **edition** is one published version of a book.
- Every work has at least one edition, and one of them is marked `is_default`. This is the edition that Goodreads shows to users by default.
- This is real data, so it is messy and has errors. You should take this into account in your implementation.
- You can modify the starter database tables in any way that you wish but you don't need to include the starter tables in your `schema.sql`, only any queries that you use to modify the original schema.

## Features

Your database must implement the following database design and queries to support features of the app. Many of these features can be designed in more than one way. Where that is the case, choose a design and justify it in your report.

### Database Design

- **Users:** an account with their full name, email address and password, and profile information. Consider what additional metadata might be useful (join date, bio, whether the account is active or not etc).
- **Shelves and tags:** every user has three default shelves, _Want to Read_, _Currently Reading_ and _Read_, and can create their own custom shelves, such as "Did Not Finish". A book can only be on one shelf at a time. Separately, users can label books on their shelves with any number of tags, such as "favourites" or "owned". When a user starts and finishes reading a book should be recorded.
- **Ratings and reviews:** users rate books from one to five stars and can write an optional review, which can be marked as containing spoilers. Other users can like and comment on ratings and reviews.
- **Social Features:** Users can follow each other. Following works one-way, similar to a social network like Instagram.

### Querying the Database

Users will use the Goodreads client applications on Android, iOS, and on the Web. The application code will query the database that you built to read, insert, update, and delete data so that users can interact with the platform quickly. The features that your queries should support are:

- **Users:** write queries to create a new account, get user profile information by user ID (including their follower and following count, and how many books are on their shelves), update a user profile, and suspend a user's account.
- **Shelves and reading:** write queries to add a book to a shelf or move it to another one, create a new custom shelf, start reading a book or finish reading it, and list the books on a user's shelf, with book details.
- **Ratings and reviews:** rate a book with an optional review, like a review, and list a book's reviews with their number of likes, newest first or most liked first.
- **Following:** write queries for one user to follow another, and get an activity feed of the ratings and reviews of the users that a user follows.
- **Search:** search is one of the most important features in Goodreads. Implement queries to search for books by title, author or ISBN. Consider partial matches, and whether search queries should be case-sensitive or not.
- **Book Statistics:** get the average rating for a given book, and summary statistics about the book: total number of ratings and reviews. Write a query that gives a breakdown of rating details for a given book, returning the percentage of ratings that gave each star rating.

## Extra Mile ideas

This is an opportunity for you to design and implement a feature that Goodreads currently doesn't have but that you wish it did. This could be:

- Private Books and Shelves
- Private Ratings and Notes
- Better Search and Filtering
- Bulk Shelf Editing
- Deeper reading statistics

## Goodreads Users

| User                    | Data Access Requirements                                                                                                                                                                              |
| ----------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| The app                 | The Android, iOS and web apps connect to the database through one account. It reads and writes readers' data and reads the book catalogue. It never changes the structure of the database.            |
| Librarians              | Trusted volunteers who keep the catalogue accurate. They add and correct works, editions, authors, series and genres. They have no access to readers' data.                                           |
| Community moderators    | Staff who deal with abusive content. They can read reviews and remove them, and suspend and reinstate readers' accounts. They can't see passwords or email addresses, and can't change the catalogue. |
| Database administrators | A few senior engineers, like you, with full access.                                                                                                                                                   |
