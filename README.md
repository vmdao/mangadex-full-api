# MangaDex Full API
An unofficial [MangaDex](https://www.mangadex.org) API built with the JSON API and web parsing.
No other NPM dependencies.

[![Version](https://img.shields.io/npm/v/mangadex-full-api.svg?style=flat)](https://www.npmjs.com/package/mangadex-full-api)
[![License](https://img.shields.io/github/license/md-y/mangadex-full-api.svg?style=flat)](https://github.com/md-y/mangadex-full-api/blob/master/LICENSE)
[![Downloads](https://img.shields.io/npm/dm/mangadex-full-api.svg?style=flat)](https://www.npmjs.com/package/mangadex-full-api)

```npm install mangadex-full-api```

```javascript

// A Couple of Examples

var manga = new Manga();
manga.fillByQuery("Ancient Magus Bride").then((manga)=>{
    console.log(`${manga.title} by ${manga.authors.join(", ")}`);
});

var group = new Group();
group.fillByQuery("MangaDex Scans").then((group)=>{
    console.log(`${group.title} has uploaded ${group.uploads} chapters and has ${group.followers} followers and ${group.views} views.`);
});

```

# Documentation

[Manga](#Manga) <br>
[Chapter](#Chapter) <br>
[Group](#Group) <br>
[User](#User) <br>
[Thread](#Thread) <br>
[Home](#Home) <br>

## Manga
|Method|Arguments|Return Type|Information|Web, JSON, Both, or Neither?
|-|-|-|-|-
|search()|```Query (String)```|```Array<String>```| (Static) Calls a MangaDex quicksearch and returns an array of manga IDs.|Both
|fill()|```Manga ID (Number)```|```Promise```| Fills an instance of Manga with information from the JSON API and web parsing.<br>Promise returns Manga object.|Both
|fillByQuery()|```Query (String)```|```Promise```| Executes ```search()``` then ```fill()``` with the most relevant manga.<br>Promise returns Manga object.|Both
|getFullURL()|```Property (String)```|```String```| Returns full URL for partial stored url (i.e. ```"id"``` returns ```"https://www.mangadex.org/title/(id)"```)|Neither

|Property|Type|Information
|-|-|-
|id|```Number```| This manga's MangaDex ID
|title|```String```| Manga's main title
|authors|```Array<String>```| All authors for this manga
|artists|```Array<String>```| All artists for this manga
|genres|```Array<Number>```| Array of the manga's genres' IDs.
|genreNames|```Array<String>```| Array of genre names in the same order as their IDs: ```.genres```
|cover|```String```| Partial main cover URL. See ```getFullURL('cover')```
|language|```String```| Original language code (e.g. JP, EN, DE). See ```language.js```
|hentai|```Boolean```| Hentai or not?
|description|```String```| HTML Formated description string
|links|```Array<String>```| Array of full URLs to additional links (e.g. MangaUpdates, MAL, BookWalker). See ```links.js```
|chapters|```Array<Chapter>```| Array of all chapters for this manga. Contains only minimal information like ID and title; use ```Chapter.fill()``` 
|views|```Number```| Amount of manga views
|rating|```Number```| Manga's Bayesian rating
|altTitles|```Array<String>```| Alternate names for this manga.

```javascript

// Example: bin/test-manga-call.js
var [manga, promise] = new Manga(mangaID);
promise.then(()=>{
    console.log(`${manga.title} by ${manga.authors.join(", ")}`);
});

```

## Chapter
|Method|Arguments|Return Type|Information|Web, JSON, Both, or Neither?
|-|-|-|-|-
|fill()|```Chapter ID (Number)```|```Promise```| Fills an instance of Chapter with information from the JSON API and web parsing.<br>Promise returns Chapter object.|JSON
|getFullURL()|```Property (String)```|```String```| Returns full URL for partial stored url (i.e. ```"id"``` returns ```"https://www.mangadex.org/chapter/(id)"```)|Neither

|Property|Type|Information
|-|-|-
|id|```Number```| This chapter's MangaDex ID
|timestamp|```Number```| Unix timestamp of chapter upload
|volume|```Number```| The volume this chapter is from
|chapter|```Number```| The chapter's number
|title|```String```| The chapter's title
|language|```String```| The language code for this chapter's translated language. See ```language.js```
|parentMangaID|```Number```| The ID of the manga this chapter is from
|groups|```Array<Group>```| The groups that translated this chapter
|commentCount|```Number```| Number of comments on this chapter
|longstrip|```Boolean```| Longstrip (e.g. WebToon style) or not?
|pages|```Array<String>```| Array of each page image's URL


```javascript

// Example: bin/test-chapter-call.js
const [chapter, promise] = new Chapter(527948);
promise.then(()=>{
    console.log(`This chapter is in ${language[chapter.language]}`);
}).catch(console.error)

```

## Group
|Method|Arguments|Return Type|Information|Web, JSON, Both, or Neither?
|-|-|-|-|-
|search()|```Query (String)```|```Array<String>```| (Static) Calls a MangaDex quicksearch and returns an array of group IDs.|Web
|fill()|```Group ID (Number)```|```Promise```| Fills an instance of Group with information from web parsing.<br>Promise returns Group object.|Web
|fillByQuery()|```Query (String)```|```Promise```| Executes ```search()``` then ```fill()``` with the most relevant group.<br>Promise returns Group object.|Web
|getFullURL()|```Property (String)```|```String```| Returns full URL for partial stored url (i.e. ```"id"``` returns ```"https://www.mangadex.org/group/(id)"```)|Neither

|Property|Type|Information
|-|-|-
|id|```Number```| This group's MangaDex ID
|title|```String```| The group's official name
|description|```String```| HTML Formated description string
|language|```String```| The language code for this group. See ```language.js```
|views|```Number```| Amount of group views
|followers|```Number```| Amount of group followers
|uploads|```Number```| Amount chapters uploaded by this group
|links|```Object```| Links to the group's website, Discord, IRC, and/or email.
|leader|```User```| The group's leader.
|members|```Array<User>```| All non-leader members of this group.


```javascript

// Example: bin/test-group-call.js
const [chapter, promise] = new Group(2233);
promise.then(()=>{
    console.log(`${group.title} has uploaded ${group.uploads} chapters and has ${group.followers} followers and ${group.views} views.`);
}).catch(console.error)

```

## User
|Method|Arguments|Return Type|Information|Web, JSON, Both, or Neither?
|-|-|-|-|-
|search()|```Query (String)```|```Array<String>```| (Static) Calls a MangaDex quicksearch and returns an array of user IDs.|Web
|fill()|```User ID (Number)```|```Promise```| Fills an instance of User with information from web parsing.<br>Promise returns User object.|Web
|fillByQuery()|```Query (String)```|```Promise```| Executes ```search()``` then ```fill()``` with the most relevant user.<br>Promise returns User object.|Web
|getFullURL()|```Property (String)```|```String```| Returns full URL for partial stored url (i.e. ```"id"``` returns ```"https://www.mangadex.org/user/(id)"```)|Neither

|Property|Type|Information
|-|-|-
|id|```Number```| This user's MangaDex ID
|username|```String```| The user's username
|biography|```String```| HTML Formated biography string
|language|```String```| The language code for this user. See ```language.js```
|views|```Number```| Amount of profile views
|uploads|```Number```| Amount chapters uploaded by this user
|website|```String```| Link to user's website
|avatar|```String```| Link to user's avatar image


```javascript

// Example: bin/test-user-call.js
var user = new User();
user.fillByQuery("mdy").then(()=>{
    console.log(`${user.username} has uploaded ${user.uploads} chapters and has ${user.views} views.`);
}).catch(console.error);

```

## Thread
|Method|Arguments|Return Type|Information|Web, JSON, Both, or Neither?
|-|-|-|-|-
|fill()|```Thread ID (Number), Pages (Number)```|```Promise```| Fills an instance of Thread with information from web parsing for X number of pages.<br>Promise returns Thread object.|Web
|getFullURL()|```Property (String)```|```String```| Returns full URL for partial stored url (i.e. ```"id"``` returns ```"https://www.mangadex.org/thread/(id)"```)|Neither

|Property|Type|Information
|-|-|-
|id|```Number```| This thread's MangaDex ID
|pages|```Number```| The number of pages searched for this thread
|posts|```Array<Post>```| An array of Post objects.

```javascript

// Example: bin/test-thread-call.js
var thread = new Thread();
thread.fill(56429, 2).then(()=>{
    console.log(`${thread.posts[0].author.username}'s original post: ${thread.posts[0].text}`);
});

```

## (Thread) Post
|Property|Type|Information
|-|-|-
|id|```Number```| This post's MangaDex ID
|author|```User```| User with minimal information; use ```User.fill()```.
|text|```String```| The post's text

## Home
|Method|Arguments|Return Type|Information|Web, JSON, Both, or Neither?
|-|-|-|-|-
|fill()|```None```|```Promise```| Fills an instance of Home with information MangaDex's homepage. <br>Promise returns Home object.|Web

|Property|Type|Information
|-|-|-
|newest|```Array<Manga>```| Array of the most recently updated manga
|top6h|```Array<Manga>```| Array of the top manga in the past 6 hours
|top24h|```Array<Manga>```| Array of the top manga in the past 24 hours
|top7d|```Array<Manga>```| Array of the top manga in the week
|topFollows|```Array<Manga>```| Array of the top manga by follows
|topRating|```Array<Manga>```| Array of the top manga by rating

```javascript

// Example: bin/test-home.js
let home = new Home();
home.fill().then(()=>{
    console.log(`${home.topRating[0].title} is the highest rated manga on MangaDex.`);
});

```

# Errors

Service may be shut down during a DDOS attack.