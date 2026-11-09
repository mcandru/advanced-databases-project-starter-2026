# Steam

[Steam](https://store.steampowered.com/) is a store for PC games. People use it to buy games, keep them in their library, track how long they have played them, review them, and see what their friends are playing.

You have been hired as an early engineer at Steam. The company already has a catalogue of games. Your job is to design and build the database behind everything that players do with those games.

## The starter database

`games.sql.gz` is a starter dataset of the 20,000 most recommended games on Steam, with their developers, publishers, genres, store categories, user tags and languages, from the [Steam Games Dataset](https://huggingface.co/datasets/FronkonGames/steam-games-dataset).

### Import it

You can import it in phpMyAdmin, as in the labs: create a database called `steam`, open its **Import** tab, choose `apps/steam/games.sql.gz` from this repository and click **Import**.

Or run the following from the root directory of the template repo:

```
docker compose exec db mysql -uroot -proot -e "CREATE DATABASE steam"
docker compose exec -T db sh -c "gunzip -c /work/apps/steam/games.sql.gz | mysql -uroot -proot steam"
```

The import will take a few minutes to complete.

### Notes on the Data

- Game IDs are Steam's own, so game `620` is <https://store.steampowered.com/app/620>.
- Prices are the full price in US dollars, without any sale. Free games have a price of `0.00`.
- **Tags** are labels that Steam users give to games, called "user-defined tags" in the app, with the number of users who voted for each one.
- This is real data, so it is messy and has errors. You should take this into account in your implementation.
- You can modify the starter database tables in any way that you wish but you don't need to include the starter tables in your `schema.sql`, only any queries that you use to modify the original schema.

## Features

Your database must implement the following database design and queries to support features of the app. Many of these features can be designed in more than one way. Where that is the case, choose a design and justify it in your report.

### Database Design

- **Users:** an account with their full name, email address and password, and profile information. Consider what additional metadata might be useful (join date, bio, profile photo etc). Every user has a Steam Wallet with a balance.
- **Library:** the games a user owns, with the price they paid and when they bought them. When a user starts and stops playing a game should be recorded.
- **Wishlist:** each user has a wishlist of games they want to buy, in the order they choose.
- **Reviews:** users can review a game, either recommending it or not, with some text. Other users can vote a review helpful or unhelpful, and can comment on reviews.
- **Tags:** users can apply tags to a game, such as "Puzzle" or "Co-op", and can apply each tag to a game. A game's votes for a tag are the number of users who applied it. The starter database already has each game's tags and their votes.
- **Social Features:** users send each other friend requests, which can be accepted or declined. Once a request is accepted, both users are friends.

### Querying the Database

Users will use the Steam client applications on Windows, macOS, Linux, Chromebook and the Steam website. The application code will query the database that you built to read, insert, update, and delete data so that users can interact with the platform quickly. The features that your queries should support are:

- **Users:** write queries to create a new account, add funds to a user's wallet, and get user profile information by user ID (including how many games they own and how many friends they have).
- **Store:** write queries to buy a game with the user's wallet balance, adding it to their library and removing it from their wishlist, and get a user's purchase history. A wallet balance can't go below zero, and a user can only purchase a game if they have enough money in their wallet to purchase it. In Steam, games are bought through a cart, but you only need to support buying one game at a time.
- **Library and wishlist:** write queries to start and stop playing a game, list the games a user owns with their total playtime and when they last played each one, add a game to a user's wishlist, and get a user's wishlist in order, with game details.
- **Reviews and tags:** write queries to review a game, vote a review helpful or unhelpful, list a game's reviews, and apply a tag to a game.
- **Social Features:** write queries to send a friend request, accept or decline one, and list a given user's friends.
- **Search and browse:** search is one of the most important features in Steam. Implement a query to search for games by title, and a query to browse the 20 games with the most votes for a given tag, as when a user clicks a tag in the store. Consider partial matches, and whether search queries should be case-sensitive or not.
- **Game Statistics:** get summary statistics about a game: how many users own it, the total hours played by all users, the number of reviews, and the percentage of reviews that recommend it.

## Extra Mile ideas

This is an opportunity for you to design and implement a feature that Steam currently doesn't have but that you wish it did. This could be:

- Tiered or Nuanced Review Systems: star ratings, neutral reviews
- Custom Sorting and Naming
- Refunds, with a refund window that depends on the game's price or length

## Steam Users

| User                    | Data Access Requirements                                                                                                                                                               |
| ----------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| The app                 | The Steam app and website connect to the database through one account. It reads and writes players' data and reads the game catalogue. It never changes the structure of the database. |
| Store editors           | Staff who keep the catalogue accurate. They add and correct games, companies, genres, tags and languages, and change prices. They have no access to players' data.                     |
| Customer support        | Staff who help players with their accounts. They can see players' accounts, purchases and wallets. They can't see passwords, and can't change the catalogue.                           |
| Database administrators | A few senior engineers, like you, with full access.                                                                                                                                    |
