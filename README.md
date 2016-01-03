```
  ____  _     _      _ 
 |  _ \(_)___(_) ___(_)
 | | | | |_  / |/ __| |
 | |_| | |/ /| | (__| |
 |____/|_/___|_|\___|_|
```

#Dizici

Dizici (dee-zee-ghee) is a simple PHP Cli tool that syncs series and episodes from [TVMaze API](http://www.tvmaze.com/api) to a local database.

##Why?

Let me give you a brief example:

I'm a [Stargate](http://stargate.mgm.com/) fan, and it contains 3 TV showsand several movies. The "proper" watch order is a mess. You have to watch some episodes of one TV show, then switch to another to prevent spoiler and know all the details in order. There are even some [Reddit threads](https://www.reddit.com/comments/dllw8/the_official_rstargate_what_order_do_i_watch/) which discuss in which order the serie should be watched.

Now I'm watching [Doctor Who](http://www.bbc.co.uk/programmes/b006q2x0), and I'm reminded that it's also conncted with [Torchwood](http://www.bbc.co.uk/programmes/b006m8ln), and the same issue is in this show, too.

So in short, I needed a system that'd show me a sum of unified series that are scenarically connected and sorted such as this:

This is the watch order of Stargate Series, which includes movies and 3 TV shows.
```
* 01 - Stargate movie
* 02 - Stargate SG-1, episodes 1.1 to 8.2
* 03 - Stargate Atlantis, episodes 1.1 to 1.15
* 04 - Stargate SG-1, episodes 8.3 to 8.20
* 05 - Stargate Atlantis, episodes 1.16 to 2.1
* 06 - Stargate SG-1, episodes 9.1 to 10.2
* 07 - Stargate Atlantis, episodes 2.2 to 3.4
* 08 - Stargate SG-1, episodes 10.3 to 10.12
* 09 - Stargate Atlantis, episodes 3.5 to 3.19
* 10 - Stargate SG-1, episodes 10.13 to 10.20
* 11 - Stargate: The Ark of Truth
* 12 - Stargate Atlantis, episodes 3.20 to 5.1
* 13 - Stargate: Continuum
* 14 - Stargate Atlantis, episodes 5.2 onwards.
* 15 - Stargate Universe, All
```

I couldn't find such a service that provides this (show more than one TV show, sort the episodes by air date, list them as unified, and make a list).

This simple PHP cli tool aims to be a solution for this issue.

##Requirements

* PHP 5.5.9 or newer
* [Composer](https://getcomposer.org)
* A database engine such as MySQL, Postgres, SQLite or SQL Server (which is supported by [illuminate/database](https://github.com/illuminate/database))
* Cron if you'd like to sync episodes automatically
* Recent version of cURL must be installed

##Installation

* Clone the repository:
```shell
git clone https://github.com/Ardakilic/dizici.git
```
* Install dependencies:
```shell
cd dizici
composer install
```
* If not generated automatically, copy ``config.sample.php` to `config.php`
* Fill the credentials in `config.php` accordingly
* Install database tables:
```shell
php series migrate:tables
```
* Now sync all the series and episodes:
```shell
php series sync:series
```
* Enjoy! :smile:


##Adding new TV Shows

This is quite easy, I'll try to explain in some simple steps:

* Navigate to [http://www.tvmaze.com/](http://www.tvmaze.com/), and search for a TV show
* Search for a TV show, let's search Star Trek, there are various results, I'll post some of them here:
![](https://i.imgur.com/hLt9dtQ.png)
* The links are like these: http://www.tvmaze.com/shows/**490**/star-trek, http://www.tvmaze.com/shows/**491**/star-trek-the-next-generation http://www.tvmaze.com/shows/**492**/star-trek-voyager, 
* As you've realized, we need the TVMaze IDs of these shows, which are **490**, **491** and **492** (and so on).
* Just add these numbers in `series` key in `config.php`

##Listing TV shows as unified
For now, you'll need to run a raw SQL query, since there's no "show group" feature implemented.
Example query that lists and provides a watch order for Doctor Who and Torchwood in MySQL:

```sql
SELECT * FROM `episodes` WHERE serie_id_external IN (210, 659) ORDER BY airdate ASC
```
You will get a result like [this image](http://imgur.com/nW2rn5Z). This will include a unified view of multiple TV shows including special editions, episode names and numbers, summaries, cover photos, episode URLs and air dates (in short, whatever resource TVMaze provides). 

##Screenshot(s)

This is a sample screenshot from console when you run the sync command:
![imgur](http://i.imgur.com/pKf7Uvd.png)
Many of the other images are provided earlier of this readme.

##TODOs
* Grouping feature to bundle multiple TV shows
* Provide an output format
* New columns for marking such as "watched", "collected" etc.
* Please feel free to provide issues and pull requests. I'll gladly consider them.

##License
MIT License