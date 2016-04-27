HOST: http://api.themoviedb.org/3/

--- The Movie Database API ---
---
Below you will find a current list of the available methods on our movie, tv (television), actor and image API. If you need help or support, please head over to our [talk page](https://www.themoviedb.org/talk/category/5047958519c29526b50017d6). To register for an API key, head into your [account page](https://www.themoviedb.org/account) on TMDb and generate a new key from within the "API" section found in the left hand sidebar.

### Useful Links
* [API Support](https://www.themoviedb.org/talk/category/5047958519c29526b50017d6)
* [Google+ Developer Group](http://j.mp/TMDbDevelopers)
* [Pingdom](http://stats.pingdom.com/jatptlnse0fd/520297)

To be notified about our service status and deprecation notices, please join our Google+ community.

###New! TV (Oct. 25, 2013)###
Our TV API has just been released. It's currently out of preview and stable. If you have any questions or comments about this, stop by the [API support forum](https://www.themoviedb.org/talk/category/5047958519c29526b50017d6) and let us know.

### Terms of Use
By using our API you are agreeing to our terms of use. You can read it [here](https://www.themoviedb.org/about/api-terms).

### Third Party Libraries
There are a number of community contributed libraries. You can check them out [here](https://www.themoviedb.org/documentation/api/wrappers-libraries).

### JSON & JSONP Response Format
Unlike previous versions of the API, v3 only supports a single format, JSON. XML and YAML support are being officially dropped. JSONP is also officially supported, just add a `callback` parameter to your request and the response will be encapsulated in the value you specified.

### Request Rate Limiting
We do enforce a small amount of rate limiting. Our current limits are 40 requests every 10 seconds and are limited by IP address, not API key. You can use the 'X-RateLimit' headers that get returned with every request to keep track of your current limits. If you exceed the limit, you will receive a 429 HTTP status with a 'Retry-After' header. As soon your cool down period expires, you are free to continue making requests.

### Status Codes
At different times while using our API you might come across an error and/or status code. For a detailed look at what these codes mean, [check out the list](https://www.themoviedb.org/documentation/api/status-codes) on our website.

### HTTPS / SSL
v3 now supports SSL. Just make an https:// call instead of http:// and you're all set.

### Required Parameters
Every request needs a valid API key and you can set this value as an HTTP parameter. A full request example looks like so:

    https://api.themoviedb.org/3/movie/550?api_key=###

### Languages
Our API supports translations just be aware that we no longer fall back to English in the event that a field hasn't been translated. If you make a request for the German translation and the overview hasn’t been translated, the field will be empty. There’s a few reasons for this but the main one is that in an effort to encourage users to add data to TMDb, it’s important for them to see when the data is missing.

If you want to fall back to English, you can make the separate call yourself and fill in missing data.

### Image Languages
* `poster_path`: The poster_path will query the `language` you specify first and default back to 'en' if one is not found. In the event that an 'en' poster isn't present, we will simply try to grab the highest rated.
* `backdrop_path`: Since 99% of backdrops don't contain a language so to speak, we simply query for the highest rated backdrop that belongs to the given entry.
* `still_path`: Like backdrops, TV episode images don't inherently have languages so to speak. We query for the highest rated.

In the event you query one of the distinct `/image` methods, there is a new way you can query additional languages for images. Think of this as kind of like a fallback. This is especially useful to grab 2 things. First, finding the backdrops and posters in a users specified language but also to grab all of the images that _haven't_ been tagged yet. Here's an example:

    https://api.themoviedb.org/3/movie/550/images?api_key=###&language=en&include_image_language=en,null

Notice the `include_image_language` parameter. We're looking for all images that match English and those that haven't been set (null).

### Appending Responses
The [movie](#movies), [tv](#tv) and [person](#person) methods support a new parameter called `append_to_response`. When this parameter is present, the API will make an additional request behind the scenes to fetch the data you're asking for. A simple example would be if you wanted the default movie, release and trailer data. Until now, you would have to issue three separate requests. Now, you can issue one. Here's what that request looks like:

    https://api.themoviedb.org/3/movie/550?api_key=###&append_to_response=releases,trailers

This request is the same as making these 3 separate requests, all we're doing is combining the responses into a single response for you. With this in mind, remember all parameters and responses will be the same as documented. Each appended request will map to an object in the JSON response with the same name.

You can of course pass the new `include_image_language` param to the images too!

    https://api.themoviedb.org/3/movie/550?api_key=###&append_to_response=images&include_image_language=en,null

### Configuration
In an effort to lower our response sizes and add simplicity to the actual API methods, we are now using a central [configuration](#configuration). The data provided with this method is required for building full image URLs, or getting a list of available image sizes.

### Apiary Transparent Proxy
This is an optional service you can use to help troubleshoot requests. You are not required to use it in any capacity. In real world use, we expect you to use our production end point.
---

--
Configuration
--

**Updated on Dec. 13, 2013**

Get the system wide configuration information. Some elements of the API require some knowledge of this configuration data. The purpose of this is to try and keep the actual API responses as light as possible. It is recommended you cache this data within your application and check for updates every few days.

This method currently holds the data relevant to building image URLs as well as the change key map.

To build an image URL, you will need 3 pieces of data. The `base_url`, `size` and `file_path`. Simply combine them all and you will have a fully qualified URL. Here’s an example URL:

    http://image.tmdb.org/t/p/w500/8uO0gUM8aNqYLs1OsTBQiXu0fEv.jpg

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
</table>

GET /configuration
> Accept: application/json
< 200
< Content-Type: application/json
{
  "images": {
    "base_url": "http://image.tmdb.org/t/p/",
    "secure_base_url": "https://image.tmdb.org/t/p/",
    "backdrop_sizes": [
      "w300",
      "w780",
      "w1280",
      "original"
    ],
    "logo_sizes": [
      "w45",
      "w92",
      "w154",
      "w185",
      "w300",
      "w500",
      "original"
    ],
    "poster_sizes": [
      "w92",
      "w154",
      "w185",
      "w342",
      "w500",
      "w780",
      "original"
    ],
    "profile_sizes": [
      "w45",
      "w185",
      "h632",
      "original"
    ],
    "still_sizes": [
      "w92",
      "w185",
      "w300",
      "original"
    ]
  },
  "change_keys": [
    "adult",
    "also_known_as",
    "alternative_titles",
    "biography",
    "birthday",
    "budget",
    "cast",
    "character_names",
    "crew",
    "deathday",
    "general",
    "genres",
    "homepage",
    "images",
    "imdb_id",
    "name",
    "original_title",
    "overview",
    "plot_keywords",
    "production_companies",
    "production_countries",
    "releases",
    "revenue",
    "runtime",
    "spoken_languages",
    "status",
    "tagline",
    "title",
    "trailers",
    "translations"
  ]
}

--
Account
--

Get the basic information for an account. You will need to have a [valid session id](#authentication).

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
<tr><td style="padding-left: 0;" colspan="2">session_id</td></tr>
</table>

GET /account
> Accept: application/json
< 200
< Content-Type: application/json
{
  "avatar": {
    "gravatar": {
      "hash": "c9e9fc152ee756a200db85757c29815d"
    }
  },
  "id": 36,
  "iso_639_1": "en",
  "iso_3166_1": "CA",
  "name": "John Doe",
  "include_adult": false,
  "username": "johndoe"
}

Get the lists that you have created and marked as a favorite.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td colspan="2">api_key</td></tr>
<tr><td colspan="2">session_id</td></tr>
<tr><td colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-right: 40px; width: 140px;">page</td><td>Minimum 1, maximum 1000.</td></tr>
<tr><td style="padding-right: 40px; width: 140px;">language</td><td>ISO 639-1 code. Without one set, we default to the language set in the requested account.</td></tr>
</table>

GET /account/{id}/lists
> Accept: application/json
< 200
< Content-Type: application/json
{
    "page": 1,
    "results": [
        {
            "description": "A list of \"frightful flicks waiting to be rediscovered\" -- compiled by the self-described leading name in the world of horror: Fangoria magazine.",
            "favorite_count": 2,
            "id": "50a6724919c295084b000028",
            "item_count": 101,
            "iso_639_1": "en",
            "list_type": "movie",
            "name": "Fangoria's 101 Best Horror Movies You've Never Seen",
            "poster_path": "/3sL73Vy4kkrH4P61RA0n6UyYGlQ.jpg"
        },
        {
            "description": "Part of the AFI 100 Years… series, AFI's 100 Years…100 Thrills is a list of the top 100 heart-pounding movies in American cinema. The list was unveiled by the American Film Institute on June 12, 2001, during a CBS special hosted by Harrison Ford.",
            "favorite_count": 2,
            "id": "50a54d01760ee32e1400000b",
            "item_count": 100,
            "iso_639_1": "en",
            "list_type": "movie",
            "name": "AFI's 100 Most Thrilling American Films",
            "poster_path": "/tayO4gCLyEo8Uul8FHkJGy3Kppb.jpg"
        },
        {
            "description": "A list of films that earned its actresses the Razzie Award for Worst Actress of the year.",
            "favorite_count": 2,
            "id": "509fca90760ee3490f000a12",
            "item_count": 40,
            "iso_639_1": "en",
            "list_type": "movie",
            "name": "Films featuring a Razzie winning Worst Actress Performance",
            "poster_path": "/eL61XP1l8H0qrBPJoKqhXdxumkw.jpg"
        },
        {
            "description": "A list of all films in the Star Wars universe including the theatrical motion picture releases.",
            "favorite_count": 2,
            "id": "509fc02119c295114a0007cb",
            "item_count": 14,
            "iso_639_1": "en",
            "list_type": "movie",
            "name": "Star Wars Expanded Universe",
            "poster_path": "/rf6uEcD5V8zs4J7Huo5L9sYgSki.jpg"
        },
        {
            "description": "Here's my list of best picture winners for the Oscars. Thought it would be neat to see them all together. There's a lot of movies here I have never even heard of.",
            "favorite_count": 1,
            "id": "509ec17c19c2950a0600050e",
            "item_count": 84,
            "iso_639_1": "en",
            "list_type": "movie",
            "name": "Best Picture Winners - The Academy Awards",
            "poster_path": "/efBm2Nm2v5kQnO0w3hYcW6hVsJU.jpg"
        },
        {
            "description": "A list of the films that were nominated at the 2008 Oscars for best picture.",
            "favorite_count": 1,
            "id": "509ec007760ee36f0c000917",
            "item_count": 5,
            "iso_639_1": "en",
            "list_type": "movie",
            "name": "80th Academy Awards (2008) - Best Picture Nominations",
            "poster_path": "/nWwFabqFaBbukXdkUCmtnrFxPNh.jpg"
        },
        {
            "description": "The View Askewniverse is a fictional universe created by writer/director Kevin Smith, featured in several films, comics and a television series; it is named for Smith's production company, View Askew Productions.",
            "favorite_count": 2,
            "id": "509691b119c29532730008fb",
            "item_count": 6,
            "iso_639_1": "en",
            "list_type": "movie",
            "name": "The View Askewniverse",
            "poster_path": "/tGmU66LL8edoRm1RO7sbrz1xuhr.jpg"
        },
        {
            "description": "Name pretty much says it all, here's the top 50 grossing films of all time.",
            "favorite_count": 2,
            "id": "50957b1419c295013800023e",
            "item_count": 50,
            "iso_639_1": "en",
            "list_type": "movie",
            "name": "Top 50 Grossing Films of All Time (Worldwide)",
            "poster_path": "/sRbZeVtRKIWybTOVpCRPZtzW5bd.jpg"
        },
        {
            "description": "A list of the films that were nominated at the 2009 Oscars for best picture.",
            "favorite_count": 2,
            "id": "50957064760ee3698a001fc1",
            "item_count": 5,
            "iso_639_1": "en",
            "list_type": "movie",
            "name": "81st Academy Awards (2009) - Best Picture Nominations",
            "poster_path": "/gShtwIvdSHgCEQRZfymNkEUEwFm.jpg"
        },
        {
            "description": "A list of the films that were nominated at the 2010 Oscars for best picture.",
            "favorite_count": 2,
            "id": "50956fd2760ee3698a001fb1",
            "item_count": 10,
            "iso_639_1": "en",
            "list_type": "movie",
            "name": "82nd Academy Awards (2010) - Best Picture Nominations",
            "poster_path": "/8iwe0iP49A6Gqcv31jBleZDZqI4.jpg"
        },
        {
            "description": "A list of the theatrical releases for Marvel's Avengers characters that are in the latest Avengers movie.",
            "favorite_count": 3,
            "id": "50953114760ee34b91000293",
            "item_count": 7,
            "iso_639_1": "en",
            "list_type": "movie",
            "name": "The Avengers",
            "poster_path": "/2i89ujVkhJebuoB2zy6QZcwbr2N.jpg"
        },
        {
            "description": "A list of the films that were nominated at the 2011 Oscars for best picture.",
            "favorite_count": 1,
            "id": "509431ad19c2950b01000004",
            "item_count": 10,
            "iso_639_1": "en",
            "list_type": "movie",
            "name": "83rd Academy Awards (2011) - Best Picture Nominations",
            "poster_path": "/iAHDZPyLD3NW7GuNV6U1TVtb0hN.jpg"
        },
        {
            "description": "Here's a list of the films that take place in the DC Comics universe.",
            "favorite_count": 2,
            "id": "5094147819c2955e4c00006b",
            "item_count": 22,
            "iso_639_1": "en",
            "list_type": "movie",
            "name": "The DC Comics Universe",
            "poster_path": "/4H1jWsgEQOgTs4KeG5j5BorKMfX.jpg"
        },
        {
            "description": "A list of the films that were nominated at the 2012 Oscars for best picture.",
            "favorite_count": 1,
            "id": "50941340760ee35da9000054",
            "item_count": 9,
            "iso_639_1": "en",
            "list_type": "movie",
            "name": "84th Academy Awards (2012) - Best Picture Nominations",
            "poster_path": "/zRqBleU93WncYnIwt8LAanQerZ7.jpg"
        },
        {
            "description": "The idea behind this list is to collect the live action comic book movies from within the Marvel franchise.",
            "favorite_count": 5,
            "id": "50941077760ee35e1500000d",
            "item_count": 33,
            "iso_639_1": "en",
            "list_type": "movie",
            "name": "The Marvel Universe",
            "poster_path": "/w2JbuNpBnfevl0yCcYEm6vNFWdi.jpg"
        }
    ],
    "total_pages": 1,
    "total_results": 15
}

Get the list of favorite movies for an account.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td colspan="2">api_key</td></tr>
<tr><td colspan="2">session_id</td></tr>
<tr><td colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-right: 40px; width: 140px;">page</td><td>Minimum 1, maximum 1000.</td></tr>
<tr><td style="padding-right: 40px; width: 140px;">sort_by</td><td>Only 'created_at` is currently supported.</td></tr>
<tr><td style="padding-right: 40px; width: 140px;">sort_by</td><td>`created_at.desc` or `created_at.asc`</td></tr>
<tr><td style="padding-right: 40px; width: 140px;">language</td><td>ISO 639-1 code. Without one set, we default to the language set in the requested account.</td></tr>
</table>

GET /account/{id}/favorite/movies
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "2daa7f981185567083364f740f4dde6b"
{
    "page": 1,
    "results": [
        {
            "backdrop_path": "/wrKcSXtHz4LedgEf1aW5e4ZVyHJ.jpg",
            "id": 9806,
            "original_title": "The Incredibles",
            "release_date": "2004-11-05",
            "poster_path": "/9k4sgKD79q0MDHSWIqNnHqOfOEV.jpg",
            "title": "The Incredibles",
            "vote_average": 8.5999999999999996,
            "vote_count": 46
        },
        {
            "backdrop_path": "/uAVdwu4vEx0TwuC4ewp9RqplKEV.jpg",
            "id": 1452,
            "original_title": "Superman Returns",
            "release_date": "2006-06-21",
            "poster_path": "/qpsarVxFcxJxtCK5dAoHLt3365R.jpg",
            "title": "Superman Returns",
            "vote_average": 6.5999999999999996,
            "vote_count": 20
        },
        {
            "backdrop_path": "/x9a4Q6Qlgh9Fk1f5z6K4r5OeLd9.jpg",
            "id": 8960,
            "original_title": "Hancock",
            "release_date": "2008-07-02",
            "poster_path": "/dsCxSr4w3g2ylhlZg3v5CB5Pid7.jpg",
            "title": "Hancock",
            "vote_average": 7.0,
            "vote_count": 34
        },
        {
            "backdrop_path": "/oPE44KcuRBKfuE15RD8sC2awh3P.jpg",
            "id": 9741,
            "original_title": "Unbreakable",
            "release_date": "2000-11-14",
            "poster_path": "/ndb1jugavBmgcfZKrfHXxC0T6al.jpg",
            "title": "Unbreakable",
            "vote_average": 7.7999999999999998,
            "vote_count": 25
        },
        {
            "backdrop_path": "/7u3pxc0K1wx32IleAkLv78MKgrw.jpg",
            "id": 603,
            "original_title": "The Matrix",
            "release_date": "1999-03-31",
            "poster_path": "/gynBNzwyaHKtXqlEKKLioNkjKgN.jpg",
            "title": "The Matrix",
            "vote_average": 9.0999999999999996,
            "vote_count": 249
        },
        {
            "backdrop_path": "/af98P1bc7lJsFjhHOVWXQgNNgSQ.jpg",
            "id": 424,
            "original_title": "Schindler's List",
            "release_date": "1993-11-30",
            "poster_path": "/I2lJ7ens7Rzlm3x5HfDbwF3ykh.jpg",
            "title": "Schindler's List",
            "vote_average": 8.6999999999999993,
            "vote_count": 41
        },
        {
            "backdrop_path": "/6kVVfNT0auG6fU5SFQ1zbayNWUC.jpg",
            "id": 7191,
            "original_title": "Cloverfield",
            "release_date": "2008-01-16",
            "poster_path": "/as01o40tJ2FhtheqeXf7bVZ0EQO.jpg",
            "title": "Cloverfield",
            "vote_average": 7.2000000000000002,
            "vote_count": 35
        },
        {
            "backdrop_path": "/6yFoLNQgFdVbA8TZMdfgVpszOla.jpg",
            "id": 218,
            "original_title": "The Terminator",
            "release_date": "1984-10-26",
            "poster_path": "/q8ffBuxQlYOHrvPniLgCbmKK4Lv.jpg",
            "title": "The Terminator",
            "vote_average": 8.5999999999999996,
            "vote_count": 67
        },
        {
            "backdrop_path": "/8uO0gUM8aNqYLs1OsTBQiXu0fEv.jpg",
            "id": 550,
            "original_title": "Fight Club",
            "release_date": "1999-10-15",
            "poster_path": "/2lECpi35Hnbpa4y46JX0aY3AWTy.jpg",
            "title": "Fight Club",
            "vote_average": 9.0999999999999996,
            "vote_count": 175
        },
        {
            "backdrop_path": "/siKq2bQo09YGLTiqvKR1pXJ5li9.jpg",
            "id": 23483,
            "original_title": "Kick-Ass",
            "release_date": "2010-04-16",
            "poster_path": "/gtfggr5n3ED1neoTqVYmsVCoSS.jpg",
            "title": "Kick-Ass",
            "vote_average": 8.3000000000000007,
            "vote_count": 108
        },
        {
            "backdrop_path": "/419GfpzSip2cXf17x8FKIRHSSPK.jpg",
            "id": 679,
            "original_title": "Aliens",
            "release_date": "1986-07-18",
            "poster_path": "/qet8RDq99dRteBfSRfGeZeqY9xe.jpg",
            "title": "Aliens",
            "vote_average": 8.8000000000000007,
            "vote_count": 107
        },
        {
            "backdrop_path": "/sCtmAY3DgrFOjlrDh6mP7luUO7t.jpg",
            "id": 489,
            "original_title": "Good Will Hunting",
            "release_date": "1997-12-05",
            "poster_path": "/3yHtbRfp32AouPSwYESoJM3cOeQ.jpg",
            "title": "Good Will Hunting",
            "vote_average": 8.8000000000000007,
            "vote_count": 32
        },
        {
            "backdrop_path": "/5YF6MwuuKBRKLUE2dz3wetkgxAE.jpg",
            "id": 453,
            "original_title": "A Beautiful Mind",
            "release_date": "2001-12-13",
            "poster_path": "/79L8f0qbawvmdXE4ThVstC5sOxP.jpg",
            "title": "A Beautiful Mind",
            "vote_average": 8.5,
            "vote_count": 28
        },
        {
            "backdrop_path": "/bYtrTSGDqvd2KlHTO78b4Y3S4E9.jpg",
            "id": 807,
            "original_title": "Se7en",
            "release_date": "1995-09-22",
            "poster_path": "/zgB9CCTDlXRv50Z70ZI4elJtNEk.jpg",
            "title": "Se7en",
            "vote_average": 8.9000000000000004,
            "vote_count": 96
        },
        {
            "backdrop_path": "/6HbOqP1GUElNLdX8Bx2uP542CIP.jpg",
            "id": 180,
            "original_title": "Minority Report",
            "release_date": "2002-06-21",
            "poster_path": "/h3lpltSn7Rj1eYTPQO1lYGdw4Bz.jpg",
            "title": "Minority Report",
            "vote_average": 8.3000000000000007,
            "vote_count": 34
        },
        {
            "backdrop_path": "/i9A0UMFg1hI2kLyCCwnmSbpT2cd.jpg",
            "id": 73,
            "original_title": "American History X",
            "release_date": "1998-10-30",
            "poster_path": "/fXepRAYOx1qC3wju7XdDGx60775.jpg",
            "title": "American History X",
            "vote_average": 8.6999999999999993,
            "vote_count": 61
        },
        {
            "backdrop_path": "/5XPPB44RQGfkBrbJxmtdndKz05n.jpg",
            "id": 19995,
            "original_title": "Avatar",
            "release_date": "2009-12-18",
            "poster_path": "/s66l83fTQp0FAJsJqY0xUmkamcx.jpg",
            "title": "Avatar",
            "vote_average": 7.9000000000000004,
            "vote_count": 516
        },
        {
            "backdrop_path": "/e1uECwkwbD5Zpm9Gc6eb5mbG4C4.jpg",
            "id": 38,
            "original_title": "Eternal Sunshine of the Spotless Mind",
            "release_date": "2004-03-19",
            "poster_path": "/gutwXkVChJkpIrclokNESybj0GC.jpg",
            "title": "Eternal Sunshine of the Spotless Mind",
            "vote_average": 8.6999999999999993,
            "vote_count": 57
        },
        {
            "backdrop_path": "/tv8J6uCTKsTlASa8luJqrFiJlBX.jpg",
            "id": 78,
            "original_title": "Blade Runner",
            "release_date": "1982-06-25",
            "poster_path": "/ewq1lwhOT8GqBcQH1w8D1BagwTh.jpg",
            "title": "Blade Runner",
            "vote_average": 9.0,
            "vote_count": 147
        },
        {
            "backdrop_path": "/2nFyTvssbtJMLC6eyYwwZ88gALD.jpg",
            "id": 10681,
            "original_title": "WALL·E",
            "release_date": "2008-06-27",
            "poster_path": "/9cJETuLMc6R0bTWRA5i7ctY9bxk.jpg",
            "title": "WALL·E",
            "vote_average": 8.6999999999999993,
            "vote_count": 164
        }
    ],
    "total_pages": 2,
    "total_results": 34
}

Get the list of favorite TV series for an account.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td colspan="2">api_key</td></tr>
<tr><td colspan="2">session_id</td></tr>
<tr><td colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-right: 40px; width: 140px;">page</td><td>Minimum 1, maximum 1000.</td></tr>
<tr><td style="padding-right: 40px; width: 140px;">sort_by</td><td>Only 'created_at` is currently supported.</td></tr>
<tr><td style="padding-right: 40px; width: 140px;">sort_by</td><td>`created_at.desc` or `created_at.asc`</td></tr>
<tr><td style="padding-right: 40px; width: 140px;">language</td><td>ISO 639-1 code. Without one set, we default to the language set in the requested account.</td></tr>
</table>

GET /account/{id}/favorite/tv
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "e802d6f19bdd56802327ebb1d47f1789"
{
  "page": 1,
  "results": [
    {
      "backdrop_path": "/4EFVV3ZsIRFGxvHJXGyTQLzThQK.jpg",
      "id": 46648,
      "original_name": "True Detective",
      "first_air_date": "2014-01-12",
      "poster_path": "/udKOK1kE3wJzpjrCfhmuMC6M7lC.jpg",
      "popularity": 1.26877805083058,
      "name": "True Detective",
      "vote_average": 9.8,
      "vote_count": 12
    },
    {
      "backdrop_path": "/5MGbVKfJO96fTHLKgAPjZIB9kgi.jpg",
      "id": 5639,
      "original_name": "Pushing Daisies",
      "first_air_date": "2007-10-03",
      "poster_path": "/aUV2ujhVT7cQHu4IlwnCBN9OQvN.jpg",
      "popularity": 0.41013242497006,
      "name": "Pushing Daisies",
      "vote_average": 9.0,
      "vote_count": 1
    },
    {
      "backdrop_path": "/8wxxyouV0QImpfNVyyp86ksOlCY.jpg",
      "id": 45,
      "original_name": "Top Gear",
      "first_air_date": "2002-10-20",
      "poster_path": "/dyIZi3PBN1nyLgx7VHUvvo1e1VG.jpg",
      "popularity": 1.90200135909042,
      "name": "Top Gear",
      "vote_average": 7.7,
      "vote_count": 5
    },
    {
      "backdrop_path": "/5CSqbAncWlzkPz2Qxvbu3vLJLqC.jpg",
      "id": 1427,
      "original_name": "Broadchurch",
      "first_air_date": "2013-03-04",
      "poster_path": "/yYg4z9XrjRbNROc2sVG4H2uq4Nl.jpg",
      "popularity": 0.417,
      "name": "Broadchurch",
      "vote_average": 9.0,
      "vote_count": 3
    },
    {
      "backdrop_path": "/3NFHfvdQhx7TP0JwQeVz2CruwaT.jpg",
      "id": 18202,
      "original_name": "Cougar Town",
      "first_air_date": "2009-09-23",
      "poster_path": "/3abzPQeu5RGK14dnYZJY5LGNQvQ.jpg",
      "popularity": 0.59198331,
      "name": "Cougar Town",
      "vote_average": 8.0,
      "vote_count": 2
    },
    {
      "backdrop_path": "/y6mTnYRk5BFTmgppVFYGjmNGBxG.jpg",
      "id": 8592,
      "original_name": "Parks and Recreation",
      "first_air_date": "2009-04-09",
      "poster_path": "/xt4FRS94639HNRf2IUCqdPRxNuD.jpg",
      "popularity": 0.972197579837567,
      "name": "Parks and Recreation",
      "vote_average": 7.8,
      "vote_count": 2
    },
    {
      "backdrop_path": "/giOapCfAphYifUmV9lfGoAJXPKo.jpg",
      "id": 2302,
      "original_name": "Huff",
      "first_air_date": "2004-11-07",
      "poster_path": "/yUUvwsDehZhI6BbghwpyRT51e6R.jpg",
      "popularity": 0.2,
      "name": "Huff",
      "vote_average": 8.0,
      "vote_count": 1
    },
    {
      "backdrop_path": "/eLiqATmcigKP8K2BhjLdELmEqsy.jpg",
      "id": 2316,
      "original_name": "The Office",
      "first_air_date": "2005-03-24",
      "poster_path": "/biU7kCVB5dQT5zTlSyqwg8cxzyL.jpg",
      "popularity": 3.02392009876581,
      "name": "The Office",
      "vote_average": 7.9,
      "vote_count": 8
    },
    {
      "backdrop_path": "/b6vgobTX9zI833OrwoN2gd880wf.jpg",
      "id": 4556,
      "original_name": "Scrubs",
      "first_air_date": "2001-10-02",
      "poster_path": "/jnoPEbDmNrttiEZRQ0ewSbCZ6oh.jpg",
      "popularity": 1.1361269821995,
      "name": "Scrubs",
      "vote_average": 8.2,
      "vote_count": 3
    },
    {
      "backdrop_path": "/qZsbe82zgPLUXPU3vQ3kfqC6Fmy.jpg",
      "id": 185,
      "original_name": "Carnivàle",
      "first_air_date": "2003-09-07",
      "poster_path": "/ur8kXMzoxc9XdUF3UW8AcXqEfhT.jpg",
      "popularity": 2.24562561259654,
      "name": "Carnivàle",
      "vote_average": 9.0,
      "vote_count": 1
    },
    {
      "backdrop_path": "/aUDXk7DNvbmsqPrJnLnG6MVi9Bv.jpg",
      "id": 1432,
      "original_name": "Veronica Mars",
      "first_air_date": "2004-09-22",
      "poster_path": "/ewG884trMtmm8FrEAxOKVQDd8cD.jpg",
      "popularity": 1.77021010065962,
      "name": "Veronica Mars",
      "vote_average": 8.9,
      "vote_count": 5
    },
    {
      "backdrop_path": "/92ZhAYYce2KfuLgBswzW5HCMKxr.jpg",
      "id": 1425,
      "original_name": "House of Cards",
      "first_air_date": "2013-01-31",
      "poster_path": "/u1MZH02xknaNZp54tyM66afeeKA.jpg",
      "popularity": 1.16861651665156,
      "name": "House of Cards",
      "vote_average": 9.4,
      "vote_count": 8
    },
    {
      "backdrop_path": "/2IslvhtVT8QIbMaTBKfFyinaQR.jpg",
      "id": 1407,
      "original_name": "Homeland",
      "first_air_date": "2011-10-02",
      "poster_path": "/iQXxNET0AY07ulvZyhrrKlhAjXI.jpg",
      "popularity": 0.463679951115582,
      "name": "Homeland",
      "vote_average": 8.1,
      "vote_count": 8
    },
    {
      "backdrop_path": "/nJ5IqCgofd8JjzgS47w2BqVSskI.jpg",
      "id": 34356,
      "original_name": "Tron: Uprising",
      "first_air_date": "2012-05-18",
      "poster_path": "/jjRKi3Ym5CrZzKbPEDswLa89R6F.jpg",
      "popularity": 0.42551056937,
      "name": "Tron: Uprising",
      "vote_average": 9.0,
      "vote_count": 1
    },
    {
      "backdrop_path": "/tvcfvV1LrI0pooEP79BetX8Cscd.jpg",
      "id": 37680,
      "original_name": "Suits",
      "first_air_date": "2011-06-23",
      "poster_path": "/9mRRbi2akSRqtwJx6EyCfo3gaky.jpg",
      "popularity": 0.840117261105396,
      "name": "Suits",
      "vote_average": 8.2,
      "vote_count": 5
    },
    {
      "backdrop_path": "/fPXruJVwikatP3FHsKtF6FkJkqw.jpg",
      "id": 1274,
      "original_name": "Six Feet Under",
      "first_air_date": "2001-06-03",
      "poster_path": "/dX8FW45Vf6U7wDgC1xPSR3I6h5D.jpg",
      "popularity": 4.07304950072839,
      "name": "Six Feet Under",
      "vote_average": 8.6,
      "vote_count": 6
    },
    {
      "backdrop_path": "/2mumk8ef26SWwpNNh0d6aPkPVth.jpg",
      "id": 1423,
      "original_name": "Ray Donovan",
      "first_air_date": "2013-06-30",
      "poster_path": "/ea1ynSNCEKuFlZY2C195CbDSis5.jpg",
      "popularity": 0.374,
      "name": "Ray Donovan",
      "vote_average": 8.0,
      "vote_count": 1
    },
    {
      "backdrop_path": "/2a6yRUuHv1DLFP31e0IKzHL1qhV.jpg",
      "id": 1419,
      "original_name": "Castle",
      "first_air_date": "2009-03-08",
      "poster_path": "/zBPHFtJ1Vl0yhQEgTdxIswcGzzV.jpg",
      "popularity": 2.86763453706342,
      "name": "Castle",
      "vote_average": 9.0,
      "vote_count": 17
    },
    {
      "backdrop_path": "/owlVhh6PhMcCbVWeQznVdLAKRLy.jpg",
      "id": 49010,
      "original_name": "The Fall",
      "first_air_date": "2013-05-13",
      "poster_path": "/8jqMTpdbHpPAViibEgzPfvHKZP8.jpg",
      "popularity": 0.4,
      "name": "The Fall",
      "vote_average": 9.0,
      "vote_count": 2
    },
    {
      "backdrop_path": "/fw0SJJuBzQJKbxSPaIbwm3SzCzo.jpg",
      "id": 46619,
      "original_name": "Rectify",
      "first_air_date": "2013-04-22",
      "poster_path": "/pgNhMwj3ggL3Ilci6Vmm0dIKTu3.jpg",
      "popularity": 0.26,
      "name": "Rectify",
      "vote_average": 9.0,
      "vote_count": 1
    }
  ],
  "total_pages": 3,
  "total_results": 46
}

Add or remove a movie to an accounts favorite list.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
<tr><td style="padding-left: 0;" colspan="2">session_id</td></tr>
<tr><td style="padding-left: 0;" colspan="2"><strong>Required JSON Body</strong></td></tr>
<tr><td style="padding-left: 0; padding-right: 40px; width: 140px;">media_type</td><td>movie | tv</td></tr>
<tr><td style="padding-left: 0; padding-right: 40px; width: 140px;">media_id</td><td>Integer</td></tr>
<tr><td style="padding-left: 0; padding-right: 40px; width: 140px;">favorite</td><td>true | false</td></tr>
</table>

POST /account/{id}/favorite
> Accept: application/json
> Content-Type: application/json
{
    "media_type": "movie",
    "media_id": 550,
    "favorite": true
}
< 200
< Content-Type: application/json
{
    "status_code": 12,
    "status_message": "The item/record was updated successfully"
}

Get the list of rated movies (and associated rating) for an account.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td colspan="2">api_key</td></tr>
<tr><td colspan="2">session_id</td></tr>
<tr><td colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-right: 40px; width: 140px;">page</td><td>Minimum 1, maximum 1000.</td></tr>
<tr><td style="padding-right: 40px; width: 140px;">sort_by</td><td>`created_at.desc` or `created_at.asc`</td></tr>
<tr><td style="padding-right: 40px; width: 140px;">language</td><td>ISO 639-1 code. Without one set, we default to the language set in the requested account.</td></tr>
</table>

GET /account/{id}/rated/movies
> Accept: application/json
< 200
< Content-Type: application/json
{
    "page": 1,
    "results": [
        {
            "backdrop_path": "/tjMZ4RBLhvk670gYJPxIN8SBfnB.jpg",
            "id": 13,
            "original_title": "Forrest Gump",
            "release_date": "1994-06-23",
            "poster_path": "/iZvCkb34CAmV9BETIrHY4yiS115.jpg",
            "rating": 8.0,
            "title": "Forrest Gump",
            "vote_average": 8.9000000000000004,
            "vote_count": 84
        },
        {
            "backdrop_path": "/e1uECwkwbD5Zpm9Gc6eb5mbG4C4.jpg",
            "id": 38,
            "original_title": "Eternal Sunshine of the Spotless Mind",
            "release_date": "2004-03-19",
            "poster_path": "/gutwXkVChJkpIrclokNESybj0GC.jpg",
            "rating": 9.0,
            "title": "Eternal Sunshine of the Spotless Mind",
            "vote_average": 8.6999999999999993,
            "vote_count": 57
        },
        {
            "backdrop_path": "/gTFGr8q5YzYYiRyDYaEMNB6QRuL.jpg",
            "id": 106,
            "original_title": "Predator",
            "release_date": "1987-06-12",
            "poster_path": "/gUpto7r2XwoM5eW7MUvd8hl1etB.jpg",
            "rating": 6.0,
            "title": "Predator",
            "vote_average": 8.4000000000000004,
            "vote_count": 49
        },
        {
            "backdrop_path": "/cNLZ7YGRikb4IsLblrzu86ndZPw.jpg",
            "id": 107,
            "original_title": "Snatch",
            "release_date": "2000-12-06",
            "poster_path": "/on9JlbGEccLsYkjeEph2Whm1DIp.jpg",
            "rating": 8.0,
            "title": "Snatch",
            "vote_average": 8.5999999999999996,
            "vote_count": 55
        },
        {
            "backdrop_path": "/pIUvQ9Ed35wlWhY2oU6OmwEsmzG.jpg",
            "id": 120,
            "original_title": "The Lord of the Rings: The Fellowship of the Ring",
            "release_date": "2001-12-19",
            "poster_path": "/9HG6pINW1KoFTAKY3LdybkoOKAm.jpg",
            "rating": 8.0,
            "title": "The Lord of the Rings: The Fellowship of the Ring",
            "vote_average": 8.8000000000000007,
            "vote_count": 125
        },
        {
            "backdrop_path": "/nnMC0BM6XbjIIrT4miYmMtPGcQV.jpg",
            "id": 155,
            "original_title": "The Dark Knight",
            "release_date": "2008-07-18",
            "poster_path": "/1hRoyzDtpgMU7Dz4JF22RANzQO7.jpg",
            "rating": 9.0,
            "title": "The Dark Knight",
            "vote_average": 8.8000000000000007,
            "vote_count": 269
        },
        {
            "backdrop_path": "/uv5BRIaJKfcPBPEBV8qfTjD6PuY.jpg",
            "id": 12,
            "original_title": "Finding Nemo",
            "release_date": "2003-05-30",
            "poster_path": "/zjqInUwldOBa0q07fOyohYCWxWX.jpg",
            "rating": 8.0,
            "title": "Finding Nemo",
            "vote_average": 8.6999999999999993,
            "vote_count": 41
        },
        {
            "backdrop_path": "/kriCxQiDhdosxzYXEf5JApMdmyq.jpg",
            "id": 187,
            "original_title": "Sin City",
            "release_date": "2005-04-01",
            "poster_path": "/eCJkepVJslq1nEtqURLaC1zLPAL.jpg",
            "rating": 8.0,
            "title": "Sin City",
            "vote_average": 8.4000000000000004,
            "vote_count": 64
        },
        {
            "backdrop_path": "/6xKCYgH16UuwEGAyroLU6p8HLIn.jpg",
            "id": 238,
            "original_title": "The Godfather",
            "release_date": "1972-03-24",
            "poster_path": "/d4KNaTrltq6bpkFS01pYtyXa09m.jpg",
            "rating": 9.0,
            "title": "The Godfather",
            "vote_average": 9.1999999999999993,
            "vote_count": 125
        },
        {
            "backdrop_path": "/iX99sWgGZCnjPpDgG29E0HomMLI.jpg",
            "id": 275,
            "original_title": "Fargo",
            "release_date": "1996-03-15",
            "poster_path": "/gO0Q7TMuu8PJ9Y736VTgvY02rVD.jpg",
            "rating": 8.0,
            "title": "Fargo",
            "vote_average": 8.1999999999999993,
            "vote_count": 29
        },
        {
            "backdrop_path": "/d9AqtruwS8nljKjL5aYzM42hQJr.jpg",
            "id": 280,
            "original_title": "Terminator 2: Judgment Day",
            "release_date": "1991-07-01",
            "poster_path": "/2y4dmgWYRMYXdD1UyJVcn2HSd1D.jpg",
            "rating": 8.0,
            "title": "Terminator 2: Judgment Day",
            "vote_average": 9.0999999999999996,
            "vote_count": 106
        },
        {
            "backdrop_path": "/xRqsFFteJrvLHxswk78NzU7zOrU.jpg",
            "id": 289,
            "original_title": "Casablanca",
            "release_date": "1942-11-26",
            "poster_path": "/sm1QVZu5RKe1vXVHZooo4SZyHMx.jpg",
            "rating": 8.0,
            "title": "Casablanca",
            "vote_average": 8.5,
            "vote_count": 27
        },
        {
            "backdrop_path": "/heNbD1JojBNz6kfHWr3O8Uk0a3a.jpg",
            "id": 296,
            "original_title": "Terminator 3: Rise of the Machines",
            "release_date": "2003-06-30",
            "poster_path": "/lz4xYdF1n09lyiCfZWtWT44SZiG.jpg",
            "rating": 7.0,
            "title": "Terminator 3: Rise of the Machines",
            "vote_average": 6.7000000000000002,
            "vote_count": 44
        },
        {
            "backdrop_path": "/wH8vtnsmU4olFJ5wSRHo8RXRhKF.jpg",
            "id": 310,
            "original_title": "Bruce Almighty",
            "release_date": "2003-05-23",
            "poster_path": "/oHA5eAeJh7ChPsHnSsppNx4ta6L.jpg",
            "rating": 8.0,
            "title": "Bruce Almighty",
            "vote_average": 7.7000000000000002,
            "vote_count": 25
        },
        {
            "backdrop_path": "/tUGSLoMcxNWZ0OIRSqDaB2jNbdE.jpg",
            "id": 503,
            "original_title": "Poseidon",
            "release_date": "2006-05-12",
            "poster_path": "/kHrdehzlhyyb190D3RvH8mdX1y2.jpg",
            "rating": 6.0,
            "title": "Poseidon",
            "vote_average": 6.2000000000000002,
            "vote_count": 9
        },
        {
            "backdrop_path": "/tBCOpjphzSLzC6oOjqzTEg6bJ7q.jpg",
            "id": 534,
            "original_title": "Terminator Salvation",
            "release_date": "2009-05-21",
            "poster_path": "/hxDfhavtxA2Ayx7O9BsQMcZRdG0.jpg",
            "rating": 6.0,
            "title": "Terminator Salvation",
            "vote_average": 7.0,
            "vote_count": 57
        },
        {
            "backdrop_path": "/6TlMTnjtBcXFq7rSCcdQoBjTNPd.jpg",
            "id": 571,
            "original_title": "The Birds",
            "release_date": "1963-03-28",
            "poster_path": "/8tGRhGAbIZE97bnUAWrYSdqq71t.jpg",
            "rating": 6.0,
            "title": "The Birds",
            "vote_average": 8.0999999999999996,
            "vote_count": 6
        },
        {
            "backdrop_path": "/7u3pxc0K1wx32IleAkLv78MKgrw.jpg",
            "id": 603,
            "original_title": "The Matrix",
            "release_date": "1999-03-31",
            "poster_path": "/gynBNzwyaHKtXqlEKKLioNkjKgN.jpg",
            "rating": 9.0,
            "title": "The Matrix",
            "vote_average": 9.0999999999999996,
            "vote_count": 249
        },
        {
            "backdrop_path": "/jB9nQa6zJIZQ0htH5827Og7Mfsw.jpg",
            "id": 615,
            "original_title": "The Passion of the Christ",
            "release_date": "2004-02-25",
            "poster_path": "/6KyvP5bDmwTYdLLnhEn10NFPDIZ.jpg",
            "rating": 6.0,
            "title": "The Passion of the Christ",
            "vote_average": 7.5,
            "vote_count": 15
        },
        {
            "backdrop_path": "/nO0GtGFQmEuuOyAQVpHjJlWBU8O.jpg",
            "id": 616,
            "original_title": "The Last Samurai",
            "release_date": "2003-12-05",
            "poster_path": "/cRz4FRx731ulws6zHuQVaDXpx73.jpg",
            "rating": 8.0,
            "title": "The Last Samurai",
            "vote_average": 8.0999999999999996,
            "vote_count": 26
        }
    ],
    "total_pages": 12,
    "total_results": 239
}

Get the list of rated TV shows (and associated rating) for an account.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td colspan="2">api_key</td></tr>
<tr><td colspan="2">session_id</td></tr>
<tr><td colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-right: 40px; width: 140px;">page</td><td>Minimum 1, maximum 1000.</td></tr>
<tr><td style="padding-right: 40px; width: 140px;">sort_by</td><td>`created_at.desc` or `created_at.asc`</td></tr>
<tr><td style="padding-right: 40px; width: 140px;">language</td><td>ISO 639-1 code. Without one set, we default to the language set in the requested account.</td></tr>
</table>

GET /account/{id}/rated/tv
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "d3a69fdeee228ff6582e6d1d7c7073ee"
{
  "page": 1,
  "results": [
    {
      "backdrop_path": "/k9NJwzO23UXmLZXoOqCbX7HhK7I.jpg",
      "id": 314,
      "original_name": "Star Trek: Enterprise",
      "first_air_date": "2001-09-26",
      "poster_path": "/9mRz3v97QMeU4I25ol8PxWx6tuG.jpg",
      "popularity": 0.95492325,
      "name": "Star Trek: Enterprise",
      "vote_average": 8.3,
      "vote_count": 4,
      "rating": 8.0
    },
    {
      "backdrop_path": "/qmG25kFDFbIdgSYIx1DF56HnMWp.jpg",
      "id": 39702,
      "original_name": "House of Lies",
      "first_air_date": "2012-01-08",
      "poster_path": "/6LnAp1mgtAiC3I1MzadoioQ9MkT.jpg",
      "popularity": 0.9665625,
      "name": "House of Lies",
      "vote_average": 8.0,
      "vote_count": 2,
      "rating": 8.0
    },
    {
      "backdrop_path": "/wdzp88gmAahtpnhREGHDHPdhcaX.jpg",
      "id": 433,
      "original_name": "Terminator: The Sarah Connor Chronicles",
      "first_air_date": "2008-01-13",
      "poster_path": "/1RygHDzvSEGYGYlEbOfYVp9T5Ck.jpg",
      "popularity": 0.32,
      "name": "Terminator: The Sarah Connor Chronicles",
      "vote_average": 8.8,
      "vote_count": 4,
      "rating": 8.0
    },
    {
      "backdrop_path": "/k5DRaUXp0ilvCzxE4pI39csaXX6.jpg",
      "id": 58811,
      "original_name": "Helix",
      "first_air_date": "2014-01-10",
      "poster_path": "/iCOv74CPbPWbO2KWgPVZrLOALff.jpg",
      "popularity": 0.651766,
      "name": "Helix",
      "vote_average": 8.8,
      "vote_count": 4,
      "rating": 9.0
    },
    {
      "backdrop_path": "/8i29MFIH2bYaqk0Ui1b2VRNTflg.jpg",
      "id": 2947,
      "original_name": "Veep",
      "first_air_date": "2012-04-22",
      "poster_path": "/nbdrg0x8jITHsMYxDiitcycQtXE.jpg",
      "popularity": 0.2,
      "name": "Veep",
      "vote_average": 9.0,
      "vote_count": 2,
      "rating": 8.0
    },
    {
      "backdrop_path": "/jHADNR5qfQGqRCa5VusuCz0tBQa.jpg",
      "id": 1982,
      "original_name": "Wonderfalls",
      "first_air_date": "2004-03-12",
      "poster_path": "/jkDQLlhX315zoeP7LRW6QEsduyx.jpg",
      "popularity": 0.50709,
      "name": "Wonderfalls",
      "vote_average": 7.5,
      "vote_count": 1,
      "rating": 7.5
    },
    {
      "backdrop_path": "/5MGbVKfJO96fTHLKgAPjZIB9kgi.jpg",
      "id": 5639,
      "original_name": "Pushing Daisies",
      "first_air_date": "2007-10-03",
      "poster_path": "/aUV2ujhVT7cQHu4IlwnCBN9OQvN.jpg",
      "popularity": 0.700441416566866,
      "name": "Pushing Daisies",
      "vote_average": 9.0,
      "vote_count": 1,
      "rating": 9.0
    },
    {
      "backdrop_path": "/4EFVV3ZsIRFGxvHJXGyTQLzThQK.jpg",
      "id": 46648,
      "original_name": "True Detective",
      "first_air_date": "2014-01-12",
      "poster_path": "/udKOK1kE3wJzpjrCfhmuMC6M7lC.jpg",
      "popularity": 2.69592683610194,
      "name": "True Detective",
      "vote_average": 9.8,
      "vote_count": 12,
      "rating": 9.5
    },
    {
      "backdrop_path": "/8wxxyouV0QImpfNVyyp86ksOlCY.jpg",
      "id": 45,
      "original_name": "Top Gear",
      "first_air_date": "2002-10-20",
      "poster_path": "/dyIZi3PBN1nyLgx7VHUvvo1e1VG.jpg",
      "popularity": 3.00667119696808,
      "name": "Top Gear",
      "vote_average": 7.7,
      "vote_count": 5,
      "rating": 9.0
    },
    {
      "backdrop_path": "/iQGyDmBgf8vERxltyHWwvR0ITq9.jpg",
      "id": 16997,
      "original_name": "The Pacific",
      "first_air_date": null,
      "poster_path": "/eitGOLIzvddgR4ZGx5GkAtX38h1.jpg",
      "popularity": 0.745294531189417,
      "name": "The Pacific",
      "vote_average": 8.5,
      "vote_count": 4,
      "rating": 9.0
    },
    {
      "backdrop_path": "/tvGPmIdP9DX778L2phz9eahNMLp.jpg",
      "id": 1420,
      "original_name": "New Girl",
      "first_air_date": "2011-09-20",
      "poster_path": "/iirKvUv5IFBFnJx9Rhhza95Gn22.jpg",
      "popularity": 0.917542068827423,
      "name": "New Girl",
      "vote_average": 6.5,
      "vote_count": 3,
      "rating": 7.0
    },
    {
      "backdrop_path": "/qj9QHowNKCxVbFTkIW1wJFfmK89.jpg",
      "id": 21494,
      "original_name": "V",
      "first_air_date": "2009-11-03",
      "poster_path": "/82oR6bSnODCK21SnDxSCVhcVQD2.jpg",
      "popularity": 0.63615856067133,
      "name": "V",
      "vote_average": 8.0,
      "vote_count": 2,
      "rating": 6.0
    },
    {
      "backdrop_path": "/gnWKBJlHSO43TLRiykeTCgOEcGn.jpg",
      "id": 44652,
      "original_name": "Last Resort",
      "first_air_date": "2012-09-27",
      "poster_path": "/qzzEV2fcjydHXg5CTrrv1DsDsZJ.jpg",
      "popularity": 0.2,
      "name": "Last Resort",
      "vote_average": 6.5,
      "vote_count": 1,
      "rating": 6.5
    },
    {
      "backdrop_path": "/l4ZYWRwSwJsTWQhNshYFpszqVs2.jpg",
      "id": 51019,
      "original_name": "Almost Human",
      "first_air_date": "2013-11-17",
      "poster_path": "/3DTKOuqa2RlwyhOWB31uBhmeLhC.jpg",
      "popularity": 0.541946163447557,
      "name": "Almost Human",
      "vote_average": 8.3,
      "vote_count": 6,
      "rating": 7.0
    },
    {
      "backdrop_path": "/eGeWSLpbgo5fEVhJonvbw3o2Xdt.jpg",
      "id": 1424,
      "original_name": "Orange Is the New Black",
      "first_air_date": "2013-07-11",
      "poster_path": "/5AuJ6FKL33fGCSiXa633xaHLV25.jpg",
      "popularity": 0.556,
      "name": "Orange Is the New Black",
      "vote_average": 8.3,
      "vote_count": 5,
      "rating": 7.5
    },
    {
      "backdrop_path": "/2Fg9H4TBJlXoZzZ4ixnAg0YV9xS.jpg",
      "id": 47141,
      "original_name": "The Bridge",
      "first_air_date": "2013-07-10",
      "poster_path": "/zBHarUl03wPbJsPOMCiKqtgsjmQ.jpg",
      "popularity": 0.50666655439,
      "name": "The Bridge",
      "vote_average": 8.3,
      "vote_count": 2,
      "rating": 8.0
    },
    {
      "backdrop_path": "/jUPkQdETILcxIW2xSNEzAxUeHgR.jpg",
      "id": 1972,
      "original_name": "Battlestar Galactica",
      "first_air_date": "2005-01-14",
      "poster_path": "/vaTO4vDEI2TU9XIoDPsPgrxbGWr.jpg",
      "popularity": 1.8217945312118,
      "name": "Battlestar Galactica",
      "vote_average": 8.9,
      "vote_count": 13,
      "rating": 9.0
    },
    {
      "backdrop_path": "/rwvHGXT5KWafoSKCCB0jLwWm66Z.jpg",
      "id": 95,
      "original_name": "Buffy the Vampire Slayer",
      "first_air_date": "1997-03-10",
      "poster_path": "/plRYSuq4fkPojnkaWUXBbHAZi1i.jpg",
      "popularity": 2.83369933148195,
      "name": "Buffy the Vampire Slayer",
      "vote_average": 9.2,
      "vote_count": 5,
      "rating": 7.5
    },
    {
      "backdrop_path": "/2ahKY0C9jQgBtNyMAtqcYFGUmVu.jpg",
      "id": 1426,
      "original_name": "Luther",
      "first_air_date": "2010-05-04",
      "poster_path": "/iRYkDSxCi3I71EW4eKP1jzPKhPN.jpg",
      "popularity": 0.2,
      "name": "Luther",
      "vote_average": 7.6,
      "vote_count": 4,
      "rating": 9.0
    },
    {
      "backdrop_path": "/mRApMhtHABq8t4IboWI3RvepWWN.jpg",
      "id": 41727,
      "original_name": "Banshee",
      "first_air_date": "2013-01-11",
      "poster_path": "/kQ26o4lj2WBfG8zD8xBLsd84GBa.jpg",
      "popularity": 0.875356985613961,
      "name": "Banshee",
      "vote_average": 9.0,
      "vote_count": 7,
      "rating": 9.0
    }
  ],
  "total_pages": 5,
  "total_results": 96
}

Get the list of rated tv episodes (and associated rating) for an account.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td colspan="2">api_key</td></tr>
<tr><td colspan="2">session_id</td></tr>
<tr><td colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-right: 40px; width: 140px;">page</td><td>Minimum 1, maximum 1000.</td></tr>
<tr><td style="padding-right: 40px; width: 140px;">sort_by</td><td>`created_at.desc` or `created_at.asc`</td></tr>
<tr><td style="padding-right: 40px; width: 140px;">language</td><td>ISO 639-1 code. Without one set, we default to the language set in the requested account.</td></tr>
</table>

GET /account/{id}/rated/tv/episodes
> Accept: application/json
< 200
< Content-Type: application/json
{
  "page": 1,
  "results": [
    {
      "air_date": "2013-10-15",
      "episode_number": 5,
      "name": "The Workplace Proximity",
      "id": 64782,
      "season_number": 7,
      "still_path": "/7kueaQBaPIEo4WFvp4WuwdUSRJo.jpg",
      "show_id": 1418,
      "vote_average": 8.5,
      "vote_count": 2,
      "rating": 8.0
    },
    {
      "air_date": "2013-12-08",
      "episode_number": 11,
      "name": "Big Man in Tehran",
      "id": 968782,
      "season_number": 3,
      "still_path": "/o4QulasZwHOhzoxk7yER2mt6jXp.jpg",
      "show_id": 1407,
      "vote_average": 9.0,
      "vote_count": 2,
      "rating": 9.0
    },
    {
      "air_date": "2014-03-23",
      "episode_number": 15,
      "name": "Dramatics, Your Honor",
      "id": 973273,
      "season_number": 5,
      "still_path": "/eGQg94nfM6EhKrxGA8A92SXe3WT.jpg",
      "show_id": 1435,
      "vote_average": 9.0,
      "vote_count": 1,
      "rating": 9.0
    },
    {
      "air_date": "2014-05-11",
      "episode_number": 6,
      "name": "The Laws of Gods and Men",
      "id": 63099,
      "season_number": 4,
      "still_path": "/vKt9b7HNYhwM91C7S53zPsAWfT3.jpg",
      "show_id": 1399,
      "vote_average": 9.5,
      "vote_count": 3,
      "rating": 8.5
    },
    {
      "air_date": "2014-05-18",
      "episode_number": 7,
      "name": "Mockingbird",
      "id": 63100,
      "season_number": 4,
      "still_path": "/r7u0uELPyDkNnBZzAFVvw1XJKo5.jpg",
      "show_id": 1399,
      "vote_average": 8.25,
      "vote_count": 2,
      "rating": 8.5
    },
    {
      "air_date": "2014-05-16",
      "episode_number": 12,
      "name": "Tome-wan",
      "id": 975339,
      "season_number": 2,
      "still_path": "/pYsO5cXmjJGYUZWVDocBI3CIM6.jpg",
      "show_id": 40008,
      "vote_average": 9.0,
      "vote_count": 1,
      "rating": 9.0
    },
    {
      "air_date": "2014-05-23",
      "episode_number": 13,
      "name": "Mizumono",
      "id": 976285,
      "season_number": 2,
      "still_path": "/40jqX3XRk6W97gscX5rj4kTy411.jpg",
      "show_id": 40008,
      "vote_average": 10.0,
      "vote_count": 2,
      "rating": 10.0
    },
    {
      "air_date": "2014-06-01",
      "episode_number": 8,
      "name": "The Mountain and the Viper",
      "id": 63101,
      "season_number": 4,
      "still_path": "/2bsd1Dolxh04UwwkFG4tt4L406S.jpg",
      "show_id": 1399,
      "vote_average": 9.75,
      "vote_count": 2,
      "rating": 9.5
    },
    {
      "air_date": "2013-09-29",
      "episode_number": 16,
      "name": "Felina",
      "id": 62161,
      "season_number": 5,
      "still_path": "/pZOEpN1obWkEMUqQZ3GCZhfV87J.jpg",
      "show_id": 1396,
      "vote_average": 9.75,
      "vote_count": 4,
      "rating": 9.0
    },
    {
      "air_date": "2014-07-13",
      "episode_number": 1,
      "name": "Night Zero",
      "id": 989249,
      "season_number": 1,
      "still_path": "/nRFLYU10Px3IzSjdOmTvTrR86jI.jpg",
      "show_id": 47640,
      "vote_average": 7.5,
      "vote_count": 1,
      "rating": 7.5
    },
    {
      "air_date": "2014-06-19",
      "episode_number": 1,
      "name": "Running with the Bull",
      "id": 976915,
      "season_number": 2,
      "still_path": "/2FJAQU1PKbMIYe7WCkZJlFgqgAc.jpg",
      "show_id": 46619,
      "vote_average": 9.5,
      "vote_count": 2,
      "rating": 9.0
    },
    {
      "air_date": "2014-08-24",
      "episode_number": 9,
      "name": "The Garveys at Their Best",
      "id": 990362,
      "season_number": 1,
      "still_path": "/gGLWdy0ac2OryUwfGd3y0TiHTpD.jpg",
      "show_id": 54344,
      "vote_average": 9.0,
      "vote_count": 1,
      "rating": 9.0
    }
  ],
  "total_pages": 1,
  "total_results": 12
}

Get the list of movies on an accounts watchlist.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td colspan="2">api_key</td></tr>
<tr><td colspan="2">session_id</td></tr>
<tr><td colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-right: 40px; width: 140px;">page</td><td>Minimum 1, maximum 1000.</td></tr>
<tr><td style="padding-right: 40px; width: 140px;">sort_by</td><td>`created_at.desc` or `created_at.asc`</td></tr>
<tr><td style="padding-right: 40px; width: 140px;">language</td><td>ISO 639-1 code. Without one set, we default to the language set in the requested account.</td></tr>
</table>

GET /account/{id}/watchlist/movies
> Accept: application/json
< 200
< Content-Type: application/json
{
    "page": 1,
    "results": [
        {
            "backdrop_path": "/xetk5vdoKxMIto5O5boqPGxWnoA.jpg",
            "id": 76726,
            "original_title": "Chronicle",
            "release_date": "2012-02-03",
            "poster_path": "/pSYRHYvOd8aFaddVUdyXJGZ8F6W.jpg",
            "title": "Chronicle",
            "vote_average": 7.2999999999999998,
            "vote_count": 32
        },
        {
            "backdrop_path": "/g9CDUXFBflsIch95wZxeNajTTX2.jpg",
            "id": 70435,
            "original_title": "Haywire",
            "release_date": "2011-11-07",
            "poster_path": "/8RlFYPPDFPr3ADrNHFGyB0JS6mo.jpg",
            "title": "Haywire",
            "vote_average": 6.9000000000000004,
            "vote_count": 18
        },
        {
            "backdrop_path": "/hbn46fQaRmlpBuUrEiFqv0GDL6Y.jpg",
            "id": 24428,
            "original_title": "The Avengers",
            "release_date": "2012-05-04",
            "poster_path": "/cezWGskPY5x7GaglTTRN4Fugfb8.jpg",
            "title": "The Avengers",
            "vote_average": 8.5,
            "vote_count": 117
        },
        {
            "backdrop_path": "/5Jj4SpbsxmH2yOlhka4LvNRtSud.jpg",
            "id": 22970,
            "original_title": "The Cabin in the Woods",
            "release_date": "2012-04-13",
            "poster_path": "/t8cW3FSCDYCaWRiNHSvI6SDuWeA.jpg",
            "title": "The Cabin in the Woods",
            "vote_average": 6.9000000000000004,
            "vote_count": 22
        },
        {
            "backdrop_path": "/8iNsXY3LPsuT0gnUiTBMoNuRZI7.jpg",
            "id": 52520,
            "original_title": "Underworld: Awakening",
            "release_date": "2012-01-20",
            "poster_path": "/76GXWKs2IDi0ZERKLT57wkToV0A.jpg",
            "title": "Underworld: Awakening",
            "vote_average": 7.5,
            "vote_count": 36
        },
        {
            "backdrop_path": "/gVfnoiCJBdv2SN7poIbU7eyNVq1.jpg",
            "id": 12437,
            "original_title": "Underworld: Rise of the Lycans",
            "release_date": "2009-01-23",
            "poster_path": "/hZtK6fkusSKzGc0f7yQ2syo14Ze.jpg",
            "title": "Underworld: Rise of the Lycans",
            "vote_average": 7.2999999999999998,
            "vote_count": 30
        },
        {
            "backdrop_path": "/LvmmDZxkTDqp0DX7mUo621ahdX.jpg",
            "id": 10195,
            "original_title": "Thor",
            "release_date": "2011-05-06",
            "poster_path": "/jqcMUV73jEhO0gv5yi28FD8JbHQ.jpg",
            "title": "Thor",
            "vote_average": 7.2000000000000002,
            "vote_count": 79
        },
        {
            "backdrop_path": "/1ueWzFe8Klk5WDurB5j8yl5qdaq.jpg",
            "id": 1771,
            "original_title": "Captain America: The First Avenger",
            "release_date": "2011-07-22",
            "poster_path": "/sBZs1jSybBRBXDwcCR8IOyHLUMc.jpg",
            "title": "Captain America: The First Avenger",
            "vote_average": 7.5,
            "vote_count": 73
        },
        {
            "backdrop_path": "/uOpFdld7CIifSEoGuRVgWqaeyFs.jpg",
            "id": 64688,
            "original_title": "21 Jump Street",
            "release_date": "2012-05-10",
            "poster_path": "/mH6xazzfGdszadd1Xy9JgdZYvn1.jpg",
            "title": "21 Jump Street",
            "vote_average": 7.5,
            "vote_count": 25
        },
        {
            "backdrop_path": "/9mnDfS7nuja6wCGK5bBG3412Qdo.jpg",
            "id": 17578,
            "original_title": "The Adventures of Tintin",
            "release_date": "2011-10-26",
            "poster_path": "/7NwqKqEqJDmFP8hkLBpV0Sk6xek.jpg",
            "title": "The Adventures of Tintin",
            "vote_average": 7.7000000000000002,
            "vote_count": 47
        },
        {
            "backdrop_path": "/wX0mOAa91dTAT2WCGRvvpWUKAeD.jpg",
            "id": 59961,
            "original_title": "Safe House",
            "release_date": "2012-02-16",
            "poster_path": "/itJBQuozyehzoTh0X4YpKwIiFLy.jpg",
            "title": "Safe House",
            "vote_average": 7.5999999999999996,
            "vote_count": 39
        },
        {
            "backdrop_path": "/elnVoZPBXwGVh5xD5PLbvIzmvOZ.jpg",
            "id": 8967,
            "original_title": "The Tree of Life",
            "release_date": "2011-05-27",
            "poster_path": "/5uiccn657MLiAbVHS5SfWFdeSTw.jpg",
            "title": "The Tree of Life",
            "vote_average": 7.4000000000000004,
            "vote_count": 19
        },
        {
            "backdrop_path": "/cnGPiAd7D50YFcpt6HK7CwTUCew.jpg",
            "id": 49530,
            "original_title": "In Time",
            "release_date": "2011-10-28",
            "poster_path": "/lnYuAr3QOPzvuEFlzpsRUq41IEy.jpg",
            "title": "In Time",
            "vote_average": 6.9000000000000004,
            "vote_count": 54
        },
        {
            "backdrop_path": "/1qQiuN2QlBR3VmIwqBXYFGCCZbl.jpg",
            "id": 49527,
            "original_title": "Man on a Ledge",
            "release_date": "2012-01-27",
            "poster_path": "/ssl4ZcThbMtEPNBEXmWpSgtT9xK.jpg",
            "title": "Man on a Ledge",
            "vote_average": 7.2000000000000002,
            "vote_count": 16
        },
        {
            "backdrop_path": "/gxbO2y8jeEvwGHDLC8ErqSf61ca.jpg",
            "id": 80389,
            "original_title": "Get the Gringo",
            "release_date": "2012-05-01",
            "poster_path": "/6yo4E8B0WbIpiHbY01Dq8r6dH8V.jpg",
            "title": "Get the Gringo",
            "vote_average": 7.7000000000000002,
            "vote_count": 8
        },
        {
            "backdrop_path": "/pbYQevN15y5aEusvcsp3bpuu5Ip.jpg",
            "id": 65754,
            "original_title": "The Girl with the Dragon Tattoo",
            "release_date": "2011-12-21",
            "poster_path": "/voxRWFTtagLiqnJQs9tWQLB0MN.jpg",
            "title": "The Girl with the Dragon Tattoo",
            "vote_average": 7.9000000000000004,
            "vote_count": 49
        },
        {
            "backdrop_path": "/3DdCnPuMq5QRXRE4PMrI3VEQI94.jpg",
            "id": 13007,
            "original_title": "Religulous",
            "release_date": "2008-10-01",
            "poster_path": "/lGo6P1rEs1GJnYJW9a6whlcljtZ.jpg",
            "title": "Religulous",
            "vote_average": 7.4000000000000004,
            "vote_count": 4
        },
        {
            "backdrop_path": "/wWwHt3NvdnGLXrGRR4n7GU6ISkh.jpg",
            "id": 64690,
            "original_title": "Drive",
            "release_date": "2012-01-26",
            "poster_path": "/fsnaaNQ4RmZwgRv6qfT7NIi41Sj.jpg",
            "title": "Drive",
            "vote_average": 7.7999999999999998,
            "vote_count": 80
        },
        {
            "backdrop_path": "/dNdrWdlOEuisZ1YyafkkPEWAihn.jpg",
            "id": 12690,
            "original_title": "Appaloosa",
            "release_date": "2008-10-03",
            "poster_path": "/5Ql3TsG08yvVBDyV5azOWUVKmuy.jpg",
            "title": "Appaloosa",
            "vote_average": 7.4000000000000004,
            "vote_count": 13
        },
        {
            "backdrop_path": "/7u3UyejCbhM3jXcZ86xzA9JJxge.jpg",
            "id": 41154,
            "original_title": "Men in Black 3",
            "release_date": "2012-05-25",
            "poster_path": "/sIuneIyme2O3qYxEZTVNyJ0F0LC.jpg",
            "title": "Men in Black 3",
            "vote_average": 7.5,
            "vote_count": 27
        }
    ],
    "total_pages": 4,
    "total_results": 67
}

Get the list of TV series on an accounts watchlist.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td colspan="2">api_key</td></tr>
<tr><td colspan="2">session_id</td></tr>
<tr><td colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-right: 40px; width: 140px;">page</td><td>Minimum 1, maximum 1000.</td></tr>
<tr><td style="padding-right: 40px; width: 140px;">sort_by</td><td>`created_at.desc` or `created_at.asc`</td></tr>
<tr><td style="padding-right: 40px; width: 140px;">language</td><td>ISO 639-1 code. Without one set, we default to the language set in the requested account.</td></tr>
</table>

GET /account/{id}/watchlist/tv
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "f32c526ef2529908508a893bdf7f3f97"
{
  "page": 1,
  "results": [
    {
      "backdrop_path": "/9deImMawq7fmsUlKCxmFgrlGpVr.jpg",
      "id": 51370,
      "original_name": "Resurrection",
      "first_air_date": "2014-03-09",
      "poster_path": "/oNiK8YaOGR995kLlNPhsM3mx506.jpg",
      "popularity": 0.99,
      "name": "Resurrection",
      "vote_average": 8.0,
      "vote_count": 1
    },
    {
      "backdrop_path": "/kbm0hkS347LkJNUtGA9wAJX7lYb.jpg",
      "id": 60573,
      "original_name": "Silicon Valley",
      "first_air_date": "2014-04-06",
      "poster_path": "/fS4IX85GZBFOwRx8vfJkgESGhrs.jpg",
      "popularity": 1.8120293804375,
      "name": "Silicon Valley",
      "vote_average": 0.0,
      "vote_count": 0
    },
    {
      "backdrop_path": "/ptZO3SuJAXT4sm0doQavofRrRfq.jpg",
      "id": 60625,
      "original_name": "Rick and Morty",
      "first_air_date": "2013-12-02",
      "poster_path": "/4LIf49No2uCJYJyaMykrVfXzIUn.jpg",
      "popularity": 0.46,
      "name": "Rick and Morty",
      "vote_average": 0.0,
      "vote_count": 0
    },
    {
      "backdrop_path": "/6FgbxsykcjfsvXGhJgobnHBuDrH.jpg",
      "id": 40293,
      "original_name": "Da Vinci's Demons",
      "first_air_date": "2013-04-12",
      "poster_path": "/zfgZdPc0zqM3CPZRtx3EFszUySQ.jpg",
      "popularity": 0.317,
      "name": "Da Vinci's Demons",
      "vote_average": 9.3,
      "vote_count": 2
    },
    {
      "backdrop_path": "/aQYd90P7ZW76ksaZLvRWR411xtj.jpg",
      "id": 60626,
      "original_name": "From Dusk till Dawn: The Series",
      "first_air_date": "2014-03-11",
      "poster_path": "/eoPTkEam7IxX0NdFXIn9mRTKDKt.jpg",
      "popularity": 1.092,
      "name": "From Dusk till Dawn: The Series",
      "vote_average": 7.0,
      "vote_count": 1
    },
    {
      "backdrop_path": "/ilucedmlirlodJoCvPcFwoBFpmc.jpg",
      "id": 58474,
      "original_name": "Cosmos: A Space-Time Odyssey",
      "first_air_date": "2014-03-09",
      "poster_path": "/jViSxrU3NBvmNsQBpf37sPBmBOq.jpg",
      "popularity": 2.457,
      "name": "Cosmos: A Space-Time Odyssey",
      "vote_average": 8.0,
      "vote_count": 4
    },
    {
      "backdrop_path": "/l0LvPl3lAJm6KtIw4RFesHTZ3zN.jpg",
      "id": 60611,
      "original_name": "Believe",
      "first_air_date": "2014-03-10",
      "poster_path": "/zWIBUpttd9SsErSGhVJ2mTIMlgr.jpg",
      "popularity": 0.830302524729031,
      "name": "Believe",
      "vote_average": 0.0,
      "vote_count": 0
    },
    {
      "backdrop_path": "/nJ5ewByOmbLN6pkY8JdqbEQU2OB.jpg",
      "id": 60622,
      "original_name": "Fargo",
      "first_air_date": "2014-04-15",
      "poster_path": "/uFrBO6au1CGeVSzqKHJtOob3Kum.jpg",
      "popularity": 9.48153125,
      "name": "Fargo",
      "vote_average": 4.5,
      "vote_count": 2
    },
    {
      "backdrop_path": null,
      "id": 60574,
      "original_name": "Peaky Blinders",
      "first_air_date": "2013-09-12",
      "poster_path": "/uYsF4E6q5GjD73VxPIgcBqb0rB6.jpg",
      "popularity": 0.58,
      "name": "Peaky Blinders",
      "vote_average": 0.0,
      "vote_count": 0
    },
    {
      "backdrop_path": "/3FAeq3AXkmfvEdXuJayW1u51Cwv.jpg",
      "id": 1430,
      "original_name": "Cosmos: A Personal Voyage",
      "first_air_date": "1980-09-28",
      "poster_path": "/3fSpmqsGZrWtdzEqvnIcdTqMSrB.jpg",
      "popularity": 0.575,
      "name": "Cosmos: A Personal Voyage",
      "vote_average": 10.0,
      "vote_count": 2
    },
    {
      "backdrop_path": "/j1QcJ98qou1lZFEhb9th6rbUDHz.jpg",
      "id": 55083,
      "original_name": "Hostages",
      "first_air_date": "2013-09-23",
      "poster_path": "/nLQAG3K9wBl9yYz5cT3uyxDxOQ3.jpg",
      "popularity": 0.26,
      "name": "Hostages",
      "vote_average": 7.0,
      "vote_count": 1
    },
    {
      "backdrop_path": "/asANEWJ6751csWnN1BwkScIMHpL.jpg",
      "id": 44305,
      "original_name": "DreamWorks Dragons",
      "first_air_date": "2012-08-07",
      "poster_path": "/dBeefOkpTRlpXPiEVqjXa1GMjL0.jpg",
      "popularity": 1.92817199090997,
      "name": "DreamWorks Dragons",
      "vote_average": 5.5,
      "vote_count": 1
    },
    {
      "backdrop_path": "/p3STM36Mc47GKWqld10GHjs8RGS.jpg",
      "id": 32573,
      "original_name": "Strike Back",
      "first_air_date": "2010-05-05",
      "poster_path": "/mbu1JI0dFI0hQ8ksfe9fqcvIvfl.jpg",
      "popularity": 1.07164437297133,
      "name": "Strike Back",
      "vote_average": 7.4,
      "vote_count": 4
    },
    {
      "backdrop_path": "/rS52WPTDWzUKput9X81JKe0UY5Q.jpg",
      "id": 32940,
      "original_name": "Accused",
      "first_air_date": "2010-11-15",
      "poster_path": "/fYkJF28Q91gMK6pEiz7ZtSa7ul3.jpg",
      "popularity": 0.309104,
      "name": "Accused",
      "vote_average": 7.5,
      "vote_count": 1
    },
    {
      "backdrop_path": "/35AV3nRuQoTSepIqFg4U92PFPWG.jpg",
      "id": 44840,
      "original_name": "Mob City",
      "first_air_date": "2013-12-04",
      "poster_path": "/7RnHXmCRCa4vPI3BK3oKBHiQVlG.jpg",
      "popularity": 0.37193,
      "name": "Mob City",
      "vote_average": 0.0,
      "vote_count": 0
    },
    {
      "backdrop_path": "/zinqlHnka4fIMQZTRY39cdnkqzq.jpg",
      "id": 51320,
      "original_name": "The Millers",
      "first_air_date": "2013-10-03",
      "poster_path": "/h2O7lbEhRABP2n9WYkWSq5n0rwA.jpg",
      "popularity": 0.32,
      "name": "The Millers",
      "vote_average": 0.0,
      "vote_count": 0
    },
    {
      "backdrop_path": "/p2gWPRGUErQiTQ3ieTqeCirdtKx.jpg",
      "id": 58942,
      "original_name": "The Michael J. Fox Show",
      "first_air_date": "2013-09-26",
      "poster_path": "/iVIhtD07Ld8envEA6R1sf8MkLOR.jpg",
      "popularity": 0.2,
      "name": "The Michael J. Fox Show",
      "vote_average": 0.0,
      "vote_count": 0
    },
    {
      "backdrop_path": "/FYp0CaACSWHt265YKVDeKf7YHV.jpg",
      "id": 1695,
      "original_name": "Monk",
      "first_air_date": "2002-07-12",
      "poster_path": "/aI3GJXEmtb1k5uWb8O8tkmNlkbQ.jpg",
      "popularity": 3.44432093018317,
      "name": "Monk",
      "vote_average": 7.8,
      "vote_count": 3
    },
    {
      "backdrop_path": "/dTKkOs7ZMCO9KN3sGqrizrN5YwE.png",
      "id": 32406,
      "original_name": "The Big C",
      "first_air_date": "2010-08-16",
      "poster_path": "/9VxPo04xWQmv5deFc5iP4XuXV3e.jpg",
      "popularity": 0.585,
      "name": "The Big C",
      "vote_average": 9.0,
      "vote_count": 1
    },
    {
      "backdrop_path": "/dBD03Hpzji8qYnuKp1Y2dGqdxwL.jpg",
      "id": 43901,
      "original_name": "Longmire",
      "first_air_date": "2012-06-03",
      "poster_path": "/fAxfZWAUUkbutwOXAcbZ2AE3qIK.jpg",
      "popularity": 0.417,
      "name": "Longmire",
      "vote_average": 7.0,
      "vote_count": 1
    }
  ],
  "total_pages": 2,
  "total_results": 36
}

Add or remove a movie to an accounts watch list.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
<tr><td style="padding-left: 0;" colspan="2">session_id</td></tr>
<tr><td style="padding-left: 0;" colspan="2"><strong>Required JSON Body</strong></td></tr>
<tr><td style="padding-left: 0; padding-right: 40px; width: 140px;">media_type</td><td>movie | tv</td></tr>
<tr><td style="padding-left: 0; padding-right: 40px; width: 140px;">media_id</td><td>Integer</td></tr>
<tr><td style="padding-left: 0; padding-right: 40px; width: 140px;">watchlist</td><td>true | false</td></tr>
</table>

POST /account/{id}/watchlist
> Accept: application/json
> Content-Type: application/json
{
    "media_type": "movie",
    "media_id": 11,
    "watchlist": true
}
< 200
< Content-Type: application/json
{
    "status_code": 1,
    "status_message": "Success"
}

--
Authentication
--

This method is used to generate a valid request token for user based authentication. A request token is required in order to request a session id. You can read more about this process <a href="https://www.themoviedb.org/documentation/api/sessions">here</a>.

You can generate any number of request tokens but they will expire after 60 minutes. As soon as a valid session id has been created the token will be destroyed.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
</table>

GET /authentication/token/new
> Accept: application/json
< 200
< Content-Type: application/json
< Authentication-Callback: http://www.themoviedb.org/authenticate/641bf16c663db167c6cffcdff41126039d4445bf
{
    "expires_at": "2012-02-09 19:50:25 UTC",
    "request_token": "641bf16c663db167c6cffcdff41126039d4445bf",
    "success": true
}

Once you have a valid request token you can use this method to authenticate a user with a TMDb username and password. You can authenticate a request token 2 different ways, both are outlined <a href="https://www.themoviedb.org/documentation/api/sessions">here</a>.

The user must have a verified email address and be registered on TMDb. As soon as the request token has been verified and attached to a user, the request token will be invalidated.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
<tr><td style="padding-left: 0;" colspan="2">request_token</td></tr>
<tr><td style="padding-left: 0;" colspan="2">username</td></tr>
<tr><td style="padding-left: 0;" colspan="2">password</td></tr>
</table>

GET /authentication/token/validate_with_login
> Accept: application/json
< 200
< Content-Type: application/json
{
    "request_token": "641bf16c663db167c6cffcdff41126039d4445bf",
    "success": true
}

This method is used to generate a session id for user based authentication. A session id is required in order to use any of the write methods.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
<tr><td style="padding-left: 0; padding-right: 40px; width: 100px;">request_token</td><td>The request token parameter is the token you generated for the user to approve. The token needs to be approved by the user before being used here. You can read more about this process <a href="https://www.themoviedb.org/documentation/api/sessions">here</a>.</td></tr>
</table>

GET /authentication/session/new
> Accept: application/json
< 200
< Content-Type: application/json
{
    "session_id": "80b2bf99520cd795ff54e31af97917bc9e3a7c8c",
    "success": true
}

This method is used to generate a guest session id.

A guest session can be used to rate movies without having a registered TMDb user account. You should only generate a single guest session per user (or device) as you will be able to attach the ratings to a TMDb user account in the future. There is also IP limits in place so you should always make sure it's the end user doing the guest session actions.

If a guest session is not used for the first time within 24 hours, it will be automatically discarded.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
</table>

GET /authentication/guest_session/new
> Accept: application/json
< 200
< Content-Type: application/json
{
    "success": true,
    "guest_session_id": "0c550fd5da2fc3f321ab3bs9b60ca108",
    "expires_at": "2012-12-04 22:51:19 UTC"
}

--
Certifications
--

Get the list of supported certifications for movies. These can be used in conjunction with the `certification_country` and `certification.lte` parameters when using discover.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
</table>

GET /certification/movie/list
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "f1552fb1837bdbe161779ae3cd8bd041"
{
  "certifications": {
    "AU": [
      {
        "certification": "E",
        "meaning": "Exempt from classification. Films that are exempt from classification must not contain contentious material (i.e. material that would ordinarily be rated M or higher).",
        "order": 1
      },
      {
        "certification": "G",
        "meaning": "General. The content is very mild in impact.",
        "order": 2
      },
      {
        "certification": "PG",
        "meaning": "Parental guidance recommended. There are no age restrictions. The content is mild in impact.",
        "order": 2
      },
      {
        "certification": "M",
        "meaning": "Recommended for mature audiences. There are no age restrictions. The content is moderate in impact.",
        "order": 3
      },
      {
        "certification": "MA15+",
        "meaning": "Mature Accompanied. Unsuitable for children younger than 15. Children younger than 15 years must be accompanied by a parent or guardian. The content is strong in impact.",
        "order": 5
      },
      {
        "certification": "R18+",
        "meaning": "Restricted to 18 years and over. Adults only. The content is high in impact.",
        "order": 6
      },
      {
        "certification": "X18+",
        "meaning": "Restricted to 18 years and over. Films with this rating have pornographic content. Films classified as X18+ are banned from being sold or rented in all Australian states and are only legally available in the Australian Capital Territory and the Northern Territory. However, importing X18+ material from the two territories to any of the Australian states is legal.The content is sexually explicit in impact.",
        "order": 7
      },
      {
        "certification": "RC",
        "meaning": "Refused Classification. Banned from sale or hire in Australia; also generally applies to importation (if inspected by and suspicious to Customs). Private Internet viewing is unenforced and attempts to legally censor such online material has resulted in controversy. Films are rated RC if their content exceeds the guidelines. The content is very high in impact.",
        "order": 8
      }
    ],
    "CA": [
      {
        "certification": "G",
        "meaning": "All ages.",
        "order": 1
      },
      {
        "certification": "PG",
        "meaning": "Parental guidance advised. There is no age restriction but some material may not be suitable for all children.",
        "order": 2
      },
      {
        "certification": "14A",
        "meaning": "Persons under 14 years of age must be accompanied by an adult.",
        "order": 3
      },
      {
        "certification": "18A",
        "meaning": "Persons under 18 years of age must be accompanied by an adult. In the Maritimes & Manitoba, children under the age of 14 are prohibited from viewing the film.",
        "order": 4
      },
      {
        "certification": "R",
        "meaning": "Admittance restricted to people 18 years of age or older.",
        "order": 5
      },
      {
        "certification": "A",
        "meaning": "Admittance restricted to people 18 years of age or older. Sole purpose of the film is the portrayal of sexually explicit activity and/or explicit violence.",
        "order": 5
      }
    ],
    "DE": [
      {
        "certification": "0",
        "meaning": "No age restriction.",
        "order": 1
      },
      {
        "certification": "6",
        "meaning": "No children younger than 6 years admitted.",
        "order": 2
      },
      {
        "certification": "12",
        "meaning": "Children 12 or older admitted, children between 6 and 11 only when accompanied by parent or a legal guardian.",
        "order": 3
      },
      {
        "certification": "16",
        "meaning": "Children 16 or older admitted, nobody under this age admitted.",
        "order": 4
      },
      {
        "certification": "18",
        "meaning": "No youth admitted, only adults.",
        "order": 5
      }
    ],
    "FR": [
      {
        "certification": "U",
        "meaning": "(Tous publics) valid for all audiences.",
        "order": 1
      },
      {
        "certification": "10",
        "meaning": "(Déconseillé aux moins de 10 ans) unsuitable for children younger than 10 (this rating is only used for TV); equivalent in theatres : \"avertissement\" (warning), some scenes may be disturbing to young children and sensitive people; equivalent on video : \"accord parental\" (parental guidance).",
        "order": 2
      },
      {
        "certification": "12",
        "meaning": "(Interdit aux moins de 12 ans) unsuitable for children younger than 12 or forbidden in cinemas for under 12.",
        "order": 3
      },
      {
        "certification": "16",
        "meaning": "(Interdit aux moins de 16 ans) unsuitable for children younger than 16 or forbidden in cinemas for under 16.",
        "order": 4
      },
      {
        "certification": "18",
        "meaning": "(Interdit aux mineurs) unsuitable for children younger than 18 or forbidden in cinemas for under 18.",
        "order": 5
      }
    ],
    "GB": [
      {
        "certification": "U",
        "meaning": "All ages admitted, there is nothing unsuitable for children.",
        "order": 1
      },
      {
        "certification": "PG",
        "meaning": "All ages admitted, but certain scenes may be unsuitable for young children. May contain mild language and sex/drugs references. May contain moderate violence if justified by context (e.g. fantasy).",
        "order": 2
      },
      {
        "certification": "12A",
        "meaning": "Films under this category are considered to be unsuitable for very young people. Those aged under 12 years are only admitted if accompanied by an adult, aged at least 18 years, at all times during the motion picture. However, it is generally not recommended that children under 12 years should watch the film. Films under this category can contain mature themes, discrimination, soft drugs, moderate swear words, infrequent strong language and moderate violence, sex references and nudity. Sexual activity may be briefly and discreetly portrayed. Sexual violence may be implied or briefly indicated.",
        "order": 3
      },
      {
        "certification": "12",
        "meaning": "Home media only since 2002. 12A-rated films are usually given a 12 certificate for the VHS/DVD version unless extra material has been added that requires a higher rating. Nobody younger than 12 can rent or buy a 12-rated VHS, DVD, Blu-ray Disc, UMD or game. The content guidelines are identical to those used for the 12A certificate.",
        "order": 4
      },
      {
        "certification": "15",
        "meaning": "Only those over 15 years are admitted. Nobody younger than 15 can rent or buy a 15-rated VHS, DVD, Blu-ray Disc, UMD or game, or watch a film in the cinema with this rating. Films under this category can contain adult themes, hard drugs, frequent strong language and limited use of very strong language, strong violence and strong sex references, and nudity without graphic detail. Sexual activity may be portrayed but without any strong detail. Sexual violence may be shown if discreet and justified by context.",
        "order": 5
      },
      {
        "certification": "18",
        "meaning": "Only adults are admitted. Nobody younger than 18 can rent or buy an 18-rated VHS, DVD, Blu-ray Disc, UMD or game, or watch a film in the cinema with this rating. Films under this category do not have limitation on the bad language that is used. Hard drugs are generally allowed, and explicit sex references along with detailed sexual activity are also allowed. Scenes of strong real sex may be permitted if justified by the context. Very strong, gory, and/or sadistic violence is usually permitted. Strong sexual violence is permitted unless it is eroticised or excessively graphic.",
        "order": 6
      },
      {
        "certification": "R18",
        "meaning": "Can only be shown at licensed adult cinemas or sold at licensed sex shops, and only to adults, those aged 18 or over. Films under this category are always hard-core pornography, defined as material intended for sexual stimulation and containing clear images of real sexual activity, strong fetish material, explicit animated images, or sight of certain acts such as triple simultaneous penetration and snowballing. There remains a range of material that is often cut from the R18 rating: strong images of injury in BDSM or spanking works, urolagnia, scenes suggesting incest even if staged, references to underage sex or childhood sexual development and aggressive behaviour such as hair-pulling or spitting on a performer are not permitted. More cuts are demanded in this category than any other category.",
        "order": 7
      }
    ],
    "IN": [
      {
        "certification": "U",
        "meaning": "Unrestricted Public Exhibition throughout India, suitable for all age groups. Films under this category should not upset children over 4. Such films may contain educational, social or family-oriented themes. Films under this category may also contain fantasy violence and/or mild bad language.",
        "order": 0
      },
      {
        "certification": "UA",
        "meaning": "All ages admitted, but it is advised that children below 12 be accompanied by a parent as the theme or content may be considered intense or inappropriate for young children. Films under this category may contain mature themes, sexual references, mild sex scenes, violence with brief gory images and/or infrequent use of crude language.",
        "order": 1
      },
      {
        "certification": "A",
        "meaning": "Restricted to adult audiences (18 years or over). Nobody below the age of 18 may buy/rent an A-rated DVD, VHS, UMD or watch a film in the cinema with this rating. Films under this category may contain adult/disturbing themes, frequent crude language, brutal violence with blood and gore, strong sex scenes and/or scenes of drug abuse which is considered unsuitable for minors.",
        "order": 2
      }
    ],
    "NZ": [
      {
        "certification": "G",
        "meaning": "Suitable for general audiences.",
        "order": 1
      },
      {
        "certification": "PG",
        "meaning": "Parental guidance recommended for younger viewers.",
        "order": 2
      },
      {
        "certification": "M",
        "meaning": "Suitable for (but not restricted to) mature audiences 16 years and up.",
        "order": 3
      },
      {
        "certification": "13",
        "meaning": "Restricted to persons 13 years of age and over.",
        "order": 4
      },
      {
        "certification": "15",
        "meaning": "Restricted to persons 15 years of age and over.",
        "order": 5
      },
      {
        "certification": "16",
        "meaning": "Restricted to persons 16 years of age and over.",
        "order": 6
      },
      {
        "certification": "18",
        "meaning": "Restricted to persons 18 years of age and over.",
        "order": 7
      },
      {
        "certification": "R",
        "meaning": "Restricted to a particular class of persons, or for particular purposes, or both.",
        "order": 8
      }
    ],
    "US": [
      {
        "certification": "NR",
        "meaning": "No rating information.",
        "order": 0
      },
      {
        "certification": "G",
        "meaning": "All ages admitted. There is no content that would be objectionable to most parents. This is one of only two ratings dating back to 1968 that still exists today.",
        "order": 1
      },
      {
        "certification": "PG",
        "meaning": "Some material may not be suitable for children under 10. These films may contain some mild language, crude/suggestive humor, scary moments and/or violence. No drug content is present. There are a few exceptions to this rule. A few racial insults may also be heard.",
        "order": 2
      },
      {
        "certification": "PG-13",
        "meaning": "Some material may be inappropriate for children under 13. Films given this rating may contain sexual content, brief or partial nudity, some strong language and innuendo, humor, mature themes, political themes, terror and/or intense action violence. However, bloodshed is rarely present. This is the minimum rating at which drug content is present.",
        "order": 3
      },
      {
        "certification": "R",
        "meaning": "Under 17 requires accompanying parent or adult guardian 21 or older. The parent/guardian is required to stay with the child under 17 through the entire movie, even if the parent gives the child/teenager permission to see the film alone. These films may contain strong profanity, graphic sexuality, nudity, strong violence, horror, gore, and strong drug use. A movie rated R for profanity often has more severe or frequent language than the PG-13 rating would permit. An R-rated movie may have more blood, gore, drug use, nudity, or graphic sexuality than a PG-13 movie would admit.",
        "order": 4
      },
      {
        "certification": "NC-17",
        "meaning": "These films contain excessive graphic violence, intense or explicit sex, depraved, abhorrent behavior, explicit drug abuse, strong language, explicit nudity, or any other elements which, at present, most parents would consider too strong and therefore off-limits for viewing by their children and teens. NC-17 does not necessarily mean obscene or pornographic in the oft-accepted or legal meaning of those words.",
        "order": 5
      }
    ]
  }
}

Get the list of supported certifications for tv shows.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
</table>

GET /certification/tv/list
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "8eaffdba2d2222a6f9406193669d8342"
{
  "certifications": {
    "AU": [
      {
        "certification": "P",
        "meaning": "Programming is intended for younger children 2–11; commercial stations must show at least 30 minutes of P-rated content each weekday and weekends at all times. No advertisements may be shown during P-rated programs.",
        "order": 1
      },
      {
        "certification": "C",
        "meaning": "Programming intended for older children 5–14; commercial stations must show at least 30 minutes of C-rated content each weekday between 7 a.m. and 8 a.m. or between 4 p.m. and 8:30 p.m. A further 2 and a half ours a week must also be shown either within these time bands or between 7 a.m. and 8:30 p.m. on weekends and school holidays, for a total of five hours a week (averaged as 260 hours over the course of a year). C-rated content is subject to certain restrictions and limitations on advertising (typically five minutes maximum per 30-minute period or seven minutes including promotions and community announcements).",
        "order": 2
      },
      {
        "certification": "G",
        "meaning": "For general exhibition; all ages are permitted to watch programming with this rating.",
        "order": 3
      },
      {
        "certification": "PG",
        "meaning": "Parental guidance is recommended for young viewers; PG-rated content may air at any time on digital-only channels, otherwise, it should only be broadcast between 8:30 a.m. and 4:00 p.m. and between 7:00 p.m. and 6:00 a.m. on weekdays, and between 10:00 a.m. and 6:00 a.m. on weekends.",
        "order": 4
      },
      {
        "certification": "M",
        "meaning": "Recommended for mature audiences; M-rated content may only be broadcast between 8:30 p.m. and 5:00 a.m. on any day, and additionally between 12:00 p.m. and 3:00 p.m. on schooldays.",
        "order": 5
      },
      {
        "certification": "MA15+",
        "meaning": "Not suitable for children and teens under 15; MA15+-rated programming may only be broadcast between 9:00 p.m. and 5:00 a.m. on any given day. Consumer advice is mandatory. Some R18+ rated movies on DVD/Blu-ray are often re-edited on free TV/cable channels to secure a more \"appropriate\" MA15+ rating. Some movies that were rated R18 on DVD have since been aired in Australian TV with MA15+ rating.",
        "order": 6
      },
      {
        "certification": "AV15+",
        "meaning": "Not suitable for children and teens under 15; this is the same as the MA15+ rating, except the \"AV\" stands for \"Adult Violence\" meaning that anything that is Classified \"MA15+\" with the consumer advice \"Frequent Violence\" or \"Strong Violence\" will automatically become AV15+ (with that same consumer advice.) The AV rating is still allowed to exceed any MA15+ content, in particular – 'Violence'. AV15+ content may only be broadcast between 9:30 p.m. and 5:00 a.m. on any day. Consumer advice is mandatory.",
        "order": 7
      },
      {
        "certification": "R18+",
        "meaning": "Not for children under 18; this is limited to Adult \"Pay Per View\" VC 196 and 197. Content may include graphic violence, sexual situations, coarse language and explicit drug use.",
        "order": 8
      }
    ],
    "CA": [
      {
        "certification": "Exempt",
        "meaning": "Shows which are exempt from ratings (such as news and sports programming) will not display an on-screen rating at all.",
        "order": 0
      },
      {
        "certification": "C",
        "meaning": "Programming suitable for children ages of 2–7 years. No profanity or sexual content of any level allowed. Contains little violence.",
        "order": 1
      },
      {
        "certification": "C8",
        "meaning": "Suitable for children ages 8+. Low level violence and fantasy horror is allowed. No foul language is allowed, but occasional \"socially offensive and discriminatory\" language is allowed if in the context of the story. No sexual content of any level allowed.",
        "order": 2
      },
      {
        "certification": "G",
        "meaning": "Suitable for general audiences. Programming suitable for the entire family with mild violence, and mild profanity and/or censored language.",
        "order": 3
      },
      {
        "certification": "PG",
        "meaning": "Parental guidance. Moderate violence and moderate profanity is allowed, as is brief nudity and sexual references if important to the context of the story.",
        "order": 4
      },
      {
        "certification": "14+",
        "meaning": "Programming intended for viewers ages 14 and older. May contain strong violence and strong profanity, and depictions of sexual activity as long as they are within the context of a story.",
        "order": 5
      },
      {
        "certification": "18+",
        "meaning": "Programming intended for viewers ages 18 and older. May contain explicit violence and sexual activity. Programming with this rating cannot air before the watershed (9:00 p.m. to 6:00 a.m.).",
        "order": 6
      }
    ],
    "FR": [
      {
        "certification": "NR",
        "meaning": "No rating information.",
        "order": 0
      },
      {
        "certification": "10",
        "meaning": "Not recommended for children under 10. Not allowed in children's television series.",
        "order": 1
      },
      {
        "certification": "12",
        "meaning": "Not recommended for children under 12. Not allowed air before 10:00 p.m. Some channels and programs are subject to exception.",
        "order": 2
      },
      {
        "certification": "16",
        "meaning": "Not recommended for children under 16. Not allowed air before 10:30 p.m. Some channels and programs are subject to exception.",
        "order": 3
      },
      {
        "certification": "18",
        "meaning": "Not recommended for persons under 18. Allowed between midnight and 5 a.m. and only in some channels, access to these programs is locked by a personal password.",
        "order": 4
      }
    ],
    "RU": [
      {
        "certification": "0+",
        "meaning": "Can be watched by Any Age.",
        "order": 1
      },
      {
        "certification": "6+",
        "meaning": "Only kids the age of 6 or older can watch.",
        "order": 2
      },
      {
        "certification": "12+",
        "meaning": "Only kids the age of 12 or older can watch.",
        "order": 3
      },
      {
        "certification": "16+",
        "meaning": "Only teens the age of 16 or older can watch.",
        "order": 4
      },
      {
        "certification": "18+",
        "meaning": "Restricted to People 18 or Older.",
        "order": 5
      }
    ],
    "US": [
      {
        "certification": "NR",
        "meaning": "No rating information.",
        "order": 0
      },
      {
        "certification": "TV-Y",
        "meaning": "This program is designed to be appropriate for all children.",
        "order": 1
      },
      {
        "certification": "TV-Y7",
        "meaning": "This program is designed for children age 7 and above.",
        "order": 2
      },
      {
        "certification": "TV-G",
        "meaning": "Most parents would find this program suitable for all ages.",
        "order": 3
      },
      {
        "certification": "TV-PG",
        "meaning": "This program contains material that parents may find unsuitable for younger children.",
        "order": 4
      },
      {
        "certification": "TV-14",
        "meaning": "This program contains some material that many parents would find unsuitable for children under 14 years of age.",
        "order": 5
      },
      {
        "certification": "TV-MA",
        "meaning": "This program is specifically designed to be viewed by adults and therefore may be unsuitable for children under 17.",
        "order": 6
      }
    ]
  }
}

--
Changes
--

Get a list of movie ids that have been edited. By default we show the last 24 hours and only 100 items per page. The maximum number of days that can be returned in a single request is 14. You can then use the [movie changes API](#get-%2F3%2Fmovie%2F%7Bid%7D%2Fchanges) to get the actual data that has been changed.

**Please note:** the change log system to support this was changed on October 5, 2012 and will only show movies that have been edited since.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td colspan="2">api_key</td></tr>
<tr><td colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-right: 40px; width: 140px;">page</td><td>Minimum 1, maximum 1000.</td></tr>
<tr><td style="padding-right: 40px; width: 140px;">start_date</td><td>YYYY-MM-DD</td></tr>
<tr><td style="padding-right: 40px; width: 140px;">end_date</td><td>YYYY-MM-DD</td></tr>
</table>

GET /movie/changes
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "0ca01e6b15143aa6fd9afa99c13d15fb"
{
    "results": [
        {
            "id": 77946,
            "adult": false
        },
        {
            "id": 125527,
            "adult": false
        },
        {
            "id": 41023,
            "adult": false
        },
        {
            "id": 41471,
            "adult": false
        },
        {
            "id": 11058,
            "adult": false
        },
        {
            "id": 136838,
            "adult": false
        },
        {
            "id": 41024,
            "adult": false
        },
        {
            "id": 136842,
            "adult": false
        },
        {
            "id": 136844,
            "adult": false
        },
        {
            "id": 136845,
            "adult": false
        },
        {
            "id": 707,
            "adult": false
        },
        {
            "id": 13572,
            "adult": false
        },
        {
            "id": 96428,
            "adult": false
        },
        {
            "id": 13377,
            "adult": false
        },
        {
            "id": 117416,
            "adult": false
        },
        {
            "id": 12405,
            "adult": false
        },
        {
            "id": 77338,
            "adult": false
        },
        {
            "id": 4643,
            "adult": false
        },
        {
            "id": 136847,
            "adult": false
        },
        {
            "id": 10264,
            "adult": false
        },
        {
            "id": 84165,
            "adult": false
        },
        {
            "id": 136849,
            "adult": false
        },
        {
            "id": 52256,
            "adult": false
        },
        {
            "id": 77697,
            "adult": false
        },
        {
            "id": 31800,
            "adult": false
        },
        {
            "id": 24808,
            "adult": false
        },
        {
            "id": 34840,
            "adult": false
        },
        {
            "id": 87428,
            "adult": false
        },
        {
            "id": 22099,
            "adult": false
        },
        {
            "id": 119372,
            "adult": false
        },
        {
            "id": 20813,
            "adult": false
        },
        {
            "id": 94671,
            "adult": false
        },
        {
            "id": 22097,
            "adult": false
        },
        {
            "id": 136510,
            "adult": false
        },
        {
            "id": 10068,
            "adult": false
        },
        {
            "id": 136850,
            "adult": false
        },
        {
            "id": 136851,
            "adult": false
        },
        {
            "id": 136852,
            "adult": false
        },
        {
            "id": 13531,
            "adult": false
        },
        {
            "id": 17971,
            "adult": false
        },
        {
            "id": 136854,
            "adult": false
        },
        {
            "id": 136855,
            "adult": false
        },
        {
            "id": 2103,
            "adult": false
        },
        {
            "id": 72933,
            "adult": false
        },
        {
            "id": 10787,
            "adult": false
        },
        {
            "id": 128508,
            "adult": false
        },
        {
            "id": 119212,
            "adult": false
        },
        {
            "id": 46929,
            "adult": false
        },
        {
            "id": 17102,
            "adult": false
        },
        {
            "id": 136811,
            "adult": false
        },
        {
            "id": 136808,
            "adult": false
        },
        {
            "id": 62974,
            "adult": false
        },
        {
            "id": 92693,
            "adult": false
        },
        {
            "id": 136803,
            "adult": false
        },
        {
            "id": 74495,
            "adult": false
        },
        {
            "id": 136856,
            "adult": false
        },
        {
            "id": 136790,
            "adult": false
        },
        {
            "id": 136857,
            "adult": false
        },
        {
            "id": 136858,
            "adult": false
        },
        {
            "id": 136859,
            "adult": false
        },
        {
            "id": 113128,
            "adult": false
        },
        {
            "id": 114150,
            "adult": false
        },
        {
            "id": 76492,
            "adult": false
        },
        {
            "id": 136861,
            "adult": false
        },
        {
            "id": 136862,
            "adult": false
        },
        {
            "id": 136863,
            "adult": false
        },
        {
            "id": 136864,
            "adult": false
        },
        {
            "id": 9833,
            "adult": false
        },
        {
            "id": 667,
            "adult": false
        },
        {
            "id": 136867,
            "adult": false
        },
        {
            "id": 1450,
            "adult": false
        },
        {
            "id": 136732,
            "adult": false
        },
        {
            "id": 98205,
            "adult": false
        },
        {
            "id": 136735,
            "adult": false
        },
        {
            "id": 136190,
            "adult": false
        },
        {
            "id": 94720,
            "adult": false
        },
        {
            "id": 136741,
            "adult": false
        },
        {
            "id": 136742,
            "adult": false
        },
        {
            "id": 136743,
            "adult": false
        },
        {
            "id": 136751,
            "adult": false
        },
        {
            "id": 136752,
            "adult": false
        },
        {
            "id": 136753,
            "adult": false
        },
        {
            "id": 31393,
            "adult": false
        },
        {
            "id": 29262,
            "adult": false
        },
        {
            "id": 33067,
            "adult": false
        },
        {
            "id": 136868,
            "adult": false
        },
        {
            "id": 123025,
            "adult": false
        },
        {
            "id": 136870,
            "adult": false
        },
        {
            "id": 11370,
            "adult": false
        },
        {
            "id": 9267,
            "adult": false
        },
        {
            "id": 57387,
            "adult": false
        },
        {
            "id": 136876,
            "adult": false
        },
        {
            "id": 134739,
            "adult": false
        },
        {
            "id": 88534,
            "adult": false
        },
        {
            "id": 120591,
            "adult": false
        },
        {
            "id": 136877,
            "adult": false
        },
        {
            "id": 134708,
            "adult": false
        },
        {
            "id": 18426,
            "adult": false
        },
        {
            "id": 53146,
            "adult": false
        },
        {
            "id": 9587,
            "adult": false
        }
    ],
    "page": 1,
    "total_pages": 7,
    "total_results": 664
}

Get a list of people ids that have been edited. By default we show the last 24 hours and only 100 items per page. The maximum number of days that can be returned in a single request is 14. You can then use the [person changes API](#get-%2F3%2Fperson%2F%7Bid%7D%2Fchanges) to get the actual data that has been changed.

**Please note:** the change log system to support this was changed on October 5, 2012 and will only show people that have been edited since.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td colspan="2">api_key</td></tr>
<tr><td colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-right: 40px; width: 140px;">page</td><td>Minimum 1, maximum 1000.</td></tr>
<tr><td style="padding-right: 40px; width: 140px;">start_date</td><td>YYYY-MM-DD</td></tr>
<tr><td style="padding-right: 40px; width: 140px;">end_date</td><td>YYYY-MM-DD</td></tr>
</table>

GET /person/changes
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "d8c9458be59f4e279c92aa1dda7c6e79"
{
    "results": [
        {
            "id": 1106271,
            "adult": false
        },
        {
            "id": 1106272,
            "adult": false
        },
        {
            "id": 1106273,
            "adult": false
        },
        {
            "id": 1106274,
            "adult": false
        },
        {
            "id": 1106275,
            "adult": false
        },
        {
            "id": 1106276,
            "adult": false
        },
        {
            "id": 1106277,
            "adult": false
        },
        {
            "id": 1106278,
            "adult": false
        },
        {
            "id": 1106279,
            "adult": false
        },
        {
            "id": 1106280,
            "adult": false
        },
        {
            "id": 1106281,
            "adult": false
        },
        {
            "id": 236354,
            "adult": false
        },
        {
            "id": 1106282,
            "adult": false
        },
        {
            "id": 1106283,
            "adult": false
        },
        {
            "id": 1106284,
            "adult": false
        },
        {
            "id": 1106285,
            "adult": false
        },
        {
            "id": 1106286,
            "adult": false
        },
        {
            "id": 1106287,
            "adult": false
        },
        {
            "id": 1106288,
            "adult": false
        },
        {
            "id": 1106289,
            "adult": false
        },
        {
            "id": 1106290,
            "adult": false
        },
        {
            "id": 1106291,
            "adult": false
        },
        {
            "id": 1106292,
            "adult": false
        },
        {
            "id": 1106293,
            "adult": false
        },
        {
            "id": 1106294,
            "adult": false
        },
        {
            "id": 1106295,
            "adult": false
        },
        {
            "id": 1106296,
            "adult": false
        },
        {
            "id": 1106297,
            "adult": false
        },
        {
            "id": 1106298,
            "adult": false
        },
        {
            "id": 1106299,
            "adult": false
        },
        {
            "id": 1106300,
            "adult": false
        },
        {
            "id": 1106301,
            "adult": false
        },
        {
            "id": 1106302,
            "adult": false
        },
        {
            "id": 1106303,
            "adult": false
        },
        {
            "id": 1106304,
            "adult": false
        },
        {
            "id": 1106305,
            "adult": false
        },
        {
            "id": 1106306,
            "adult": false
        },
        {
            "id": 1106307,
            "adult": false
        },
        {
            "id": 1106308,
            "adult": false
        },
        {
            "id": 1106309,
            "adult": false
        },
        {
            "id": 1106310,
            "adult": false
        },
        {
            "id": 1106311,
            "adult": false
        },
        {
            "id": 1106312,
            "adult": false
        },
        {
            "id": 1106313,
            "adult": false
        },
        {
            "id": 56649,
            "adult": false
        },
        {
            "id": 1106314,
            "adult": false
        },
        {
            "id": 1106315,
            "adult": false
        },
        {
            "id": 1106316,
            "adult": false
        },
        {
            "id": 233631,
            "adult": false
        },
        {
            "id": 1106317,
            "adult": false
        },
        {
            "id": 172150,
            "adult": false
        },
        {
            "id": 184180,
            "adult": false
        },
        {
            "id": 109046,
            "adult": false
        },
        {
            "id": 1106318,
            "adult": false
        },
        {
            "id": 1106319,
            "adult": false
        },
        {
            "id": 1106320,
            "adult": false
        },
        {
            "id": 1085800,
            "adult": false
        },
        {
            "id": 1106321,
            "adult": false
        },
        {
            "id": 1106322,
            "adult": false
        },
        {
            "id": 1106323,
            "adult": false
        },
        {
            "id": 1106324,
            "adult": false
        },
        {
            "id": 1106325,
            "adult": false
        },
        {
            "id": 1106326,
            "adult": false
        },
        {
            "id": 1106327,
            "adult": false
        },
        {
            "id": 1106328,
            "adult": false
        },
        {
            "id": 1106329,
            "adult": false
        },
        {
            "id": 1106330,
            "adult": false
        },
        {
            "id": 1106331,
            "adult": false
        },
        {
            "id": 1106332,
            "adult": false
        },
        {
            "id": 1106333,
            "adult": false
        },
        {
            "id": 1106334,
            "adult": false
        },
        {
            "id": 1106335,
            "adult": false
        },
        {
            "id": 1106336,
            "adult": false
        },
        {
            "id": 1106352,
            "adult": false
        },
        {
            "id": 1106355,
            "adult": false
        },
        {
            "id": 1106356,
            "adult": false
        },
        {
            "id": 1106358,
            "adult": false
        },
        {
            "id": 1106367,
            "adult": false
        },
        {
            "id": 1106397,
            "adult": false
        },
        {
            "id": 1106399,
            "adult": false
        },
        {
            "id": 1106408,
            "adult": false
        },
        {
            "id": 1106411,
            "adult": false
        },
        {
            "id": 1106413,
            "adult": false
        },
        {
            "id": 1106414,
            "adult": false
        },
        {
            "id": 1106432,
            "adult": false
        },
        {
            "id": 1106511,
            "adult": false
        },
        {
            "id": 1106512,
            "adult": false
        },
        {
            "id": 1106513,
            "adult": false
        },
        {
            "id": 1106514,
            "adult": false
        },
        {
            "id": 1106515,
            "adult": false
        },
        {
            "id": 1106516,
            "adult": false
        },
        {
            "id": 1106517,
            "adult": false
        },
        {
            "id": 1106518,
            "adult": false
        },
        {
            "id": 1106519,
            "adult": false
        },
        {
            "id": 1106520,
            "adult": false
        },
        {
            "id": 1106521,
            "adult": false
        },
        {
            "id": 1106522,
            "adult": false
        },
        {
            "id": 1106523,
            "adult": false
        },
        {
            "id": 1106524,
            "adult": false
        },
        {
            "id": 1106525,
            "adult": false
        }
    ],
    "page": 1,
    "total_pages": 2,
    "total_results": 164
}

Get a list of TV show ids that have been edited. By default we show the last 24 hours and only 100 items per page. The maximum number of days that can be returned in a single request is 14. You can then use the [TV changes API](#get-%2F3%2Ftv%2F%7Bid%7D%2Fchanges) to get the actual data that has been changed.

**Please note:** the change log system to properly support TV was updated on May 13, 2014. You'll likely only find the edits made since then to be useful in the change log system.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td colspan="2">api_key</td></tr>
<tr><td colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-right: 40px; width: 140px;">page</td><td>Minimum 1, maximum 1000.</td></tr>
<tr><td style="padding-right: 40px; width: 140px;">start_date</td><td>YYYY-MM-DD</td></tr>
<tr><td style="padding-right: 40px; width: 140px;">end_date</td><td>YYYY-MM-DD</td></tr>
</table>

GET /tv/changes
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "8d2e4e2467b2673a26253780c50a66c3"
{
  "results": [
    {
      "id": 1415,
      "adult": false
    },
    {
      "id": 14686,
      "adult": false
    },
    {
      "id": 39896,
      "adult": false
    },
    {
      "id": 39925,
      "adult": false
    },
    {
      "id": 39881,
      "adult": false
    },
    {
      "id": 37971,
      "adult": false
    },
    {
      "id": 39924,
      "adult": false
    },
    {
      "id": 29774,
      "adult": false
    },
    {
      "id": 39920,
      "adult": false
    },
    {
      "id": 25759,
      "adult": false
    },
    {
      "id": 39856,
      "adult": false
    },
    {
      "id": 7812,
      "adult": false
    },
    {
      "id": 39921,
      "adult": false
    },
    {
      "id": 39964,
      "adult": false
    },
    {
      "id": 22759,
      "adult": false
    },
    {
      "id": 60709,
      "adult": null
    },
    {
      "id": 17610,
      "adult": false
    },
    {
      "id": 36336,
      "adult": false
    },
    {
      "id": 39917,
      "adult": false
    },
    {
      "id": 16386,
      "adult": false
    },
    {
      "id": 8895,
      "adult": false
    },
    {
      "id": 849,
      "adult": false
    },
    {
      "id": 60726,
      "adult": false
    },
    {
      "id": 1441,
      "adult": false
    },
    {
      "id": 43121,
      "adult": false
    },
    {
      "id": 1403,
      "adult": false
    },
    {
      "id": 60694,
      "adult": false
    },
    {
      "id": 60573,
      "adult": false
    },
    {
      "id": 4613,
      "adult": false
    },
    {
      "id": 4614,
      "adult": false
    },
    {
      "id": 4297,
      "adult": false
    },
    {
      "id": 57018,
      "adult": false
    },
    {
      "id": 1973,
      "adult": false
    },
    {
      "id": 1399,
      "adult": false
    },
    {
      "id": 60049,
      "adult": false
    },
    {
      "id": 600,
      "adult": false
    },
    {
      "id": 1411,
      "adult": false
    },
    {
      "id": 3034,
      "adult": false
    },
    {
      "id": 39959,
      "adult": false
    },
    {
      "id": 37305,
      "adult": false
    },
    {
      "id": 39883,
      "adult": false
    },
    {
      "id": 60727,
      "adult": false
    },
    {
      "id": 45501,
      "adult": false
    },
    {
      "id": 42105,
      "adult": false
    },
    {
      "id": 26234,
      "adult": false
    },
    {
      "id": 53,
      "adult": false
    },
    {
      "id": 39827,
      "adult": false
    }
  ],
  "page": 1,
  "total_pages": 1,
  "total_results": 47
}

--
Collections
--

Get the basic collection information for a specific collection id. You can get the ID needed for this method by making a [/movie/{id}](#get-%2F3%2Fmovie%2F%7Bid%7D) request and paying attention to the `belongs_to_collection` hash.

Movie parts are not sorted in any particular order. If you would like to sort them yourself you can use the provided `release_date`.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
<tr><td style="padding-left: 0;" colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-left: 0; width: 140px;">language</td><td>ISO 639-1 code.</td></tr>
<tr><td style="padding-left: 0; width: 140px;">append_to_response</td><td>Comma separated, any collection method</td></tr>
</table>

GET /collection/{id}
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "087ff1516832c736d3c736c38b2bd96f"
{
  "backdrop_path": "/mOTtuakUTb1qY6jG6lzMfjdhLwc.jpg",
  "id": 10,
  "name": "Star Wars Collection",
  "parts": [{
      "backdrop_path": "/mOTtuakUTb1qY6jG6lzMfjdhLwc.jpg",
      "id": 11,
      "poster_path": "/qoETrQ73Jbd2LDN8EUfNgUerhzG.jpg",
      "release_date": "1977-12-27",
      "title": "Star Wars: Episode IV: A New Hope"
  },
  {
      "backdrop_path": null,
      "id": 1891,
      "poster_path": null,
      "release_date": "1980-05-21",
      "title": "Star Wars: Episode V: The Empire Strikes Back"
  },
  {
      "backdrop_path": null,
      "id": 1892,
      "poster_path": null,
      "release_date": "1983-05-25",
      "title": "Star Wars: Episode VI: Return of the Jedi"
  },
  {
      "backdrop_path": null,
      "id": 1893,
      "poster_path": null,
      "release_date": "1999-05-19",
      "title": "Star Wars: Episode I: The Phantom Menace"
  },
  {
      "backdrop_path": null,
      "id": 1894,
      "poster_path": null,
      "release_date": "2002-05-16",
      "title": "Star Wars: Episode II: Attack of the Clones"
  },
  {
      "backdrop_path": null,
      "id": 1895,
      "poster_path": null,
      "release_date": "2005-05-19",
      "title": "Star Wars: Episode III: Revenge of the Sith"
  }],
  "poster_path": "/6rddZZpxMQkGlpQYVVxb2LdQRI3.jpg"
}

Get all of the images for a particular collection by collection id.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
<tr><td style="padding-left: 0;" colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-left: 0; width: 140px;">language</td><td>ISO 639-1 code.</td></tr>
<tr><td style="padding-left: 0; width: 140px;">append_to_response</td><td>Comma separated, any collection method</td></tr>
<tr><td style="padding-left: 0; width: 140px;">include_image_language</td><td>Comma separated, a valid ISO 69-1. Maximum 5 per request.</td></tr>
</table>

GET /collection/{id}/images
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "8672576f48eb67cc63e87d3661486671"
{
  "backdrops": [{
    "aspect_ratio": 1.78,
    "file_path": "/mOTtuakUTb1qY6jG6lzMfjdhLwc.jpg",
    "height": 1080,
    "iso_639_1": null,
    "width": 1920
  },
  {
    "aspect_ratio": 1.78,
    "file_path": "/6dSG8wQBuZ9s0HhKspneFFZI58j.jpg",
    "height": 1080,
    "iso_639_1": null,
    "width": 1920
  }],
  "id": 11,
  "posters": [{
    "aspect_ratio": 0.67,
    "file_path": "/qoETrQ73Jbd2LDN8EUfNgUerhzG.jpg",
    "height": 1500,
    "iso_639_1": "en",
    "width": 1000
  },
  {
    "aspect_ratio": 0.67,
    "file_path": "/fiPNADHtDsOBvOIye79cLui3aEQ.jpg",
    "height": 1500,
    "iso_639_1": null,
    "width": 1000
  },
  {
    "aspect_ratio": 0.67,
    "file_path": "/rUCmBo09lVZbbzCR5zbAMWazvOa.jpg",
    "height": 1500,
    "iso_639_1": null,
    "width": 1000
  },
  {
    "aspect_ratio": 0.67,
    "file_path": "/7b95oMhjiWby189P9DwutN5k374.jpg",
    "height": 1350,
    "iso_639_1": "de",
    "width": 900
  }
  ]
}

--
Companies
--

This method is used to retrieve all of the basic information about a company.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
<tr><td style="padding-left: 0;" colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-left: 0; width: 140px;">append_to_response</td><td>Comma separated, any company method</td></tr>
</table>

GET /company/{id}
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "8a58d391bb3dbd16d2a659f7d8823529"
{
    "description": null,
    "headquarters": "San Francisco, California",
    "homepage": "http://www.lucasfilm.com",
    "id": 1,
    "logo_path": "/8rUnVMVZjlmQsJ45UGotD0Uznxj.png",
    "name": "Lucasfilm",
    "parent_company": null
}

Get the list of movies associated with a particular company.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td colspan="2">api_key</td></tr>
<tr><td colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td padding-right: 40px; width: 140px;">page</td><td>Minimum 1, maximum 1000.</td></tr>
<tr><td style="padding-right: 40px; width: 140px;">language</td><td>ISO 639-1 code.</td></tr>
<tr><td style="width: 140px;">append_to_response</td><td>Comma separated, any company method</td></tr>
</table>

GET /company/{id}/movies
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "0d4728bc0fc91d3418755c39f0a78511"
{
    "id": 1,
    "page": 1,
    "results": [
        {
            "backdrop_path": "/gmLMaDXi4lFWG8WitaCYOJS5GtL.jpg",
            "id": 12180,
            "original_title": "Star Wars: The Clone Wars",
            "release_date": "2008-08-15",
            "poster_path": "/xd6yhmtS6mEURZLwUDT5raEMbf.jpg",
            "title": "Star Wars: The Clone Wars",
            "vote_average": 7.7000000000000002,
            "vote_count": 12
        },
        {
            "backdrop_path": "/alNMTW47nZABwvwz3Gjwarl4btE.jpg",
            "id": 217,
            "original_title": "Indiana Jones and the Kingdom of the Crystal Skull",
            "release_date": "2008-05-22",
            "poster_path": "/6Lv49E0aEusW9vKEMgQgLdetlmO.jpg",
            "title": "Indiana Jones and the Kingdom of the Crystal Skull",
            "vote_average": 6.7000000000000002,
            "vote_count": 48
        },
        {
            "backdrop_path": "/gZxY7VDOI48gjhnDJK1E6n9uHWk.jpg",
            "id": 42979,
            "original_title": "Robot Chicken: Star Wars",
            "release_date": "2007-07-17",
            "poster_path": "/3vcBL7zNVMhLdFSEh3xiquhdp1x.jpg",
            "title": "Robot Chicken: Star Wars",
            "vote_average": 8.0,
            "vote_count": 1
        },
        {
            "backdrop_path": "/zHrxWn6P6Zjip9fJmmuEbzDd76o.jpg",
            "id": 1895,
            "original_title": "Star Wars: Episode III - Revenge of the Sith",
            "release_date": "2005-05-19",
            "poster_path": "/tgr5Pdy7ehZYBqBkN2K7Q02xgOb.jpg",
            "title": "Star Wars: Episode III - Revenge of the Sith",
            "vote_average": 7.9000000000000004,
            "vote_count": 59
        },
        {
            "backdrop_path": "/560F7BPaxRy8BsOfVU6cW4ivM46.jpg",
            "id": 1894,
            "original_title": "Star Wars: Episode II - Attack of the Clones",
            "release_date": "2002-05-16",
            "poster_path": "/2vcNFtrZXNwIcBgH5e2xXCmVR8t.jpg",
            "title": "Star Wars: Episode II - Attack of the Clones",
            "vote_average": 7.5999999999999996,
            "vote_count": 69
        },
        {
            "backdrop_path": "/4KD7RynRLIGqWjDvwwta5iBDvgS.jpg",
            "id": 47025,
            "original_title": "R2-D2: Beneath the Dome",
            "release_date": "2001-11-25",
            "poster_path": "/wTqLGbnUn5yMgh3fD3HUWrv0gKl.jpg",
            "title": "R2-D2: Beneath the Dome",
            "vote_average": 0.0,
            "vote_count": 0
        },
        {
            "backdrop_path": "/rtG5TRrQXf11jlO9WqcYTq46jKa.jpg",
            "id": 1893,
            "original_title": "Star Wars: Episode I - The Phantom Menace",
            "release_date": "1999-05-19",
            "poster_path": "/n8V09dDc02KsSN6Q4hC2BX6hN8X.jpg",
            "title": "Star Wars: Episode I - The Phantom Menace",
            "vote_average": 7.2000000000000002,
            "vote_count": 74
        },
        {
            "backdrop_path": "/sBAyZPvS3RmX6SFr0fJ9bDeRmP5.jpg",
            "id": 74823,
            "original_title": "Star Wars: Droids - Treasure of the Hidden Planet",
            "release_date": "1997-02-11",
            "poster_path": "/sFQatyz7dCONSlQg6NP3F3kJ0QP.jpg",
            "title": "Star Wars: Droids - Treasure of the Hidden Planet",
            "vote_average": 0.0,
            "vote_count": 0
        },
        {
            "backdrop_path": null,
            "id": 75380,
            "original_title": "Star Wars: Droids - The Pirate and the Prince",
            "release_date": "1997-02-11",
            "poster_path": "/tzMyVe7heytMepJJCJq21n6FGWV.jpg",
            "title": "Star Wars: Droids - The Pirate and the Prince",
            "vote_average": 0.0,
            "vote_count": 0
        },
        {
            "backdrop_path": "/pY7VAT4gtvVy38UuVEPhTPyfiYJ.jpg",
            "id": 89,
            "original_title": "Indiana Jones and the Last Crusade",
            "release_date": "1989-05-24",
            "poster_path": "/1xlQmxqKbkCb2THC9rwgr75p25B.jpg",
            "title": "Indiana Jones and the Last Crusade",
            "vote_average": 8.8000000000000007,
            "vote_count": 58
        },
        {
            "backdrop_path": "/n1wLikbs9EjLbZXOPcymEsrJctZ.jpg",
            "id": 847,
            "original_title": "Willow",
            "release_date": "1988-05-20",
            "poster_path": "/5iara9JMotSn9r7TM4tL7LFyfdu.jpg",
            "title": "Willow",
            "vote_average": 8.0,
            "vote_count": 11
        },
        {
            "backdrop_path": "/9zNu80EjtkvDImE4goHFsI6z3JW.jpg",
            "id": 10372,
            "original_title": "Ewoks: The Battle for Endor",
            "release_date": "1985-11-24",
            "poster_path": "/qKWa0i67AY2XNzvg7yXfGLX1Vcf.jpg",
            "title": "Ewoks: The Battle for Endor",
            "vote_average": 10.0,
            "vote_count": 2
        },
        {
            "backdrop_path": "/dmLfnPklVIXpOYbJaUKF3pgXsWk.jpg",
            "id": 1884,
            "original_title": "The Ewok Adventure",
            "release_date": "1984-11-25",
            "poster_path": "/y6HdTlqgcZ6EdsKR1uP03WgBe0C.jpg",
            "title": "The Ewok Adventure",
            "vote_average": 10.0,
            "vote_count": 4
        },
        {
            "backdrop_path": "/uG6AChg2FA3EmRuJDezDqoIpQzB.jpg",
            "id": 87,
            "original_title": "Indiana Jones and the Temple of Doom",
            "release_date": "1984-05-23",
            "poster_path": "/c8wjjTnqxnG835iSEeWUJJifSVp.jpg",
            "title": "Indiana Jones and the Temple of Doom",
            "vote_average": 7.7999999999999998,
            "vote_count": 40
        },
        {
            "backdrop_path": "/i4Qh3cHNS3vcJAJoGoVSXePMGQV.jpg",
            "id": 36751,
            "original_title": "Twice Upon a Time",
            "release_date": "1983-08-05",
            "poster_path": "/r9kyqDYhzgelcnSakBp0w5C5i7z.jpg",
            "title": "Twice Upon a Time",
            "vote_average": 8.5,
            "vote_count": 1
        },
        {
            "backdrop_path": "/bvJOpyHYWACDusvQvXxKEHFNjce.jpg",
            "id": 1892,
            "original_title": "Star Wars: Episode VI - Return of the Jedi",
            "release_date": "1983-05-25",
            "poster_path": "/jx5p0aHlbPXqe3AH9G15NvmWaqQ.jpg",
            "title": "Star Wars: Episode VI - Return of the Jedi",
            "vote_average": 8.6999999999999993,
            "vote_count": 105
        },
        {
            "backdrop_path": "/i5ubqxYkkqZpwgw6nHHUudOuGTh.jpg",
            "id": 85,
            "original_title": "Raiders of the Lost Ark",
            "release_date": "1981-06-12",
            "poster_path": "/tjGpsV0vKKHECtdsQoVwIXsDj3.jpg",
            "title": "Raiders of the Lost Ark",
            "vote_average": 8.6999999999999993,
            "vote_count": 59
        },
        {
            "backdrop_path": "/AkE7LQs2hPMG5tpWYcum847Knre.jpg",
            "id": 1891,
            "original_title": "Star Wars: Episode V - The Empire Strikes Back",
            "release_date": "1980-05-21",
            "poster_path": "/dKz98Lyda4m1QaUUUsUpXFpYCXN.jpg",
            "title": "Star Wars: Episode V - The Empire Strikes Back",
            "vote_average": 9.0,
            "vote_count": 144
        },
        {
            "backdrop_path": "/r0v9dayXd1IH5WPWFBWv52tGHkB.jpg",
            "id": 11,
            "original_title": "Star Wars: Episode IV - A New Hope",
            "release_date": "1977-05-25",
            "poster_path": "/tvSlBzAdRE29bZe5yYWrJ2ds137.jpg",
            "title": "Star Wars: Episode IV - A New Hope",
            "vote_average": 9.0,
            "vote_count": 149
        },
        {
            "backdrop_path": "/qCECROwx3TRUEgoZv2Mz2D723QC.jpg",
            "id": 10,
            "original_title": "Star Wars Collection",
            "release_date": null,
            "poster_path": "/ghd5zOQnDaDW1mxO7R5fXXpZMu.jpg",
            "title": "Star Wars Collection",
            "vote_average": 0.0,
            "vote_count": 0
        }
    ],
    "total_pages": 1,
    "total_results": 20
}

--
Credits
--

Get the detailed information about a particular credit record. This is currently only supported with the new credit model found in TV. These ids can be found from any TV credit response as well as the `tv_credits` and `combined_credits` methods for people.

The episodes object returns a list of episodes and are generally going to be guest stars. The season array will return a list of season numbers. Season credits are credits that were marked with the "add to every season" option in the editing interface and are assumed to be "season regulars".

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
<tr><td style="padding-left: 0;" colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-left: 0; padding-right: 40px; width: 140px;">language</td><td>ISO 639-1 code.</td></tr>
</table>

GET /credit/{credit_id}
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "ccbdd8ac09c878d3c2ccc9590810d28a"
{
  "credit_type": "cast",
  "department": "Actors",
  "job": "Actor",
  "media": {
    "id": 5,
    "name": "Seinfeld",
    "original_name": "Seinfeld",
    "character": "",
    "episodes": [
      {
        "air_date": "1994-11-17",
        "episode_number": 8,
        "name": "The Mom & Pop Store",
        "overview": "A salesman convinces George to buy a convertible once owned by \"Jon Voight.\" Kramer tries to save a small shoe-repair business, but his good intentions affect Jerry in a big way. Elaine wins tickets for Mr. Pitt, who's always wanted to participate in the Macy's Thanksgiving Day Parade.",
        "season_number": 6,
        "still_path": null
      },
      {
        "air_date": "1995-01-19",
        "episode_number": 12,
        "name": "The Label Maker",
        "overview": "Elaine and Jerry are suspicious of a friend's gift, when a gift Elaine gave him, a label maker, is given to Jerry in return for some Superbowl tickets Jerry has but can't use because \"The Drake\" is getting married on Superbowl Sunday and he is in the wedding party. George convinces his girlfriend to get her male roommate to move out, that he soon regrets. Kramer takes playing a game of Risk against Newman seriously. The Superbowl tickets pass through several hands and Jerry sees the game with his worst nightmare.",
        "season_number": 6,
        "still_path": null
      },
      {
        "air_date": "1997-04-24",
        "episode_number": 19,
        "name": "The Yada Yada",
        "overview": "George's girlfriend is big on using the phrase \"yada yada\"; Jerry says at least she is succinct. Jerry's dentist just became Jewish and he is already making jokes that make Jerry uncomfortable. Kramer and Mickey double date but they can't decide which of the women is right for them. Elaine is a character reference for a couple who is trying to adopt; a story she tells during the interview destroys all hope of adoption. George drops by Jerry's dental appointment. Mickey and Kramer continue to fight over who gets Karen or Julie. George determines that his girlfriend might be leaving out some significant details with her overuse of the phrase \"yada yada.\" He gets her to fill in the details and discovers more than he wants to know. Jerry confesses to a priest about what he thinks about Tim's conversion. George drops by Jerry's confession. Kramer decides on the right woman and Mickey also decides to make his a commitment. Tim hears about Jerry's dental joke. After hearing Jerry's complaints about Tim, Kramer accuses Jerry of being an \"anti-dentite.\" Elaine lobbies on behalf of Beth and Arnie and makes a sacrifice to try getting them a child. Meanwhile, Beth comes to Jerry for help when her marriage is falling apart. It does and she accompanies Jerry to Mickey's wedding where she reveals an unknown side of her personality.",
        "season_number": 8,
        "still_path": null
      },
      {
        "air_date": "1997-12-18",
        "episode_number": 10,
        "name": "The Strike",
        "overview": "George, Elaine and Jerry attend Tim Whatley's Hanukkah party. Jerry meets an attractive woman with whom he sets up a date. Elaine meets a man in a bad denim vest and gives him her fake number. George is offended by Whatley's gift to him, a donation in his name to a charity. George is also reminded of the Festivus holiday his father created many years ago. Elaine's quest to become a submarine captain and get her free sub sandwich is ruined when she realizes she used her punch card at the party to give her fake number to the denim vested guy. Kramer gets word he can return to his job at the bagel place, it seems he has been on strike for the past 12 years. Elaine goes to the place, an off-track betting parlor, that her fake number reaches. She wanted to give them her real number, so when the denim vest guy calls, she can connect with him. The men at the parlor are interested in connecting with her, so she gives the number for the bagel shop where Kramer is working. Jerry meets his date, Gwen, at a restaurant, but it turns out she is two-faced. Sometimes Gwen looks great, other times she's plain; it all depends on the viewer's angle and the lighting. George decides to use the Whatley approach when giving out Christmas gifts at Krugers; however, he makes up his own charity called the \"Human Fund.\" Kramer is intrigued by the concept of the Festivus holiday and contacts Frank, who becomes excited at the prospect of rekindling \"Festivus for the rest-of-us.\" Kramer asks to get the 23rd of December off, when he can't get it, he resumes the strike; meanwhile Elaine waits at the bagel place for a phone call from the denim vest guy. The look of Jerry's girlfriend keeps changing.\nJerry decides that Gwen looks best in the back booth at Monk's, something she grows to dislike. George passes out his gifts at Krugers and reaps great rewards. Kramer warns Elaine about the sabotage he committed; the bagel place becomes very steamy and makes Elaine look ugly. Kruger gives George a check for donates $20,000 to the \"Human Fund\" and later accounting informs him the charity doesn't exist. Gwen finds out from Kramer that Jerry is seeing another woman, Kramer has seen her and she's not Gwen. Gwen thinks Jerry is two-timing her with an ugly woman. George tries to convince Kruger that he passed out the fake gift cards because he didn't want to be ridiculed for the holiday his family traditionally celebrates, Festivus. To prove it, George brings Kruger to his father's Festivus dinner, where everyone comes together.",
        "season_number": 9,
        "still_path": null
      },
      {
        "air_date": "1998-05-14",
        "episode_number": 23,
        "name": "The Finale, Part 1",
        "overview": "Jerry and George discuss the movies and George's desire to get his fifteen minutes of fame. Kramer is off to the beach. Elaine calls a friend, whose father is in the hospital, with her cell phone; Jerry and George tell her that is a social faux pas. Jerry gets a message from NBC that they want to talk about the pilot. So Jerry and George go to meet with the new vice president of programming, who is interested in turning their pilot into a 13 episode series. Jerry and George begin to make plans to move to California. Jerry interrupts Elaine's phone call to her friend to tell her about the NBC deal. When he finds out what she did, he tells her that was an even greater faux pas than the cell phone. Jerry and George's parents are excited by the news about the NBC deal. NBC offers Jerry George a perk, free use of one of their private jets to anywhere they want. Kramer returns from the beach, but has a little bit of water trapped in his ear. Kramer warns they'll never come back from LA, \"she's a seductress.\" Hey! He did. The foursome decides where they want to take the private jet. They finally decide on Paris. As they are ready to leave, Elaine plans to call her friend again; Jerry tells her it is not right to rush that kind of phone call. Elaine avoids a faux pas. Newman begs to be brought along, when Jerry denies him, he vows to be there at Jerry's day of reckoning. The private jet, except for George who wanted the one Ted Danson would have gotten impresses everyone. With water still in his ear, Kramer tries to get it out mid-flight. He stumbles into the cockpit and the plane starts going into a crash dive. During the descent, George confesses he cheated during \"the contest\" and Elaine and Jerry are about to tell each other something important, when the plane corrects itself. The plane puts down in the small town of Latham, Massachusetts for a checkup. The foursome goes into town and debates about if they are going to get back on the plane. They witness the robbery of a fat guy, which they all mock and Kramer videotapes. They are arrested under the Good Samaritan law established by the town. They are looking at a fine of a maximum of $85,000 and up to five years in prison. The guard assumes they are going to be prosecuted since this is the first offense of this kind in the country. Jackie Chiles is called in for their defense. The prosecution decides to look into the past of these four and build a case that will destroy their characters. Rivera Live covers the trial. Jerry and George's parents prepare to go to Latham for the trial. Newman (who's absolutely delighted), Uncle Leo, Peterman, Puddy, Mickey, Bania, Mr. Mrs. Ross, Rabbi Glickman, Keith Hernandez and George Steinbrenner also make their way to Latham. Jackie tries to give George a moral compass. The judge, Arthur Vandelay, begins the trial. George thinks the name might be a good sign. The trial begins with opening arguments.",
        "season_number": 9,
        "still_path": null
      }
    ],
    "seasons": []
  },
  "media_type": "tv",
  "id": "5240760b5dbf5b0c2c0139db",
  "person": {
    "name": "Bryan Cranston",
    "id": 17419
  }
}

--
Discover
--

Discover movies by different types of data like average rating, number of votes, genres and certifications. You can get a valid list of certifications from the [/certifications](#certifications) method.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td colspan="2">api_key</td></tr>
<tr><td colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-right: 40px;">certification_country</td><td>Only include movies with certifications for a specific country. When this value is specified, you must specify a <strong>'certification'</strong> or <strong>'certification.lte'</strong> value. A ISO 3166-1 is expected.</td></tr>
<tr><td style="padding-right: 40px;">certification</td><td>Filter movies that have the specified certification. You must specify a valid 'certification_country' in order to use this filter. Expected value is a valid certification for the specificed country, (<a href="http://docs.themoviedb.apiary.io/#reference/certifications/get">see here</a> for valid values).</td></tr>
<tr><td style="padding-right: 40px;">certification.lte</td><td>Filter movies that have this certification and "lower". You must specify a valid 'certification_country' and a valid certification for the specified value, (<a href="http://docs.themoviedb.apiary.io/#reference/certifications/get">see here</a> for valid values).</td></tr>
<tr><td style="padding-right: 40px;">include_adult</td><td>Toggle the inclusion of adult titles. Expected value is a boolean, true or false. Default is false.</td></tr>
<tr><td style="padding-right: 40px;">include_video</td><td>Toggle the inclusion of items marked as a video. Expected value is a boolean, true or false. Default is true.</td></tr>
<tr><td style="padding-right: 40px;">language</td><td>ISO 639-1 code.</td></tr>
<tr><td style="padding-right: 40px; width: 140px;">page</td><td>Minimum 1, maximum 1000.</td></tr>
<tr><td style="padding-right: 40px;">primary_release_year</td><td>Filter the results so that only the primary release date year has this value. Expected value is a year.</td></tr>
<tr><td style="padding-right: 40px;">primary_release_date.gte</td><td>Filter by the primary release date and only include those which are greater than or equal to the specified value. Expected format is YYYY-MM-DD.</td></tr>
<tr><td style="padding-right: 40px;">primary_release_date.lte</td><td>Filter by the primary release date and only include those which are greater than or equal to the specified value. Expected format is YYYY-MM-DD.</td></tr>
<tr><td style="padding-right: 40px;">release_date.gte</td><td>Filter by all available release dates and only include those which are greater or equal to the specified value. Expected format is YYYY-MM-DD.</td></tr>
<tr><td style="padding-right: 40px;">release_date.lte</td><td>Filter by all available release dates and only include those which are less or equal to the specified value. Expected format is YYYY-MM-DD.</td></tr>
<tr><td style="padding-right: 40px;">sort_by</td><td>Available options are: <ul><li>popularity.asc</li><li>popularity.desc</li><li>release_date.asc</li><li>release_date.desc</li><li>revenue.asc</li><li>revenue.desc</li><li>primary_release_date.asc</li><li>primary_release_date.desc</li><li>original_title.asc</li><li>original_title.desc</li><li>vote_average.asc</li><li>vote_average.desc</li><li>vote_count.asc</li><li>vote_count.desc</li></ul></td></tr>
<tr><td style="padding-right: 40px;">vote_count.gte</td><td>Filter movies by their vote count and only include movies that have a vote count that is equal to or lower than the specified value.</td></tr>
<tr><td style="padding-right: 40px;">vote_count.lte</td><td>Filter movies by their vote count and only include movies that have a vote count that is equal to or lower than the specified value. Expected value is an integer.</td></tr>
<tr><td style="padding-right: 40px;">vote_average.gte</td><td>Filter movies by their vote average and only include those that have an average rating that is equal to or higher than the specified value. Expected value is a float.</td></tr>
<tr><td style="padding-right: 40px;">vote_average.lte</td><td>Filter movies by their vote average and only include those that have an average rating that is equal to or lower than the specified value. Expected value is a float.</td></tr>
<tr><td style="padding-right: 40px;">with_cast</td><td>Only include movies that have this person id added as a cast member. Expected value is an integer (the id of a person). Comma separated indicates an 'AND' query, while a pipe (|) separated value indicates an 'OR'.</td></tr>
<tr><td style="padding-right: 40px;">with_crew</td><td>Only include movies that have this person id added as a crew member. Expected value is an integer (the id of a person). Comma separated indicates an 'AND' query, while a pipe (|) separated value indicates an 'OR'.</td></tr>
<tr><td style="padding-right: 40px;">with_companies</td><td>Filter movies to include a specific company. Expected value is an integer (the id of a company). Comma separated indicates an 'AND' query, while a pipe (|) separated value indicates an 'OR'.</td></tr>
<tr><td style="padding-right: 40px;">with_genres</td><td>Only include movies with the specified genres. Expected value is an integer (the id of a genre). Multiple values can be specified. Comma separated indicates an 'AND' query, while a pipe (|) separated value indicates an 'OR'.</td></tr>
<tr><td style="padding-right: 40px;">with_keywords</td><td>Only include movies with the specified keywords. Expected value is an integer (the id of a keyword). Multiple values can be specified. Comma separated indicates an 'AND' query, while a pipe (|) separated value indicates an 'OR'.</td></tr>
<tr><td style="padding-right: 40px;">with_people</td><td>Only include movies that have these person id's added as a cast or crew member. Expected value is an integer (the id or ids of a person). Comma separated indicates an 'AND' query, while a pipe (|) separated value indicates an 'OR'.</td></tr>
<tr><td style="padding-right: 40px;">year</td><td>Filter the results by all available release dates that have the specified value added as a year. Expected value is an integer (year).</td></tr>
</table>

GET /discover/movie
> Accept: application/json
< 200
< Content-Type: application/json
{
  "page": 1,
  "results": [
    {
      "adult": false,
      "backdrop_path": "/dkMD5qlogeRMiEixC4YNPUvax2T.jpg",
      "genre_ids": [
        28,
        12,
        878,
        53
      ],
      "id": 135397,
      "original_language": "en",
      "original_title": "Jurassic World",
      "overview": "Twenty-two years after the events of Jurassic Park, Isla Nublar now features a fully functioning dinosaur theme park, Jurassic World, as originally envisioned by John Hammond.",
      "release_date": "2015-06-12",
      "poster_path": "/uXZYawqUsChGSj54wcuBtEdUJbh.jpg",
      "popularity": 88.551849,
      "title": "Jurassic World",
      "video": false,
      "vote_average": 7.1,
      "vote_count": 435
    },
    {
      "adult": false,
      "backdrop_path": "/jxPeRkfOoWs6gFybOa8C4xrHLrm.jpg",
      "genre_ids": [
        53,
        28,
        12
      ],
      "id": 76341,
      "original_language": "en",
      "original_title": "Mad Max: Fury Road",
      "overview": "An apocalyptic story set in the furthest reaches of our planet, in a stark desert landscape where humanity is broken, and most everyone is crazed fighting for the necessities of life. Within this world exist two rebels on the run who just might be able to restore order. There's Max, a man of action and a man of few words, who seeks peace of mind following the loss of his wife and child in the aftermath of the chaos. And Furiosa, a woman of action and a woman who believes her path to survival may be achieved if she can make it across the desert back to her childhood homeland.",
      "release_date": "2015-05-15",
      "poster_path": "/kqjL17yufvn9OVLyXYpvtyrFfak.jpg",
      "popularity": 35.88189,
      "title": "Mad Max: Fury Road",
      "video": false,
      "vote_average": 7.9,
      "vote_count": 815
    },
    {
      "adult": false,
      "backdrop_path": "/cUfGqafAVQkatQ7N4y08RNV3bgu.jpg",
      "genre_ids": [
        28,
        18,
        53
      ],
      "id": 254128,
      "original_language": "en",
      "original_title": "San Andreas",
      "overview": "In the aftermath of a massive earthquake in California, a rescue-chopper pilot makes a dangerous journey across the state in order to rescue his estranged daughter.",
      "release_date": "2015-05-29",
      "poster_path": "/6iQ4CMtYorKFfAmXEpAQZMnA0Qe.jpg",
      "popularity": 25.002,
      "title": "San Andreas",
      "video": false,
      "vote_average": 6.2,
      "vote_count": 234
    },
    {
      "adult": false,
      "backdrop_path": "/2BXd0t9JdVqCp9sKf6kzMkr7QjB.jpg",
      "genre_ids": [
        12,
        10751,
        16,
        28,
        35
      ],
      "id": 177572,
      "original_language": "en",
      "original_title": "Big Hero 6",
      "overview": "The special bond that develops between plus-sized inflatable robot Baymax, and prodigy Hiro Hamada, who team up with a group of friends to form a band of high-tech heroes.",
      "release_date": "2014-11-07",
      "poster_path": "/3zQvuSAUdC3mrx9vnSEpkFX0968.jpg",
      "popularity": 19.674015,
      "title": "Big Hero 6",
      "video": false,
      "vote_average": 7.9,
      "vote_count": 1471
    },
    {
      "adult": false,
      "backdrop_path": "/xjjO3JIdneMBTsS282JffiPqfHW.jpg",
      "genre_ids": [
        10749,
        14,
        10751,
        18
      ],
      "id": 150689,
      "original_language": "en",
      "original_title": "Cinderella",
      "overview": "When her father unexpectedly passes away, young Ella finds herself at the mercy of her cruel stepmother and her daughters. Never one to give up hope, Ella's fortunes begin to change after meeting a dashing stranger in the woods.",
      "release_date": "2015-03-13",
      "poster_path": "/2i0JH5WqYFqki7WDhUW56Sg0obh.jpg",
      "popularity": 18.919939,
      "title": "Cinderella",
      "video": false,
      "vote_average": 7.2,
      "vote_count": 276
    },
    {
      "adult": false,
      "backdrop_path": "/9eKd1DDDAbrDNXR2he7ZJEu7UkI.jpg",
      "genre_ids": [
        80,
        35,
        28,
        12
      ],
      "id": 207703,
      "original_language": "en",
      "original_title": "Kingsman: The Secret Service",
      "overview": "Kingsman: The Secret Service tells the story of a super-secret spy organization that recruits an unrefined but promising street kid into the agency's ultra-competitive training program just as a global threat emerges from a twisted tech genius.",
      "release_date": "2015-02-13",
      "poster_path": "/oAISjx6DvR2yUn9dxj00vP8OcJJ.jpg",
      "popularity": 17.71923,
      "title": "Kingsman: The Secret Service",
      "video": false,
      "vote_average": 7.7,
      "vote_count": 1044
    },
    {
      "adult": false,
      "backdrop_path": "/y5lG7TBpeOMG0jxAaTK0ghZSzBJ.jpg",
      "genre_ids": [
        28,
        878,
        53
      ],
      "id": 198184,
      "original_language": "en",
      "original_title": "Chappie",
      "overview": "Every child comes into the world full of promise, and none more so than Chappie: he is gifted, special, a prodigy. Like any child, Chappie will come under the influence of his surroundings—some good, some bad—and he will rely on his heart and soul to find his way in the world and become his own man. But there's one thing that makes Chappie different from any one else: he is a robot.",
      "release_date": "2015-03-06",
      "poster_path": "/saF3HtAduvrP9ytXDxSnQJP3oqx.jpg",
      "popularity": 17.66911,
      "title": "Chappie",
      "video": false,
      "vote_average": 6.7,
      "vote_count": 492
    },
    {
      "adult": false,
      "backdrop_path": "/xu9zaAevzQ5nnrsXN6JcahLnG4i.jpg",
      "genre_ids": [
        18,
        878
      ],
      "id": 157336,
      "original_language": "en",
      "original_title": "Interstellar",
      "overview": "Interstellar chronicles the adventures of a group of explorers who make use of a newly discovered wormhole to surpass the limitations on human space travel and conquer the vast distances involved in an interstellar voyage.",
      "release_date": "2014-11-05",
      "poster_path": "/nBNZadXqJSdt05SHLqgT0HuC5Gm.jpg",
      "popularity": 16.688744,
      "title": "Interstellar",
      "video": false,
      "vote_average": 8.4,
      "vote_count": 2485
    },
    {
      "adult": false,
      "backdrop_path": "/qhH3GyIfAnGv1pjdV3mw03qAilg.jpg",
      "genre_ids": [
        12,
        14
      ],
      "id": 122917,
      "original_language": "en",
      "original_title": "The Hobbit: The Battle of the Five Armies",
      "overview": "Mere seconds after the events of \"Desolation\", Bilbo and Company continue to claim a mountain of treasure that was guarded long ago: But with Gandalf the Grey also facing some formidable foes of his own, the Hobbit is outmatched when the brutal army of orcs led by Azog the Defiler returns. But with other armies such as the elves and the men of Lake-Town, which are unsure to be trusted, are put to the ultimate test when Smaug's wrath, Azog's sheer strength, and Sauron's force of complete ends attack. All in all, the trusted armies have two choices: unite or die. But even worse, Bilbo gets put on a knife edge and finds himself fighting with Hobbit warfare with all of his might for his dwarf-friends, as the hope for Middle-Earth is all put in Bilbo's hands. The one \"precious\" thing to end it all.",
      "release_date": "2014-12-17",
      "poster_path": "/qrFwjJ5nvFnpBCmXLI4YoeHJNBH.jpg",
      "popularity": 15.434565,
      "title": "The Hobbit: The Battle of the Five Armies",
      "video": false,
      "vote_average": 7.2,
      "vote_count": 1471
    },
    {
      "adult": false,
      "backdrop_path": "/bIlYH4l2AyYvEysmS2AOfjO7Dn8.jpg",
      "genre_ids": [
        878,
        28,
        53,
        12
      ],
      "id": 87101,
      "original_language": "en",
      "original_title": "Terminator Genisys",
      "overview": "The year is 2029. John Connor, leader of the resistance continues the war against the machines. At the Los Angeles offensive, John's fears of the unknown future begin to emerge when TECOM spies reveal a new plot by SkyNet that will attack him from both fronts; past and future, and will ultimately change warfare forever.",
      "release_date": "2015-07-01",
      "poster_path": "/qOoFD4HD9a2EEUymdzBQN9XF1UJ.jpg",
      "popularity": 15.394013,
      "title": "Terminator Genisys",
      "video": false,
      "vote_average": 7.3,
      "vote_count": 35
    },
    {
      "adult": false,
      "backdrop_path": "/4liSXBZZdURI0c1Id1zLJo6Z3Gu.jpg",
      "genre_ids": [
        878,
        14,
        28,
        12
      ],
      "id": 76757,
      "original_language": "en",
      "original_title": "Jupiter Ascending",
      "overview": "In a universe where human genetic material is the most precious commodity, an impoverished young Earth woman becomes the key to strategic maneuvers and internal strife within a powerful dynasty…",
      "release_date": "2015-02-27",
      "poster_path": "/aMEsvTUklw0uZ3gk3Q6lAj6302a.jpg",
      "popularity": 15.209428,
      "title": "Jupiter Ascending",
      "video": false,
      "vote_average": 5.4,
      "vote_count": 682
    },
    {
      "adult": false,
      "backdrop_path": "/fii9tPZTpy75qOCJBulWOb0ifGp.jpg",
      "genre_ids": [
        36,
        18,
        53,
        10752
      ],
      "id": 205596,
      "original_language": "en",
      "original_title": "The Imitation Game",
      "overview": "Based on the real life story of legendary cryptanalyst Alan Turing, the film portrays the nail-biting race against time by Turing and his brilliant team of code-breakers at Britain's top-secret Government Code and Cypher School at Bletchley Park, during the darkest days of World War II.",
      "release_date": "2014-11-14",
      "poster_path": "/noUp0XOqIcmgefRnRZa1nhtRvWO.jpg",
      "popularity": 15.06502,
      "title": "The Imitation Game",
      "video": false,
      "vote_average": 8.1,
      "vote_count": 1222
    },
    {
      "adult": false,
      "backdrop_path": "/rFtsE7Lhlc2jRWF7SRAU0fvrveQ.jpg",
      "genre_ids": [
        12,
        878,
        28
      ],
      "id": 99861,
      "original_language": "en",
      "original_title": "Avengers: Age of Ultron",
      "overview": "When Tony Stark tries to jumpstart a dormant peacekeeping program, things go awry and Earth’s Mightiest Heroes are put to the ultimate test as the fate of the planet hangs in the balance. As the villainous Ultron emerges, it is up to The Avengers to stop him from enacting his terrible plans, and soon uneasy alliances and unexpected action pave the way for an epic and unique global adventure.",
      "release_date": "2015-05-01",
      "poster_path": "/t90Y3G8UGQp0f0DrP60wRu9gfrH.jpg",
      "popularity": 14.81874,
      "title": "Avengers: Age of Ultron",
      "video": false,
      "vote_average": 7.8,
      "vote_count": 1259
    },
    {
      "adult": false,
      "backdrop_path": "/bHarw8xrmQeqf3t8HpuMY7zoK4x.jpg",
      "genre_ids": [
        878,
        14,
        12
      ],
      "id": 118340,
      "original_language": "en",
      "original_title": "Guardians of the Galaxy",
      "overview": "Light years from Earth, 26 years after being abducted, Peter Quill finds himself the prime target of a manhunt after discovering an orb wanted by Ronan the Accuser.",
      "release_date": "2014-08-01",
      "poster_path": "/9gm3lL8JMTTmc3W4BmNMCuRLdL8.jpg",
      "popularity": 14.51119,
      "title": "Guardians of the Galaxy",
      "video": false,
      "vote_average": 8.2,
      "vote_count": 2706
    },
    {
      "adult": false,
      "backdrop_path": "/kJre98tnbNXbk5L5altHkQWGwD3.jpg",
      "genre_ids": [
        28,
        12,
        878,
        9648
      ],
      "id": 158852,
      "original_language": "en",
      "original_title": "Tomorrowland",
      "overview": "Bound by a shared destiny, a bright, optimistic teen bursting with scientific curiosity and a former boy-genius inventor jaded by disillusionment embark on a danger-filled mission to unearth the secrets of an enigmatic place somewhere in time and space that exists in their collective memory as \"Tomorrowland.\"",
      "release_date": "2015-05-22",
      "poster_path": "/69Cz9VNQZy39fUE2g0Ggth6SBTM.jpg",
      "popularity": 14.07089,
      "title": "Tomorrowland",
      "video": false,
      "vote_average": 6.6,
      "vote_count": 239
    },
    {
      "adult": false,
      "backdrop_path": "/6zJCJtcxmMUU7pjL4Ss1xkeGiyX.jpg",
      "genre_ids": [
        28,
        80,
        18,
        9648,
        53
      ],
      "id": 241554,
      "original_language": "en",
      "original_title": "Run All Night",
      "overview": "Brooklyn mobster and prolific hit man Jimmy Conlon, once known as The Gravedigger, has seen better days. Longtime best friend of mob boss. Jimmy, now 55, is haunted by the sins of his past—as well as a dogged police detective who’s been one step behind Jimmy for 30 years. But when Jimmy’s estranged son, becomes a target, Jimmy must make a choice between the crime family he chose and the real family he abandoned long ago. Now, with nowhere safe to turn, Jimmy just has one night to figure out exactly where his loyalties lie and to see if he can finally make things right.",
      "release_date": "2015-03-13",
      "poster_path": "/aqNJrAxudMRNo8jg3HOUQqdl2xr.jpg",
      "popularity": 13.651485,
      "title": "Run All Night",
      "video": false,
      "vote_average": 6.4,
      "vote_count": 162
    },
    {
      "adult": false,
      "backdrop_path": "/fUn5I5f4069vwGFEEvA3HXt9xPP.jpg",
      "genre_ids": [
        878,
        12,
        53
      ],
      "id": 131631,
      "original_language": "en",
      "original_title": "The Hunger Games: Mockingjay - Part 1",
      "overview": "Katniss Everdeen reluctantly becomes the symbol of a mass rebellion against the autocratic Capitol.",
      "release_date": "2014-11-20",
      "poster_path": "/cWERd8rgbw7bCMZlwP207HUXxym.jpg",
      "popularity": 12.730425,
      "title": "The Hunger Games: Mockingjay - Part 1",
      "video": false,
      "vote_average": 7.0,
      "vote_count": 1379
    },
    {
      "adult": false,
      "backdrop_path": "/anItUS64TeGKPv6MJ99DMv7o0Z0.jpg",
      "genre_ids": [
        35,
        10402
      ],
      "id": 254470,
      "original_language": "en",
      "original_title": "Pitch Perfect 2",
      "overview": "The Bellas are back, and they are better than ever. After being humiliated in front of none other than the President of the United States of America, the Bellas are taken out of the Aca-Circuit. In order to clear their name, and regain their status, the Bellas take on a seemingly impossible task: winning an international competition no American team has ever won. In order to accomplish this monumental task, they need to strengthen the bonds of friendship and sisterhood and blow away the competition with their amazing aca-magic! With all new friends and old rivals tagging along for the trip, the Bellas can hopefully accomplish their dreams.",
      "release_date": "2015-05-15",
      "poster_path": "/qSjruLiFB4uqRtz2xheQPxG8uaB.jpg",
      "popularity": 12.179013,
      "title": "Pitch Perfect 2",
      "video": false,
      "vote_average": 7.3,
      "vote_count": 189
    },
    {
      "adult": false,
      "backdrop_path": "/8dHsvdiZLBdppKwRiZ0XZYngbeN.jpg",
      "genre_ids": [
        35
      ],
      "id": 257091,
      "original_language": "en",
      "original_title": "Get Hard",
      "overview": "When obscenely rich hedge-fund manager James is convicted of fraud and sentenced to a stretch in San Quentin, the judge gives him one month to get his affairs in order. Knowing that he won't survive more than a few minutes in prison on his own, James desperately turns to Darnell-- a black businessman who's never even had a parking ticket -- for help. As Darnell puts James through the wringer, both learn that they were wrong about many things, including each other.",
      "release_date": "2015-03-27",
      "poster_path": "/qRzUSrN4p6B7fzA5XGm4ebFg3co.jpg",
      "popularity": 11.9924,
      "title": "Get Hard",
      "video": false,
      "vote_average": 6.0,
      "vote_count": 122
    },
    {
      "adult": false,
      "backdrop_path": "/razvUuLkF7CX4XsLyj02ksC0ayy.jpg",
      "genre_ids": [
        80,
        28,
        53
      ],
      "id": 260346,
      "original_language": "en",
      "original_title": "Taken 3",
      "overview": "Ex-government operative Bryan Mills finds his life is shattered when he's falsely accused of a murder that hits close to home. As he's pursued by a savvy police inspector, Mills employs his particular set of skills to track the real killer and exact his unique brand of justice.",
      "release_date": "2015-01-09",
      "poster_path": "/c2SSjUVYawDUnQ92bmTqsZsPEiB.jpg",
      "popularity": 11.737899,
      "title": "Taken 3",
      "video": false,
      "vote_average": 6.2,
      "vote_count": 698
    }
  ],
  "total_pages": 11543,
  "total_results": 230847
}

Discover TV shows by different types of data like average rating, number of votes, genres, the network they aired on and air dates.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td colspan="2">api_key</td></tr>
<tr><td colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-right: 40px;">air_date.gte</td><td>The minimum episode air date to include. Expected format is YYYY-MM-DD. Can be used in conjunction with a specified timezone.</td></tr>
<tr><td style="padding-right: 40px;">air_date.lte</td><td>The maximum episode air date to include. Expected format is YYYY-MM-DD. Can be used in conjunction with a specified timezone.</td></tr>
<tr><td style="padding-right: 40px;">first_air_date.gte</td><td>The minimum release to include. Expected format is YYYY-MM-DD.</td></tr>
<tr><td style="padding-right: 40px;">first_air_date.lte</td><td>The maximum release to include. Expected format is YYYY-MM-DD.</td></tr>
<tr><td style="padding-right: 40px;">first_air_date_year</td><td>Filter the results release dates to matches that include this value. Expected value is a year.</td></tr>
<tr><td style="padding-right: 40px;">language</td><td>ISO 639-1 code.</td></tr>
<tr><td style="padding-right: 40px; width: 140px;">page</td><td>Minimum 1, maximum 1000.</td></tr>
<tr><td style="padding-right: 40px;">sort_by</td><td>Available options are vote_average.desc, vote_average.asc, first_air_date.desc, first_air_date.asc, popularity.desc, popularity.asc</td></tr>
<tr><td style="padding-right: 40px;">timezone</td><td>Specify a timeone to calculate proper date offsets. A list of valid timezones can be found by using the <a href="http://docs.themoviedb.apiary.io/#reference/timezones/timezoneslist/get">timezones/list</a> method.</td></tr>
<tr><td style="padding-right: 40px;">vote_average.gte</td><td>Only include TV shows that are equal to, or have a higher average rating than this value. Expected value is a float.</td></tr>
<tr><td style="padding-right: 40px;">vote_count.gte</td><td>Only include TV shows that are equal to, or have a vote count higher than this value. Expected value is an integer.</td></tr>
<tr><td style="padding-right: 40px;">with_genres</td><td>Only include TV shows with the specified genres. Expected value is an integer (the id of a genre). Multiple values can be specified. Comma separated indicates an 'AND' query, while a pipe (|) separated value indicates an 'OR'.</td></tr>
<tr><td style="padding-right: 40px;">with_networks</td><td>Filter TV shows to include a specific network. Expected value is an integer (the id of a network). Multiple values can be specified. Comma separated indicates an 'AND' query, while a pipe (|) separated value indicates an 'OR'.</td></tr>
</table>

GET /discover/tv
> Accept: application/json
< 200
< Content-Type: application/json
{
  "page": 1,
  "results": [
    {
      "backdrop_path": "/aKz3lXU71wqdslC1IYRC3yHD6yw.jpg",
      "first_air_date": "2011-04-17",
      "genre_ids": [
        10765,
        18
      ],
      "id": 1399,
      "original_language": "en",
      "original_name": "Game of Thrones",
      "overview": "Seven noble families fight for control of the mythical land of Westeros. Friction between the houses leads to full-scale war. All while a very ancient evil awakens in the farthest north. Amidst the war, a neglected military order of misfits, the Night's Watch, is all that stands between the realms of men and icy horrors beyond.\n\n",
      "origin_country": [
        "US"
      ],
      "poster_path": "/jIhL6mlT7AblhbHJgEoiBIOUVl1.jpg",
      "popularity": 36.072708,
      "name": "Game of Thrones",
      "vote_average": 9.1,
      "vote_count": 273
    },
    {
      "backdrop_path": "/kohPYEYHuQLWX3gjchmrWWOEycD.jpg",
      "first_air_date": "2015-06-12",
      "genre_ids": [
        878
      ],
      "id": 62425,
      "original_language": "en",
      "original_name": "Dark Matter",
      "overview": "The six-person crew of a derelict spaceship awakens from stasis in the farthest reaches of space. Their memories wiped clean, they have no recollection of who they are or how they got on board. The only clue to their identities is a cargo bay full of weaponry and a destination: a remote mining colony that is about to become a war zone. With no idea whose side they are on, they face a deadly decision. Will these amnesiacs turn their backs on history, or will their pasts catch up with them?",
      "origin_country": [
        "CA"
      ],
      "poster_path": "/iDSXueb3hjerXMq5w92rBP16LWY.jpg",
      "popularity": 27.373853,
      "name": "Dark Matter",
      "vote_average": 6.4,
      "vote_count": 4
    },
    {
      "backdrop_path": "/nGsNruW3W27V6r4gkyc3iiEGsKR.jpg",
      "first_air_date": "2007-09-24",
      "genre_ids": [
        35
      ],
      "id": 1418,
      "original_language": "en",
      "original_name": "The Big Bang Theory",
      "overview": "The Big Bang Theory is centered on five characters living in Pasadena, California: roommates Leonard Hofstadter and Sheldon Cooper; Penny, a waitress and aspiring actress who lives across the hall; and Leonard and Sheldon's equally geeky and socially awkward friends and co-workers, mechanical engineer Howard Wolowitz and astrophysicist Raj Koothrappali. The geekiness and intellect of the four guys is contrasted for comic effect with Penny's social skills and common sense.",
      "origin_country": [
        "US"
      ],
      "poster_path": "/8SUIoe1ENMHWLZ0fe2sMqMP3eZD.jpg",
      "popularity": 21.903248,
      "name": "The Big Bang Theory",
      "vote_average": 8.0,
      "vote_count": 181
    },
    {
      "backdrop_path": "/jXpndJTekLFYcx3xX0H3sDqFnJU.jpg",
      "first_air_date": "2015-06-02",
      "genre_ids": [
        878
      ],
      "id": 62196,
      "original_language": "en",
      "original_name": "Stitchers",
      "overview": "A young woman is recruited into a secret government agency to be “stitched” into the minds of the recently deceased, using their memories to investigate murders.",
      "origin_country": [
        "US"
      ],
      "poster_path": "/2Ni5y120OSt6lSHGDavU1nDrHHP.jpg",
      "popularity": 19.064832,
      "name": "Stitchers",
      "vote_average": 7.5,
      "vote_count": 4
    },
    {
      "backdrop_path": "/t6JB8bgobNr7qNnO1vXBOYayo5T.jpg",
      "first_air_date": "2015-06-14",
      "genre_ids": [
        18,
        878
      ],
      "id": 62822,
      "original_language": "en",
      "original_name": "Humans",
      "overview": "In a parallel present where the latest must-have gadget for any busy family is a 'Synth' - a highly-developed robotic servant that's so similar to a real human it's transforming the way we live.",
      "origin_country": [
        "GB",
        "US"
      ],
      "poster_path": "/gJCyS65ieDT827F2NR9Nx9ZLuw5.jpg",
      "popularity": 18.931591,
      "name": "Humans",
      "vote_average": 9.2,
      "vote_count": 3
    },
    {
      "backdrop_path": "/tacmO3UpPX26bb1EQz8c11lQezR.jpg",
      "first_air_date": "2015-06-16",
      "genre_ids": [
        35
      ],
      "id": 62771,
      "original_language": "en",
      "original_name": "Clipped",
      "overview": "Clipped centers on a group of barbershop coworkers who all went to high school together but ran in very different crowds. Now they find themselves working together at Buzzy's, a barbershop in Charlestown, Massachusetts.",
      "origin_country": [
        "US"
      ],
      "poster_path": "/a7Tp2hVM3aMINT9L5blINV6AweY.jpg",
      "popularity": 18.628063,
      "name": "Clipped",
      "vote_average": 2.3,
      "vote_count": 2
    },
    {
      "backdrop_path": "/bYjc2HolyI2HWGhftWsyo5HgqTC.jpg",
      "first_air_date": "2014-06-27",
      "genre_ids": [
        10751,
        35
      ],
      "id": 46028,
      "original_language": "en",
      "original_name": "Girl Meets World",
      "overview": "Based on ABC's hugely popular 1993-2000 sitcom, this comedy, set in New York City, tells the wonderfully funny, heartfelt stories that \"Boy Meets World\" is renowned for – only this time from a tween girl's perspective – as the curious and bright 7th grader Riley Matthews and her quick-witted friend Maya Fox embark on an unforgettable middle school experience. But their plans for a carefree year will be adjusted slightly under the watchful eyes of Riley's parents – dad Cory, who's also a faculty member (and their new History teacher), and mom Topanga, who owns a trendy afterschool hangout that specializes in pudding.",
      "origin_country": [
        "US"
      ],
      "poster_path": "/yeRT4hAVoCz6RjLUiwdZ8Of34Th.jpg",
      "popularity": 18.319562,
      "name": "Girl Meets World",
      "vote_average": 7.5,
      "vote_count": 1
    },
    {
      "backdrop_path": "/eSzpy96DwBujGFj0xMbXBcGcfxX.jpg",
      "first_air_date": "2008-01-19",
      "genre_ids": [
        18
      ],
      "id": 1396,
      "original_language": "en",
      "original_name": "Breaking Bad",
      "overview": "Breaking Bad is an American crime drama television series created and produced by Vince Gilligan. Set and produced in Albuquerque, New Mexico, Breaking Bad is the story of Walter White, a struggling high school chemistry teacher who is diagnosed with inoperable lung cancer at the beginning of the series. He turns to a life of crime, producing and selling methamphetamine, in order to secure his family's financial future before he dies, teaming with his former student, Jesse Pinkman. Heavily serialized, the series is known for positioning its characters in seemingly inextricable corners and has been labeled a contemporary western by its creator.",
      "origin_country": [
        "US"
      ],
      "poster_path": "/4yMXf3DW6oCL0lVPZaZM2GypgwE.jpg",
      "popularity": 18.095686,
      "name": "Breaking Bad",
      "vote_average": 8.9,
      "vote_count": 245
    },
    {
      "backdrop_path": "/sCx2r2bw49OqxnjWaU9DsPwdR0C.jpg",
      "first_air_date": "1994-09-22",
      "genre_ids": [
        35
      ],
      "id": 1668,
      "original_language": "en",
      "original_name": "Friends",
      "overview": "Friends is an American sitcom created by David Crane and Marta Kauffman, which aired on NBC from September 22, 1994 to May 6, 2004. The series revolves around a group of friends in the New York City borough of Manhattan. The series was produced by Bright/Kauffman/Crane Productions, in association with Warner Bros. Television. The original executive producers were Crane, Kauffman, and Kevin S. Bright, with numerous others being promoted in later seasons.\n\nKauffman and Crane began developing Friends under the title Insomnia Cafe in November/December 1993. They presented the idea to Bright, with whom they had previously worked, and together they pitched a seven-page treatment of the series to NBC. After several script rewrites and changes, including a second title change to Friends Like Us, the series was finally named Friends and premiered on NBC's coveted Thursday 8:30 pm timeslot. Filming for the series took place at Warner Bros. Studios in Burbank, California in front of a live studio audience. The series finale, airing on May 6, 2004, was watched by 52.5 million American viewers, making it the fourth most watched series finale in television history and the most watched episode of the decade.",
      "origin_country": [
        "US"
      ],
      "poster_path": "/7buCWBTpiPrCF5Lt023dSC60rgS.jpg",
      "popularity": 17.348013,
      "name": "Friends",
      "vote_average": 8.0,
      "vote_count": 83
    },
    {
      "backdrop_path": "/lnnrirKFGwFW18GiH3AmuYy40cz.jpg",
      "first_air_date": "1989-12-16",
      "genre_ids": [
        16,
        35,
        10751
      ],
      "id": 456,
      "original_language": "en",
      "original_name": "The Simpsons",
      "overview": "The Simpsons is an American animated sitcom created by Matt Groening for the Fox Broadcasting Company. The series is a satirical parody of a middle class American lifestyle epitomized by its family of the same name, which consists of Homer, Marge, Bart, Lisa, and Maggie. The show is set in the fictional town of Springfield and parodies American culture, society, television, and many aspects of the human condition.",
      "origin_country": [
        "US"
      ],
      "poster_path": "/yTZQkSsxUFJZJe67IenRM0AEklc.jpg",
      "popularity": 15.360667,
      "name": "The Simpsons",
      "vote_average": 8.6,
      "vote_count": 94
    },
    {
      "backdrop_path": "/trypBo0ldWAZ4ULeQDjkzzW7To3.jpg",
      "first_air_date": "2010-06-08",
      "genre_ids": [
        9648,
        18
      ],
      "id": 31917,
      "original_language": "en",
      "original_name": "Pretty Little Liars",
      "overview": "Based on the Pretty Little Liars series of young adult novels by Sara Shepard, the series follows the lives of four girls — Spencer, Hanna, Aria, and Emily — whose clique falls apart after the disappearance of their queen bee, Alison. One year later, they begin receiving messages from someone using the name \"A\" who threatens to expose their secrets — including long-hidden ones they thought only Alison knew. ",
      "origin_country": [
        "US"
      ],
      "poster_path": "/2rOLNdUPahiAq6VuoA1I6sA45cs.jpg",
      "popularity": 15.188609,
      "name": "Pretty Little Liars",
      "vote_average": 6.8,
      "vote_count": 27
    },
    {
      "backdrop_path": "/lkQ4fEbAkrHeKWq8GAbFqnwXweo.jpg",
      "first_air_date": "2013-10-10",
      "genre_ids": [
        18
      ],
      "id": 40008,
      "original_language": "en",
      "original_name": "Hannibal",
      "overview": "Hannibal is an American thriller television series developed by Bryan Fuller for NBC. The series is based on characters and elements appearing in the novel Red Dragon by Thomas Harris and focuses on the budding relationship between FBI special investigator Will Graham and Dr. Hannibal Lecter, a forensic psychiatrist destined to become Graham's most cunning enemy.",
      "origin_country": [
        "US"
      ],
      "poster_path": "/kArcGHvTb7m1v6RFHdU40jpiGZD.jpg",
      "popularity": 14.592192,
      "name": "Hannibal",
      "vote_average": 8.4,
      "vote_count": 52
    },
    {
      "backdrop_path": "/bzFDAvhN6dcUOTbXduHl9pMvdX0.jpg",
      "first_air_date": "2015-06-18",
      "genre_ids": [
        18
      ],
      "id": 62458,
      "original_language": "en",
      "original_name": "The Astronaut Wives Club",
      "overview": "Based on the book by Lily Koppel, this 10 episode limited series tells the story of the women who were key players behind some of the biggest events in American history. As America’s astronauts were launched on death-defying missions, Life Magazine documented the astronauts’ families, capturing the behind-the-scenes lives of their young wives. Overnight, these women were transformed from military spouses into American royalty. As their celebrity rose and tragedy began to touch their lives, they rallied together.",
      "origin_country": [
        "US"
      ],
      "poster_path": "/cCg0WE4tTC20gW034rQ9mm1uJvW.jpg",
      "popularity": 14.565064,
      "name": "The Astronaut Wives Club",
      "vote_average": 5.5,
      "vote_count": 1
    },
    {
      "backdrop_path": "/heXGzDx18UCQALqUKSLwasp5j18.jpg",
      "first_air_date": "2015-06-19",
      "genre_ids": [
        878
      ],
      "id": 62413,
      "original_language": "en",
      "original_name": "Killjoys",
      "overview": "An action-packed adventure series following a trio of interplanetary bounty hunters as they navigate through political turmoil, questionable morality and complicated relationships.",
      "origin_country": [
        "CA"
      ],
      "poster_path": "/ziPB638w1DGOJ0wbFhmHaew4Rz.jpg",
      "popularity": 14.522814,
      "name": "Killjoys",
      "vote_average": 0.0,
      "vote_count": 0
    },
    {
      "backdrop_path": "/v0VWoRbWpwlNwvI7TJgNcSa0NcV.jpg",
      "first_air_date": "2015-06-16",
      "genre_ids": [
        10765,
        18
      ],
      "id": 62812,
      "original_language": "en",
      "original_name": "Proof",
      "overview": "A brilliant surgeon searches for proof of life after death.",
      "origin_country": [],
      "poster_path": "/iHIdJMlle0bSAHbpWMDY0iS7q7M.jpg",
      "popularity": 14.267329,
      "name": "Proof",
      "vote_average": 7.0,
      "vote_count": 1
    },
    {
      "backdrop_path": "/gyuV7OHhsvXgVdMznBo9MTBCaGG.jpg",
      "first_air_date": "2015-06-17",
      "genre_ids": [
        18
      ],
      "id": 62681,
      "original_language": "de",
      "original_name": "Deutschland 83",
      "overview": "A gripping coming-of-age story set against the real culture wars and political events of Germany in the 1980s. The drama follows Martin Rauch as the 24 year-old East Germany native is pulled from the world as he knows it and sent to the West as an undercover spy for the Stasi foreign service. Hiding in plain sight in the West German army, he must gather the secrets of NATO military strategy. Everything is new, nothing is quite what it seems and everyone he encounters is harboring secrets, both political and personal.",
      "origin_country": [
        "DE"
      ],
      "poster_path": "/yL7XdYrbjwPQg9Xt4NUspKLyM1K.jpg",
      "popularity": 14.227498,
      "name": "Deutschland 83",
      "vote_average": 0.0,
      "vote_count": 0
    },
    {
      "backdrop_path": "/zYFQM9G5j9cRsMNMuZAX64nmUMf.jpg",
      "first_air_date": "2010-10-31",
      "genre_ids": [
        18,
        27,
        53
      ],
      "id": 1402,
      "original_language": "en",
      "original_name": "The Walking Dead",
      "overview": "The Walking Dead is an American horror drama television series developed by Frank Darabont. It is based on the comic book series of the same name by Robert Kirkman, Tony Moore, and Charlie Adlard. The series stars Andrew Lincoln as sheriff's deputy Rick Grimes, who awakens from a coma to find a post-apocalyptic world dominated by flesh-eating zombies. He sets out to find his family and encounters many other survivors along the way.",
      "origin_country": [
        "US"
      ],
      "poster_path": "/vxuoMW6YBt6UsxvMfRNwRl9LtWS.jpg",
      "popularity": 14.044885,
      "name": "The Walking Dead",
      "vote_average": 7.9,
      "vote_count": 160
    },
    {
      "backdrop_path": "/7QewmjzcCQDct2FAQC4sNolJWYL.jpg",
      "first_air_date": "2015-06-18",
      "genre_ids": [
        18
      ],
      "id": 62817,
      "original_language": "en",
      "original_name": "Complications",
      "overview": "John Ellis, a disillusioned suburban ER doctor, who finds his existence transformed when he intervenes in a drive-by shooting, saving a young boy's life and killing one of his attackers. When he learns the boy is still marked for death he finds himself compelled to save him at any cost and discovers that his life and his outlook on medicine may never be the same.",
      "origin_country": [
        "US"
      ],
      "poster_path": "/vSz47YjZ9qDDLaouyYA4s6m5aXW.jpg",
      "popularity": 13.889071,
      "name": "Complications",
      "vote_average": 0.0,
      "vote_count": 0
    },
    {
      "backdrop_path": "/dpNeXLEnuKzAvbNwveJhNEiQvXZ.jpg",
      "first_air_date": "2015-04-10",
      "genre_ids": [
        28,
        80
      ],
      "id": 61889,
      "original_language": "en",
      "original_name": "Marvel's Daredevil",
      "overview": "Lawyer-by-day Matt Murdock uses his heightened senses from being blinded as a young boy to fight crime at night on the streets of Hell’s Kitchen as Daredevil.",
      "origin_country": [
        "US"
      ],
      "poster_path": "/itrCovJkuH7I8SJ98jxrInnwm2y.jpg",
      "popularity": 13.805904,
      "name": "Marvel's Daredevil",
      "vote_average": 7.9,
      "vote_count": 33
    },
    {
      "backdrop_path": "/g3Cke7hDiIq80HFv6agwxTx8AfT.jpg",
      "first_air_date": "2009-09-23",
      "genre_ids": [
        35,
        10751
      ],
      "id": 1421,
      "original_language": "en",
      "original_name": "Modern Family",
      "overview": "Modern Family is an American comedy series which debuted on ABC on September 23, 2009. Presented in mockumentary style, the fictional characters frequently talk directly into the camera. The program tells of Jay Pritchett, his second wife, their infant son and his stepson, and his two children and their families. Christopher Lloyd and Steven Levitan conceived the series while sharing stories of their own \"modern families\".",
      "origin_country": [
        "US"
      ],
      "poster_path": "/vmokjFmPtDZySnNTQd6uqYcTjNF.jpg",
      "popularity": 13.643311,
      "name": "Modern Family",
      "vote_average": 8.4,
      "vote_count": 52
    }
  ],
  "total_pages": 3089,
  "total_results": 61761
}

--
Find
--

The find method makes it easy to search for objects in our database by an external id. For instance, an IMDB ID. This will search all objects (movies, TV shows and people) and return the results in a single response.

The supported external sources for each object are as follows:

- **Movies**: imdb_id
- **People**: imdb_id, freebase_mid, freebase_id, tvrage_id
- **TV Series**: imdb_id, freebase_mid, freebase_id, tvdb_id, tvrage_id
- **TV Seasons**: freebase_mid, freebase_id, tvdb_id, tvrage_id
- **TV Episodes**: imdb_id, freebase_mid, freebase_id, tvdb_id, tvrage_id

An example request looks like: `https://api.themoviedb.org/3/find/tt0266543?external_source=imdb_id&api_key={API_KEY}`

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
<tr><td style="padding-left: 0;" colspan="2">external_source</td></tr>
<tr><td style="padding-left: 0;" colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-left: 0; width: 140px;">language</td><td>ISO 639-1 code.</td></tr>
</table>

GET /find/{id}
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "ba34da62c6ed15baab692a5a8cba2822"
{
  "movie_results": [
    {
      "adult": false,
      "backdrop_path": "/n2vIGWw4ezslXjlP0VNxkp9wqwU.jpg",
      "genre_ids": [
        12,
        16,
        35,
        10751
      ],
      "id": 12,
      "original_language": "en",
      "original_title": "Finding Nemo",
      "overview": "A tale which follows the comedic and eventful journeys of two fish, the fretful Marlin and his young son Nemo, who are separated from each other in the Great Barrier Reef when Nemo is unexpectedly taken from his home and thrust into a fish tank in a dentist's office overlooking Sydney Harbor. Buoyed by the companionship of a friendly but forgetful fish named Dory, the overly cautious Marlin embarks on a dangerous trek and finds himself the unlikely hero of an epic journey to rescue his son.",
      "release_date": "2003-05-30",
      "poster_path": "/zjqInUwldOBa0q07fOyohYCWxWX.jpg",
      "popularity": 4.255547,
      "title": "Finding Nemo",
      "video": false,
      "vote_average": 7.2,
      "vote_count": 2417
    }
  ],
  "person_results": [],
  "tv_results": [],
  "tv_episode_results": [],
  "tv_season_results": []
}

--
Genres
--

Get the list of movie genres.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
<tr><td style="padding-left: 0;" colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-left: 0; padding-right: 40px; width: 140px;">language</td><td>ISO 639-1 code.</td></tr>
</table>

GET /genre/movie/list
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "a50dfcb86a14a582d3985d8df03992b7"
{
  "genres": [
    {
      "id": 28,
      "name": "Action"
    },
    {
      "id": 12,
      "name": "Adventure"
    },
    {
      "id": 16,
      "name": "Animation"
    },
    {
      "id": 35,
      "name": "Comedy"
    },
    {
      "id": 80,
      "name": "Crime"
    },
    {
      "id": 99,
      "name": "Documentary"
    },
    {
      "id": 18,
      "name": "Drama"
    },
    {
      "id": 10751,
      "name": "Family"
    },
    {
      "id": 14,
      "name": "Fantasy"
    },
    {
      "id": 10769,
      "name": "Foreign"
    },
    {
      "id": 36,
      "name": "History"
    },
    {
      "id": 27,
      "name": "Horror"
    },
    {
      "id": 10402,
      "name": "Music"
    },
    {
      "id": 9648,
      "name": "Mystery"
    },
    {
      "id": 10749,
      "name": "Romance"
    },
    {
      "id": 878,
      "name": "Science Fiction"
    },
    {
      "id": 10770,
      "name": "TV Movie"
    },
    {
      "id": 53,
      "name": "Thriller"
    },
    {
      "id": 10752,
      "name": "War"
    },
    {
      "id": 37,
      "name": "Western"
    }
  ]
}

Get the list of TV genres.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
<tr><td style="padding-left: 0;" colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-left: 0; padding-right: 40px; width: 140px;">language</td><td>ISO 639-1 code.</td></tr>
</table>

GET /genre/tv/list
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "e4c6e2b911d5a620828061b58b7fc3db"
{
  "genres": [
    {
      "id": 10759,
      "name": "Action & Adventure"
    },
    {
      "id": 16,
      "name": "Animation"
    },
    {
      "id": 35,
      "name": "Comedy"
    },
    {
      "id": 99,
      "name": "Documentary"
    },
    {
      "id": 18,
      "name": "Drama"
    },
    {
      "id": 10761,
      "name": "Education"
    },
    {
      "id": 10751,
      "name": "Family"
    },
    {
      "id": 10762,
      "name": "Kids"
    },
    {
      "id": 9648,
      "name": "Mystery"
    },
    {
      "id": 10763,
      "name": "News"
    },
    {
      "id": 10764,
      "name": "Reality"
    },
    {
      "id": 10765,
      "name": "Sci-Fi & Fantasy"
    },
    {
      "id": 10766,
      "name": "Soap"
    },
    {
      "id": 10767,
      "name": "Talk"
    },
    {
      "id": 10768,
      "name": "War & Politics"
    },
    {
      "id": 37,
      "name": "Western"
    }
  ]
}

Get the list of movies for a particular genre by id. By default, only movies with 10 or more votes are included.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td colspan="2">api_key</td></tr>
<tr><td colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-right: 40px; width: 140px;">page</td><td>Minimum 1, maximum 1000.</td></tr>
<tr><td style="padding-right: 40px; width: 140px;">language</td><td>ISO 639-1 code.</td></tr>
<tr><td style="padding-right: 40px; width: 140px;">include_all_movies</td><td>Toggle the inclusion of all movies and not just those with 10 or more ratings. Expected value is: true or false</td></tr>
<tr><td style="padding-right: 40px; width: 140px;">include_adult</td><td>Toggle the inclusion of adult titles. Expected value is: true or false</td></tr>
</table>

GET /genre/{id}/movies
> Accept: application/json
< 200
< Content-Type: application/json
{
    "id": 18,
    "page": 1,
    "results": [
        {
            "backdrop_path": "/6xKCYgH16UuwEGAyroLU6p8HLIn.jpg",
            "id": 238,
            "original_title": "The Godfather",
            "release_date": "1972-03-24",
            "poster_path": "/d4KNaTrltq6bpkFS01pYtyXa09m.jpg",
            "title": "The Godfather",
            "vote_average": 9.1999999999999993,
            "vote_count": 125
        },
        {
            "backdrop_path": "/3R6vDW1yBBzejsma8SzzWuxj2AE.jpg",
            "id": 311,
            "original_title": "Once Upon a Time in America",
            "release_date": "1984-02-17",
            "poster_path": "/fqP3Q7DWMFqW7mh11hWXbNwN9rz.jpg",
            "title": "Once Upon a Time in America",
            "vote_average": 9.0999999999999996,
            "vote_count": 34
        },
        {
            "backdrop_path": "/jWDegvAE8XO2XDUHP9eQYlepJv1.jpg",
            "id": 240,
            "original_title": "The Godfather: Part II",
            "release_date": "1974-12-20",
            "poster_path": "/tHbMIIF51rguMNSastqoQwR0sBs.jpg",
            "title": "The Godfather: Part II",
            "vote_average": 9.0999999999999996,
            "vote_count": 61
        },
        {
            "backdrop_path": "/8uO0gUM8aNqYLs1OsTBQiXu0fEv.jpg",
            "id": 550,
            "original_title": "Fight Club",
            "release_date": "1999-10-15",
            "poster_path": "/2lECpi35Hnbpa4y46JX0aY3AWTy.jpg",
            "title": "Fight Club",
            "vote_average": 9.0999999999999996,
            "vote_count": 175
        },
        {
            "backdrop_path": "/mpYypl40Wg2njmuflqEC3Bs3cFB.jpg",
            "id": 19,
            "original_title": "Metropolis",
            "release_date": "1927-03-13",
            "poster_path": "/6oyiv4kiTtroUr42H4kqNwg60rj.jpg",
            "title": "Metropolis",
            "vote_average": 9.0,
            "vote_count": 23
        },
        {
            "backdrop_path": "/AplR1QRswlXiM65GoifX8sDadME.jpg",
            "id": 213,
            "original_title": "North by Northwest",
            "release_date": "1959-07-17",
            "poster_path": "/rEHP8ylmL3pcsDUEA6Z5qhajYui.jpg",
            "title": "North by Northwest",
            "vote_average": 9.0,
            "vote_count": 28
        },
        {
            "backdrop_path": "/qrI7ZgBgBMIsJSbhyezrfP44frb.jpg",
            "id": 670,
            "original_title": "올드보이",
            "release_date": "2003-11-21",
            "poster_path": "/jB7ol6ry8dlqMp6kKlKHLfPke4e.jpg",
            "title": "Oldboy",
            "vote_average": 9.0,
            "vote_count": 45
        },
        {
            "backdrop_path": "/fRQATr704bMpLc5Skzpj3V9b2iW.jpg",
            "id": 197,
            "original_title": "Braveheart",
            "release_date": "1995-05-24",
            "poster_path": "/4T4xQJXSdTNbzUrlT6Eh13kiy5H.jpg",
            "title": "Braveheart",
            "vote_average": 9.0,
            "vote_count": 65
        },
        {
            "backdrop_path": "/8BPZO0Bf8TeAy8znF43z8soK3ys.jpg",
            "id": 122,
            "original_title": "The Lord of the Rings: The Return of the King",
            "release_date": "2003-12-17",
            "poster_path": "/j6NCjU6Zh7SkfIeN5zDaoTmBn4m.jpg",
            "title": "The Lord of the Rings: The Return of the King",
            "vote_average": 9.0,
            "vote_count": 135
        },
        {
            "backdrop_path": "/tv8J6uCTKsTlASa8luJqrFiJlBX.jpg",
            "id": 78,
            "original_title": "Blade Runner",
            "release_date": "1982-06-25",
            "poster_path": "/ewq1lwhOT8GqBcQH1w8D1BagwTh.jpg",
            "title": "Blade Runner",
            "vote_average": 9.0,
            "vote_count": 147
        },
        {
            "backdrop_path": "/xBKGJQsAIeweesB79KC89FpBrVr.jpg",
            "id": 278,
            "original_title": "The Shawshank Redemption",
            "release_date": "1994-09-14",
            "poster_path": "/9O7gLzmreU0nGkIB6K3BsJbzvNv.jpg",
            "title": "The Shawshank Redemption",
            "vote_average": 9.0,
            "vote_count": 173
        },
        {
            "backdrop_path": "/8oCkj3t9yvT363svYMssXqGxTRf.jpg",
            "id": 824,
            "original_title": "Moulin Rouge!",
            "release_date": "2001-05-16",
            "poster_path": "/vgYM0ZDoeQKZEGxrGAO221J5rIG.jpg",
            "title": "Moulin Rouge!",
            "vote_average": 8.9000000000000004,
            "vote_count": 11
        },
        {
            "backdrop_path": "/6yFtd1s69XRTGIOD6yIoZ3EebKx.jpg",
            "id": 7549,
            "original_title": "Fearless",
            "release_date": "2006-01-26",
            "poster_path": "/5gqM2IUANPvM5JnB4m5wePhH3Nt.jpg",
            "title": "Fearless",
            "vote_average": 8.9000000000000004,
            "vote_count": 18
        },
        {
            "backdrop_path": "/xATZyEpnZ0Z0iO9z5K8RZsraGKI.jpg",
            "id": 840,
            "original_title": "Close Encounters of the Third Kind",
            "release_date": "1977-11-16",
            "poster_path": "/mABOVIUl5lB0WF4HG28rfamgxG1.jpg",
            "title": "Close Encounters of the Third Kind",
            "vote_average": 8.9000000000000004,
            "vote_count": 19
        },
        {
            "backdrop_path": "/tEm71iwofHgNcqb0Fn1AyH11BNl.jpg",
            "id": 11036,
            "original_title": "The Notebook",
            "release_date": "2004-06-25",
            "poster_path": "/gMfstesBXKdsHToAUXVPHujUDfb.jpg",
            "title": "The Notebook",
            "vote_average": 8.9000000000000004,
            "vote_count": 20
        },
        {
            "backdrop_path": "/mtJrV7i76PtXJMmWz3tu6dWav6n.jpg",
            "id": 510,
            "original_title": "One Flew Over the Cuckoo's Nest",
            "release_date": "1975-11-19",
            "poster_path": "/bYZumDpc1o0adyuAZjXqgkZ182m.jpg",
            "title": "One Flew Over the Cuckoo's Nest",
            "vote_average": 8.9000000000000004,
            "vote_count": 38
        },
        {
            "backdrop_path": "/xDEOxA01480uLTWuvQCw61VmDBt.jpg",
            "id": 769,
            "original_title": "Goodfellas",
            "release_date": "1990-09-12",
            "poster_path": "/ddMDzejWFbSas5ipUCUCjAlD5dI.jpg",
            "title": "Goodfellas",
            "vote_average": 8.9000000000000004,
            "vote_count": 39
        },
        {
            "backdrop_path": "/9yJehIjvR7ULOLxeDJoth4kDDNe.jpg",
            "id": 5915,
            "original_title": "Into the Wild",
            "release_date": "2007-09-21",
            "poster_path": "/lHyYgaocXR6KcJLxVmxZDj115hH.jpg",
            "title": "Into the Wild",
            "vote_average": 8.9000000000000004,
            "vote_count": 41
        },
        {
            "backdrop_path": "/k4BAPrE5WkNLvpsPsiMfu8W4Zyi.jpg",
            "id": 598,
            "original_title": "Cidade de Deus",
            "release_date": "2002-05-18",
            "poster_path": "/gCqnQaq8T4CfioP9uETLx9iMJF4.jpg",
            "title": "City of God",
            "vote_average": 8.9000000000000004,
            "vote_count": 41
        },
        {
            "backdrop_path": "/ooqPNPS2WdBH7DgIF4em9e0nEld.jpg",
            "id": 857,
            "original_title": "Saving Private Ryan",
            "release_date": "1998-07-24",
            "poster_path": "/35CMz4t7PuUiQqt5h4u5nbrXZlF.jpg",
            "title": "Saving Private Ryan",
            "vote_average": 8.9000000000000004,
            "vote_count": 83
        }
    ],
    "total_pages": 25,
    "total_results": 499
}

--
Guest Sessions
--

Get a list of rated movies for a specific guest session id.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td colspan="2">api_key</td></tr>
<tr><td colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-right: 40px; width: 140px;">page</td><td>Minimum 1, maximum 1000.</td></tr>
<tr><td style="padding-right: 40px; width: 140px;">language</td><td>ISO 639-1 code.</td></tr>
<tr><td style="padding-right: 40px; width: 140px;">sort_by</td><td>Only 'created_at` is currently supported.</td></tr>
<tr><td style="padding-right: 40px; width: 140px;">sort_order</td><td>asc | desc</td></tr>
</table>

GET /guest_session/{guest_session_id}/rated/movies
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "9ae429059641e2d904096f99a7996049"
{
  "page": 1,
  "results": [
    {
      "adult": false,
      "backdrop_path": "/8uO0gUM8aNqYLs1OsTBQiXu0fEv.jpg",
      "genre_ids": [
        18
      ],
      "id": 550,
      "original_language": "en",
      "original_title": "Fight Club",
      "overview": "A ticking-time-bomb insomniac and a slippery soap salesman channel primal male aggression into a shocking new form of therapy. Their concept catches on, with underground \"fight clubs\" forming in every town, until an eccentric gets in the way and ignites an out-of-control spiral toward oblivion.",
      "release_date": "1999-10-14",
      "poster_path": "/811DjJTon9gD6hZ8nCjSitaIXFQ.jpg",
      "popularity": 3.39844,
      "title": "Fight Club",
      "video": false,
      "vote_average": 7.8,
      "vote_count": 3527,
      "rating": 7.5
    },
    {
      "adult": false,
      "backdrop_path": "/tJCyWjBGSkuzPH6fkYYi5ubBGC7.jpg",
      "genre_ids": [
        35,
        10749
      ],
      "id": 1819,
      "original_language": "en",
      "original_title": "You, Me and Dupree",
      "overview": "After standing in as best man for his longtime friend Carl Petersen, Randy Dupree loses his job, becomes a barfly and attaches himself to the newlywed couple almost permanently -- as their houseguest. But the longer Dupree camps out on their couch, the closer he gets to Carl's bride, Molly, leaving the frustrated groom wondering when his pal will be moving out.",
      "release_date": "2006-07-14",
      "poster_path": "/mtdjrysC3phP4ZtCjdTJsHfZtIU.jpg",
      "popularity": 0.435632,
      "title": "You, Me and Dupree",
      "video": false,
      "vote_average": 5.2,
      "vote_count": 118,
      "rating": 7.5
    }
  ],
  "total_pages": 1,
  "total_results": 2
}

Get a list of rated TV series for a specific guest session id.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td colspan="2">api_key</td></tr>
<tr><td colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-right: 40px; width: 140px;">page</td><td>Minimum 1, maximum 1000.</td></tr>
<tr><td style="padding-right: 40px; width: 140px;">language</td><td>ISO 639-1 code.</td></tr>
<tr><td style="padding-right: 40px; width: 140px;">sort_by</td><td>Only 'created_at` is currently supported.</td></tr>
<tr><td style="padding-right: 40px; width: 140px;">sort_order</td><td>asc | desc</td></tr>
</table>

GET /guest_session/{guest_session_id}/rated/tv
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "9ae429059641e2d904096f99a7996049"
{
  "page": 1,
  "results": [
    {
      "backdrop_path": "/aKz3lXU71wqdslC1IYRC3yHD6yw.jpg",
      "first_air_date": "2011-04-17",
      "genre_ids": [
        10765,
        18
      ],
      "id": 1399,
      "original_language": null,
      "original_name": "Game of Thrones",
      "overview": "Seven noble families fight for control of the mythical land of Westeros. Friction between the houses leads to full-scale war. All while a very ancient evil awakens in the farthest north. Amidst the war, a neglected military order of misfits, the Night's Watch, is all that stands between the realms of men and icy horrors beyond.\n\n",
      "origin_country": [
        "US"
      ],
      "poster_path": "/jIhL6mlT7AblhbHJgEoiBIOUVl1.jpg",
      "popularity": 35.072708,
      "name": "Game of Thrones",
      "vote_average": 9.1,
      "vote_count": 274,
      "rating": 9.5
    }
  ],
  "total_pages": 1,
  "total_results": 1
}

Get a list of rated TV episodes for a specific guest session id.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td colspan="2">api_key</td></tr>
<tr><td colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-right: 40px; width: 140px;">page</td><td>Minimum 1, maximum 1000.</td></tr>
<tr><td style="padding-right: 40px; width: 140px;">language</td><td>ISO 639-1 code.</td></tr>
<tr><td style="padding-right: 40px; width: 140px;">sort_by</td><td>Only 'created_at` is currently supported.</td></tr>
<tr><td style="padding-right: 40px; width: 140px;">sort_order</td><td>asc | desc</td></tr>
</table>

GET /guest_session/{guest_session_id}/rated/tv/episodes
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "9ae429059641e2d904096f99a7996049"
{
  "page": 1,
  "results": [
    {
      "air_date": "2008-01-19",
      "episode_number": 1,
      "name": "Pilot",
      "id": 62085,
      "season_number": 1,
      "still_path": "/88Z0fMP8a88EpQWMCs1593G0ngu.jpg",
      "show_id": 1396,
      "vote_average": 7.83333333333333,
      "vote_count": 18,
      "rating": 8.5
    }
  ],
  "total_pages": 1,
  "total_results": 1
}

--
Jobs
--

Get a list of valid jobs.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td><strong>Required Parameters</strong></td></tr>
<tr><td>api_key</td></tr>
</table>

GET /job/list
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "ccbdd8ac09c878d3c2ccc9590810d28a"
{
  "jobs": [
    {
      "department": "Writing",
      "job_list": [
        "Screenplay",
        "Author",
        "Novel",
        "Characters",
        "Theatre Play",
        "Adaptation",
        "Dialogue",
        "Writer",
        "Other",
        "Storyboard",
        "Original Story",
        "Scenario Writer",
        "Screenstory",
        "Musical",
        "Idea",
        "Story",
        "Creative Producer",
        "Teleplay",
        "Texte",
        "Opera",
        "Co-Writer"
      ]
    },
    {
      "department": "Directing",
      "job_list": [
        "Director",
        "Script Supervisor",
        "Other",
        "Layout",
        "Script Coordinator",
        "Special Guest Director"
      ]
    },
    {
      "department": "Actors",
      "job_list": [
        "Actor",
        "Stunt Double",
        "Voice",
        "Cameo",
        "Special Guest"
      ]
    },
    {
      "department": "Camera",
      "job_list": [
        "Director of Photography",
        "Underwater Camera",
        "Camera Operator",
        "Still Photographer",
        "Camera Department Manager",
        "Camera Supervisor",
        "Camera Technician",
        "Other",
        "Grip",
        "Steadicam Operator",
        "Additional Camera",
        "Camera Intern",
        "Additional Photography",
        "Helicopter Camera"
      ]
    },
    {
      "department": "Editing",
      "job_list": [
        "Editor",
        "Supervising Film Editor",
        "Additional Editing",
        "Editorial Manager",
        "First Assistant Editor",
        "Additional Editorial Assistant",
        "Editorial Coordinator",
        "Editorial Production Assistant",
        "Editorial Services",
        "Dialogue Editor",
        "Archival Footage Coordinator",
        "Archival Footage Research",
        "Color Timer",
        "Digital Intermediate",
        "Other"
      ]
    },
    {
      "department": "Art",
      "job_list": [
        "Production Design",
        "Art Direction",
        "Set Decoration",
        "Set Designer",
        "Conceptual Design",
        "Interior Designer",
        "Settings",
        "Assistant Art Director",
        "Art Department Coordinator",
        "Other",
        "Art Department Manager",
        "Sculptor",
        "Art Department Assistant",
        "Background Designer",
        "Co-Art Director",
        "Set Decoration Buyer",
        "Production Illustrator",
        "Standby Painter",
        "Location Scout",
        "Leadman",
        "Greensman",
        "Gun Wrangler",
        "Construction Coordinator",
        "Construction Foreman",
        "Lead Painter",
        "Sign Painter"
      ]
    },
    {
      "department": "Costume & Make-Up",
      "job_list": [
        "Costume Design",
        "Makeup Artist",
        "Hairstylist",
        "Set Dressing Artist",
        "Set Dressing Supervisor",
        "Set Dressing Manager",
        "Set Dressing Production Assistant",
        "Facial Setup Artist",
        "Hair Setup",
        "Costume Supervisor",
        "Set Costumer",
        "Makeup Department Head",
        "Wigmaker",
        "Maske",
        "Shoe Design",
        "Other",
        "Co-Costume Designer"
      ]
    },
    {
      "department": "Production",
      "job_list": [
        "Producer",
        "Executive Producer",
        "Casting",
        "Production Manager",
        "Unit Production Manager",
        "Line Producer",
        "Location Manager",
        "Other",
        "Production Supervisor",
        "Production Accountant",
        "Production Office Coordinator",
        "Finance",
        "Executive Consultant",
        "Character Technical Supervisor",
        "Development Manager",
        "Administration",
        "Executive In Charge Of Post Production",
        "Herstellungsleitung",
        "Produzent",
        "Production Director",
        "Executive In Charge Of Production",
        "Publicist"
      ]
    },
    {
      "department": "Sound",
      "job_list": [
        "Original Music Composer",
        "Sound Designer",
        "Sound Editor",
        "Sound Director",
        "Sound mixer",
        "Music Editor",
        "Sound Effects Editor",
        "Production Sound Mixer",
        "Additional Soundtrack",
        "Supervising Sound Editor",
        "Supervising Sound Effects Editor",
        "Sound Re-Recording Mixer",
        "Recording Supervision",
        "Boom Operator",
        "Sound Montage Associate",
        "Songs",
        "Music",
        "ADR & Dubbing",
        "Sound Engineer",
        "Foley",
        "Additional Music Supervisor",
        "First Assistant Sound Editor",
        "Scoring Mixer",
        "Dolby Consultant",
        "Tonbearbeitung",
        "Other"
      ]
    },
    {
      "department": "Visual Effects",
      "job_list": [
        "Animation",
        "Visual Effects",
        "Chief Technician / Stop-Motion Expert",
        "Creature Design",
        "Shading",
        "Modeling",
        "CG Painter",
        "Visual Development",
        "Animation Manager",
        "Animation Director",
        "Fix Animator",
        "Animation Department Coordinator",
        "Animation Fix Coordinator",
        "Animation Production Assistant",
        "Visual Effects Supervisor",
        "Mechanical & Creature Designer",
        "Battle Motion Coordinator",
        "Animation Supervisor",
        "VFX Supervisor",
        "Cloth Setup",
        "VFX Artist",
        "CG Engineer",
        "24 Frame Playback",
        "Imaging Science",
        "I/O Supervisor",
        "Visual Effects Producer",
        "VFX Production Coordinator",
        "I/O Manager",
        "Additional Effects Development",
        "Color Designer",
        "Simulation & Effects Production Assistant",
        "Simulation & Effects Artist"
      ]
    },
    {
      "department": "Crew",
      "job_list": [
        "Special Effects",
        "Post Production Supervisor",
        "Second Unit",
        "Choreographer",
        "Stunts",
        "Sound Recordist",
        "Stunt Coordinator",
        "Special Effects Coordinator",
        "Supervising Technical Director",
        "Supervising Animator",
        "Production Artist",
        "Sequence Leads",
        "Second Film Editor",
        "Temp Music Editor",
        "Temp Sound Editor",
        "Sequence Supervisor",
        "Software Team Lead",
        "Software Engineer",
        "Documentation & Support",
        "Machinist",
        "Photoscience Manager",
        "Department Administrator",
        "Schedule Coordinator",
        "Supervisor of Production Resources",
        "Production Office Assistant",
        "Information Systems Manager",
        "Systems Administrators & Support",
        "Projection",
        "Post Production Assistant",
        "Sound Design Assistant",
        "Mix Technician",
        "Motion Actor",
        "Sets & Props Supervisor",
        "Compositors",
        "Tattooist",
        "Sets & Props Artist",
        "Motion Capture Artist",
        "Sequence Artist",
        "Mixing Engineer",
        "Special Sound Effects",
        "Post-Production Manager",
        "Dialect Coach",
        "Picture Car Coordinator",
        "Property Master",
        "Cableman",
        "Set Production Assistant",
        "Video Assist Operator",
        "Unit Publicist",
        "Set Medic",
        "Stand In",
        "Transportation Coordinator",
        "Transportation Captain",
        "Supervising Art Director",
        "Art Direction",
        "Stunts Coordinator",
        "Post Production Consulting",
        "Production Intern",
        "Utility Stunts",
        "Actor's Assistant",
        "Set Production Intern",
        "Production Controller",
        "Studio Teachers",
        "Chef",
        "Craft Service",
        "Scenic Artist",
        "Propmaker",
        "Prop Maker",
        "Transportation Co-Captain",
        "Driver",
        "Security",
        "Second Unit Cinematographer",
        "Loader",
        "Manager of Operations",
        "Quality Control Supervisor",
        "Legal Services",
        "Public Relations",
        "Score Engineer",
        "Translator",
        "Title Graphics",
        "Telecine Colorist",
        "Comic-Zeichner",
        "Unit Production Manager",
        "Unit Production Manager",
        "Animatronic and Prosthetic Effects",
        "Martial Arts Choreographer",
        "Cinematography",
        "Steadycam",
        "Regieassistenz",
        "Executive Visual Effects Producer",
        "Visual Effects Design Consultant",
        "Digital Effects Supervisor",
        "Digital Producer",
        "CG Supervisor",
        "Visual Effects Art Director",
        "Visual Effects Editor",
        "executive in charge of Finance",
        "Associate Choreographer",
        "Makeup Effects",
        "Maskenbildner",
        "Redaktion",
        "treatment",
        "Dramaturgie",
        "Lighting Camera",
        "Technical Supervisor",
        "CGI Supervisor",
        "Creative Consultant",
        "Script",
        "Executive Music Producer",
        "Tongestaltung",
        "Commissioning Editor",
        "Klimatechnik",
        "Tiertrainer",
        "Additional Writing",
        "Additional Music",
        "Poem",
        "Thanks",
        "Szenografie",
        "Mischung",
        "Titelgestaltung",
        "Musikmischung",
        "Comic-Zeichner",
        "Creator",
        "Additional Dialogue",
        "Video Game",
        "Graphic Novel Illustrator",
        "Other",
        "Series Writer",
        "Radio Play"
      ]
    },
    {
      "department": "Lighting",
      "job_list": [
        "Lighting Technician",
        "Best Boy Electric",
        "Gaffer",
        "Lighting Technician",
        "Rigging Gaffer",
        "Lighting Supervisor",
        "Lighting Manager",
        "Directing Lighting Artist",
        "Master Lighting Artist",
        "Lighting Artist",
        "Lighting Coordinator",
        "Lighting Production Assistant",
        "Best Boy Electrician",
        "Electrician",
        "Rigging Grip",
        "Other"
      ]
    }
  ]
}

--
Keywords
--

Get the basic information for a specific keyword id.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
</table>

GET /keyword/{id}
> Accept: application/json
< 200
< Content-Type: application/json
{
    "id": 1721,
    "name": "fight"
}

Get the list of movies for a particular keyword by id.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td colspan="2">api_key</td></tr>
<tr><td colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-right: 40px; width: 140px;">page</td><td>Minimum 1, maximum 1000.</td></tr>
<tr><td style="padding-right: 40px; width: 140px;">language</td><td>ISO 639-1 code.</td></tr>
</table>

GET /keyword/{id}/movies
> Accept: application/json
< 200
< Content-Type: application/json
{
    "id": 1721,
    "page": 1,
    "results": [
        {
            "backdrop_path": "/6xKCYgH16UuwEGAyroLU6p8HLIn.jpg",
            "id": 238,
            "original_title": "The Godfather",
            "release_date": "1972-03-24",
            "poster_path": "/d4KNaTrltq6bpkFS01pYtyXa09m.jpg",
            "title": "The Godfather",
            "vote_average": 9.1999999999999993,
            "vote_count": 125
        },
        {
            "backdrop_path": "/3R6vDW1yBBzejsma8SzzWuxj2AE.jpg",
            "id": 311,
            "original_title": "Once Upon a Time in America",
            "release_date": "1984-02-17",
            "poster_path": "/fqP3Q7DWMFqW7mh11hWXbNwN9rz.jpg",
            "title": "Once Upon a Time in America",
            "vote_average": 9.0999999999999996,
            "vote_count": 34
        },
        {
            "backdrop_path": "/jWDegvAE8XO2XDUHP9eQYlepJv1.jpg",
            "id": 240,
            "original_title": "The Godfather: Part II",
            "release_date": "1974-12-20",
            "poster_path": "/tHbMIIF51rguMNSastqoQwR0sBs.jpg",
            "title": "The Godfather: Part II",
            "vote_average": 9.0999999999999996,
            "vote_count": 61
        },
        {
            "backdrop_path": "/8uO0gUM8aNqYLs1OsTBQiXu0fEv.jpg",
            "id": 550,
            "original_title": "Fight Club",
            "release_date": "1999-10-15",
            "poster_path": "/2lECpi35Hnbpa4y46JX0aY3AWTy.jpg",
            "title": "Fight Club",
            "vote_average": 9.0999999999999996,
            "vote_count": 175
        },
        {
            "backdrop_path": "/mpYypl40Wg2njmuflqEC3Bs3cFB.jpg",
            "id": 19,
            "original_title": "Metropolis",
            "release_date": "1927-03-13",
            "poster_path": "/6oyiv4kiTtroUr42H4kqNwg60rj.jpg",
            "title": "Metropolis",
            "vote_average": 9.0,
            "vote_count": 23
        },
        {
            "backdrop_path": "/AplR1QRswlXiM65GoifX8sDadME.jpg",
            "id": 213,
            "original_title": "North by Northwest",
            "release_date": "1959-07-17",
            "poster_path": "/rEHP8ylmL3pcsDUEA6Z5qhajYui.jpg",
            "title": "North by Northwest",
            "vote_average": 9.0,
            "vote_count": 28
        },
        {
            "backdrop_path": "/qrI7ZgBgBMIsJSbhyezrfP44frb.jpg",
            "id": 670,
            "original_title": "올드보이",
            "release_date": "2003-11-21",
            "poster_path": "/jB7ol6ry8dlqMp6kKlKHLfPke4e.jpg",
            "title": "Oldboy",
            "vote_average": 9.0,
            "vote_count": 45
        },
        {
            "backdrop_path": "/fRQATr704bMpLc5Skzpj3V9b2iW.jpg",
            "id": 197,
            "original_title": "Braveheart",
            "release_date": "1995-05-24",
            "poster_path": "/4T4xQJXSdTNbzUrlT6Eh13kiy5H.jpg",
            "title": "Braveheart",
            "vote_average": 9.0,
            "vote_count": 65
        },
        {
            "backdrop_path": "/8BPZO0Bf8TeAy8znF43z8soK3ys.jpg",
            "id": 122,
            "original_title": "The Lord of the Rings: The Return of the King",
            "release_date": "2003-12-17",
            "poster_path": "/j6NCjU6Zh7SkfIeN5zDaoTmBn4m.jpg",
            "title": "The Lord of the Rings: The Return of the King",
            "vote_average": 9.0,
            "vote_count": 135
        },
        {
            "backdrop_path": "/tv8J6uCTKsTlASa8luJqrFiJlBX.jpg",
            "id": 78,
            "original_title": "Blade Runner",
            "release_date": "1982-06-25",
            "poster_path": "/ewq1lwhOT8GqBcQH1w8D1BagwTh.jpg",
            "title": "Blade Runner",
            "vote_average": 9.0,
            "vote_count": 147
        },
        {
            "backdrop_path": "/xBKGJQsAIeweesB79KC89FpBrVr.jpg",
            "id": 278,
            "original_title": "The Shawshank Redemption",
            "release_date": "1994-09-14",
            "poster_path": "/9O7gLzmreU0nGkIB6K3BsJbzvNv.jpg",
            "title": "The Shawshank Redemption",
            "vote_average": 9.0,
            "vote_count": 173
        },
        {
            "backdrop_path": "/8oCkj3t9yvT363svYMssXqGxTRf.jpg",
            "id": 824,
            "original_title": "Moulin Rouge!",
            "release_date": "2001-05-16",
            "poster_path": "/vgYM0ZDoeQKZEGxrGAO221J5rIG.jpg",
            "title": "Moulin Rouge!",
            "vote_average": 8.9000000000000004,
            "vote_count": 11
        },
        {
            "backdrop_path": "/6yFtd1s69XRTGIOD6yIoZ3EebKx.jpg",
            "id": 7549,
            "original_title": "Fearless",
            "release_date": "2006-01-26",
            "poster_path": "/5gqM2IUANPvM5JnB4m5wePhH3Nt.jpg",
            "title": "Fearless",
            "vote_average": 8.9000000000000004,
            "vote_count": 18
        },
        {
            "backdrop_path": "/xATZyEpnZ0Z0iO9z5K8RZsraGKI.jpg",
            "id": 840,
            "original_title": "Close Encounters of the Third Kind",
            "release_date": "1977-11-16",
            "poster_path": "/mABOVIUl5lB0WF4HG28rfamgxG1.jpg",
            "title": "Close Encounters of the Third Kind",
            "vote_average": 8.9000000000000004,
            "vote_count": 19
        },
        {
            "backdrop_path": "/tEm71iwofHgNcqb0Fn1AyH11BNl.jpg",
            "id": 11036,
            "original_title": "The Notebook",
            "release_date": "2004-06-25",
            "poster_path": "/gMfstesBXKdsHToAUXVPHujUDfb.jpg",
            "title": "The Notebook",
            "vote_average": 8.9000000000000004,
            "vote_count": 20
        },
        {
            "backdrop_path": "/mtJrV7i76PtXJMmWz3tu6dWav6n.jpg",
            "id": 510,
            "original_title": "One Flew Over the Cuckoo's Nest",
            "release_date": "1975-11-19",
            "poster_path": "/bYZumDpc1o0adyuAZjXqgkZ182m.jpg",
            "title": "One Flew Over the Cuckoo's Nest",
            "vote_average": 8.9000000000000004,
            "vote_count": 38
        },
        {
            "backdrop_path": "/xDEOxA01480uLTWuvQCw61VmDBt.jpg",
            "id": 769,
            "original_title": "Goodfellas",
            "release_date": "1990-09-12",
            "poster_path": "/ddMDzejWFbSas5ipUCUCjAlD5dI.jpg",
            "title": "Goodfellas",
            "vote_average": 8.9000000000000004,
            "vote_count": 39
        },
        {
            "backdrop_path": "/9yJehIjvR7ULOLxeDJoth4kDDNe.jpg",
            "id": 5915,
            "original_title": "Into the Wild",
            "release_date": "2007-09-21",
            "poster_path": "/lHyYgaocXR6KcJLxVmxZDj115hH.jpg",
            "title": "Into the Wild",
            "vote_average": 8.9000000000000004,
            "vote_count": 41
        },
        {
            "backdrop_path": "/k4BAPrE5WkNLvpsPsiMfu8W4Zyi.jpg",
            "id": 598,
            "original_title": "Cidade de Deus",
            "release_date": "2002-05-18",
            "poster_path": "/gCqnQaq8T4CfioP9uETLx9iMJF4.jpg",
            "title": "City of God",
            "vote_average": 8.9000000000000004,
            "vote_count": 41
        },
        {
            "backdrop_path": "/ooqPNPS2WdBH7DgIF4em9e0nEld.jpg",
            "id": 857,
            "original_title": "Saving Private Ryan",
            "release_date": "1998-07-24",
            "poster_path": "/35CMz4t7PuUiQqt5h4u5nbrXZlF.jpg",
            "title": "Saving Private Ryan",
            "vote_average": 8.9000000000000004,
            "vote_count": 83
        }
    ],
    "total_pages": 5,
    "total_results": 84
}

--
Lists
--

Get a list by id.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
</table>

GET /list/{id}
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "d0db9614fd3971f3928f9f044a7202e0"
{
  "created_by": "Travis Bell",
  "description": "Here's my list of best picture winners for the Oscars. Thought it would be neat to see them all together. There's a lot of movies here I have never even heard of.",
  "favorite_count": 18,
  "id": "509ec17b19c2950a0600050d",
  "items": [
    {
      "adult": false,
      "backdrop_path": "/7MGIEdu0Mokqt8i7snflYRLbooI.jpg",
      "id": 68734,
      "original_title": "Argo",
      "release_date": "2012-10-12",
      "poster_path": "/oai3xLBQHpIh18VJdRCcL7D0Yg0.jpg",
      "popularity": 8.87774381845068,
      "title": "Argo",
      "vote_average": 6.6,
      "vote_count": 1379
    },
    {
      "adult": false,
      "backdrop_path": "/tr6OExnBfjLADi9OyZoW308mPAp.jpg",
      "id": 74643,
      "original_title": "The Artist",
      "release_date": "2011-10-11",
      "poster_path": "/i0mpvMIIjyubXsVKug9vX0lYpnd.jpg",
      "popularity": 2.74829074958879,
      "title": "The Artist",
      "vote_average": 7.0,
      "vote_count": 203
    },
    {
      "adult": false,
      "backdrop_path": "/hAQKDTVKk04LWn82OYWLBPpsaVa.jpg",
      "id": 45269,
      "original_title": "The King's Speech",
      "release_date": "2010-09-06",
      "poster_path": "/v8M5Sytbut7vBXyZ1HDy8lUVVcB.jpg",
      "popularity": 3.45469925426767,
      "title": "The King's Speech",
      "vote_average": 6.8,
      "vote_count": 633
    },
    {
      "adult": false,
      "backdrop_path": "/heYHgkaxA1z7IJG0pLitaXmd2Pm.jpg",
      "id": 12405,
      "original_title": "Slumdog Millionaire",
      "release_date": "2008-11-12",
      "poster_path": "/ojgf8iJpS4VX6jJfWGLpuEx0wm.jpg",
      "popularity": 5.56008500729782,
      "title": "Slumdog Millionaire",
      "vote_average": 6.9,
      "vote_count": 468
    },
    {
      "adult": false,
      "backdrop_path": "/3iPnZOXR9mpcK8RwvAW7b7Axr8v.jpg",
      "id": 12162,
      "original_title": "The Hurt Locker",
      "release_date": "2009-06-25",
      "poster_path": "/3ZEqU9Ykmn8zGDUwWnmTfHaaWRB.jpg",
      "popularity": 4.15219918317637,
      "title": "The Hurt Locker",
      "vote_average": 6.7,
      "vote_count": 365
    },
    {
      "adult": false,
      "backdrop_path": "/7hx7ANh11TbbvHLDXUuywYkg5rK.jpg",
      "id": 6977,
      "original_title": "No Country for Old Men",
      "release_date": "2007-11-08",
      "poster_path": "/6o0UWX2naW7HK45PDNYmoMIk5qs.jpg",
      "popularity": 5.10706403201925,
      "title": "No Country for Old Men",
      "vote_average": 7.0,
      "vote_count": 444
    },
    {
      "adult": false,
      "backdrop_path": "/8Od5zV7Q7zNOX0y9tyNgpTmoiGA.jpg",
      "id": 1422,
      "original_title": "The Departed",
      "release_date": "2006-10-05",
      "poster_path": "/tGLO9zw5ZtCeyyEWgbYGgsFxC6i.jpg",
      "popularity": 9.02263067356166,
      "title": "The Departed",
      "vote_average": 7.2,
      "vote_count": 1018
    },
    {
      "adult": false,
      "backdrop_path": "/o4OSztAvDYNDXkn9PJGbaQAO8VG.jpg",
      "id": 70,
      "original_title": "Million Dollar Baby",
      "release_date": "2004-12-14",
      "poster_path": "/h4VZKi2Jt4VoBYJmtC4c3bO8KqM.jpg",
      "popularity": 3.78805660450045,
      "title": "Million Dollar Baby",
      "vote_average": 6.8,
      "vote_count": 328
    },
    {
      "adult": false,
      "backdrop_path": "/nvDo8teZGqlDeHxjWkM4RBf4YMO.jpg",
      "id": 1640,
      "original_title": "Crash",
      "release_date": "2004-09-10",
      "poster_path": "/lmozwVOTY7sa9KlrSbKMqKMuKUM.jpg",
      "popularity": 3.00932841502733,
      "title": "Crash",
      "vote_average": 6.9,
      "vote_count": 266
    },
    {
      "adult": false,
      "backdrop_path": "/8BPZO0Bf8TeAy8znF43z8soK3ys.jpg",
      "id": 122,
      "original_title": "The Lord of the Rings: The Return of the King",
      "release_date": "2003-12-17",
      "poster_path": "/50LoR9gJhbWZ5PpoHgi8MNTYgzd.jpg",
      "popularity": 10.9536005909551,
      "title": "The Lord of the Rings: The Return of the King",
      "vote_average": 7.5,
      "vote_count": 3162
    },
    {
      "adult": false,
      "backdrop_path": "/qXLXEvYSycdllvdKvmhFXLUcFhM.jpg",
      "id": 1574,
      "original_title": "Chicago",
      "release_date": "2002-12-27",
      "poster_path": "/18pCc2XZ5MO7wsywOYEbhoeuxNw.jpg",
      "popularity": 1.62073245663158,
      "title": "Chicago",
      "vote_average": 5.9,
      "vote_count": 105
    },
    {
      "adult": false,
      "backdrop_path": "/5YF6MwuuKBRKLUE2dz3wetkgxAE.jpg",
      "id": 453,
      "original_title": "A Beautiful Mind",
      "release_date": "2001-12-12",
      "poster_path": "/4SFqHDZ1NvWdysucWbgnYlobdxC.jpg",
      "popularity": 35.4165683476143,
      "title": "A Beautiful Mind",
      "vote_average": 6.8,
      "vote_count": 550
    },
    {
      "adult": false,
      "backdrop_path": "/sHg0lJLtfTuGQxUz3Yie5AGW4v0.jpg",
      "id": 98,
      "original_title": "Gladiator",
      "release_date": "2000-05-01",
      "poster_path": "/6WBIzCgmDCYrqh64yDREGeDk9d3.jpg",
      "popularity": 6.73772199351565,
      "title": "Gladiator",
      "vote_average": 7.2,
      "vote_count": 1884
    },
    {
      "adult": false,
      "backdrop_path": "/mAwd34SAC8KqBKRm2MwHPLhLDU5.jpg",
      "id": 14,
      "original_title": "American Beauty",
      "release_date": "1999-09-15",
      "poster_path": "/3UBQGKS8c1dxRnDiu5kUK6ej3pP.jpg",
      "popularity": 4.095987790479,
      "title": "American Beauty",
      "vote_average": 6.9,
      "vote_count": 497
    },
    {
      "adult": false,
      "backdrop_path": "/qvhpBNfb53wLnEbVnmC5GNGFRHO.jpg",
      "id": 1934,
      "original_title": "Shakespeare in Love",
      "release_date": "1998-12-11",
      "poster_path": "/sAN5jedynbs3pa3ww0UXQ1k0lRF.jpg",
      "popularity": 3.25405271007016,
      "title": "Shakespeare in Love",
      "vote_average": 6.4,
      "vote_count": 88
    },
    {
      "adult": false,
      "backdrop_path": "/fVcZErSWa7gyENuj8IWp8eAfCnL.jpg",
      "id": 597,
      "original_title": "Titanic",
      "release_date": "1997-11-01",
      "poster_path": "/f9iH7Javzxokvnkiz2yHD1dcmUy.jpg",
      "popularity": 7.40796003893298,
      "title": "Titanic",
      "vote_average": 6.7,
      "vote_count": 2306
    },
    {
      "adult": false,
      "backdrop_path": "/cD7lPdjFyf5dQIlc6ToehqFrIZU.jpg",
      "id": 409,
      "original_title": "The English Patient",
      "release_date": "1996-11-14",
      "poster_path": "/niqc0v3Lclh99Mmmxm49qZTIo2e.jpg",
      "popularity": 1.08769436121621,
      "title": "The English Patient",
      "vote_average": 7.0,
      "vote_count": 52
    },
    {
      "adult": false,
      "backdrop_path": "/qUa7vuqh3lkVBw5IJ4nfhfoOykQ.jpg",
      "id": 197,
      "original_title": "Braveheart",
      "release_date": "1995-05-24",
      "poster_path": "/2qAgGeYdLjelOEqjW9FYvPHpplC.jpg",
      "popularity": 3.85144239936905,
      "title": "Braveheart",
      "vote_average": 7.2,
      "vote_count": 1189
    },
    {
      "adult": false,
      "backdrop_path": "/wMgbnUVS9wbRGAdki8fqxKU1O0N.jpg",
      "id": 13,
      "original_title": "Forrest Gump",
      "release_date": "1994-06-22",
      "poster_path": "/z4ROnCrL77ZMzT0MsNXY5j25wS2.jpg",
      "popularity": 10.0820028516641,
      "title": "Forrest Gump",
      "vote_average": 7.5,
      "vote_count": 1987
    },
    {
      "adult": false,
      "backdrop_path": "/af98P1bc7lJsFjhHOVWXQgNNgSQ.jpg",
      "id": 424,
      "original_title": "Schindler's List",
      "release_date": "1993-11-29",
      "poster_path": "/tvOvW7Qjj63zbQW5TZ8CjPThAUd.jpg",
      "popularity": 4.82655908578256,
      "title": "Schindler's List",
      "vote_average": 7.5,
      "vote_count": 1062
    },
    {
      "adult": false,
      "backdrop_path": "/A9YUJXy0wLwYMvnUmPnyPOriTHp.jpg",
      "id": 33,
      "original_title": "Unforgiven",
      "release_date": "1992-08-07",
      "poster_path": "/9oPodyvCWyPMZJDjg29tBfFRwtG.jpg",
      "popularity": 3.13034142901934,
      "title": "Unforgiven",
      "vote_average": 7.1,
      "vote_count": 195
    },
    {
      "adult": false,
      "backdrop_path": "/upTn8UYt1n92opIEIyzh8HljB0F.jpg",
      "id": 274,
      "original_title": "The Silence of the Lambs",
      "release_date": "1991-02-13",
      "poster_path": "/qjAyTj2BSth1EQ89vNfo0JYVPFN.jpg",
      "popularity": 1.92532169228635,
      "title": "The Silence of the Lambs",
      "vote_average": 7.3,
      "vote_count": 943
    },
    {
      "adult": false,
      "backdrop_path": "/bRT9Z3yK3SBbUMNoV4TUCEHgscG.jpg",
      "id": 581,
      "original_title": "Dances with Wolves",
      "release_date": "1990-10-19",
      "poster_path": "/hpmclspug1I8EwKSWhL7pWWltA.jpg",
      "popularity": 2.68950819207689,
      "title": "Dances with Wolves",
      "vote_average": 6.7,
      "vote_count": 199
    },
    {
      "adult": false,
      "backdrop_path": "/uK0ihZKox9aatW6KH1yjZh8wkt3.jpg",
      "id": 403,
      "original_title": "Driving Miss Daisy",
      "release_date": "1989-12-13",
      "poster_path": "/iMN2pXVh0ra5fIX3jsVDRGK9FZw.jpg",
      "popularity": 1.4339925,
      "title": "Driving Miss Daisy",
      "vote_average": 7.0,
      "vote_count": 33
    },
    {
      "adult": false,
      "backdrop_path": "/si2eYu8vpfb2Gu9jHnnbBtSXnNJ.jpg",
      "id": 380,
      "original_title": "Rain Man",
      "release_date": "1988-12-11",
      "poster_path": "/mTCpNdDTMGEyrkETRkVqRVkHpx1.jpg",
      "popularity": 3.36142354299863,
      "title": "Rain Man",
      "vote_average": 6.8,
      "vote_count": 273
    },
    {
      "adult": false,
      "backdrop_path": "/4FfjHGlzloUqJe9h3mZK1T2olty.jpg",
      "id": 746,
      "original_title": "The Last Emperor",
      "release_date": "1987-10-29",
      "poster_path": "/KTirnfG6FZLKWbm8Am3EEKmZDx.jpg",
      "popularity": 2.72225743125,
      "title": "The Last Emperor",
      "vote_average": 7.1,
      "vote_count": 27
    },
    {
      "adult": false,
      "backdrop_path": "/ufqPsRItWyFzdFz3MevDkxRF8pT.jpg",
      "id": 792,
      "original_title": "Platoon",
      "release_date": "1986-12-18",
      "poster_path": "/sYPOQI57JVNmjiLI3KeZ5KA8O9i.jpg",
      "popularity": 4.02588777113632,
      "title": "Platoon",
      "vote_average": 6.9,
      "vote_count": 229
    },
    {
      "adult": false,
      "backdrop_path": "/w6VwFCdQypTKEZAkzWtyPVii3zp.jpg",
      "id": 606,
      "original_title": "Out of Africa",
      "release_date": "1985-12-10",
      "poster_path": "/bLXD2jp0img4RJiczAwnS7m7jF9.jpg",
      "popularity": 0.971907130473401,
      "title": "Out of Africa",
      "vote_average": 7.1,
      "vote_count": 24
    },
    {
      "adult": false,
      "backdrop_path": "/sHsPEij3nojL34xcnaLdnuKEfmH.jpg",
      "id": 279,
      "original_title": "Amadeus",
      "release_date": "1984-11-13",
      "poster_path": "/flnoqdC38mbaulAeptjynOFO7yi.jpg",
      "popularity": 2.01916357491556,
      "title": "Amadeus",
      "vote_average": 6.9,
      "vote_count": 199
    },
    {
      "adult": false,
      "backdrop_path": "/8jOZrHAbHmMtcZOhFeJDtwzylRs.jpg",
      "id": 11050,
      "original_title": "Terms of Endearment",
      "release_date": "1983-11-23",
      "poster_path": "/xKna5S3i1ZuM0xlPpb0hgbcrfkx.jpg",
      "popularity": 0.95771028610751,
      "title": "Terms of Endearment",
      "vote_average": 6.6,
      "vote_count": 13
    },
    {
      "adult": false,
      "backdrop_path": "/yN4mQzPthU1l3PQWxYhzWKargL5.jpg",
      "id": 783,
      "original_title": "Gandhi",
      "release_date": "1982-11-30",
      "poster_path": "/2z9A4FSu1YySrhhcuqkdMIXpgyN.jpg",
      "popularity": 2.70371190796283,
      "title": "Gandhi",
      "vote_average": 6.9,
      "vote_count": 131
    },
    {
      "adult": false,
      "backdrop_path": "/8Fnr1gMf4dliGrfqi4382P7Lqer.jpg",
      "id": 9443,
      "original_title": "Chariots of Fire",
      "release_date": "1981-03-30",
      "poster_path": "/tDt0QreHAiM44km0Iek0UwLdyIW.jpg",
      "popularity": 2.06773614114441,
      "title": "Chariots of Fire",
      "vote_average": 6.6,
      "vote_count": 20
    },
    {
      "adult": false,
      "backdrop_path": "/ooNn9rDJye7Y6cTmGniGTe12mec.jpg",
      "id": 16619,
      "original_title": "Ordinary People",
      "release_date": "1980-09-19",
      "poster_path": "/fxXvhczta3Pqy04JoHRfyhXuyo6.jpg",
      "popularity": 0.8784252,
      "title": "Ordinary People",
      "vote_average": 8.5,
      "vote_count": 11
    },
    {
      "adult": false,
      "backdrop_path": "/uykne2LCkVuD5NEBzicCmCHFqBa.jpg",
      "id": 12102,
      "original_title": "Kramer vs. Kramer",
      "release_date": "1979-12-19",
      "poster_path": "/AwqsWJGD3P9YJKpOQ48DcXptyEy.jpg",
      "popularity": 0.913807067776869,
      "title": "Kramer vs. Kramer",
      "vote_average": 7.8,
      "vote_count": 29
    },
    {
      "adult": false,
      "backdrop_path": "/6y7tez5wmDxe3NktQQAQxO9OGKP.jpg",
      "id": 11778,
      "original_title": "The Deer Hunter",
      "release_date": "1978-12-08",
      "poster_path": "/slNJESItHPqp1CENEJQUPw8d7WE.jpg",
      "popularity": 2.20243834939047,
      "title": "The Deer Hunter",
      "vote_average": 7.2,
      "vote_count": 171
    },
    {
      "adult": false,
      "backdrop_path": "/dbNBU5LbDQLXQEX1V0jAOY7qjO3.jpg",
      "id": 703,
      "original_title": "Annie Hall",
      "release_date": "1977-04-20",
      "poster_path": "/lmTPXU1xResvcAiU5DOpmvEpJ0s.jpg",
      "popularity": 1.92268724493778,
      "title": "Annie Hall",
      "vote_average": 7.5,
      "vote_count": 67
    },
    {
      "adult": false,
      "backdrop_path": "/2kkyt0FLROrXt41IgSdE7goCFNQ.jpg",
      "id": 1366,
      "original_title": "Rocky",
      "release_date": "1976-11-21",
      "poster_path": "/lmwGr6J5y6kngFNQuFV2y1yw4OB.jpg",
      "popularity": 6.28233152794742,
      "title": "Rocky",
      "vote_average": 6.7,
      "vote_count": 318
    },
    {
      "adult": false,
      "backdrop_path": "/oXaPao83XXVtyQbr1eH8HOqnz7x.jpg",
      "id": 510,
      "original_title": "One Flew Over the Cuckoo's Nest",
      "release_date": "1975-11-18",
      "poster_path": "/srr59GKJdDXPwnWlew9NoYfOvYV.jpg",
      "popularity": 7.0259376675077,
      "title": "One Flew Over the Cuckoo's Nest",
      "vote_average": 7.5,
      "vote_count": 582
    },
    {
      "adult": false,
      "backdrop_path": "/xUU1melxrkb7IXl1F7PXrtZAWWl.jpg",
      "id": 240,
      "original_title": "The Godfather: Part II",
      "release_date": "1974-12-20",
      "poster_path": "/tHbMIIF51rguMNSastqoQwR0sBs.jpg",
      "popularity": 6.14372855467655,
      "title": "The Godfather: Part II",
      "vote_average": 7.8,
      "vote_count": 986
    },
    {
      "adult": false,
      "backdrop_path": "/qdTTCAt35taHlFeeKZwBXLuJ5z7.jpg",
      "id": 9277,
      "original_title": "The Sting",
      "release_date": "1973-12-25",
      "poster_path": "/9UL58y2Lcbr8UpiXKiomYhKTuIs.jpg",
      "popularity": 2.32875,
      "title": "The Sting",
      "vote_average": 7.5,
      "vote_count": 72
    },
    {
      "adult": false,
      "backdrop_path": "/6xKCYgH16UuwEGAyroLU6p8HLIn.jpg",
      "id": 238,
      "original_title": "The Godfather",
      "release_date": "1972-03-15",
      "poster_path": "/d4KNaTrltq6bpkFS01pYtyXa09m.jpg",
      "popularity": 8.54426972351326,
      "title": "The Godfather",
      "vote_average": 7.9,
      "vote_count": 1955
    },
    {
      "adult": false,
      "backdrop_path": "/oQl0ahcEndQGHMRfq97THau9TT1.jpg",
      "id": 1051,
      "original_title": "The French Connection",
      "release_date": "1971-10-07",
      "poster_path": "/n5O4322nJr6yM3MhUA3ixQy60jZ.jpg",
      "popularity": 1.55596784715126,
      "title": "The French Connection",
      "vote_average": 6.1,
      "vote_count": 37
    },
    {
      "adult": false,
      "backdrop_path": "/iCLuDtSSU0cFLH1oTM1828STIOB.jpg",
      "id": 11202,
      "original_title": "Patton",
      "release_date": "1970-01-25",
      "poster_path": "/6pyN7udgYaGr6uNIP2MuLUcqmPh.jpg",
      "popularity": 2.9303869443641,
      "title": "Patton",
      "vote_average": 7.0,
      "vote_count": 102
    },
    {
      "adult": false,
      "backdrop_path": "/6bkR6eMCafikq0Nvi665MwsUddk.jpg",
      "id": 3116,
      "original_title": "Midnight Cowboy",
      "release_date": "1969-05-25",
      "poster_path": "/cwC4TUi7nfpGDEdk4zRem4aSUmY.jpg",
      "popularity": 0.8787276937,
      "title": "Midnight Cowboy",
      "vote_average": 7.1,
      "vote_count": 26
    },
    {
      "adult": false,
      "backdrop_path": "/lfD3hi4AVk05sTngDvybbuLVzqR.jpg",
      "id": 17917,
      "original_title": "Oliver!",
      "release_date": "1968-09-26",
      "poster_path": "/t44XtV1pNAKYrb5jIRFpcBo0XSe.jpg",
      "popularity": 0.989556095697334,
      "title": "Oliver!",
      "vote_average": 6.8,
      "vote_count": 15
    },
    {
      "adult": false,
      "backdrop_path": "/nXBR8MS7v8V1i0yjHrmpLieaKHe.jpg",
      "id": 10633,
      "original_title": "In the Heat of the Night",
      "release_date": "1967-08-02",
      "poster_path": "/w3S3UHDpoEYiNjXgULwmbJ5ZWVa.jpg",
      "popularity": 1.21521523836591,
      "title": "In the Heat of the Night",
      "vote_average": 7.6,
      "vote_count": 9
    },
    {
      "adult": false,
      "backdrop_path": "/ivssZ1NdLhovWIEUgi9wN69dR3s.jpg",
      "id": 874,
      "original_title": "A Man for All Seasons",
      "release_date": "1966-12-12",
      "poster_path": "/mLFfDCdSH6wetMIIbG08hg51PbD.jpg",
      "popularity": 0.74956530475,
      "title": "A Man for All Seasons",
      "vote_average": 8.1,
      "vote_count": 4
    },
    {
      "adult": false,
      "backdrop_path": "/wZD4CHcQOP38OtbGAWt4OnVxIfj.jpg",
      "id": 15121,
      "original_title": "The Sound of Music",
      "release_date": "1965-03-02",
      "poster_path": "/f5hui5D3lgzWiYyImY5i5Qzrfns.jpg",
      "popularity": 2.10353453092086,
      "title": "The Sound of Music",
      "vote_average": 6.8,
      "vote_count": 260
    },
    {
      "adult": false,
      "backdrop_path": "/soKwqxI1j5rNkVfDeuarmTHetcH.jpg",
      "id": 11113,
      "original_title": "My Fair Lady",
      "release_date": "1964-10-21",
      "poster_path": "/sKEylXQWa15RFLaB54TpBI2eJuy.jpg",
      "popularity": 1.02208906195364,
      "title": "My Fair Lady",
      "vote_average": 7.4,
      "vote_count": 46
    },
    {
      "adult": false,
      "backdrop_path": "/lBwta7cIfHKSJ6m4eEPkydhkuOv.jpg",
      "id": 5769,
      "original_title": "Tom Jones",
      "release_date": "1963-10-06",
      "poster_path": "/7pn0MuzLX0tkBnNymdJtF52QQZV.jpg",
      "popularity": 0.692875,
      "title": "Tom Jones",
      "vote_average": 6.0,
      "vote_count": 2
    },
    {
      "adult": false,
      "backdrop_path": "/aSYxlJmM6393RF77ubSEYVKS5Ip.jpg",
      "id": 947,
      "original_title": "Lawrence of Arabia",
      "release_date": "1962-12-10",
      "poster_path": "/we2lCeDzaeExUk68qyTDflHAq5m.jpg",
      "popularity": 7.080669083372,
      "title": "Lawrence of Arabia",
      "vote_average": 7.1,
      "vote_count": 206
    },
    {
      "adult": false,
      "backdrop_path": "/wgjArGrch7ezI5hGjaB1oj2yb2C.jpg",
      "id": 1725,
      "original_title": "West Side Story",
      "release_date": "1961-10-18",
      "poster_path": "/zRQhCSREdR9h4OzEVvwhdlZNZ6m.jpg",
      "popularity": 0.938434080257577,
      "title": "West Side Story",
      "vote_average": 6.3,
      "vote_count": 47
    },
    {
      "adult": false,
      "backdrop_path": "/r1u2bLVO83dvNDODn2oTNLlzYjG.jpg",
      "id": 284,
      "original_title": "The Apartment",
      "release_date": "1960-06-15",
      "poster_path": "/8nrQKQjD6z0SJouKHQapXzmjFc6.jpg",
      "popularity": 2.12692886831603,
      "title": "The Apartment",
      "vote_average": 8.0,
      "vote_count": 37
    },
    {
      "adult": false,
      "backdrop_path": "/kV0wiMw24cgac7BqcppgkmUSGIw.jpg",
      "id": 665,
      "original_title": "Ben-Hur",
      "release_date": "1959-11-18",
      "poster_path": "/zKenAABNei8DgLweAuJCSYBemtk.jpg",
      "popularity": 3.62577042294738,
      "title": "Ben-Hur",
      "vote_average": 6.7,
      "vote_count": 120
    },
    {
      "adult": false,
      "backdrop_path": "/sivrafalYBFYiuHQZ1vUrbstTKe.jpg",
      "id": 17281,
      "original_title": "Gigi",
      "release_date": "1958-05-15",
      "poster_path": "/1TEeqGYW08HJoZOcEEWKTq7lGe8.jpg",
      "popularity": 0.4485,
      "title": "Gigi",
      "vote_average": 6.2,
      "vote_count": 6
    },
    {
      "adult": false,
      "backdrop_path": "/zt0iYqy0xNEnTu0RX2MfLSo2nz1.jpg",
      "id": 826,
      "original_title": "The Bridge on the River Kwai",
      "release_date": "1957-12-14",
      "poster_path": "/vtPR6tSHeu35rF6qTDw3Yjr9eDg.jpg",
      "popularity": 1.98733062344074,
      "title": "The Bridge on the River Kwai",
      "vote_average": 6.9,
      "vote_count": 81
    },
    {
      "adult": false,
      "backdrop_path": "/lYDstR3BYCkS7IMnbYwvJuHBTt4.jpg",
      "id": 2897,
      "original_title": "Around the World in Eighty Days",
      "release_date": "1956-10-17",
      "poster_path": "/7zyt7xVJzN2e9bjIiNsCP7Js74t.jpg",
      "popularity": 0.633665181014225,
      "title": "Around the World in Eighty Days",
      "vote_average": 6.5,
      "vote_count": 5
    },
    {
      "adult": false,
      "backdrop_path": "/A4v8kRlaLtnijs0aRLQJ21tEFm9.jpg",
      "id": 15919,
      "original_title": "Marty",
      "release_date": "1955-04-11",
      "poster_path": "/go3g5mfa97py2tf2jQmdCv6VSY0.jpg",
      "popularity": 0.667,
      "title": "Marty",
      "vote_average": 8.0,
      "vote_count": 3
    },
    {
      "adult": false,
      "backdrop_path": "/zXp2ydvhO9qGzpIsb1CWeKnn5yg.jpg",
      "id": 654,
      "original_title": "On the Waterfront",
      "release_date": "1954-07-28",
      "poster_path": "/sJR7CDGQZ4Rv5tL9p3LHpWPjrdI.jpg",
      "popularity": 2.06455876175876,
      "title": "On the Waterfront",
      "vote_average": 8.3,
      "vote_count": 22
    },
    {
      "adult": false,
      "backdrop_path": "/9LkxpkVwf7eGGKrBlZ4DqywwDEl.jpg",
      "id": 11426,
      "original_title": "From Here to Eternity",
      "release_date": "1953-08-05",
      "poster_path": "/bvWfIi0ZLeX4yaCH5rQWRiOT4aT.jpg",
      "popularity": 1.90440489895896,
      "title": "From Here to Eternity",
      "vote_average": 7.3,
      "vote_count": 6
    },
    {
      "adult": false,
      "backdrop_path": "/pGF0cu8ksgqgnSC4fmxkIvKLkMY.jpg",
      "id": 27191,
      "original_title": "The Greatest Show on Earth",
      "release_date": "1952-01-10",
      "poster_path": "/2Zh8VfgVgTIeqwhmmPGkNzesNV.jpg",
      "popularity": 0.721674555060053,
      "title": "The Greatest Show on Earth",
      "vote_average": 5.7,
      "vote_count": 3
    },
    {
      "adult": false,
      "backdrop_path": "/bX1cRtMnTIKnjkXteoB5ZtbOPOu.jpg",
      "id": 2769,
      "original_title": "An American in Paris",
      "release_date": "1951-10-04",
      "poster_path": "/nydhf86r3wyEiJjPL9h6tr98KPF.jpg",
      "popularity": 0.6759846095494,
      "title": "An American in Paris",
      "vote_average": 7.4,
      "vote_count": 9
    },
    {
      "adult": false,
      "backdrop_path": "/gt7HbwIA5xS1s8jrjy18t3Pfnvz.jpg",
      "id": 705,
      "original_title": "All About Eve",
      "release_date": "1950-10-13",
      "poster_path": "/2z2J3mxpGcispYfFjg1conBlrGI.jpg",
      "popularity": 0.6141996403992,
      "title": "All About Eve",
      "vote_average": 7.4,
      "vote_count": 14
    },
    {
      "adult": false,
      "backdrop_path": "/9fbRWjx3szlRKvqf3nSNxtTAkkZ.jpg",
      "id": 25430,
      "original_title": "All the King's Men",
      "release_date": "1949-11-08",
      "poster_path": "/xjICuR5RauJcZDNjLV5EvaPXhmN.jpg",
      "popularity": 0.46,
      "title": "All the King's Men",
      "vote_average": 6.8,
      "vote_count": 2
    },
    {
      "adult": false,
      "backdrop_path": "/2JJSyQX2elrTY7sqHwCl1xrsCdy.jpg",
      "id": 23383,
      "original_title": "Hamlet",
      "release_date": "1948-05-04",
      "poster_path": "/nWFbMI774M238az4k9vZk1hLyob.jpg",
      "popularity": 0.957348903011286,
      "title": "Hamlet",
      "vote_average": 6.8,
      "vote_count": 2
    },
    {
      "adult": false,
      "backdrop_path": "/3DNb1C2NHf5Sbrk5LbuNJydTjXE.jpg",
      "id": 33667,
      "original_title": "Gentleman's Agreement",
      "release_date": "1947-11-11",
      "poster_path": "/zKdVejqnpbWCyoQnudb8R0o9bMx.jpg",
      "popularity": 0.36455,
      "title": "Gentleman's Agreement",
      "vote_average": 7.3,
      "vote_count": 3
    },
    {
      "adult": false,
      "backdrop_path": "/kLTBdVlZuszlIqir4W4jOWNtbbQ.jpg",
      "id": 887,
      "original_title": "The Best Years of Our Lives",
      "release_date": "1946-11-21",
      "poster_path": "/7BeaPOkVFTKmEGaXicY3vXVhlPO.jpg",
      "popularity": 1.1628842868725,
      "title": "The Best Years of Our Lives",
      "vote_average": 7.1,
      "vote_count": 7
    },
    {
      "adult": false,
      "backdrop_path": "/rLegRrtRlZwKbqLs9H1bcTPT7Gi.jpg",
      "id": 28580,
      "original_title": "The Lost Weekend",
      "release_date": "1945-11-16",
      "poster_path": "/4oLDY92VXJxBFu4VILtyGnAeYzH.jpg",
      "popularity": 0.92,
      "title": "The Lost Weekend",
      "vote_average": 8.0,
      "vote_count": 5
    },
    {
      "adult": false,
      "backdrop_path": "/vuYHS9E4ru61uO5DuGCGUsYopu1.jpg",
      "id": 17661,
      "original_title": "Going My Way",
      "release_date": "1944-08-06",
      "poster_path": "/2VhlXVPqyU2XXjCss83oXtHgage.jpg",
      "popularity": 0.701105578521982,
      "title": "Going My Way",
      "vote_average": 7.3,
      "vote_count": 2
    },
    {
      "adult": false,
      "backdrop_path": "/nhHsH7qUySVTY57mxf231xO7Fga.jpg",
      "id": 289,
      "original_title": "Casablanca",
      "release_date": "1942-11-26",
      "poster_path": "/sm1QVZu5RKe1vXVHZooo4SZyHMx.jpg",
      "popularity": 2.62623659678994,
      "title": "Casablanca",
      "vote_average": 7.2,
      "vote_count": 323
    },
    {
      "adult": false,
      "backdrop_path": "/cv1OdM7N9VyYLEGSG8iQbQfaxZ4.jpg",
      "id": 27367,
      "original_title": "Mrs. Miniver",
      "release_date": "1942-06-04",
      "poster_path": "/kipfyFuAAVMjRRych2uH5BBgakr.jpg",
      "popularity": 0.763231375440324,
      "title": "Mrs. Miniver",
      "vote_average": 7.8,
      "vote_count": 2
    },
    {
      "adult": false,
      "backdrop_path": "/gfOagwHX2LGlfbRoIgCkVfNH8XT.jpg",
      "id": 43266,
      "original_title": "How Green Was My Valley",
      "release_date": "1941-10-28",
      "poster_path": "/ezvSKGRHVuU7U7nhpYXlHuTwGyQ.jpg",
      "popularity": 0.3335,
      "title": "How Green Was My Valley",
      "vote_average": 7.8,
      "vote_count": 3
    },
    {
      "adult": false,
      "backdrop_path": "/rKsPmJyGl75Jp6o7XS4rxROpywa.jpg",
      "id": 223,
      "original_title": "Rebecca",
      "release_date": "1940-04-12",
      "poster_path": "/nPQZKmdymaNI5ZbZf2VF5xqF666.jpg",
      "popularity": 0.910069813889976,
      "title": "Rebecca",
      "vote_average": 6.8,
      "vote_count": 33
    },
    {
      "adult": false,
      "backdrop_path": "/bGRUMgSvs6wzYQRqehY4Fsup4f1.jpg",
      "id": 770,
      "original_title": "Gone with the Wind",
      "release_date": "1939-12-15",
      "poster_path": "/2m9qsuDpAb6aBMxbKOBU1jsP9qf.jpg",
      "popularity": 2.88743571799339,
      "title": "Gone with the Wind",
      "vote_average": 6.7,
      "vote_count": 208
    },
    {
      "adult": false,
      "backdrop_path": "/w2GoONlwGRuf4Z5aQhvruVmlcSs.jpg",
      "id": 34106,
      "original_title": "You Can't Take It with You",
      "release_date": "1938-08-23",
      "poster_path": "/oqWTDqBDYOFuheepreZ1zvBLVhd.jpg",
      "popularity": 0.81285629695597,
      "title": "You Can't Take It with You",
      "vote_average": 8.0,
      "vote_count": 8
    },
    {
      "adult": false,
      "backdrop_path": "/2Z9hu3pAr6UUmztRJdxUE4eRHWt.jpg",
      "id": 43278,
      "original_title": "The Life of Emile Zola",
      "release_date": "1937-10-02",
      "poster_path": "/o5plFFBqrXVxThU7xQPqmJRgV8x.jpg",
      "popularity": 0.46,
      "title": "The Life of Emile Zola",
      "vote_average": 6.2,
      "vote_count": 3
    },
    {
      "adult": false,
      "backdrop_path": "/zrZ90x3TFnt2HAj2OjqKhQQWruz.jpg",
      "id": 43277,
      "original_title": "The Great Ziegfeld",
      "release_date": "1936-04-08",
      "poster_path": "/wyB7xg4kANE0S70S6ibX5WmQncm.jpg",
      "popularity": 0.41608112193136,
      "title": "The Great Ziegfeld",
      "vote_average": 3.5,
      "vote_count": 1
    },
    {
      "adult": false,
      "backdrop_path": "/yJdiJtbv62zuBd42eA66yBX8hfM.jpg",
      "id": 12311,
      "original_title": "Mutiny on the Bounty",
      "release_date": "1935-11-08",
      "poster_path": "/kZ8pTLL4V4s0EwE2oTyOA1fGTje.jpg",
      "popularity": 0.858371440730106,
      "title": "Mutiny on the Bounty",
      "vote_average": 7.9,
      "vote_count": 5
    },
    {
      "adult": false,
      "backdrop_path": "/3wTCVC7d8bhvX67ScQfbBJSobpM.jpg",
      "id": 3078,
      "original_title": "It Happened One Night",
      "release_date": "1934-02-22",
      "poster_path": "/3jAG2wFHSJB07TxGbFTYZttBqr4.jpg",
      "popularity": 0.817210228766524,
      "title": "It Happened One Night",
      "vote_average": 7.6,
      "vote_count": 24
    },
    {
      "adult": false,
      "backdrop_path": "/adzlio3r2bYO4rSnBhNtWQ0UuhM.jpg",
      "id": 56164,
      "original_title": "Cavalcade",
      "release_date": "1933-01-01",
      "poster_path": "/uVmkQoxPlF8Vvx2TrLOrF08STNH.jpg",
      "popularity": 0.586546,
      "title": "Cavalcade",
      "vote_average": 5.5,
      "vote_count": 1
    },
    {
      "adult": false,
      "backdrop_path": "/uiIwVCclBveUt2PVO8F7xkPe5Cj.jpg",
      "id": 33680,
      "original_title": "Grand Hotel",
      "release_date": "1932-04-14",
      "poster_path": "/zyHtotFohx0wXA7wqx2gylyUqUh.jpg",
      "popularity": 0.60988978689375,
      "title": "Grand Hotel",
      "vote_average": 8.2,
      "vote_count": 5
    },
    {
      "adult": false,
      "backdrop_path": "/vD2YkSnsXG52pOhoIulDMQCE0lL.jpg",
      "id": 42861,
      "original_title": "Cimarron",
      "release_date": "1931-01-01",
      "poster_path": "/o5TfT5lgfEfuf25S5kPG92dd8jm.jpg",
      "popularity": 0.368,
      "title": "Cimarron",
      "vote_average": 6.0,
      "vote_count": 2
    },
    {
      "adult": false,
      "backdrop_path": "/zC9aNreN9ECI61EHbQwYPAe1QHv.jpg",
      "id": 143,
      "original_title": "All Quiet on the Western Front",
      "release_date": "1930-04-21",
      "poster_path": "/qP9CwUQqfqxep7Ln7pUdAmfouf7.jpg",
      "popularity": 1.48726897632244,
      "title": "All Quiet on the Western Front",
      "vote_average": 7.7,
      "vote_count": 13
    },
    {
      "adult": false,
      "backdrop_path": "/69NfvNrpPI6bjj3Txp2cDsRZ5se.jpg",
      "id": 65203,
      "original_title": "The Broadway Melody",
      "release_date": "1929-06-06",
      "poster_path": "/dJxFUKiO7JTDBSUr4Nzct4iJikX.jpg",
      "popularity": 0.34355675,
      "title": "The Broadway Melody",
      "vote_average": 5.3,
      "vote_count": 3
    },
    {
      "adult": false,
      "backdrop_path": "/xpUbeDxBAGG30qtVn62lLU2S5SD.jpg",
      "id": 28966,
      "original_title": "Wings",
      "release_date": "1927-08-12",
      "poster_path": "/negcQiFVemelvl3RKv2iLj1MH17.jpg",
      "popularity": 1.3312092375,
      "title": "Wings",
      "vote_average": 7.9,
      "vote_count": 7
    }
  ],
  "item_count": 85,
  "iso_639_1": "en",
  "name": "Best Picture Winners - The Academy Awards",
  "poster_path": "/efBm2Nm2v5kQnO0w3hYcW6hVsJU.jpg"
}

Check to see if a movie ID is already added to a list.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
<tr><td style="padding-left: 0;">movie_id</td><td>Check to see if this movie ID (integer) is already part of a list or not.</td></tr>
</table>

GET /list/{id}/item_status
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "d1dbe6e1a870fc2a80c41e02ddf279a1"
{
  "id": "509ec17b19c2950a0600050d",
  "item_present": true
}

This method lets users create a new list. A [valid session](#authentication) id is required.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
<tr><td style="padding-left: 0;" colspan="2">session_id</td></tr>
<tr><td style="padding-left: 0;" colspan="2"><strong>Required JSON Body</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">name</td></tr>
<tr><td style="padding-left: 0;" colspan="2">description</td></tr>
<tr><td style="padding-left: 0;" colspan="2"><strong>Optional JSON Body</strong></td></tr>
<tr><td style="padding-left: 0; padding-right: 40px; width: 140px;">language</td><td>ISO 639-1 code.</td></tr>
</table>

POST /list
> Accept: application/json
> Content-Type: application/json
{
    "name": "My Totally Awesome List",
    "description": "This list was created to share all of the totally awesome movies I've seen."
}
< 200
< Content-Type: application/json
{
    "status_code": 1,
    "status_message": "Success",
    "list_id": "50941077760ee35e1500000c"
}

This method lets users add new movies to a list that they created. A [valid session](#authentication) id is required.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
<tr><td style="padding-left: 0;" colspan="2">session_id</td></tr>
<tr><td style="padding-left: 0;" colspan="2"><strong>Required JSON Body</strong></td></tr>
<tr><td style="padding-left: 0;">media_id</td><td>A movie id</td></tr>
</table>

POST /list/{id}/add_item
> Accept: application/json
> Content-Type: application/json
{
    "media_id": 550
}
< 201
< Content-Type: application/json
{
    "status_code": 12,
    "status_message": "The item/record was updated successfully"
}

This method lets users delete movies from a list that they created. A [valid session](#authentication) id is required.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
<tr><td style="padding-left: 0;" colspan="2">session_id</td></tr>
<tr><td style="padding-left: 0;" colspan="2"><strong>Required JSON Body</strong></td></tr>
<tr><td style="padding-left: 0;">media_id</td><td>A movie id</td></tr>
</table>

POST /list/{id}/remove_item
> Accept: application/json
> Content-Type: application/json
{
    "media_id": 550
}
< 201
< Content-Type: application/json
{
    "status_code": 12,
    "status_message": "The item/record was updated successfully"
}

Clear all of the items within a list. This is a irreversible action and should be treated with caution. A [valid session](#authentication) id is required. A call without `confirm=true` will return [status code 29](https://www.themoviedb.org/documentation/api/status-codes).

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
<tr><td style="padding-left: 0;">confirm</td><td>true | false</td></tr>
</table>

POST /list/{id}/clear
> Accept: application/json
< 201
< Content-Type: application/json
{
    "status_code": 12,
    "status_message": "The item/record was updated successfully"
}


This method lets users delete a list that they created. A [valid session](#authentication) id is required.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
<tr><td style="padding-left: 0;" colspan="2">session_id</td></tr>
</table>

DELETE /list/{id}
> Accept: application/json
> Content-Type: application/json
{
    "media_id": 550
}
< 200
< Content-Type: application/json
{
    "status_code": 13,
    "status_message": "The item/record was deleted successfully"
}

--
Movies
--

Get the basic movie information for a specific movie id.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
<tr><td style="padding-left: 0;" colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-left: 0; width: 140px;">language</td><td>ISO 639-1 code.</td></tr>
<tr><td style="padding-left: 0; width: 140px;">append_to_response</td><td>Comma separated, any movie method</td></tr>
</table>

GET /movie/{id}
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "517bfb9f394e27a444a8fcba967944d0"
{
  "adult": false,
  "backdrop_path": "/hNFMawyNDWZKKHU4GYCBz1krsRM.jpg",
  "belongs_to_collection": null,
  "budget": 63000000,
  "genres": [
    {
      "id": 18,
      "name": "Drama"
    }
  ],
  "homepage": "",
  "id": 550,
  "imdb_id": "tt0137523",
  "original_language": "en",
  "original_title": "Fight Club",
  "overview": "A ticking-time-bomb insomniac and a slippery soap salesman channel primal male aggression into a shocking new form of therapy. Their concept catches on, with underground \"fight clubs\" forming in every town, until an eccentric gets in the way and ignites an out-of-control spiral toward oblivion.",
  "popularity": 2.50307202280779,
  "poster_path": "/2lECpi35Hnbpa4y46JX0aY3AWTy.jpg",
  "production_companies": [
    {
      "name": "20th Century Fox",
      "id": 25
    },
    {
      "name": "Fox 2000 Pictures",
      "id": 711
    },
    {
      "name": "Regency Enterprises",
      "id": 508
    }
  ],
  "production_countries": [
    {
      "iso_3166_1": "DE",
      "name": "Germany"
    },
    {
      "iso_3166_1": "US",
      "name": "United States of America"
    }
  ],
  "release_date": "1999-10-14",
  "revenue": 100853753,
  "runtime": 139,
  "spoken_languages": [
    {
      "iso_639_1": "en",
      "name": "English"
    }
  ],
  "status": "Released",
  "tagline": "How much can you know about yourself if you've never been in a fight?",
  "title": "Fight Club",
  "video": false,
  "vote_average": 7.7,
  "vote_count": 3185
}

This method lets a TMDb user account get the status of whether or not the movie has been rated or added to their favourite or movie watch list. A [valid session](#authentication) id is required.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
<tr><td style="padding-left: 0;" colspan="2">session_id</td></tr>
</table>

GET /movie/{id}/account_states
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "6c0d967beb5470908268734d7c112ea0"
{
  "id": 550,
  "favorite": false,
  "rated": {
    "value": 7.5
  },
  "watchlist": false
}

Get the alternative titles for a specific movie id.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
<tr><td style="padding-left: 0;" colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-left: 0; width: 140px;">country</td><td>ISO 3166-1 code.</td></tr>
<tr><td style="padding-left: 0; width: 140px;">append_to_response</td><td>Comma separated, any movie method</td></tr>
</table>

GET /movie/{id}/alternative_titles
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "a7664969471f59db2c272c672df92ae1"
{
    "id": 550,
    "titles": [
        {
            "iso_3166_1": "PL",
            "title": "Podziemny krąg"
        },
        {
            "iso_3166_1": "TW",
            "title": "鬥陣俱樂部"
        }
    ]
}

Get the cast and crew information for a specific movie id.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
<tr><td style="padding-left: 0;" colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-left: 0; width: 140px;">append_to_response</td><td>Comma separated, any movie method</td></tr>
</table>

GET /movie/{id}/credits
> Accept: application/json
< 200
< Content-Type: application/json
{
  "id": 550,
  "cast": [
    {
      "cast_id": 4,
      "character": "The Narrator",
      "credit_id": "52fe4250c3a36847f80149f3",
      "id": 819,
      "name": "Edward Norton",
      "order": 0,
      "profile_path": "/iUiePUAQKN4GY6jorH9m23cbVli.jpg"
    },
    {
      "cast_id": 5,
      "character": "Tyler Durden",
      "credit_id": "52fe4250c3a36847f80149f7",
      "id": 287,
      "name": "Brad Pitt",
      "order": 1,
      "profile_path": "/kc3M04QQAuZ9woUvH3Ju5T7ZqG5.jpg"
    },
    {
      "cast_id": 6,
      "character": "Marla Singer",
      "credit_id": "52fe4250c3a36847f80149fb",
      "id": 1283,
      "name": "Helena Bonham Carter",
      "order": 2,
      "profile_path": "/58oJPFG1wefMC0Vj7sFzHPrm67J.jpg"
    },
    {
      "cast_id": 7,
      "character": "Robert 'Bob' Paulson",
      "credit_id": "52fe4250c3a36847f80149ff",
      "id": 7470,
      "name": "Meat Loaf",
      "order": 3,
      "profile_path": "/pwNyXgegO1nlZ8uWT847JM8EjGj.jpg"
    },
    {
      "cast_id": 30,
      "character": "Angel Face",
      "credit_id": "52fe4250c3a36847f8014a51",
      "id": 7499,
      "name": "Jared Leto",
      "order": 4,
      "profile_path": "/msugySeTCyCmlRWtyB6sMixTQYY.jpg"
    },
    {
      "cast_id": 31,
      "character": "Richard Chesler",
      "credit_id": "52fe4250c3a36847f8014a55",
      "id": 7471,
      "name": "Zach Grenier",
      "order": 5,
      "profile_path": "/jghYiKdNkVehKpiVyE97AWrU9KQ.jpg"
    },
    {
      "cast_id": 32,
      "character": "The Mechanic",
      "credit_id": "52fe4250c3a36847f8014a59",
      "id": 7497,
      "name": "Holt McCallany",
      "order": 6,
      "profile_path": "/hQBfcw9KVszdenlTZTR8AIrSpex.jpg"
    },
    {
      "cast_id": 33,
      "character": "Ricky",
      "credit_id": "52fe4250c3a36847f8014a5d",
      "id": 7498,
      "name": "Eion Bailey",
      "order": 7,
      "profile_path": "/4MnRgrwuiJvHsfoiJrIUL4TkfoC.jpg"
    },
    {
      "cast_id": 34,
      "character": "Intern",
      "credit_id": "52fe4250c3a36847f8014a61",
      "id": 7472,
      "name": "Richmond Arquette",
      "order": 8,
      "profile_path": "/xW4bb2qsPYilO27ex4Xoy2Ik1Ai.jpg"
    },
    {
      "cast_id": 35,
      "character": "Thomas",
      "credit_id": "52fe4250c3a36847f8014a65",
      "id": 7219,
      "name": "David Andrews",
      "order": 9,
      "profile_path": "/pxmxn29UHW9r6uvLrd7bEwLswlQ.jpg"
    },
    {
      "cast_id": 36,
      "character": "Group Leader",
      "credit_id": "52fe4250c3a36847f8014a69",
      "id": 68277,
      "name": "Christina Cabot",
      "order": 10,
      "profile_path": "/7UBTv5lW6apPdVLnOqTTBMTJWwY.jpg"
    },
    {
      "cast_id": 37,
      "character": "Inspector Bird",
      "credit_id": "52fe4250c3a36847f8014a6d",
      "id": 956719,
      "name": "Tim De Zarn",
      "order": 11,
      "profile_path": "/bvj6Kaq1VzAEBkqCGVDvOaQKOhi.jpg"
    },
    {
      "cast_id": 38,
      "character": "Inspector Dent",
      "credit_id": "52fe4250c3a36847f8014a71",
      "id": 59285,
      "name": "Ezra Buzzington",
      "order": 12,
      "profile_path": "/dl0SIqpOqS05UpJHKuDQqZTwUvP.jpg"
    },
    {
      "cast_id": 39,
      "character": "Airport Security Officer",
      "credit_id": "52fe4250c3a36847f8014a75",
      "id": 17449,
      "name": "Bob Stephenson",
      "order": 13,
      "profile_path": "/BdlHJDys3K6tMKGR8zIB3QMw4o.jpg"
    },
    {
      "cast_id": 40,
      "character": "Walter",
      "credit_id": "52fe4250c3a36847f8014a79",
      "id": 56112,
      "name": "David Lee Smith",
      "order": 14,
      "profile_path": "/xYkMA9AWtUN93KV5hWzlDkcnebB.jpg"
    },
    {
      "cast_id": 41,
      "character": "Detective Stern",
      "credit_id": "52fe4250c3a36847f8014a7d",
      "id": 1219497,
      "name": "Thom Gossom Jr.",
      "order": 15,
      "profile_path": "/8je5ISnUinU4RfjRGqW0ktZLneX.jpg"
    },
    {
      "cast_id": 42,
      "character": "Lou's Body Guard",
      "credit_id": "52fe4250c3a36847f8014a81",
      "id": 42824,
      "name": "Carl Ciarfalio",
      "order": 16,
      "profile_path": "/1JyIKBSkpK1tADOXpYYrO1khcQH.jpg"
    },
    {
      "cast_id": 43,
      "character": "Car Salesman",
      "credit_id": "52fe4251c3a36847f8014a85",
      "id": 40277,
      "name": "Stuart Blumberg",
      "order": 17,
      "profile_path": "/nvHQBUin3CXD0kBsET1KBNaiekW.jpg"
    },
    {
      "cast_id": 44,
      "character": "Man #2 at Auto Shop",
      "credit_id": "52fe4251c3a36847f8014a89",
      "id": 122805,
      "name": "Mark Fite",
      "order": 18,
      "profile_path": "/A46OLuNRFPu1NA61VQBf0NzQNFN.jpg"
    },
    {
      "cast_id": 45,
      "character": "Seminary Student",
      "credit_id": "52fe4251c3a36847f8014a8d",
      "id": 35521,
      "name": "Matt Winston",
      "order": 19,
      "profile_path": "/8FQ6ef5Zjc9T8HsfMDRI3jhK2fV.jpg"
    },
    {
      "cast_id": 46,
      "character": "Channel 4 Reporter",
      "credit_id": "52fe4251c3a36847f8014a91",
      "id": 1224996,
      "name": "Lauren Sánchez",
      "order": 20,
      "profile_path": "/iQ16P9a8TEzb4WsN8fjLsMCtvMA.jpg"
    },
    {
      "cast_id": 47,
      "character": "Policeman",
      "credit_id": "52fe4251c3a36847f8014a95",
      "id": 109100,
      "name": "David Jean Thomas",
      "order": 21,
      "profile_path": "/f5YBSiswUU9rctXbJQoXi0CdJBn.jpg"
    },
    {
      "cast_id": 48,
      "character": "Salvator, Winking Bartender",
      "credit_id": "52fe4251c3a36847f8014a99",
      "id": 1221838,
      "name": "Paul Carafotes",
      "order": 22,
      "profile_path": "/mZQtztyVeIWD7586FhvJLeVYNM3.jpg"
    },
    {
      "cast_id": 49,
      "character": "Proprietor of Dry Cleaners",
      "credit_id": "52fe4251c3a36847f8014a9d",
      "id": 145531,
      "name": "Christopher John Fields",
      "order": 23,
      "profile_path": "/jTWw4B74VhrPo8AN6Q9jq31eYDD.jpg"
    },
    {
      "cast_id": 50,
      "character": "Bartender in Halo",
      "credit_id": "52fe4251c3a36847f8014aa1",
      "id": 9291,
      "name": "Michael Shamus Wiles",
      "order": 24,
      "profile_path": "/upfSW6BGze446iqsZRehzcToNm8.jpg"
    },
    {
      "cast_id": 51,
      "character": "Detective Andrew",
      "credit_id": "52fe4251c3a36847f8014aa5",
      "id": 41352,
      "name": "Van Quattro",
      "order": 25,
      "profile_path": "/kNmOCRKD6PyG8t9tcDOpBFOrast.jpg"
    },
    {
      "cast_id": 52,
      "character": "Detective Kevin",
      "credit_id": "52fe4251c3a36847f8014aa9",
      "id": 1226835,
      "name": "Markus Redmond",
      "order": 26,
      "profile_path": "/yxMbPCGa8rMSrquc8v4UN7QLlWX.jpg"
    }
  ],
  "crew": [
    {
      "credit_id": "52fe4250c3a36847f80149ef",
      "department": "Writing",
      "id": 7469,
      "job": "Author",
      "name": "Jim Uhls",
      "profile_path": null
    },
    {
      "credit_id": "52fe4250c3a36847f8014a05",
      "department": "Production",
      "id": 7474,
      "job": "Producer",
      "name": "Ross Grayson Bell",
      "profile_path": null
    },
    {
      "credit_id": "52fe4250c3a36847f8014a0b",
      "department": "Production",
      "id": 7475,
      "job": "Producer",
      "name": "Ceán Chaffin",
      "profile_path": null
    },
    {
      "credit_id": "52fe4250c3a36847f8014a11",
      "department": "Production",
      "id": 1254,
      "job": "Producer",
      "name": "Art Linson",
      "profile_path": "/dEtVivCXxQBtIzmJcUNupT1AB4H.jpg"
    },
    {
      "credit_id": "52fe4250c3a36847f8014a17",
      "department": "Sound",
      "id": 7477,
      "job": "Original Music Composer",
      "name": "John King",
      "profile_path": null
    },
    {
      "credit_id": "52fe4250c3a36847f8014a1d",
      "department": "Sound",
      "id": 7478,
      "job": "Original Music Composer",
      "name": "Michael Simpson",
      "profile_path": null
    },
    {
      "credit_id": "52fe4250c3a36847f8014a23",
      "department": "Camera",
      "id": 7479,
      "job": "Director of Photography",
      "name": "Jeff Cronenweth",
      "profile_path": null
    },
    {
      "credit_id": "52fe4250c3a36847f8014a29",
      "department": "Editing",
      "id": 7480,
      "job": "Editor",
      "name": "James Haygood",
      "profile_path": null
    },
    {
      "credit_id": "52fe4250c3a36847f8014a2f",
      "department": "Production",
      "id": 7481,
      "job": "Casting",
      "name": "Laray Mayfield",
      "profile_path": null
    },
    {
      "credit_id": "52fe4250c3a36847f8014a35",
      "department": "Art",
      "id": 1303,
      "job": "Production Design",
      "name": "Alex McDowell",
      "profile_path": null
    },
    {
      "credit_id": "52fe4250c3a36847f8014a3b",
      "department": "Sound",
      "id": 7763,
      "job": "Sound Editor",
      "name": "Ren Klyce",
      "profile_path": null
    },
    {
      "credit_id": "52fe4250c3a36847f8014a41",
      "department": "Sound",
      "id": 7764,
      "job": "Sound Editor",
      "name": "Richard Hymns",
      "profile_path": null
    },
    {
      "credit_id": "52fe4250c3a36847f8014a47",
      "department": "Directing",
      "id": 7467,
      "job": "Director",
      "name": "David Fincher",
      "profile_path": "/dcBHejOsKvzVZVozWJAPzYthb8X.jpg"
    },
    {
      "credit_id": "52fe4250c3a36847f8014a4d",
      "department": "Writing",
      "id": 7468,
      "job": "Novel",
      "name": "Chuck Palahniuk",
      "profile_path": "/8nOJDJ6SqwV2h7PjdLBDTvIxXvx.jpg"
    }
  ]
}

Get the images (posters and backdrops) for a specific movie id.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
<tr><td style="padding-left: 0;" colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-left: 0; width: 140px;">language</td><td>ISO 639-1 code.</td></tr>
<tr><td style="padding-left: 0; width: 140px;">append_to_response</td><td>Comma separated, any movie method</td></tr>
<tr><td style="padding-left: 0; width: 140px;">include_image_language</td><td>Comma separated, a valid ISO 69-1. Maximum 5 per request.</td></tr>
</table>

GET /movie/{id}/images
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "362a205aa9b3eeef6c7e28444ccf2f10"
{
    "id": 550,
    "backdrops": [
        {
            "file_path": "/8uO0gUM8aNqYLs1OsTBQiXu0fEv.jpg",
            "width": 1280,
            "height": 720,
            "iso_639_1": null,
            "aspect_ratio": 1.78,
            "vote_average": 6.6470588235294121,
            "vote_count": 17
        },
        {
            "file_path": "/8T0hpqWsgzKhWGsXGD8ilkwRPkC.jpg",
            "width": 1280,
            "height": 720,
            "iso_639_1": null,
            "aspect_ratio": 1.78,
            "vote_average": 6.375,
            "vote_count": 12
        },
        {
            "file_path": "/hNFMawyNDWZKKHU4GYCBz1krsRM.jpg",
            "width": 1280,
            "height": 720,
            "iso_639_1": null,
            "aspect_ratio": 1.78,
            "vote_average": 5.7142857142857144,
            "vote_count": 14
        },
        {
            "file_path": "/mMZRKb3NVo5ZeSPEIaNW9buLWQ0.jpg",
            "width": 1920,
            "height": 1080,
            "iso_639_1": null,
            "aspect_ratio": 1.78,
            "vote_average": 5.5625,
            "vote_count": 8
        },
        {
            "file_path": "/9Kr6UzouF674Smw3D9Hp2DlH1Vo.jpg",
            "width": 1280,
            "height": 720,
            "iso_639_1": null,
            "aspect_ratio": 1.78,
            "vote_average": 5.5499999999999998,
            "vote_count": 10
        },
        {
            "file_path": "/eTVdpy2JXaGFit2V2ToZ79v9D7R.jpg",
            "width": 1920,
            "height": 1080,
            "iso_639_1": null,
            "aspect_ratio": 1.78,
            "vote_average": 5.25,
            "vote_count": 12
        },
        {
            "file_path": "/fuSeIUKsizmfiPIwDH7lKiFNQoD.jpg",
            "width": 1280,
            "height": 720,
            "iso_639_1": null,
            "aspect_ratio": 1.78,
            "vote_average": 4.2999999999999998,
            "vote_count": 5
        }
    ],
    "posters": [
        {
            "file_path": "/2lECpi35Hnbpa4y46JX0aY3AWTy.jpg",
            "width": 1000,
            "height": 1500,
            "iso_639_1": "en",
            "aspect_ratio": 0.67000000000000004,
            "vote_average": 6.1395348837209305,
            "vote_count": 43
        },
        {
            "file_path": "/shFj1K58Tn55Qz2p2v0RqxXiXyo.jpg",
            "width": 1000,
            "height": 1408,
            "iso_639_1": "en",
            "aspect_ratio": 0.70999999999999996,
            "vote_average": 6.0,
            "vote_count": 2
        },
        {
            "file_path": "/jQ2iUsXI2jmUcOFGjOaONCLwaVp.jpg",
            "width": 675,
            "height": 1000,
            "iso_639_1": "en",
            "aspect_ratio": 0.68000000000000005,
            "vote_average": 5.546875,
            "vote_count": 32
        },
        {
            "file_path": "/6cMPLU1jv8pG9BMmieXCHDTVRPm.jpg",
            "width": 1479,
            "height": 2166,
            "iso_639_1": "es",
            "aspect_ratio": 0.68000000000000005,
            "vote_average": 5.375,
            "vote_count": 4
        },
        {
            "file_path": "/pKGrxTB35AkLsZCT8d8h6F9BSMN.jpg",
            "width": 1000,
            "height": 1500,
            "iso_639_1": "en",
            "aspect_ratio": 0.67000000000000004,
            "vote_average": 5.3611111111111107,
            "vote_count": 18
        },
        {
            "file_path": "/5qEBvDRdY2S2CJ9tsoFktIt8Xue.jpg",
            "width": 2689,
            "height": 3539,
            "iso_639_1": "es",
            "aspect_ratio": 0.76000000000000001,
            "vote_average": 5.25,
            "vote_count": 2
        },
        {
            "file_path": "/1dTxsr21UmFewu2CJ886JNqqkqn.jpg",
            "width": 2030,
            "height": 3000,
            "iso_639_1": "en",
            "aspect_ratio": 0.68000000000000005,
            "vote_average": 5.25,
            "vote_count": 6
        },
        {
            "file_path": "/olTEZdZJ7UKLE1o4hkFDq4jXBs8.jpg",
            "width": 713,
            "height": 1002,
            "iso_639_1": "hu",
            "aspect_ratio": 0.70999999999999996,
            "vote_average": 5.25,
            "vote_count": 2
        }
    ]
}

Get the plot keywords for a specific movie id.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
<tr><td style="padding-left: 0;" colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-left: 0; width: 140px;">append_to_response</td><td>Comma separated, any movie method</td></tr>
</table>

GET /movie/{id}/keywords
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "06ed999e72a65a881228dda2f93186ae"
{
    "id": 550,
    "keywords": [
        {
            "id": 397,
            "name": "bare knuckle boxing"
        },
        {
            "id": 33851,
            "name": "capitalism"
        },
        {
            "id": 738,
            "name": "sexuality"
        },
        {
            "id": 825,
            "name": "support group"
        },
        {
            "id": 851,
            "name": "dual identity"
        },
        {
            "id": 158066,
            "name": "underground"
        },
        {
            "id": 1541,
            "name": "nihilism"
        },
        {
            "id": 162252,
            "name": "fight"
        },
        {
            "id": 3848,
            "name": "support"
        },
        {
            "id": 3927,
            "name": "rage and hate"
        },
        {
            "id": 4142,
            "name": "insomnia"
        },
        {
            "id": 11250,
            "name": "boxing"
        },
        {
            "id": 14681,
            "name": "martial arts"
        },
        {
            "id": 160882,
            "name": "brawl"
        },
        {
            "id": 17977,
            "name": "underground fighting"
        },
        {
            "id": 19813,
            "name": "flashback"
        },
        {
            "id": 19701,
            "name": "friendship"
        },
        {
            "id": 158439,
            "name": " liposuction"
        }
    ]
}

Get the release dates, certifications and related information by country for a specific movie id.

The results are keyed by `iso_3166_1` code and contain a type value which on our system, maps to:

1. Premiere
2. Theatrical (limited)
3. Theatrical
4. Digital
5. Physical
6. TV

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
<tr><td style="padding-left: 0;" colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-left: 0; width: 140px;">append_to_response</td><td>Comma separated, any movie method</td></tr>
</table>

GET /movie/{id}/release_dates
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "8cd68d030256f9d5659c6118e085feb6"
{
  "id": 550,
  "results": [
    {
      "iso_3166_1": "ES",
      "release_dates": [
        {
          "certification": "",
          "iso_639_1": "",
          "note": "",
          "release_date": "1999-11-05T00:00:00.000Z",
          "type": 3
        }
      ]
    },
    {
      "iso_3166_1": "DE",
      "release_dates": [
        {
          "certification": "18",
          "iso_639_1": "",
          "note": "",
          "release_date": "1999-11-10T00:00:00.000Z",
          "type": 3
        }
      ]
    },
    {
      "iso_3166_1": "BR",
      "release_dates": [
        {
          "certification": "18 anos",
          "iso_639_1": "",
          "note": "",
          "release_date": "1999-11-12T00:00:00.000Z",
          "type": 3
        }
      ]
    },
    {
      "iso_3166_1": "AU",
      "release_dates": [
        {
          "certification": "R18+",
          "iso_639_1": "",
          "note": "",
          "release_date": "1999-11-11T00:00:00.000Z",
          "type": 3
        }
      ]
    },
    {
      "iso_3166_1": "BG",
      "release_dates": [
        {
          "certification": "c",
          "iso_639_1": "",
          "note": "",
          "release_date": "2012-08-28T00:00:00.000Z",
          "type": 3
        }
      ]
    },
    {
      "iso_3166_1": "TR",
      "release_dates": [
        {
          "certification": "",
          "iso_639_1": "",
          "note": "",
          "release_date": "1999-12-10T00:00:00.000Z",
          "type": 3
        }
      ]
    },
    {
      "iso_3166_1": "FI",
      "release_dates": [
        {
          "certification": "K-18",
          "iso_639_1": "",
          "note": "",
          "release_date": "1999-11-12T00:00:00.000Z",
          "type": 3
        }
      ]
    },
    {
      "iso_3166_1": "FR",
      "release_dates": [
        {
          "certification": "16",
          "iso_639_1": "",
          "note": "",
          "release_date": "1999-11-10T00:00:00.000Z",
          "type": 3
        }
      ]
    },
    {
      "iso_3166_1": "IT",
      "release_dates": [
        {
          "certification": "VM14",
          "iso_639_1": "",
          "note": "",
          "release_date": "1999-10-29T00:00:00.000Z",
          "type": 3
        }
      ]
    },
    {
      "iso_3166_1": "GB",
      "release_dates": [
        {
          "certification": "18",
          "iso_639_1": "",
          "note": "",
          "release_date": "1999-11-12T00:00:00.000Z",
          "type": 3
        }
      ]
    },
    {
      "iso_3166_1": "GR",
      "release_dates": [
        {
          "certification": "",
          "iso_639_1": "",
          "note": "",
          "release_date": "2000-02-18T00:00:00.000Z",
          "type": 3
        }
      ]
    },
    {
      "iso_3166_1": "US",
      "release_dates": [
        {
          "certification": "R",
          "iso_639_1": "",
          "note": "",
          "release_date": "1999-10-14T00:00:00.000Z",
          "type": 3
        }
      ]
    }
  ]
}

Get the videos (trailers, teasers, clips, etc...) for a specific movie id.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
<tr><td style="padding-left: 0;" colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-left: 0; width: 140px;">language</td><td>ISO 639-1 code.</td></tr>
<tr><td style="padding-left: 0; width: 140px;">append_to_response</td><td>Comma separated, any movie method</td></tr>
</table>

GET /movie/{id}/videos
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "8385922582a7720b7ae1305803ab5819"
{
  "id": 550,
  "results": [
    {
      "id": "533ec654c3a36854480003eb",
      "iso_639_1": "en",
      "key": "SUXWAEX2jlg",
      "name": "Trailer 1",
      "site": "YouTube",
      "size": 720,
      "type": "Trailer"
    }
  ]
}

Get the translations for a specific movie id.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
<tr><td style="padding-left: 0;" colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-left: 0; width: 140px;">append_to_response</td><td>Comma separated, any movie method</td></tr>
</table>

GET /movie/{id}/translations
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "8a7020d7e666b95e7e796f384f6b3c43"
{
    "id": 550,
    "translations": [
        {
            "iso_639_1": "en",
            "name": "English",
            "english_name": "English"
        },
        {
            "iso_639_1": "es",
            "name": "Español",
            "english_name": "Spanish"
        },
        {
            "iso_639_1": "uk",
            "name": "Український",
            "english_name": "Ukrainian"
        },
        {
            "iso_639_1": "de",
            "name": "Deutsch",
            "english_name": "German"
        },
        {
            "iso_639_1": "pt",
            "name": "Português",
            "english_name": "Portuguese"
        },
        {
            "iso_639_1": "fr",
            "name": "Français",
            "english_name": "French"
        },
        {
            "iso_639_1": "nl",
            "name": "Nederlands",
            "english_name": "Dutch"
        },
        {
            "iso_639_1": "hu",
            "name": "Magyar",
            "english_name": "Hungarian"
        },
        {
            "iso_639_1": "ru",
            "name": "Pусский",
            "english_name": "Russian"
        },
        {
            "iso_639_1": "it",
            "name": "Italiano",
            "english_name": "Italian"
        },
        {
            "iso_639_1": "tr",
            "name": "Türkçe",
            "english_name": "Turkish"
        },
        {
            "iso_639_1": "zh",
            "name": "中国",
            "english_name": "Mandarin"
        },
        {
            "iso_639_1": "da",
            "name": "Dansk",
            "english_name": "Danish"
        },
        {
            "iso_639_1": "sv",
            "name": "svenska",
            "english_name": "Swedish"
        },
        {
            "iso_639_1": "pl",
            "name": "Polski",
            "english_name": "Polish"
        },
        {
            "iso_639_1": "cs",
            "name": "Český",
            "english_name": "Czech"
        },
        {
            "iso_639_1": "fi",
            "name": "suomi",
            "english_name": "Finnish"
        },
        {
            "iso_639_1": "he",
            "name": "עִבְרִית",
            "english_name": "Hebrew"
        }
    ]
}

Get the similar movies for a specific movie id.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td colspan="2">api_key</td></tr>
<tr><td colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-right: 40px; width: 140px;">page</td><td>Minimum 1, maximum 1000.</td></tr>
<tr><td style="padding-right: 40px; width: 140px;">language</td><td>ISO 639-1 code.</td></tr>
<tr><td style="width: 140px;">append_to_response</td><td>Comma separated, any movie method</td></tr>
</table>

GET /movie/{id}/similar
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "ed6d3600ec1cf6d4ab30c24f6f33625e"
{
  "page": 1,
  "results": [
    {
      "adult": false,
      "backdrop_path": "/99qMHgawGX9QWx6zllecsMbbLkj.jpg",
      "id": 17169,
      "original_title": "Any Which Way You Can",
      "release_date": "1980-12-17",
      "poster_path": "/fcwYEVdBa251yqeuTfFQXHlMrHt.jpg",
      "popularity": 0.612450827242979,
      "title": "Any Which Way You Can",
      "vote_average": 6.6,
      "vote_count": 9
    },
    {
      "adult": false,
      "backdrop_path": "/5X0l0G0S95iTzJMptyIMZO80XNS.jpg",
      "id": 54833,
      "original_title": "The Big Man",
      "release_date": "1991-08-09",
      "poster_path": "/v2RAbH8HJuLdMYnwC9RAFx08aeU.jpg",
      "popularity": 0.69152162618085,
      "title": "The Big Man",
      "vote_average": 0.0,
      "vote_count": 0
    },
    {
      "adult": false,
      "backdrop_path": "/Ai8pP8nbpUBAMbAcQ6EnVepReKF.jpg",
      "id": 43522,
      "original_title": "Gentleman Jim",
      "release_date": "1942-11-14",
      "poster_path": "/nS7UHGfViBWZw6SDtYDeVLAYVgX.jpg",
      "popularity": 0.46,
      "title": "Gentleman Jim",
      "vote_average": 0.0,
      "vote_count": 0
    },
    {
      "adult": false,
      "backdrop_path": "/m4YwE8eChe6X57VgFmVFKCu5ObZ.jpg",
      "id": 9103,
      "original_title": "The Quest",
      "release_date": "1996-04-19",
      "poster_path": "/nQgMrXHYPAxVTYgnbhRGyCLcbHV.jpg",
      "popularity": 0.92391,
      "title": "The Quest",
      "vote_average": 5.1,
      "vote_count": 8
    },
    {
      "adult": false,
      "backdrop_path": "/ac966FhrHvPAgCn2Id3uf1OegCT.jpg",
      "id": 21001,
      "original_title": "Like It Is",
      "release_date": "1998-04-17",
      "poster_path": "/bMw3TUvXSd4ZijcOaDdqZBCmgcM.jpg",
      "popularity": 0.437,
      "title": "Like It Is",
      "vote_average": 4.0,
      "vote_count": 1
    },
    {
      "adult": false,
      "backdrop_path": "/pbp2t6p44dt5Ll6MQoFTZKrnDPL.jpg",
      "id": 52591,
      "original_title": "Confessions of a Pit Fighter",
      "release_date": "2005-06-10",
      "poster_path": "/qOmYQRhnhBhCM51e212dOaTIrYB.jpg",
      "popularity": 0.23,
      "title": "Confessions of a Pit Fighter",
      "vote_average": 3.5,
      "vote_count": 1
    },
    {
      "adult": false,
      "backdrop_path": "/rq79zHm5BgBLd0hbAb6NUQpCcvW.jpg",
      "id": 164457,
      "original_title": "Out of the Furnace",
      "release_date": "2013-12-06",
      "poster_path": "/dTiCv161SKqLErR4sL4CwRgQQhw.jpg",
      "popularity": 2.45387688862988,
      "title": "Out of the Furnace",
      "vote_average": 6.8,
      "vote_count": 14
    },
    {
      "adult": false,
      "backdrop_path": "/aqC31QMmjd0cWWNL9QyJabl2sX7.jpg",
      "id": 3469,
      "original_title": "Far from the Madding Crowd",
      "release_date": "1967-06-18",
      "poster_path": "/1c0JODXUddz2gyJsg4HzqzlBM7d.jpg",
      "popularity": 0.4485,
      "title": "Far from the Madding Crowd",
      "vote_average": 6.0,
      "vote_count": 2
    },
    {
      "adult": false,
      "backdrop_path": "/nCK3Api5TteYOhbc7JTrbcD9OlO.jpg",
      "id": 10796,
      "original_title": "The One",
      "release_date": "2001-11-02",
      "poster_path": "/baw9Zdb1zJDWs4iJBrmfWKnx62p.jpg",
      "popularity": 3.09495070264636,
      "title": "The One",
      "vote_average": 5.6,
      "vote_count": 66
    },
    {
      "adult": false,
      "backdrop_path": null,
      "id": 213744,
      "original_title": "Un uomo dalla pelle dura",
      "release_date": "1972-02-02",
      "poster_path": "/xPF1Tp9IpR1sna3XNaLQBOnohi.jpg",
      "popularity": 0.23,
      "title": "The Boxer",
      "vote_average": 0.0,
      "vote_count": 0
    },
    {
      "adult": false,
      "backdrop_path": null,
      "id": 63844,
      "original_title": "Undefeated",
      "release_date": "2003-07-26",
      "poster_path": "/8RdVfoIMuvA7UDGHBGWf6zpDMzQ.jpg",
      "popularity": 0.33005,
      "title": "Undefeated",
      "vote_average": 0.0,
      "vote_count": 0
    },
    {
      "adult": false,
      "backdrop_path": null,
      "id": 12815,
      "original_title": "White Terror",
      "release_date": "2005-01-01",
      "poster_path": null,
      "popularity": 0.23,
      "title": "White Terror",
      "vote_average": 0.0,
      "vote_count": 0
    },
    {
      "adult": false,
      "backdrop_path": "/eRYTGxWbJazstrjkbVABVyErfWJ.jpg",
      "id": 73967,
      "original_title": "Ring of Steel",
      "release_date": "1994-01-01",
      "poster_path": "/8JOPSoUBvUkyScqNauY80MPL1Dg.jpg",
      "popularity": 0.46,
      "title": "Ring of Steel",
      "vote_average": 6.5,
      "vote_count": 2
    },
    {
      "adult": false,
      "backdrop_path": "/qUf4ruSfHMAOBqsw2UK9q16CaZF.jpg",
      "id": 1567,
      "original_title": "Seul contre tous",
      "release_date": "1998-05-16",
      "poster_path": "/p40pv0pJbmY8gagHHMHHBPezdez.jpg",
      "popularity": 0.299,
      "title": "I Stand Alone",
      "vote_average": 7.3,
      "vote_count": 7
    },
    {
      "adult": false,
      "backdrop_path": "/j7vkU19uJa0Fj74hL7XIKpfNS76.jpg",
      "id": 15417,
      "original_title": "Every Which Way But Loose",
      "release_date": "1978-12-19",
      "poster_path": "/7DvlQQiTWYIC3FYwBlkdgceAZFo.jpg",
      "popularity": 1.2657058084445,
      "title": "Every Which Way But Loose",
      "vote_average": 6.9,
      "vote_count": 14
    },
    {
      "adult": false,
      "backdrop_path": "/cne1AaNoVPHOEUiwcdS7sSo1uJ5.jpg",
      "id": 670,
      "original_title": "올드보이",
      "release_date": "2003-12-21",
      "poster_path": "/fct7n9V10E8t8a7wOR90Ccw0i48.jpg",
      "popularity": 6.18944607849304,
      "title": "Oldboy",
      "vote_average": 7.5,
      "vote_count": 329
    },
    {
      "adult": false,
      "backdrop_path": "/i9PzVmqbSwDN193c1pLq3VrNRVY.jpg",
      "id": 12877,
      "original_title": "Dead Man's Shoes",
      "release_date": "2004-10-01",
      "poster_path": "/bJK4Nb5VvdeZRqgv8OUCk1nuYM.jpg",
      "popularity": 0.983197786173302,
      "title": "Dead Man's Shoes",
      "vote_average": 7.1,
      "vote_count": 8
    },
    {
      "adult": false,
      "backdrop_path": "/ba4CpvnaxvAgff2jHiaqJrVpZJ5.jpg",
      "id": 807,
      "original_title": "Se7en",
      "release_date": "1995-09-22",
      "poster_path": "/zgB9CCTDlXRv50Z70ZI4elJtNEk.jpg",
      "popularity": 7.89723975539919,
      "title": "Se7en",
      "vote_average": 7.4,
      "vote_count": 1446
    },
    {
      "adult": false,
      "backdrop_path": "/lREt2ifyEjxdxmsqkWnmoFaIcZF.jpg",
      "id": 11971,
      "original_title": "Much Ado About Nothing",
      "release_date": "1993-05-07",
      "poster_path": "/3w93X93zCp6NVhft9F60pEdGF1t.jpg",
      "popularity": 1.40866636647632,
      "title": "Much Ado About Nothing",
      "vote_average": 7.6,
      "vote_count": 21
    },
    {
      "adult": false,
      "backdrop_path": "/vmpyNlphrzFlsiG8XcnaGeQ93WW.jpg",
      "id": 159,
      "original_title": "Der Bewegte Mann",
      "release_date": "1994-10-05",
      "poster_path": "/7UEzKYus31qTtyGDftRhao7kxbb.jpg",
      "popularity": 0.64515,
      "title": "Maybe... Maybe Not",
      "vote_average": 6.0,
      "vote_count": 1
    }
  ],
  "total_pages": 2271,
  "total_results": 45401
}

Get the reviews for a particular movie id.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td colspan="2">api_key</td></tr>
<tr><td colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-right: 40px; width: 140px;">page</td><td>Minimum 1, maximum 1000.</td></tr>
<tr><td style="padding-right: 40px; width: 140px;">language</td><td>ISO 639-1 code.</td></tr>
<tr><td style="width: 140px;">append_to_response</td><td>Comma separated, any movie method</td></tr>
</table>

GET /movie/{id}/reviews
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "ed6d3600ec1cf6d4ab30c24f6f33625e"
{
  "id": 49026,
  "page": 1,
  "results": [
    {
      "id": "5010553819c2952d1b000451",
      "author": "Travis Bell",
      "content": "I felt like this was a tremendous end to Nolan's Batman trilogy. The Dark Knight Rises may very well have been the weakest of all 3 films but when you're talking about a scale of this magnitude, it still makes this one of the best movies I've seen in the past few years.\r\n\r\nI expected a little more _Batman_ than we got (especially with a runtime of 2:45) but while the story around the fall of Bruce Wayne and Gotham City was good I didn't find it amazing. This might be in fact, one of my only criticisms—it was a long movie but still, maybe too short for the story I felt was really being told. I feel confident in saying this big of a story could have been split into two movies.\r\n\r\nThe acting, editing, pacing, soundtrack and overall theme were the same 'as-close-to-perfect' as ever with any of Christopher Nolan's other films. Man does this guy know how to make a movie!\r\n\r\nYou don't have to be a Batman fan to enjoy these movies and I hope any of you who feel this way re-consider. These 3 movies are without a doubt in my mind, the finest display of comic mythology ever told on the big screen. They are damn near perfect.",
      "url": "http://j.mp/QSjAK2"
    },
    {
      "id": "5013bc76760ee372cb00253e",
      "author": "Chris",
      "content": "I personally thought this film is on par if not better than the Dark Knight.\r\n\r\nWhilst some think the film is too long for the story I didn't find this. The length of the film is longer than some (but doesn't feel it), I liked that the film took it's time rather than shoving more elements in it - I think this contributed to the dramatic ending (much like a classical piece of music will be relaxed and calm before the final crescendo).\r\n\r\nAt the end of the day whether you like this film will boil down to if you like films Christopher Nolan has directed and/or you like the Christopher Nolan Batman series so far.\r\n\r\nStupendously good film in my opinion.",
      "url": "http://j.mp/P18dg1"
    },
    {
      "id": "51429a7519c29552e10eba16",
      "author": "GeekMasher",
      "content": "The Dark Knight Rises is one of the best movies to come out in 2012. The story compels you to watch it time and time again. It also has I of, I my opinion, the best bad guys in any movie, Bane! Batman was well played as all ways and the cast where well selected. I think this movie is the best batman to see the light of day or the darkest nights (pun intended).",
      "url": "http://j.mp/19nyIWg"
    }
  ],
  "total_pages": 1,
  "total_results": 3
}

Get the lists that the movie belongs to.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td colspan="2">api_key</td></tr>
<tr><td colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-right: 40px; width: 140px;">page</td><td>Minimum 1, maximum 1000.</td></tr>
<tr><td style="padding-right: 40px; width: 140px;">language</td><td>ISO 639-1 code.</td></tr>
<tr><td style="width: 140px;">append_to_response</td><td>Comma separated, any movie method</td></tr>
</table>

GET /movie/{id}/lists
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "ed6d3600ec1cf6d4ab30c24f6f33625e"
{
    "id": 578,
    "page": 1,
    "results": [
        {
            "description": "Part of the AFI 100 Years… series, AFI's 100 Years…100 Thrills is a list of the top 100 heart-pounding movies in American cinema. The list was unveiled by the American Film Institute on June 12, 2001, during a CBS special hosted by Harrison Ford.",
            "favorite_count": 2,
            "id": "50a510fb760ee34f2c001574",
            "item_count": 100,
            "iso_639_1": "en",
            "name": "AFI's 100 Most Thrilling American Films",
            "poster_path": "/tayO4gCLyEo8Uul8FHkJGy3Kppb.jpg"
        }
    ],
    "total_pages": 1,
    "total_results": 1
}

Get the changes for a specific movie id.

Changes are grouped by key, and ordered by date in descending order. By default, only the last 24 hours of changes are returned. The maximum number of days that can be returned in a single request is 14. The language is present on fields that are translatable.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
<tr><td style="padding-left: 0;" colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-left: 0; padding-right: 40px; width: 140px;">start_date</td><td>YYYY-MM-DD</td></tr>
<tr><td style="padding-left: 0; padding-right: 40px; width: 140px;">end_date</td><td>YYYY-MM-DD</td></tr>
</table>

GET /movie/{id}/changes
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "691e0c90d7869a78ead95cb777b870dd"
{
    "changes": [
        {
            "key": "general",
            "items": [
                {
                    "id": "507436ef19c2957f27000054",
                    "action": "created",
                    "time": "2012-10-09 14:38:39 UTC"
                }
            ]
        },
        {
            "key": "original_title",
            "items": [
                {
                    "id": "507436ef19c2957f27000055",
                    "action": "added",
                    "time": "2012-10-09 14:38:39 UTC",
                    "value": "The Duke"
                }
            ]
        },
        {
            "key": "overview",
            "items": [
                {
                    "id": "507436f619c2957efe000048",
                    "action": "deleted",
                    "time": "2012-10-09 14:38:46 UTC",
                    "iso_639_1": "en",
                    "original_value": "hello hello"
                },
                {
                    "id": "507436ef19c2957f27000056",
                    "action": "added",
                    "time": "2012-10-09 14:38:39 UTC",
                    "iso_639_1": "en",
                    "value": "hello hello"
                }
            ]
        },
        {
            "key": "imdb_id",
            "items": [
                {
                    "id": "507436ef19c2957f27000057",
                    "action": "added",
                    "time": "2012-10-09 14:38:39 UTC",
                    "value": "tt0196516"
                }
            ]
        },
        {
            "key": "budget",
            "items": [
                {
                    "id": "507436ef19c2957f27000058",
                    "action": "added",
                    "time": "2012-10-09 14:38:39 UTC",
                    "value": 0
                }
            ]
        },
        {
            "key": "revenue",
            "items": [
                {
                    "id": "507436ef19c2957f27000059",
                    "action": "added",
                    "time": "2012-10-09 14:38:39 UTC",
                    "value": 0
                }
            ]
        },
        {
            "key": "translations",
            "items": [
                {
                    "id": "507436ef19c2957f2700005a",
                    "action": "added",
                    "time": "2012-10-09 14:38:39 UTC",
                    "value": "en"
                }
            ]
        },
        {
            "key": "runtime",
            "items": [
                {
                    "id": "507436ef19c2957f2700005b",
                    "action": "added",
                    "time": "2012-10-09 14:38:39 UTC",
                    "iso_639_1": "en",
                    "value": 88
                }
            ]
        },
        {
            "key": "adult",
            "items": [
                {
                    "id": "507436ef19c2957f2700005c",
                    "action": "updated",
                    "time": "2012-10-09 14:38:39 UTC",
                    "value": false
                }
            ]
        },
        {
            "key": "spoken_languages",
            "items": [
                {
                    "id": "5074370019c2957efe00004c",
                    "action": "added",
                    "time": "2012-10-09 14:38:56 UTC",
                    "value": "en"
                }
            ]
        },
        {
            "key": "production_countries",
            "items": [
                {
                    "id": "5074370719c2957f2700005e",
                    "action": "added",
                    "time": "2012-10-09 14:39:03 UTC",
                    "value": "US"
                },
                {
                    "id": "5074370419c29518f6000902",
                    "action": "added",
                    "time": "2012-10-09 14:39:00 UTC",
                    "value": "GB"
                }
            ]
        },
        {
            "key": "genres",
            "items": [
                {
                    "id": "5074371019c2955c9c0001ee",
                    "action": "added",
                    "time": "2012-10-09 14:39:12 UTC",
                    "value": {
                        "name": "Family",
                        "id": 10751
                    }
                },
                {
                    "id": "5074371019c2955c9c0001ed",
                    "action": "added",
                    "time": "2012-10-09 14:39:12 UTC",
                    "value": {
                        "name": "Drama",
                        "id": 18
                    }
                },
                {
                    "id": "5074371019c2955c9c0001ec",
                    "action": "added",
                    "time": "2012-10-09 14:39:12 UTC",
                    "value": {
                        "name": "Comedy",
                        "id": 35
                    }
                }
            ]
        },
        {
            "key": "releases",
            "items": [
                {
                    "id": "5074374b19c2957ec200004e",
                    "action": "added",
                    "time": "2012-10-09 14:40:11 UTC",
                    "value": {
                        "iso_3166_1": "US",
                        "release_date": "1999-01-01",
                        "certification": "G",
                        "primary": true
                    }
                }
            ]
        },
        {
            "key": "crew",
            "items": [
                {
                    "id": "507437d319c2957f27000078",
                    "action": "added",
                    "time": "2012-10-09 14:42:27 UTC",
                    "value": {
                        "person_id": 61247,
                        "department": "Sound",
                        "job": "Music"
                    }
                },
                {
                    "id": "507437c719c29518f600091b",
                    "action": "added",
                    "time": "2012-10-09 14:42:15 UTC",
                    "value": {
                        "person_id": 61244,
                        "department": "Production",
                        "job": "Executive Producer"
                    }
                },
                {
                    "id": "507437be19c2955c9c00020c",
                    "action": "added",
                    "time": "2012-10-09 14:42:06 UTC",
                    "value": {
                        "person_id": 94980,
                        "department": "Production",
                        "job": "Executive Producer"
                    }
                },
                {
                    "id": "507437b519c2955c9c000207",
                    "action": "added",
                    "time": "2012-10-09 14:41:57 UTC",
                    "value": {
                        "person_id": 955496,
                        "department": "Production",
                        "job": "Executive Producer"
                    }
                },
                {
                    "id": "507437a919c295228f0012bf",
                    "action": "added",
                    "time": "2012-10-09 14:41:45 UTC",
                    "value": {
                        "person_id": 950332,
                        "department": "Production",
                        "job": "Producer"
                    }
                },
                {
                    "id": "5074379b19c2955c9c000202",
                    "action": "added",
                    "time": "2012-10-09 14:41:31 UTC",
                    "value": {
                        "person_id": 61244,
                        "department": "Writing",
                        "job": "Screenplay"
                    }
                },
                {
                    "id": "5074379119c295228f0012bb",
                    "action": "added",
                    "time": "2012-10-09 14:41:21 UTC",
                    "value": {
                        "person_id": 94980,
                        "department": "Writing",
                        "job": "Screenplay"
                    }
                },
                {
                    "id": "5074378819c2957f2700006a",
                    "action": "added",
                    "time": "2012-10-09 14:41:12 UTC",
                    "value": {
                        "person_id": 1106571,
                        "department": "Writing",
                        "job": "Screenplay"
                    }
                },
                {
                    "id": "5074378119c29518f6000911",
                    "action": "added",
                    "time": "2012-10-09 14:41:05 UTC",
                    "value": {
                        "person_id": 1106570,
                        "department": "Writing",
                        "job": "Story"
                    }
                },
                {
                    "id": "5074377119c295228f0012b4",
                    "action": "added",
                    "time": "2012-10-09 14:40:49 UTC",
                    "value": {
                        "person_id": 105746,
                        "department": "Directing",
                        "job": "Director"
                    }
                }
            ]
        },
        {
            "key": "cast",
            "items": [
                {
                    "id": "5074382d19c2951aaa00055c",
                    "action": "added",
                    "time": "2012-10-09 14:43:57 UTC",
                    "value": {
                        "person_id": 129996,
                        "character": "Basil Rathwood",
                        "order": 9
                    }
                },
                {
                    "id": "5074382719c29518f6000925",
                    "action": "added",
                    "time": "2012-10-09 14:43:51 UTC",
                    "value": {
                        "person_id": 178914,
                        "character": "Lord Huffbottom",
                        "order": 8
                    }
                },
                {
                    "id": "5074381c19c29519b0000505",
                    "action": "added",
                    "time": "2012-10-09 14:43:40 UTC",
                    "value": {
                        "person_id": 165458,
                        "character": "Mrs. Puddingforth",
                        "order": 7
                    }
                },
                {
                    "id": "5074381519c2957efe000061",
                    "action": "added",
                    "time": "2012-10-09 14:43:33 UTC",
                    "value": {
                        "person_id": 41234,
                        "character": "Lady Fautblossom",
                        "order": 6
                    }
                },
                {
                    "id": "5074380e19c2955c9c000221",
                    "action": "added",
                    "time": "2012-10-09 14:43:26 UTC",
                    "value": {
                        "person_id": 186327,
                        "character": "Shamela Stewart",
                        "order": 5
                    }
                },
                {
                    "id": "5074380819c2955c9c00021e",
                    "action": "added",
                    "time": "2012-10-09 14:43:20 UTC",
                    "value": {
                        "person_id": 93035,
                        "character": "Cecil Cavendish",
                        "order": 4
                    }
                },
                {
                    "id": "5074380219c29518f6000921",
                    "action": "added",
                    "time": "2012-10-09 14:43:14 UTC",
                    "value": {
                        "person_id": 98429,
                        "character": "Florian",
                        "order": 3
                    }
                },
                {
                    "id": "507437fc19c295228f0012ca",
                    "action": "added",
                    "time": "2012-10-09 14:43:08 UTC",
                    "value": {
                        "person_id": 149837,
                        "character": "Charlotte",
                        "order": 2
                    }
                },
                {
                    "id": "507437f419c2957f2700007e",
                    "action": "added",
                    "time": "2012-10-09 14:43:00 UTC",
                    "value": {
                        "person_id": 1751,
                        "character": "Clive Chives",
                        "order": 1
                    }
                },
                {
                    "id": "507437ec19c2957ec2000063",
                    "action": "added",
                    "time": "2012-10-09 14:42:52 UTC",
                    "value": {
                        "person_id": 12642,
                        "character": "The Duke",
                        "order": 0
                    }
                }
            ]
        }
    ]
}

This method lets users create (or delete) a rating on a movie. A [valid session](#authentication) id **or** [guest session](#get-%2F3%2Fauthentication%2Fguest_session%2Fnew) id is required.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
<tr><td style="padding-left: 0;" colspan="2">session_id</td></tr>
<tr><td style="padding-left: 0;" colspan="2">guest_session_id</td></tr>
<tr><td style="padding-left: 0;" colspan="2"><strong>Required JSON Body (when POST-ing)</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">value</td></tr>
</table>

POST /movie/{id}/rating
> Accept: application/json
> Content-Type: application/json
{
    "value": 7.5
}
< 201
< Content-Type: application/json
{
    "status_code": 1,
    "status_message": "Success"
}

DELETE /movie/{id}/rating
> Accept: application/json
> Content-Type: application/json
< 200
< Content-Type: application/json
{
  "status_code": 13,
  "status_message": "The item/record was deleted successfully."
}

Get the latest movie id.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
</table>

GET /movie/latest
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "395a404fd5fdecc5701224b91a49e592"
{
    "adult": false,
    "backdrop_path": null,
    "belongs_to_collection": null,
    "budget": 0,
    "genres": [],
    "homepage": null,
    "id": 133255,
    "imdb_id": "tt0031406",
    "original_title": "Harlem Rides the Range",
    "overview": null,
    "popularity": 0.0,
    "poster_path": null,
    "production_companies": [
        {
            "name": "Hollywood Pictures Corporation",
            "id": 10747
        }
    ],
    "production_countries": [
        {
            "iso_3166_1": "US",
            "name": "United States of America"
        }
    ],
    "release_date": "1939-02-01",
    "revenue": 0,
    "runtime": null,
    "spoken_languages": [
        {
            "iso_639_1": "en",
            "name": "English"
        }
    ],
    "tagline": null,
    "title": "Harlem Rides the Range",
    "vote_average": 0.0,
    "vote_count": 0
}

Get the list of movies playing that have been, or are being released this week. This list refreshes every day.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td colspan="2">api_key</td></tr>
<tr><td colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-right: 40px; width: 140px;">page</td><td>Minimum 1, maximum 1000.</td></tr>
<tr><td style="padding-right: 40px; width: 140px;">language</td><td>ISO 639-1 code.</td></tr>
</table>

GET /movie/now_playing
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "d69629e64c4236ba3a7a9096fa6ed00c"
{
  "dates": {
    "minimum": "2015-05-13",
    "maximum": "2015-06-24"
  },
  "page": 1,
  "results": [
    {
      "adult": false,
      "backdrop_path": "/dkMD5qlogeRMiEixC4YNPUvax2T.jpg",
      "genre_ids": [
        28,
        12,
        878,
        53
      ],
      "id": 135397,
      "original_language": "en",
      "original_title": "Jurassic World",
      "overview": "Twenty-two years after the events of Jurassic Park, Isla Nublar now features a fully functioning dinosaur theme park, Jurassic World, as originally envisioned by John Hammond.",
      "release_date": "2015-06-12",
      "poster_path": "/uXZYawqUsChGSj54wcuBtEdUJbh.jpg",
      "popularity": 88.551849,
      "title": "Jurassic World",
      "video": false,
      "vote_average": 7.1,
      "vote_count": 435
    },
    {
      "adult": false,
      "backdrop_path": "/jxPeRkfOoWs6gFybOa8C4xrHLrm.jpg",
      "genre_ids": [
        53,
        28,
        12
      ],
      "id": 76341,
      "original_language": "en",
      "original_title": "Mad Max: Fury Road",
      "overview": "An apocalyptic story set in the furthest reaches of our planet, in a stark desert landscape where humanity is broken, and most everyone is crazed fighting for the necessities of life. Within this world exist two rebels on the run who just might be able to restore order. There's Max, a man of action and a man of few words, who seeks peace of mind following the loss of his wife and child in the aftermath of the chaos. And Furiosa, a woman of action and a woman who believes her path to survival may be achieved if she can make it across the desert back to her childhood homeland.",
      "release_date": "2015-05-15",
      "poster_path": "/kqjL17yufvn9OVLyXYpvtyrFfak.jpg",
      "popularity": 35.88189,
      "title": "Mad Max: Fury Road",
      "video": false,
      "vote_average": 7.9,
      "vote_count": 815
    },
    {
      "adult": false,
      "backdrop_path": "/cUfGqafAVQkatQ7N4y08RNV3bgu.jpg",
      "genre_ids": [
        28,
        18,
        53
      ],
      "id": 254128,
      "original_language": "en",
      "original_title": "San Andreas",
      "overview": "In the aftermath of a massive earthquake in California, a rescue-chopper pilot makes a dangerous journey across the state in order to rescue his estranged daughter.",
      "release_date": "2015-05-29",
      "poster_path": "/6iQ4CMtYorKFfAmXEpAQZMnA0Qe.jpg",
      "popularity": 25.002,
      "title": "San Andreas",
      "video": false,
      "vote_average": 6.2,
      "vote_count": 234
    },
    {
      "adult": false,
      "backdrop_path": "/kJre98tnbNXbk5L5altHkQWGwD3.jpg",
      "genre_ids": [
        28,
        12,
        878,
        9648
      ],
      "id": 158852,
      "original_language": "en",
      "original_title": "Tomorrowland",
      "overview": "Bound by a shared destiny, a bright, optimistic teen bursting with scientific curiosity and a former boy-genius inventor jaded by disillusionment embark on a danger-filled mission to unearth the secrets of an enigmatic place somewhere in time and space that exists in their collective memory as \"Tomorrowland.\"",
      "release_date": "2015-05-22",
      "poster_path": "/69Cz9VNQZy39fUE2g0Ggth6SBTM.jpg",
      "popularity": 14.07089,
      "title": "Tomorrowland",
      "video": false,
      "vote_average": 6.6,
      "vote_count": 239
    },
    {
      "adult": false,
      "backdrop_path": "/anItUS64TeGKPv6MJ99DMv7o0Z0.jpg",
      "genre_ids": [
        35,
        10402
      ],
      "id": 254470,
      "original_language": "en",
      "original_title": "Pitch Perfect 2",
      "overview": "The Bellas are back, and they are better than ever. After being humiliated in front of none other than the President of the United States of America, the Bellas are taken out of the Aca-Circuit. In order to clear their name, and regain their status, the Bellas take on a seemingly impossible task: winning an international competition no American team has ever won. In order to accomplish this monumental task, they need to strengthen the bonds of friendship and sisterhood and blow away the competition with their amazing aca-magic! With all new friends and old rivals tagging along for the trip, the Bellas can hopefully accomplish their dreams.",
      "release_date": "2015-05-15",
      "poster_path": "/qSjruLiFB4uqRtz2xheQPxG8uaB.jpg",
      "popularity": 12.179013,
      "title": "Pitch Perfect 2",
      "video": false,
      "vote_average": 7.3,
      "vote_count": 189
    },
    {
      "adult": false,
      "backdrop_path": "/tGxBYYSKSmBUpjSHDpRelsbNeBy.jpg",
      "genre_ids": [
        80,
        53,
        28
      ],
      "id": 334074,
      "original_language": "en",
      "original_title": "Survivor",
      "overview": "A Foreign Service Officer in London tries to prevent a terrorist attack set to hit New York, but is forced to go on the run when she is framed for crimes she did not commit.",
      "release_date": "2015-05-29",
      "poster_path": "/hHuM1sYhGkWu3wTLotnNeiSJrpz.jpg",
      "popularity": 9.928704,
      "title": "Survivor",
      "video": false,
      "vote_average": 5.6,
      "vote_count": 43
    },
    {
      "adult": false,
      "backdrop_path": "/wd66GNMSUe0UCNCmOvMJkmQb2mn.jpg",
      "genre_ids": [
        28
      ],
      "id": 290764,
      "original_language": "en",
      "original_title": "Tracers",
      "overview": "Wanted by the mafia, a New York City bike messenger escapes into the world of parkour after meeting a beautiful stranger.",
      "release_date": "2015-06-18",
      "poster_path": "/cGW7Ly3dcOo7ilSwTiwGGSwEQZd.jpg",
      "popularity": 8.294641,
      "title": "Tracers",
      "video": false,
      "vote_average": 6.0,
      "vote_count": 92
    },
    {
      "adult": false,
      "backdrop_path": "/szytSpLAyBh3ULei3x663mAv5ZT.jpg",
      "genre_ids": [
        35,
        16,
        10751
      ],
      "id": 150540,
      "original_language": "en",
      "original_title": "Inside Out",
      "overview": "Growing up can be a bumpy road, and it's no exception for Riley, who is uprooted from her Midwest life when her father starts a new job in San Francisco. Like all of us, Riley is guided by her emotions - Joy, Fear, Anger, Disgust and Sadness. The emotions live in Headquarters, the control center inside Riley's mind, where they help advise her through everyday life. As Riley and her emotions struggle to adjust to a new life in San Francisco, turmoil ensues in Headquarters. Although Joy, Riley's main and most important emotion, tries to keep things positive, the emotions conflict on how best to navigate a new city, house and school.",
      "release_date": "2015-06-19",
      "poster_path": "/rDycdoAXtBb7hoWlBpZqbwk2F44.jpg",
      "popularity": 7.595773,
      "title": "Inside Out",
      "video": false,
      "vote_average": 8.8,
      "vote_count": 29
    },
    {
      "adult": false,
      "backdrop_path": "/7ZkoybuGj8cF6Wi5O3vToIruMPt.jpg",
      "genre_ids": [
        27,
        53
      ],
      "id": 243688,
      "original_language": "en",
      "original_title": "Poltergeist",
      "overview": "Legendary filmmaker Sam Raimi and director Gil Kenan reimagine and contemporize the classic tale about a family whose suburban home is invaded by angry spirits. When the terrifying apparitions escalate their attacks and take the youngest daughter, the family must come together to rescue her.",
      "release_date": "2015-05-22",
      "poster_path": "/fWOPN0XBvHXFYr3RsPr74qBge2I.jpg",
      "popularity": 5.908358,
      "title": "Poltergeist",
      "video": false,
      "vote_average": 5.0,
      "vote_count": 62
    },
    {
      "adult": false,
      "backdrop_path": "/mH748p6mdDe1GKoTsQDasgeQZGm.jpg",
      "genre_ids": [
        99
      ],
      "id": 318256,
      "original_language": "en",
      "original_title": "Hot Girls Wanted",
      "overview": "A first-ever look at the realities of the professional “amateur” porn world and the steady stream of 18-to-19-year old girls entering into it.",
      "release_date": "2015-05-29",
      "poster_path": "/oyGqHDwuZTv8HRmDIoJW6nCmYqC.jpg",
      "popularity": 5.673525,
      "title": "Hot Girls Wanted",
      "video": false,
      "vote_average": 6.0,
      "vote_count": 24
    },
    {
      "adult": false,
      "backdrop_path": "/rohVTacbOboHVjLkng0UMeXtLUR.jpg",
      "genre_ids": [
        35
      ],
      "id": 188222,
      "original_language": "en",
      "original_title": "Entourage",
      "overview": "Movie star Vincent Chase, together with his boys, Eric, Turtle and Johnny, are back…and back in business with super agent-turned-studio head Ari Gold. Some of their ambitions have changed, but the bond between them remains strong as they navigate the capricious and often cutthroat world of Hollywood.",
      "release_date": "2015-06-03",
      "poster_path": "/bgmI7iQFSd1y9QeixTme6YcaPsy.jpg",
      "popularity": 5.504801,
      "title": "Entourage",
      "video": false,
      "vote_average": 6.0,
      "vote_count": 32
    },
    {
      "adult": false,
      "backdrop_path": "/qhhTwFPParNevfpvushwsPquOpw.jpg",
      "genre_ids": [
        27
      ],
      "id": 280092,
      "original_language": "en",
      "original_title": "Insidious: Chapter 3",
      "overview": "A twisted new tale of terror begins for a teenage girl and her family, predating the haunting of the Lambert family in the earlier movies and revealing more mysteries of the otherworldly realm The Further.",
      "release_date": "2015-06-05",
      "poster_path": "/ixANIbPRTNMAstlqJa2FlfBesyQ.jpg",
      "popularity": 4.500807,
      "title": "Insidious: Chapter 3",
      "video": false,
      "vote_average": 7.1,
      "vote_count": 48
    },
    {
      "adult": false,
      "backdrop_path": "/3R0eqYG58LbizQUVqcEzDijnbV6.jpg",
      "genre_ids": [
        28,
        35
      ],
      "id": 238713,
      "original_language": "en",
      "original_title": "Spy",
      "overview": "A desk-bound CIA analyst volunteers to go undercover to infiltrate the world of a deadly arms dealer, and prevent diabolical global disaster.",
      "release_date": "2015-06-05",
      "poster_path": "/gCBw0AQDhlo0bNetkjsSRWzrxpW.jpg",
      "popularity": 5.283511,
      "title": "Spy",
      "video": false,
      "vote_average": 7.5,
      "vote_count": 96
    },
    {
      "adult": false,
      "backdrop_path": "/ykjKoP9YTcpf2pPwxmdXtqjO9X7.jpg",
      "genre_ids": [
        80,
        18
      ],
      "id": 271397,
      "original_language": "uk",
      "original_title": "Plemya",
      "overview": "Deaf mute Sergey enters a specialized boarding school for deaf-and-dumb. In this new place, he needs to find his way through the hierarchy of the school’s network dealing with crimes and prostitution, the Tribe. By taking part of several robberies, he gets propelled higher into the organization. Then he meets one of the Chief’s concubines Anna, and unwillingly breaks all the unwritten rules of the tribe.",
      "release_date": "2015-06-17",
      "poster_path": "/1q2UUtdZF0N0lyDQwpJJ8wwO4zJ.jpg",
      "popularity": 3.352383,
      "title": "The Tribe",
      "video": false,
      "vote_average": 6.1,
      "vote_count": 10
    },
    {
      "adult": false,
      "backdrop_path": "/ujszMBAGoUO1efIPcjOjDoSte6n.jpg",
      "genre_ids": [
        35,
        27
      ],
      "id": 255798,
      "original_language": "en",
      "original_title": "Burying the Ex",
      "overview": "A young man's romance with his dream girl takes an unexpected turn when his dead ex-girlfriend rises from the grave... and thinks they are still dating!",
      "release_date": "2015-06-19",
      "poster_path": "/mYB22nPaa9HfJ1UxyjZHccTePQb.jpg",
      "popularity": 4.327044,
      "title": "Burying the Ex",
      "video": false,
      "vote_average": 6.1,
      "vote_count": 4
    },
    {
      "adult": false,
      "backdrop_path": "/lC3DPzQag4QiSozvDpxY1ZgCBVw.jpg",
      "genre_ids": [
        28
      ],
      "id": 332177,
      "original_language": "en",
      "original_title": "Vendetta",
      "overview": "A hard-nosed detective deliberately commits a crime to get thrown in prison, allowing him the chance to seek vengeance on a criminal serving a life sentence for brutally murdering his wife.",
      "release_date": "2015-06-12",
      "poster_path": "/arG342eGVoePcscBH5ELeWw8U2O.jpg",
      "popularity": 4.26368,
      "title": "Vendetta",
      "video": false,
      "vote_average": 5.0,
      "vote_count": 3
    },
    {
      "adult": false,
      "backdrop_path": "/W0MNr3XN95U5KLD9xIQD96YKcS.jpg",
      "genre_ids": [
        878,
        35,
        28,
        14
      ],
      "id": 251516,
      "original_language": "en",
      "original_title": "Kung Fury",
      "overview": "During an unfortunate series of events, a friend of Kung Fury is assassinated by the most dangerous kung fu master criminal of all time, Adolf Hitler, a.k.a Kung Führer. Kung Fury decides to travel back in time to Nazi Germany in order to kill Hitler and end the Nazi empire once and for all.",
      "release_date": "2015-05-28",
      "poster_path": "/oJWzpGCLIj3uYa0ux19T2WwzTOf.jpg",
      "popularity": 4.224303,
      "title": "Kung Fury",
      "video": false,
      "vote_average": 8.5,
      "vote_count": 64
    },
    {
      "adult": false,
      "backdrop_path": null,
      "genre_ids": [],
      "id": 344938,
      "original_language": "en",
      "original_title": "The Ark Of Mr. Chow",
      "overview": "Bright teens recruited for a special college program have trouble fitting into the student body.",
      "release_date": "2015-06-19",
      "poster_path": "/iwtWt4OwGKgHBv6mIk5jjQlHMqR.jpg",
      "popularity": 4.0237,
      "title": "The Ark Of Mr. Chow",
      "video": false,
      "vote_average": 0.0,
      "vote_count": 0
    },
    {
      "adult": false,
      "backdrop_path": "/ifQIqHqfvifmPvDoCzC4mn95PgZ.jpg",
      "genre_ids": [
        18,
        35
      ],
      "id": 308639,
      "original_language": "en",
      "original_title": "Dope",
      "overview": "Malcolm is carefully surviving life in a tough neighborhood in Los Angeles while juggling college applications, academic interviews, and the SAT. A chance invitation to an underground party leads him into an adventure that could allow him to go from being a geek, to being dope, to ultimately being himself.",
      "release_date": "2015-06-19",
      "poster_path": "/oZ6tXopDrgfzuJZgmO1SFP69nFm.jpg",
      "popularity": 4.013508,
      "title": "Dope",
      "video": false,
      "vote_average": 8.5,
      "vote_count": 4
    },
    {
      "adult": false,
      "backdrop_path": null,
      "genre_ids": [
        10402,
        99
      ],
      "id": 345068,
      "original_language": "en",
      "original_title": "Big Gold Dream: Scottish Post-Punk and Infiltrating the Mainstream",
      "overview": "Documentary on the independent Edinburgh record label Fast Product and Postcard Records and associated bands like Fire Engines, Scars and Josef K",
      "release_date": "2015-06-19",
      "poster_path": null,
      "popularity": 4.0,
      "title": "Big Gold Dream: Scottish Post-Punk and Infiltrating the Mainstream",
      "video": false,
      "vote_average": 0.0,
      "vote_count": 0
    }
  ],
  "total_pages": 29,
  "total_results": 578
}

Get the list of popular movies on The Movie Database. This list refreshes every day.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td colspan="2">api_key</td></tr>
<tr><td colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-right: 40px; width: 140px;">page</td><td>Minimum 1, maximum 1000.</td></tr>
<tr><td style="padding-right: 40px; width: 140px;">language</td><td>ISO 639-1 code.</td></tr>
</table>

GET /movie/popular
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "cb5f7598ec93dd70035f93953954d611"
{
  "page": 1,
  "results": [
    {
      "adult": false,
      "backdrop_path": "/dkMD5qlogeRMiEixC4YNPUvax2T.jpg",
      "genre_ids": [
        28,
        12,
        878,
        53
      ],
      "id": 135397,
      "original_language": "en",
      "original_title": "Jurassic World",
      "overview": "Twenty-two years after the events of Jurassic Park, Isla Nublar now features a fully functioning dinosaur theme park, Jurassic World, as originally envisioned by John Hammond.",
      "release_date": "2015-06-12",
      "poster_path": "/uXZYawqUsChGSj54wcuBtEdUJbh.jpg",
      "popularity": 88.551849,
      "title": "Jurassic World",
      "video": false,
      "vote_average": 7.1,
      "vote_count": 435
    },
    {
      "adult": false,
      "backdrop_path": "/jxPeRkfOoWs6gFybOa8C4xrHLrm.jpg",
      "genre_ids": [
        53,
        28,
        12
      ],
      "id": 76341,
      "original_language": "en",
      "original_title": "Mad Max: Fury Road",
      "overview": "An apocalyptic story set in the furthest reaches of our planet, in a stark desert landscape where humanity is broken, and most everyone is crazed fighting for the necessities of life. Within this world exist two rebels on the run who just might be able to restore order. There's Max, a man of action and a man of few words, who seeks peace of mind following the loss of his wife and child in the aftermath of the chaos. And Furiosa, a woman of action and a woman who believes her path to survival may be achieved if she can make it across the desert back to her childhood homeland.",
      "release_date": "2015-05-15",
      "poster_path": "/kqjL17yufvn9OVLyXYpvtyrFfak.jpg",
      "popularity": 35.88189,
      "title": "Mad Max: Fury Road",
      "video": false,
      "vote_average": 7.9,
      "vote_count": 815
    },
    {
      "adult": false,
      "backdrop_path": "/cUfGqafAVQkatQ7N4y08RNV3bgu.jpg",
      "genre_ids": [
        28,
        18,
        53
      ],
      "id": 254128,
      "original_language": "en",
      "original_title": "San Andreas",
      "overview": "In the aftermath of a massive earthquake in California, a rescue-chopper pilot makes a dangerous journey across the state in order to rescue his estranged daughter.",
      "release_date": "2015-05-29",
      "poster_path": "/6iQ4CMtYorKFfAmXEpAQZMnA0Qe.jpg",
      "popularity": 25.002,
      "title": "San Andreas",
      "video": false,
      "vote_average": 6.2,
      "vote_count": 234
    },
    {
      "adult": false,
      "backdrop_path": "/2BXd0t9JdVqCp9sKf6kzMkr7QjB.jpg",
      "genre_ids": [
        12,
        10751,
        16,
        28,
        35
      ],
      "id": 177572,
      "original_language": "en",
      "original_title": "Big Hero 6",
      "overview": "The special bond that develops between plus-sized inflatable robot Baymax, and prodigy Hiro Hamada, who team up with a group of friends to form a band of high-tech heroes.",
      "release_date": "2014-11-07",
      "poster_path": "/3zQvuSAUdC3mrx9vnSEpkFX0968.jpg",
      "popularity": 19.674015,
      "title": "Big Hero 6",
      "video": false,
      "vote_average": 7.9,
      "vote_count": 1471
    },
    {
      "adult": false,
      "backdrop_path": "/xjjO3JIdneMBTsS282JffiPqfHW.jpg",
      "genre_ids": [
        10749,
        14,
        10751,
        18
      ],
      "id": 150689,
      "original_language": "en",
      "original_title": "Cinderella",
      "overview": "When her father unexpectedly passes away, young Ella finds herself at the mercy of her cruel stepmother and her daughters. Never one to give up hope, Ella's fortunes begin to change after meeting a dashing stranger in the woods.",
      "release_date": "2015-03-13",
      "poster_path": "/2i0JH5WqYFqki7WDhUW56Sg0obh.jpg",
      "popularity": 18.919939,
      "title": "Cinderella",
      "video": false,
      "vote_average": 7.2,
      "vote_count": 276
    },
    {
      "adult": false,
      "backdrop_path": "/7LMp3YY6WrJEExwKEyVaQnRlC76.jpg",
      "genre_ids": [
        80,
        35,
        28,
        12
      ],
      "id": 207703,
      "original_language": "en",
      "original_title": "Kingsman: The Secret Service",
      "overview": "Kingsman: The Secret Service tells the story of a super-secret spy organization that recruits an unrefined but promising street kid into the agency's ultra-competitive training program just as a global threat emerges from a twisted tech genius.",
      "release_date": "2015-02-13",
      "poster_path": "/oAISjx6DvR2yUn9dxj00vP8OcJJ.jpg",
      "popularity": 17.71923,
      "title": "Kingsman: The Secret Service",
      "video": false,
      "vote_average": 7.7,
      "vote_count": 1044
    },
    {
      "adult": false,
      "backdrop_path": "/y5lG7TBpeOMG0jxAaTK0ghZSzBJ.jpg",
      "genre_ids": [
        28,
        878,
        53
      ],
      "id": 198184,
      "original_language": "en",
      "original_title": "Chappie",
      "overview": "Every child comes into the world full of promise, and none more so than Chappie: he is gifted, special, a prodigy. Like any child, Chappie will come under the influence of his surroundings—some good, some bad—and he will rely on his heart and soul to find his way in the world and become his own man. But there's one thing that makes Chappie different from any one else: he is a robot.",
      "release_date": "2015-03-06",
      "poster_path": "/saF3HtAduvrP9ytXDxSnQJP3oqx.jpg",
      "popularity": 17.66911,
      "title": "Chappie",
      "video": false,
      "vote_average": 6.7,
      "vote_count": 492
    },
    {
      "adult": false,
      "backdrop_path": "/xu9zaAevzQ5nnrsXN6JcahLnG4i.jpg",
      "genre_ids": [
        18,
        878
      ],
      "id": 157336,
      "original_language": "en",
      "original_title": "Interstellar",
      "overview": "Interstellar chronicles the adventures of a group of explorers who make use of a newly discovered wormhole to surpass the limitations on human space travel and conquer the vast distances involved in an interstellar voyage.",
      "release_date": "2014-11-05",
      "poster_path": "/nBNZadXqJSdt05SHLqgT0HuC5Gm.jpg",
      "popularity": 16.688744,
      "title": "Interstellar",
      "video": false,
      "vote_average": 8.4,
      "vote_count": 2485
    },
    {
      "adult": false,
      "backdrop_path": "/qhH3GyIfAnGv1pjdV3mw03qAilg.jpg",
      "genre_ids": [
        12,
        14
      ],
      "id": 122917,
      "original_language": "en",
      "original_title": "The Hobbit: The Battle of the Five Armies",
      "overview": "Mere seconds after the events of \"Desolation\", Bilbo and Company continue to claim a mountain of treasure that was guarded long ago: But with Gandalf the Grey also facing some formidable foes of his own, the Hobbit is outmatched when the brutal army of orcs led by Azog the Defiler returns. But with other armies such as the elves and the men of Lake-Town, which are unsure to be trusted, are put to the ultimate test when Smaug's wrath, Azog's sheer strength, and Sauron's force of complete ends attack. All in all, the trusted armies have two choices: unite or die. But even worse, Bilbo gets put on a knife edge and finds himself fighting with Hobbit warfare with all of his might for his dwarf-friends, as the hope for Middle-Earth is all put in Bilbo's hands. The one \"precious\" thing to end it all.",
      "release_date": "2014-12-17",
      "poster_path": "/qrFwjJ5nvFnpBCmXLI4YoeHJNBH.jpg",
      "popularity": 16.434565,
      "title": "The Hobbit: The Battle of the Five Armies",
      "video": false,
      "vote_average": 7.2,
      "vote_count": 1470
    },
    {
      "adult": false,
      "backdrop_path": "/bIlYH4l2AyYvEysmS2AOfjO7Dn8.jpg",
      "genre_ids": [
        878,
        28,
        53,
        12
      ],
      "id": 87101,
      "original_language": "en",
      "original_title": "Terminator Genisys",
      "overview": "The year is 2029. John Connor, leader of the resistance continues the war against the machines. At the Los Angeles offensive, John's fears of the unknown future begin to emerge when TECOM spies reveal a new plot by SkyNet that will attack him from both fronts; past and future, and will ultimately change warfare forever.",
      "release_date": "2015-07-01",
      "poster_path": "/qOoFD4HD9a2EEUymdzBQN9XF1UJ.jpg",
      "popularity": 15.394013,
      "title": "Terminator Genisys",
      "video": false,
      "vote_average": 7.3,
      "vote_count": 35
    },
    {
      "adult": false,
      "backdrop_path": "/4liSXBZZdURI0c1Id1zLJo6Z3Gu.jpg",
      "genre_ids": [
        878,
        14,
        28,
        12
      ],
      "id": 76757,
      "original_language": "en",
      "original_title": "Jupiter Ascending",
      "overview": "In a universe where human genetic material is the most precious commodity, an impoverished young Earth woman becomes the key to strategic maneuvers and internal strife within a powerful dynasty…",
      "release_date": "2015-02-27",
      "poster_path": "/aMEsvTUklw0uZ3gk3Q6lAj6302a.jpg",
      "popularity": 15.209428,
      "title": "Jupiter Ascending",
      "video": false,
      "vote_average": 5.4,
      "vote_count": 682
    },
    {
      "adult": false,
      "backdrop_path": "/fii9tPZTpy75qOCJBulWOb0ifGp.jpg",
      "genre_ids": [
        18,
        36,
        53,
        10752
      ],
      "id": 205596,
      "original_language": "en",
      "original_title": "The Imitation Game",
      "overview": "Based on the real life story of legendary cryptanalyst Alan Turing, the film portrays the nail-biting race against time by Turing and his brilliant team of code-breakers at Britain's top-secret Government Code and Cypher School at Bletchley Park, during the darkest days of World War II.",
      "release_date": "2014-11-14",
      "poster_path": "/noUp0XOqIcmgefRnRZa1nhtRvWO.jpg",
      "popularity": 14.06502,
      "title": "The Imitation Game",
      "video": false,
      "vote_average": 8.1,
      "vote_count": 1222
    },
    {
      "adult": false,
      "backdrop_path": "/rFtsE7Lhlc2jRWF7SRAU0fvrveQ.jpg",
      "genre_ids": [
        12,
        878,
        28
      ],
      "id": 99861,
      "original_language": "en",
      "original_title": "Avengers: Age of Ultron",
      "overview": "When Tony Stark tries to jumpstart a dormant peacekeeping program, things go awry and Earth’s Mightiest Heroes are put to the ultimate test as the fate of the planet hangs in the balance. As the villainous Ultron emerges, it is up to The Avengers to stop him from enacting his terrible plans, and soon uneasy alliances and unexpected action pave the way for an epic and unique global adventure.",
      "release_date": "2015-05-01",
      "poster_path": "/t90Y3G8UGQp0f0DrP60wRu9gfrH.jpg",
      "popularity": 14.81874,
      "title": "Avengers: Age of Ultron",
      "video": false,
      "vote_average": 7.8,
      "vote_count": 1259
    },
    {
      "adult": false,
      "backdrop_path": "/bHarw8xrmQeqf3t8HpuMY7zoK4x.jpg",
      "genre_ids": [
        878,
        14,
        12
      ],
      "id": 118340,
      "original_language": "en",
      "original_title": "Guardians of the Galaxy",
      "overview": "Light years from Earth, 26 years after being abducted, Peter Quill finds himself the prime target of a manhunt after discovering an orb wanted by Ronan the Accuser.",
      "release_date": "2014-08-01",
      "poster_path": "/9gm3lL8JMTTmc3W4BmNMCuRLdL8.jpg",
      "popularity": 14.51119,
      "title": "Guardians of the Galaxy",
      "video": false,
      "vote_average": 8.2,
      "vote_count": 2706
    },
    {
      "adult": false,
      "backdrop_path": "/kJre98tnbNXbk5L5altHkQWGwD3.jpg",
      "genre_ids": [
        28,
        12,
        878,
        9648
      ],
      "id": 158852,
      "original_language": "en",
      "original_title": "Tomorrowland",
      "overview": "Bound by a shared destiny, a bright, optimistic teen bursting with scientific curiosity and a former boy-genius inventor jaded by disillusionment embark on a danger-filled mission to unearth the secrets of an enigmatic place somewhere in time and space that exists in their collective memory as \"Tomorrowland.\"",
      "release_date": "2015-05-22",
      "poster_path": "/69Cz9VNQZy39fUE2g0Ggth6SBTM.jpg",
      "popularity": 14.07089,
      "title": "Tomorrowland",
      "video": false,
      "vote_average": 6.6,
      "vote_count": 239
    },
    {
      "adult": false,
      "backdrop_path": "/6zJCJtcxmMUU7pjL4Ss1xkeGiyX.jpg",
      "genre_ids": [
        28,
        80,
        18,
        9648,
        53
      ],
      "id": 241554,
      "original_language": "en",
      "original_title": "Run All Night",
      "overview": "Brooklyn mobster and prolific hit man Jimmy Conlon, once known as The Gravedigger, has seen better days. Longtime best friend of mob boss. Jimmy, now 55, is haunted by the sins of his past—as well as a dogged police detective who’s been one step behind Jimmy for 30 years. But when Jimmy’s estranged son, becomes a target, Jimmy must make a choice between the crime family he chose and the real family he abandoned long ago. Now, with nowhere safe to turn, Jimmy just has one night to figure out exactly where his loyalties lie and to see if he can finally make things right.",
      "release_date": "2015-03-13",
      "poster_path": "/aqNJrAxudMRNo8jg3HOUQqdl2xr.jpg",
      "popularity": 13.651485,
      "title": "Run All Night",
      "video": false,
      "vote_average": 6.4,
      "vote_count": 162
    },
    {
      "adult": false,
      "backdrop_path": "/fUn5I5f4069vwGFEEvA3HXt9xPP.jpg",
      "genre_ids": [
        878,
        12,
        53
      ],
      "id": 131631,
      "original_language": "en",
      "original_title": "The Hunger Games: Mockingjay - Part 1",
      "overview": "Katniss Everdeen reluctantly becomes the symbol of a mass rebellion against the autocratic Capitol.",
      "release_date": "2014-11-20",
      "poster_path": "/cWERd8rgbw7bCMZlwP207HUXxym.jpg",
      "popularity": 12.730425,
      "title": "The Hunger Games: Mockingjay - Part 1",
      "video": false,
      "vote_average": 7.0,
      "vote_count": 1379
    },
    {
      "adult": false,
      "backdrop_path": "/anItUS64TeGKPv6MJ99DMv7o0Z0.jpg",
      "genre_ids": [
        35,
        10402
      ],
      "id": 254470,
      "original_language": "en",
      "original_title": "Pitch Perfect 2",
      "overview": "The Bellas are back, and they are better than ever. After being humiliated in front of none other than the President of the United States of America, the Bellas are taken out of the Aca-Circuit. In order to clear their name, and regain their status, the Bellas take on a seemingly impossible task: winning an international competition no American team has ever won. In order to accomplish this monumental task, they need to strengthen the bonds of friendship and sisterhood and blow away the competition with their amazing aca-magic! With all new friends and old rivals tagging along for the trip, the Bellas can hopefully accomplish their dreams.",
      "release_date": "2015-05-15",
      "poster_path": "/qSjruLiFB4uqRtz2xheQPxG8uaB.jpg",
      "popularity": 12.179013,
      "title": "Pitch Perfect 2",
      "video": false,
      "vote_average": 7.3,
      "vote_count": 189
    },
    {
      "adult": false,
      "backdrop_path": "/8dHsvdiZLBdppKwRiZ0XZYngbeN.jpg",
      "genre_ids": [
        35
      ],
      "id": 257091,
      "original_language": "en",
      "original_title": "Get Hard",
      "overview": "When obscenely rich hedge-fund manager James is convicted of fraud and sentenced to a stretch in San Quentin, the judge gives him one month to get his affairs in order. Knowing that he won't survive more than a few minutes in prison on his own, James desperately turns to Darnell-- a black businessman who's never even had a parking ticket -- for help. As Darnell puts James through the wringer, both learn that they were wrong about many things, including each other.",
      "release_date": "2015-03-27",
      "poster_path": "/qRzUSrN4p6B7fzA5XGm4ebFg3co.jpg",
      "popularity": 11.9924,
      "title": "Get Hard",
      "video": false,
      "vote_average": 6.0,
      "vote_count": 122
    },
    {
      "adult": false,
      "backdrop_path": "/razvUuLkF7CX4XsLyj02ksC0ayy.jpg",
      "genre_ids": [
        80,
        28,
        53
      ],
      "id": 260346,
      "original_language": "en",
      "original_title": "Taken 3",
      "overview": "Ex-government operative Bryan Mills finds his life is shattered when he's falsely accused of a murder that hits close to home. As he's pursued by a savvy police inspector, Mills employs his particular set of skills to track the real killer and exact his unique brand of justice.",
      "release_date": "2015-01-09",
      "poster_path": "/c2SSjUVYawDUnQ92bmTqsZsPEiB.jpg",
      "popularity": 11.737899,
      "title": "Taken 3",
      "video": false,
      "vote_average": 6.2,
      "vote_count": 698
    }
  ],
  "total_pages": 11324,
  "total_results": 226478
}

Get the list of top rated movies. By default, this list will only include movies that have 50 or more votes. This list refreshes every day.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td colspan="2">api_key</td></tr>
<tr><td colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-right: 40px; width: 140px;">page</td><td>Minimum 1, maximum 1000.</td></tr>
<tr><td style="padding-right: 40px; width: 140px;">language</td><td>ISO 639-1 code.</td></tr>
</table>

GET /movie/top_rated
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "6c0d967beb5470908268734d7c112ea0"
{
  "page": 1,
  "results": [
    {
      "adult": false,
      "backdrop_path": "/6bbZ6XyvgfjhQwbplnUh1LSj1ky.jpg",
      "genre_ids": [
        18
      ],
      "id": 244786,
      "original_language": "en",
      "original_title": "Whiplash",
      "overview": "Under the direction of a ruthless instructor, a talented young drummer begins to pursue perfection at any cost, even his humanity.",
      "release_date": "2014-10-10",
      "poster_path": "/lIv1QinFqz4dlp5U4lQ6HaiskOZ.jpg",
      "popularity": 8.441533,
      "title": "Whiplash",
      "video": false,
      "vote_average": 8.5,
      "vote_count": 856
    },
    {
      "adult": false,
      "backdrop_path": "/60cvl34Go8dvtDiHs9L82a79VXm.jpg",
      "genre_ids": [
        10749,
        35,
        16,
        18,
        10751
      ],
      "id": 293299,
      "original_language": "en",
      "original_title": "Feast",
      "overview": "This Oscar-winning animated short film tells the story of one man's love life is seen through the eyes of his best friend and dog, Winston, and revealed bite by bite through the meals they share.",
      "release_date": "2014-11-07",
      "poster_path": "/4XFN435sO7t9sMiWGMtWcV9qfmq.jpg",
      "popularity": 2.406332,
      "title": "Feast",
      "video": false,
      "vote_average": 8.5,
      "vote_count": 80
    },
    {
      "adult": false,
      "backdrop_path": "/W0MNr3XN95U5KLD9xIQD96YKcS.jpg",
      "genre_ids": [
        878,
        35,
        28,
        14
      ],
      "id": 251516,
      "original_language": "en",
      "original_title": "Kung Fury",
      "overview": "During an unfortunate series of events, a friend of Kung Fury is assassinated by the most dangerous kung fu master criminal of all time, Adolf Hitler, a.k.a Kung Führer. Kung Fury decides to travel back in time to Nazi Germany in order to kill Hitler and end the Nazi empire once and for all.",
      "release_date": "2015-05-28",
      "poster_path": "/oJWzpGCLIj3uYa0ux19T2WwzTOf.jpg",
      "popularity": 4.224303,
      "title": "Kung Fury",
      "video": false,
      "vote_average": 8.5,
      "vote_count": 64
    },
    {
      "adult": false,
      "backdrop_path": "/vPbAhbnXFn183UAqOLbaXDX5I2u.jpg",
      "genre_ids": [
        878,
        12
      ],
      "id": 313106,
      "original_language": "en",
      "original_title": "Doctor Who: The Day of the Doctor",
      "overview": "In 2013, something terrible is awakening in London's National Gallery; in 1562, a murderous plot is afoot in Elizabethan England; and somewhere in space an ancient battle reaches its devastating conclusion. All of reality is at stake as the Doctor's own dangerous past comes back to haunt him.",
      "release_date": "2013-11-23",
      "poster_path": "/dKSwfLWxQVR37YOmZFb5S072P3G.jpg",
      "popularity": 2.553939,
      "title": "Doctor Who: The Day of the Doctor",
      "video": false,
      "vote_average": 8.4,
      "vote_count": 55
    },
    {
      "adult": false,
      "backdrop_path": "/xu9zaAevzQ5nnrsXN6JcahLnG4i.jpg",
      "genre_ids": [
        18,
        878
      ],
      "id": 157336,
      "original_language": "en",
      "original_title": "Interstellar",
      "overview": "Interstellar chronicles the adventures of a group of explorers who make use of a newly discovered wormhole to surpass the limitations on human space travel and conquer the vast distances involved in an interstellar voyage.",
      "release_date": "2014-11-05",
      "poster_path": "/nBNZadXqJSdt05SHLqgT0HuC5Gm.jpg",
      "popularity": 16.688744,
      "title": "Interstellar",
      "video": false,
      "vote_average": 8.4,
      "vote_count": 2485
    },
    {
      "adult": false,
      "backdrop_path": "/cqn1ynw78Wan37jzs1Ckm7va97G.jpg",
      "genre_ids": [
        16,
        10749,
        10751
      ],
      "id": 140420,
      "original_language": "en",
      "original_title": "Paperman",
      "overview": "An urban office worker finds that paper airplanes are instrumental in meeting a girl in ways he never expected.",
      "release_date": "2012-11-02",
      "poster_path": "/xBo8dd2zUbMvcypwScDN3mpN7IZ.jpg",
      "popularity": 2.684451,
      "title": "Paperman",
      "video": false,
      "vote_average": 8.4,
      "vote_count": 162
    },
    {
      "adult": false,
      "backdrop_path": "/sFQ10h9DnjOYIF4HjtLQuZ8pnb4.jpg",
      "genre_ids": [
        16,
        10751
      ],
      "id": 13042,
      "original_language": "en",
      "original_title": "Presto",
      "overview": "Dignity. Poise. Mystery. We expect nothing less from the great turn-of-the-century magician, Presto. But when Presto neglects to feed his rabbit one too many times, the magician finds he isn't the only one with a few tricks up his sleeve!",
      "release_date": "2008-06-26",
      "poster_path": "/A2rxR8g3y6kcjIoR2fcwtq9eppc.jpg",
      "popularity": 1.929923,
      "title": "Presto",
      "video": false,
      "vote_average": 8.3,
      "vote_count": 164
    },
    {
      "adult": false,
      "backdrop_path": "/icWOFbzKGyU35nuzIHExYSjnLcc.jpg",
      "genre_ids": [
        16,
        10751,
        14
      ],
      "id": 110416,
      "original_language": "en",
      "original_title": "Song of the Sea",
      "overview": "The story of the last Seal Child’s journey home. After their mother’s disappearance, Ben and Saoirse are sent to live with Granny in the city. When they resolve to return to their home by the sea, their journey becomes a race against time as they are drawn into a world Ben knows only from his mother’s folktales. But this is no bedtime story; these fairy folk have been in our world far too long. It soon becomes clear to Ben that Saoirse is the key to their survival.",
      "release_date": "2014-12-19",
      "poster_path": "/uvNv23Arf2ZYtimiStSB2c1DAEX.jpg",
      "popularity": 2.34488,
      "title": "Song of the Sea",
      "video": false,
      "vote_average": 8.3,
      "vote_count": 60
    },
    {
      "adult": false,
      "backdrop_path": "/6Vb5tERWpWYlIeISYH1MDcmt499.jpg",
      "genre_ids": [
        18
      ],
      "id": 265177,
      "original_language": "en",
      "original_title": "Mommy",
      "overview": "A widowed single mother, raising her violent son alone, finds new hope when a mysterious neighbor inserts herself into their household.",
      "release_date": "2014-09-19",
      "poster_path": "/7Oq8T5XsvwgsJjc1btukMRVP4K3.jpg",
      "popularity": 1.952857,
      "title": "Mommy",
      "video": false,
      "vote_average": 8.2,
      "vote_count": 119
    },
    {
      "adult": false,
      "backdrop_path": "/bHarw8xrmQeqf3t8HpuMY7zoK4x.jpg",
      "genre_ids": [
        878,
        14,
        12
      ],
      "id": 118340,
      "original_language": "en",
      "original_title": "Guardians of the Galaxy",
      "overview": "Light years from Earth, 26 years after being abducted, Peter Quill finds himself the prime target of a manhunt after discovering an orb wanted by Ronan the Accuser.",
      "release_date": "2014-08-01",
      "poster_path": "/9gm3lL8JMTTmc3W4BmNMCuRLdL8.jpg",
      "popularity": 14.51119,
      "title": "Guardians of the Galaxy",
      "video": false,
      "vote_average": 8.2,
      "vote_count": 2706
    },
    {
      "adult": false,
      "backdrop_path": "/xBKGJQsAIeweesB79KC89FpBrVr.jpg",
      "genre_ids": [
        18,
        80
      ],
      "id": 278,
      "original_language": "en",
      "original_title": "The Shawshank Redemption",
      "overview": "Framed in the 1940s for the double murder of his wife and her lover, upstanding banker Andy Dufresne begins a new life at the Shawshank prison, where he puts his accounting skills to work for an amoral warden. During his long stretch in prison, Dufresne comes to be admired by the other inmates -- including an older prisoner named Red -- for his integrity and unquenchable sense of hope.",
      "release_date": "1994-09-14",
      "poster_path": "/9O7gLzmreU0nGkIB6K3BsJbzvNv.jpg",
      "popularity": 4.054268,
      "title": "The Shawshank Redemption",
      "video": false,
      "vote_average": 8.2,
      "vote_count": 3852
    },
    {
      "adult": false,
      "backdrop_path": "/qQzxMLMJMmkEAB6cYTEa9n2Emvz.jpg",
      "genre_ids": [
        16,
        14,
        10751,
        18
      ],
      "id": 110420,
      "original_language": "ja",
      "original_title": "Ookami kodomo no Ame to Yuki",
      "overview": "Hana, a nineteen-year-old college student, falls in love with a man only for him to reveal his secret; he is a Wolf Man. Eventually the couple bear two children together; a son and daughter they name Ame and Yuki who both inherit the ability to transform into wolves from their father. When the man Hana fell in love with suddenly dies, she makes the decision to move to a rural town isolated from society to continue raising the children in protection.",
      "release_date": "2012-08-29",
      "poster_path": "/rDMxjCYEVnvLC4nsBpB6wjL0LDy.jpg",
      "popularity": 2.046142,
      "title": "Wolf Children",
      "video": false,
      "vote_average": 8.2,
      "vote_count": 76
    },
    {
      "adult": false,
      "backdrop_path": "/qLxZZuENWJiBrmOD17Wf3qAHGQb.jpg",
      "genre_ids": [
        18,
        10749
      ],
      "id": 237791,
      "original_language": "pt",
      "original_title": "Hoje Eu Quero Voltar Sozinho",
      "overview": "Leonardo is a blind teenager searching for independence. His everyday life, the relationship with his best friend, Giovana, and the way he sees the world change completely with the arrival of Gabriel.",
      "release_date": "2014-03-28",
      "poster_path": "/oAPCsQiWV6YUd0Gt62BOwb8aSth.jpg",
      "popularity": 1.746087,
      "title": "The Way He Looks",
      "video": false,
      "vote_average": 8.2,
      "vote_count": 55
    },
    {
      "adult": false,
      "backdrop_path": "/lH2Ga8OzjU1XlxJ73shOlPx6cRw.jpg",
      "genre_ids": [
        18
      ],
      "id": 389,
      "original_language": "en",
      "original_title": "12 Angry Men",
      "overview": "The defense and the prosecution have rested and the jury is filing into the jury room to decide if a young Spanish-American is guilty or innocent of murdering his father. What begins as an open and shut case soon becomes a mini-drama of each of the jurors' prejudices and preconceptions about the trial, the accused, and each other.",
      "release_date": "1957-04-10",
      "poster_path": "/qcL1YfkCxfhsdO6sDDJ0PpzMF9n.jpg",
      "popularity": 3.04376,
      "title": "12 Angry Men",
      "video": false,
      "vote_average": 8.2,
      "vote_count": 675
    },
    {
      "adult": false,
      "backdrop_path": "/uIhEU2VUVgez3tKyPmMG9pf1q0g.jpg",
      "genre_ids": [
        14,
        16,
        18
      ],
      "id": 149871,
      "original_language": "ja",
      "original_title": "Kaguya Hime no Monogatari",
      "overview": "Found inside a shining stalk of bamboo by an old bamboo cutter and his wife, a tiny girl grows rapidly into an exquisite young lady. The mysterious young princess enthralls all who encounter her - but ultimately she must confront her fate, the punishment for her crime.",
      "release_date": "2013-11-23",
      "poster_path": "/11Az4sMt1C9sm8atgB199Z0BsIQ.jpg",
      "popularity": 2.902988,
      "title": "The Tale of the Princess Kaguya",
      "video": false,
      "vote_average": 8.1,
      "vote_count": 70
    },
    {
      "adult": false,
      "backdrop_path": "/6xKCYgH16UuwEGAyroLU6p8HLIn.jpg",
      "genre_ids": [
        80,
        18
      ],
      "id": 238,
      "original_language": "en",
      "original_title": "The Godfather",
      "overview": "The story spans the years from 1945 to 1955 and chronicles the fictional Italian-American Corleone crime family. When organized crime family patriarch Vito Corleone barely survives an attempt on his life, his youngest son, Michael, steps in to take care of the would-be killers, launching a campaign of bloody revenge.",
      "release_date": "1972-03-15",
      "poster_path": "/d4KNaTrltq6bpkFS01pYtyXa09m.jpg",
      "popularity": 3.50568,
      "title": "The Godfather",
      "video": false,
      "vote_average": 8.1,
      "vote_count": 2451
    },
    {
      "adult": false,
      "backdrop_path": "/ddlMODFJUjvhzrymuW7O7KPuhVL.jpg",
      "genre_ids": [
        18
      ],
      "id": 169813,
      "original_language": "en",
      "original_title": "Short Term 12",
      "overview": "A 20-something supervising staff member of a residential treatment facility navigates the troubled waters of that world alongside her co-worker and longtime boyfriend.",
      "release_date": "2013-08-23",
      "poster_path": "/wYkiNNMM1O5c2yEcj8Lf9UbaB1a.jpg",
      "popularity": 2.287634,
      "title": "Short Term 12",
      "video": false,
      "vote_average": 8.1,
      "vote_count": 122
    },
    {
      "adult": false,
      "backdrop_path": "/zXp2ydvhO9qGzpIsb1CWeKnn5yg.jpg",
      "genre_ids": [
        80,
        18
      ],
      "id": 654,
      "original_language": "en",
      "original_title": "On the Waterfront",
      "overview": "Terry Malloy dreams about being a prize fighter, while tending his pigeons and running errands at the docks for Johnny Friendly, the corrupt boss of the dockers union. Terry witnesses a murder by two of Johnny's thugs, and later meets the dead man's sister and feels responsible for his death. She introduces him to Father Barry, who tries to force him to provide information for the courts that will smash the dock racketeers.",
      "release_date": "1954-06-22",
      "poster_path": "/xXRgZKrdIT1GmTssy4EJoax725A.jpg",
      "popularity": 0.702477,
      "title": "On the Waterfront",
      "video": false,
      "vote_average": 8.1,
      "vote_count": 60
    },
    {
      "adult": false,
      "backdrop_path": "/fii9tPZTpy75qOCJBulWOb0ifGp.jpg",
      "genre_ids": [
        36,
        18,
        53,
        10752
      ],
      "id": 205596,
      "original_language": "en",
      "original_title": "The Imitation Game",
      "overview": "Based on the real life story of legendary cryptanalyst Alan Turing, the film portrays the nail-biting race against time by Turing and his brilliant team of code-breakers at Britain's top-secret Government Code and Cypher School at Bletchley Park, during the darkest days of World War II.",
      "release_date": "2014-11-14",
      "poster_path": "/noUp0XOqIcmgefRnRZa1nhtRvWO.jpg",
      "popularity": 15.06502,
      "title": "The Imitation Game",
      "video": false,
      "vote_average": 8.1,
      "vote_count": 1222
    },
    {
      "adult": false,
      "backdrop_path": "/ihWaJZCUIon2dXcosjQG2JHJAPN.jpg",
      "genre_ids": [
        18,
        35
      ],
      "id": 77338,
      "original_language": "fr",
      "original_title": "Intouchables",
      "overview": "A true story of two men who should never have met - a quadriplegic aristocrat who was injured in a paragliding accident and a young man from the projects.",
      "release_date": "2011-11-02",
      "poster_path": "/4mFsNQwbD0F237Tx7gAPotd0nbJ.jpg",
      "popularity": 3.972343,
      "title": "The Intouchables",
      "video": false,
      "vote_average": 8.1,
      "vote_count": 1465
    }
  ],
  "total_pages": 166,
  "total_results": 3316
}

Get the list of upcoming movies by release date. This list refreshes every day.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td colspan="2">api_key</td></tr>
<tr><td colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-right: 40px; width: 140px;">page</td><td>Minimum 1, maximum 1000.</td></tr>
<tr><td style="padding-right: 40px; width: 140px;">language</td><td>ISO 639-1 code.</td></tr>
</table>

GET /movie/upcoming
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "6062d4825a2cd1efa5a7757c91a5f7a8"
{
  "dates": {
    "minimum": "2015-06-24",
    "maximum": "2015-07-15"
  },
  "page": 1,
  "results": [
    {
      "adult": false,
      "backdrop_path": "/bIlYH4l2AyYvEysmS2AOfjO7Dn8.jpg",
      "genre_ids": [
        878,
        28,
        53,
        12
      ],
      "id": 87101,
      "original_language": "en",
      "original_title": "Terminator Genisys",
      "overview": "The year is 2029. John Connor, leader of the resistance continues the war against the machines. At the Los Angeles offensive, John's fears of the unknown future begin to emerge when TECOM spies reveal a new plot by SkyNet that will attack him from both fronts; past and future, and will ultimately change warfare forever.",
      "release_date": "2015-07-01",
      "poster_path": "/qOoFD4HD9a2EEUymdzBQN9XF1UJ.jpg",
      "popularity": 15.394013,
      "title": "Terminator Genisys",
      "video": false,
      "vote_average": 7.3,
      "vote_count": 35
    },
    {
      "adult": false,
      "backdrop_path": "/g4CbTb1zMseYMqrqU4mBvjx8bGp.jpg",
      "genre_ids": [
        35
      ],
      "id": 214756,
      "original_language": "en",
      "original_title": "Ted 2",
      "overview": "Newlywed couple Ted and Tami-Lynn want to have a baby, but in order to qualify to be a parent, Ted will have to prove he's a person in a court of law.",
      "release_date": "2015-06-26",
      "poster_path": "/A7HtCxFe7Ms8H7e7o2zawppbuDT.jpg",
      "popularity": 5.118746,
      "title": "Ted 2",
      "video": false,
      "vote_average": 7.3,
      "vote_count": 38
    },
    {
      "adult": false,
      "backdrop_path": "/9scUkyWZ1oAhfOGzhFLRdQ1BuIO.jpg",
      "genre_ids": [
        10751,
        16,
        12,
        35
      ],
      "id": 211672,
      "original_language": "en",
      "original_title": "Minions",
      "overview": "Minions Stuart, Kevin and Bob are recruited by Scarlet Overkill, a super-villain who, alongside her inventor husband Herb, hatches a plot to take over the world.",
      "release_date": "2015-07-10",
      "poster_path": "/s5uMY8ooGRZOL0oe4sIvnlTsYQO.jpg",
      "popularity": 3.32247,
      "title": "Minions",
      "video": false,
      "vote_average": 8.3,
      "vote_count": 41
    },
    {
      "adult": false,
      "backdrop_path": "/76q81bYJbORdYRYiasZWTMyxLP3.jpg",
      "genre_ids": [
        35
      ],
      "id": 276844,
      "original_language": "en",
      "original_title": "The Little Death",
      "overview": "A comedy film that looks into the loosely connected lives of people with strange sexual fantasies. A woman with a dangerous fantasy and her partner's struggle to please her. A man who begins an affair with his own wife without her knowing anything about it. A couple struggling to keep things together after a sexual experiment spins out of control. A woman who can only find pleasure in her husband's pain. A call centre operator caught in the middle of a dirty and chaotic phone call. And the distractingly charming new neighbour who connects them all.",
      "release_date": "2015-06-26",
      "poster_path": "/eFxgujAuoCZ3mwqAjhHi5nvZ8Sd.jpg",
      "popularity": 1.656292,
      "title": "The Little Death",
      "video": false,
      "vote_average": 6.0,
      "vote_count": 29
    },
    {
      "adult": false,
      "backdrop_path": "/7KV2lhasRMdwOn6wr5N1iZFbnmw.jpg",
      "genre_ids": [
        10751,
        18,
        12
      ],
      "id": 272878,
      "original_language": "en",
      "original_title": "Max",
      "overview": "A dog that helped soldiers in Afghanistan returns to the U.S. and is adopted by his handler's family after suffering a traumatic experience.",
      "release_date": "2015-06-26",
      "poster_path": "/gNGAHcK1DBWYTUgKExb3MnhlJ09.jpg",
      "popularity": 2.604804,
      "title": "Max",
      "video": false,
      "vote_average": 9.8,
      "vote_count": 2
    },
    {
      "adult": false,
      "backdrop_path": "/eWjXLMCsCXEXWZoVnNGyu4Dd72Y.jpg",
      "genre_ids": [
        27,
        53
      ],
      "id": 263472,
      "original_language": "en",
      "original_title": "Knock Knock",
      "overview": "Two young women show up at the home of a married man and begin to systematically destroy his idyllic life.",
      "release_date": "2015-06-26",
      "poster_path": "/id9Sw7VIn97W3crPd1MIRHJ6t9Y.jpg",
      "popularity": 1.572557,
      "title": "Knock Knock",
      "video": false,
      "vote_average": 4.0,
      "vote_count": 4
    },
    {
      "adult": false,
      "backdrop_path": "/b7jcdzZ4HU5yAlkfnksQHCSgpGb.jpg",
      "genre_ids": [
        18,
        10749
      ],
      "id": 292431,
      "original_language": "en",
      "original_title": "Love",
      "overview": "A sexual melodrama about a boy and a girl and another girl. It's a love story, which celebrates sex in a joyous way.",
      "release_date": "2015-07-15",
      "poster_path": "/s3VeuIEnTgDEuUxlPAUmWItOnIt.jpg",
      "popularity": 2.507412,
      "title": "Love",
      "video": false,
      "vote_average": 7.0,
      "vote_count": 1
    },
    {
      "adult": false,
      "backdrop_path": "/1DeBPZiBb0qeZ7NdzE6nGflgIKd.jpg",
      "genre_ids": [
        35,
        18,
        10749
      ],
      "id": 283227,
      "original_language": "en",
      "original_title": "A Little Chaos",
      "overview": "A landscape gardener is hired by famous architect Le Notre to construct the grand gardens at the palace of Versailles. As the two work on the palace, they find themselves drawn to each other and are thrown into rivalries within the court of King Louis XIV.",
      "release_date": "2015-06-26",
      "poster_path": "/AeKFi9acqgTGZGqvBvFdMx32Cda.jpg",
      "popularity": 1.911189,
      "title": "A Little Chaos",
      "video": false,
      "vote_average": 4.4,
      "vote_count": 5
    },
    {
      "adult": false,
      "backdrop_path": "/u7GSYEvjx0bIzoloDqj6Ea3xD1.jpg",
      "genre_ids": [
        35,
        27
      ],
      "id": 323373,
      "original_language": "en",
      "original_title": "Deathgasm",
      "overview": "Two teenage boys unwittingly summon an ancient evil entity known as The Blind One by delving into black magic while trying to escape their mundane lives.",
      "release_date": "2015-06-26",
      "poster_path": "/yz3L43i3G6OxnXGLYJKepIX9FbM.jpg",
      "popularity": 0.836165,
      "title": "Deathgasm",
      "video": false,
      "vote_average": 0.0,
      "vote_count": 0
    },
    {
      "adult": false,
      "backdrop_path": null,
      "genre_ids": [
        18,
        27
      ],
      "id": 342991,
      "original_language": "en",
      "original_title": "Prognosis",
      "overview": "A woman receives a dire prognosis.",
      "release_date": "2015-06-25",
      "poster_path": "/jVw04CEKTfAWni7uF8KB1X4u4t7.jpg",
      "popularity": 0.784143,
      "title": "Prognosis",
      "video": false,
      "vote_average": 0.0,
      "vote_count": 0
    },
    {
      "adult": false,
      "backdrop_path": "/hPAf2ifJe2nQcswhtMMdedyBJ5v.jpg",
      "genre_ids": [
        35,
        18,
        10402
      ],
      "id": 264999,
      "original_language": "en",
      "original_title": "Magic Mike XXL",
      "overview": "Three years after Mike bowed out of the stripper life at the top of his game, he and the remaining Kings of Tampa hit the road to Myrtle Beach to put on one last blow-out performance.",
      "release_date": "2015-07-01",
      "poster_path": "/8lBqG0KycfeSr0DBy1uauIvFEu.jpg",
      "popularity": 1.770784,
      "title": "Magic Mike XXL",
      "video": false,
      "vote_average": 7.8,
      "vote_count": 2
    },
    {
      "adult": false,
      "backdrop_path": "/bAwGhvTqbeOu9zNoQsvGRe0MS6L.jpg",
      "genre_ids": [
        27,
        53
      ],
      "id": 310137,
      "original_language": "en",
      "original_title": "Reversal",
      "overview": "A young girl, chained in the basement of a sexual predator, manages to escape and then turns the tables on her captor.",
      "release_date": "2015-06-28",
      "poster_path": "/n5EXevWKnakKu2aawST1p5VBs8L.jpg",
      "popularity": 0.764869,
      "title": "Bound to Vengeance",
      "video": false,
      "vote_average": 0.0,
      "vote_count": 0
    },
    {
      "adult": false,
      "backdrop_path": null,
      "genre_ids": [
        35,
        27
      ],
      "id": 296503,
      "original_language": "en",
      "original_title": "Dude Bro Party Massacre III",
      "overview": "In the wake of two back-to-back mass murders on Chico's frat row, loner Brent Chirino must infiltrate the ranks of a popular fraternity to investigate his twin brother's murder at the hands of the serial killer known as \"Motherface.\"",
      "release_date": "2015-06-30",
      "poster_path": "/4lkWO3enHPP7Ga9EPtz7pE8IB1K.jpg",
      "popularity": 0.727813,
      "title": "Dude Bro Party Massacre III",
      "video": false,
      "vote_average": 0.0,
      "vote_count": 0
    },
    {
      "adult": false,
      "backdrop_path": null,
      "genre_ids": [
        18,
        10752
      ],
      "id": 343843,
      "original_language": "en",
      "original_title": "연평해전",
      "overview": "The movies follows the incident knows as the second battle of Yeonpyeong which happened in 2002.",
      "release_date": "2015-06-24",
      "poster_path": "/1eto5bnM4m8MvA7fKLYKvmKd9vZ.jpg",
      "popularity": 0.710686,
      "title": "Northern Limit Line",
      "video": false,
      "vote_average": 0.0,
      "vote_count": 0
    },
    {
      "adult": false,
      "backdrop_path": "/fzHOiL0PXlXII9k2KZTKiUZlET5.jpg",
      "genre_ids": [
        27
      ],
      "id": 299245,
      "original_language": "en",
      "original_title": "The Gallows",
      "overview": "20 years after a horrific accident during a small town school play, students at the school resurrect the failed show in a misguided attempt to honor the anniversary of the tragedy - but soon discover that some things are better left alone.",
      "release_date": "2015-07-10",
      "poster_path": "/8KQ0LPgGcASjJFf6qB8VtssqoRa.jpg",
      "popularity": 1.709617,
      "title": "The Gallows",
      "video": false,
      "vote_average": 0.0,
      "vote_count": 0
    },
    {
      "adult": false,
      "backdrop_path": "/30HAX7kZZEaCltD9tvJJmCn9vBN.jpg",
      "genre_ids": [
        27
      ],
      "id": 312831,
      "original_language": "en",
      "original_title": "The Woods",
      "overview": "A family who move into a remote milllhouse in Ireland find themselves in a fight for survival with demonic creatures living in the woods.",
      "release_date": "2015-06-26",
      "poster_path": "/7NQuwtcNyHnOa7cmZEY71VPynAX.jpg",
      "popularity": 0.708282,
      "title": "The Woods",
      "video": false,
      "vote_average": 0.0,
      "vote_count": 0
    },
    {
      "adult": false,
      "backdrop_path": null,
      "genre_ids": [
        80,
        18,
        53
      ],
      "id": 200511,
      "original_language": "en",
      "original_title": "7 Minutes",
      "overview": "A young athlete takes a wild turn in life after suffering a serious injury.",
      "release_date": "2015-06-26",
      "poster_path": "/hc72bbafFvLGun0eAx7KjugSIIM.jpg",
      "popularity": 0.649238,
      "title": "7 Minutes",
      "video": false,
      "vote_average": 6.5,
      "vote_count": 2
    },
    {
      "adult": false,
      "backdrop_path": null,
      "genre_ids": [
        99
      ],
      "id": 343880,
      "original_language": "en",
      "original_title": "A Murder in the Park",
      "overview": "Documentary filmmakers assert that Anthony Porter - a former death-row inmate who was spared the death penalty thanks to the efforts of a college journalism program - was actually guilty, and an innocent man was sent to prison.",
      "release_date": "2015-06-26",
      "poster_path": "/1oD2FUvNJN6SEmkNybKsEZj9APh.jpg",
      "popularity": 0.641767,
      "title": "A Murder in the Park",
      "video": false,
      "vote_average": 0.0,
      "vote_count": 0
    },
    {
      "adult": false,
      "backdrop_path": null,
      "genre_ids": [
        35,
        878
      ],
      "id": 86828,
      "original_language": "en",
      "original_title": "Absolutely Anything",
      "overview": "A teacher experiences a series of mishaps after discovering he has magical powers.",
      "release_date": "2015-07-03",
      "poster_path": "/diZK6K2wRhgvfTJ5HRPjM4liRt8.jpg",
      "popularity": 0.626652,
      "title": "Absolutely Anything",
      "video": false,
      "vote_average": 0.0,
      "vote_count": 0
    },
    {
      "adult": false,
      "backdrop_path": "/nUs37LYXf8uczW1nFsyeHRp2EiE.jpg",
      "genre_ids": [
        35
      ],
      "id": 306943,
      "original_language": "en",
      "original_title": "The Outskirts",
      "overview": "After falling victim to a humiliating prank by the high school Queen Bee, best friends and world-class geeks, Mindy and Jodi, decide to get their revenge by uniting the outcasts of the school against her and her circle of friends.",
      "release_date": "2015-06-26",
      "poster_path": "/6sJCmrCVVdYvcEtLzAItQwZaacv.jpg",
      "popularity": 1.622033,
      "title": "The Outskirts",
      "video": false,
      "vote_average": 0.0,
      "vote_count": 0
    }
  ],
  "total_pages": 5,
  "total_results": 91
}

--
Networks
--

This method is used to retrieve the basic information about a TV network. You can use this ID to search for TV shows with the [discover](#discover). At this time we don't have much but this will be fleshed out over time.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
</table>

GET /network/{id}
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "de3d5fb10d305637265caffd2c310568"
{
  "id": 49,
  "name": "HBO"
}

--
People
--

Get the general person information for a specific id.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
<tr><td style="padding-left: 0;" colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-left: 0; width: 140px;">append_to_response</td><td>Comma separated, any person method</td></tr>
</table>

GET /person/{id}
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "6538fd4d8a83e8e2f26bc5dbc34a3dbe"
{
    "adult": false,
    "also_known_as": [],
    "biography": "From Wikipedia, the free encyclopedia.\n\nWilliam Bradley \"Brad\" Pitt (born December 18, 1963) is an American actor and film producer. Pitt has received two Academy Award nominations and four Golden Globe Award nominations, winning one. He has been described as one of the world's most attractive men, a label for which he has received substantial media attention.\n\nPitt began his acting career with television guest appearances, including a role on the CBS prime-time soap opera Dallas in 1987. He later gained recognition as the cowboy hitchhiker who seduces Geena Davis's character in the 1991 road movie Thelma & Louise. Pitt's first leading roles in big-budget productions came with A River Runs Through It (1992) and Interview with the Vampire (1994). He was cast opposite Anthony Hopkins in the 1994 drama Legends of the Fall, which earned him his first Golden Globe nomination. In 1995 he gave critically acclaimed performances in the crime thriller Seven and the science fiction film 12 Monkeys, the latter securing him a Golden Globe Award for Best Supporting Actor and an Academy Award nomination. Four years later, in 1999, Pitt starred in the cult hit Fight Club. He then starred in the major international hit as Rusty Ryan in Ocean's Eleven (2001) and its sequels, Ocean's Twelve (2004) and Ocean's Thirteen (2007). His greatest commercial successes have been Troy (2004) and Mr. & Mrs. Smith (2005). Pitt received his second Academy Award nomination for his title role performance in the 2008 film The Curious Case of Benjamin Button.\n\nFollowing a high-profile relationship with actress Gwyneth Paltrow, Pitt was married to actress Jennifer Aniston for five years. Pitt lives with actress Angelina Jolie in a relationship that has generated wide publicity. He and Jolie have six children—Maddox, Pax, Zahara, Shiloh, Knox, and Vivienne. Since beginning his relationship with Jolie, he has become increasingly involved in social issues both in the United States and internationally. Pitt owns a production company named Plan B Entertainment, whose productions include the 2007 Academy Award winning Best Picture, The Departed.\n\nDescription above from the Wikipedia article Brad Pitt, licensed under CC-BY-SA, full list of contributors on Wikipedia.",
    "birthday": "1963-12-18",
    "deathday": "",
    "homepage": "http://simplybrad.com/",
    "id": 287,
    "name": "Brad Pitt",
    "place_of_birth": "Shawnee, Oklahoma, United States",
    "profile_path": "/w8zJQuN7tzlm6FY9mfGKihxp3Cb.jpg"
}

Get the movie credits for a specific person id.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
<tr><td style="padding-left: 0;" colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-left: 0; width: 140px;">language</td><td>ISO 639-1 code.</td></tr>
<tr><td style="padding-left: 0; width: 140px;">append_to_response</td><td>Comma separated, any person method</td></tr>
</table>

GET /person/{id}/movie_credits
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "ed59e17b5c7940de30c6f597ebbf0200"
{
  "cast": [
    {
      "adult": false,
      "character": "Jeffrey Goines",
      "credit_id": "52fe4212c3a36847f8001b39",
      "id": 63,
      "original_title": "Twelve Monkeys",
      "poster_path": "/6Sj9wDu3YugthXsU0Vry5XFAZGg.jpg",
      "release_date": "1995-12-27",
      "title": "Twelve Monkeys"
    },
    {
      "adult": false,
      "character": "Mickey O'Neil",
      "credit_id": "52fe4218c3a36847f8003be5",
      "id": 107,
      "original_title": "Snatch",
      "poster_path": "/on9JlbGEccLsYkjeEph2Whm1DIp.jpg",
      "release_date": "2000-12-06",
      "title": "Snatch"
    },
    {
      "adult": false,
      "character": "Rusty Ryan",
      "credit_id": "52fe4220c3a36847f800616b",
      "id": 161,
      "original_title": "Ocean's Eleven",
      "poster_path": "/o0h76DVXvk5OKjmNez5YY0GODC2.jpg",
      "release_date": "2001-12-06",
      "title": "Ocean's Eleven"
    },
    {
      "adult": false,
      "character": "Rusty Ryan",
      "credit_id": "52fe4221c3a36847f80062e5",
      "id": 163,
      "original_title": "Ocean's Twelve",
      "poster_path": "/4irBEiDC7SpYBuw5z1c7oLfF0EI.jpg",
      "release_date": "2004-12-09",
      "title": "Ocean's Twelve"
    },
    {
      "adult": false,
      "character": "Paul Maclean",
      "credit_id": "52fe4233c3a36847f800bb79",
      "id": 293,
      "original_title": "A River Runs Through It",
      "poster_path": "/xX4H1hZG9IgSRkC0LANbPQ0StJi.jpg",
      "release_date": "1992-10-09",
      "title": "A River Runs Through It"
    },
    {
      "adult": false,
      "character": "Joe Black",
      "credit_id": "52fe4234c3a36847f800bdbb",
      "id": 297,
      "original_title": "Meet Joe Black",
      "poster_path": "/nlxPnkZY3vY1iehJriKMQcT6eua.jpg",
      "release_date": "1998-11-12",
      "title": "Meet Joe Black"
    },
    {
      "adult": false,
      "character": "Robert “Rusty” Charles Ryan",
      "credit_id": "52fe4234c3a36847f800bf0f",
      "id": 298,
      "original_title": "Ocean's Thirteen",
      "poster_path": "/k7jNRNN9UuUc2VQg21YXYn3JGVY.jpg",
      "release_date": "2007-06-07",
      "title": "Ocean's Thirteen"
    },
    {
      "adult": false,
      "character": "Floyd",
      "credit_id": "52fe4237c3a36847f800cdd3",
      "id": 319,
      "original_title": "True Romance",
      "poster_path": "/xBO8R3CZfrJ9rrwrZoJ68PgJyAR.jpg",
      "release_date": "1993-09-09",
      "title": "True Romance"
    },
    {
      "adult": false,
      "character": "Himself",
      "credit_id": "52fe4249c3a36847f801293b",
      "id": 492,
      "original_title": "Being John Malkovich",
      "poster_path": "/gLhl4MBEC6yHTInwV7TxV1D3FLp.jpg",
      "release_date": "1999-09-30",
      "title": "Being John Malkovich"
    },
    {
      "adult": false,
      "character": "Tyler Durden",
      "credit_id": "52fe4250c3a36847f80149f7",
      "id": 550,
      "original_title": "Fight Club",
      "poster_path": "/2lECpi35Hnbpa4y46JX0aY3AWTy.jpg",
      "release_date": "1999-10-14",
      "title": "Fight Club"
    },
    {
      "adult": false,
      "character": "Louis de Pointe du Lac",
      "credit_id": "52fe4260c3a36847f80199f9",
      "id": 628,
      "original_title": "Interview with the Vampire",
      "poster_path": "/qHRoxkDokG0rpRFeHHykgDf3Chy.jpg",
      "release_date": "1994-11-11",
      "title": "Interview with the Vampire"
    },
    {
      "adult": false,
      "character": "Achilles",
      "credit_id": "52fe4264c3a36847f801b083",
      "id": 652,
      "original_title": "Troy",
      "poster_path": "/edMlij7nw2NMla32xskDnzMCFBM.jpg",
      "release_date": "2004-05-13",
      "title": "Troy"
    },
    {
      "adult": false,
      "character": "John Smith",
      "credit_id": "52fe4276c3a36847f80208cb",
      "id": 787,
      "original_title": "Mr. & Mrs. Smith",
      "poster_path": "/dqs5BmwSULtB28Kls3IB6khTQwp.jpg",
      "release_date": "2005-06-09",
      "title": "Mr. & Mrs. Smith"
    },
    {
      "adult": false,
      "character": "Detective David Mills",
      "credit_id": "52fe4279c3a36847f802178b",
      "id": 807,
      "original_title": "Se7en",
      "poster_path": "/zgB9CCTDlXRv50Z70ZI4elJtNEk.jpg",
      "release_date": "1995-09-22",
      "title": "Se7en"
    },
    {
      "adult": false,
      "character": "Michael Sullivan",
      "credit_id": "52fe427bc3a36847f80222a7",
      "id": 819,
      "original_title": "Sleepers",
      "poster_path": "/80LV2GvhvMEuOutxgQwbRYpo0Vv.jpg",
      "release_date": "1996-10-18",
      "title": "Sleepers"
    },
    {
      "adult": false,
      "character": "Heinrich Harrer",
      "credit_id": "52fe4295c3a36847f802a10d",
      "id": 978,
      "original_title": "Seven Years in Tibet",
      "poster_path": "/cflSeFUVDCf73Tzh5sB204JbQ6j.jpg",
      "release_date": "1997-10-08",
      "title": "Seven Years in Tibet"
    },
    {
      "adult": false,
      "character": "Richard",
      "credit_id": "52fe42e9c3a36847f802c221",
      "id": 1164,
      "original_title": "Babel",
      "poster_path": "/fxneN0EQZwTfAfhTGUvUuIn6PLi.jpg",
      "release_date": "2006-10-27",
      "title": "Babel"
    },
    {
      "adult": false,
      "character": "Tom Bishop",
      "credit_id": "52fe42fbc3a36847f8031727",
      "id": 1535,
      "original_title": "Spy Game",
      "poster_path": "/hsb8hBeU3tkTX8SUYW6YYw6JPYD.jpg",
      "release_date": "2001-11-18",
      "title": "Spy Game"
    },
    {
      "adult": false,
      "character": "J.D.",
      "credit_id": "52fe42fbc3a36847f8031a6d",
      "id": 1541,
      "original_title": "Thelma & Louise",
      "poster_path": "/uxfIGF3rHOfu9IyvHbGVmFvvPgH.jpg",
      "release_date": "1991-05-24",
      "title": "Thelma & Louise"
    },
    {
      "adult": false,
      "character": "Tristan Ludlow",
      "credit_id": "52fe43c4c3a36847f806e20d",
      "id": 4476,
      "original_title": "Legends of the Fall",
      "poster_path": "/uh0sJcx3SLtclJSuKAXl6Tt6AV0.jpg",
      "release_date": "1994-12-16",
      "title": "Legends of the Fall"
    },
    {
      "adult": false,
      "character": "Frankie McGuire",
      "credit_id": "52fe43c4c3a36847f806e2b9",
      "id": 4477,
      "original_title": "The Devil's Own",
      "poster_path": "/1zusKhAtJ3YhbuA6J184Vm4INB3.jpg",
      "release_date": "1997-03-13",
      "title": "The Devil's Own"
    },
    {
      "adult": false,
      "character": "Jesse James",
      "credit_id": "52fe43c7c3a36847f806eed5",
      "id": 4512,
      "original_title": "The Assassination of Jesse James by the Coward Robert Ford",
      "poster_path": "/lSFYLoaL4eW7Q5VQ7SZQP4EHRCt.jpg",
      "release_date": "2007-09-20",
      "title": "The Assassination of Jesse James by the Coward Robert Ford"
    },
    {
      "adult": false,
      "character": "Early Grayce",
      "credit_id": "52fe43ce9251416c7501ef13",
      "id": 10909,
      "original_title": "Kalifornia",
      "poster_path": "/y8SJxeBmkmSWPTurQEacZmrEyey.jpg",
      "release_date": "1993-09-03",
      "title": "Kalifornia"
    },
    {
      "adult": false,
      "character": "Billy Canton",
      "credit_id": "52fe43d09251416c7501f331",
      "id": 10917,
      "original_title": "Too Young to Die?",
      "poster_path": "/7qwH5pdWhScueSoZlorJLxqoNvH.jpg",
      "release_date": "1990-02-26",
      "title": "Too Young to Die?"
    },
    {
      "adult": false,
      "character": "Brad, 1st Bachelor",
      "credit_id": "52fe43e2c3a36847f807610f",
      "id": 4912,
      "original_title": "Confessions of a Dangerous Mind",
      "poster_path": "/vQqxMsivH5cKNlEJJcYBcBQZ5rZ.jpg",
      "release_date": "2002-12-30",
      "title": "Confessions of a Dangerous Mind"
    },
    {
      "adult": false,
      "character": "Benjamin Button",
      "credit_id": "52fe43e2c3a36847f807632f",
      "id": 4922,
      "original_title": "The Curious Case of Benjamin Button",
      "poster_path": "/4O4INOPtWTfHq3dd5vYTPV0TCwa.jpg",
      "release_date": "2008-11-24",
      "title": "The Curious Case of Benjamin Button"
    },
    {
      "adult": false,
      "character": "Chad Feldheimer",
      "credit_id": "52fe43e6c3a36847f8076fe7",
      "id": 4944,
      "original_title": "Burn After Reading",
      "poster_path": "/w1Hb2rU2rMhvnbPkrj8KKc4CbMx.jpg",
      "release_date": "2008-09-12",
      "title": "Burn After Reading"
    },
    {
      "adult": false,
      "character": "Himself",
      "credit_id": "52fe44259251416c91006683",
      "id": 30565,
      "original_title": "The Hamster Factor and Other Tales of Twelve Monkeys",
      "poster_path": "/sfPxX29hVdBq8cP5839Dx91cCW9.jpg",
      "release_date": "1997-01-01",
      "title": "The Hamster Factor and Other Tales of Twelve Monkeys"
    },
    {
      "adult": false,
      "character": "Dwight Ingalls",
      "credit_id": "52fe4428c3a368484e012c37",
      "id": 21799,
      "original_title": "Cutting Class",
      "poster_path": "/y8eBkmikGYZmhmZfNjgzWD5Lo1d.jpg",
      "release_date": "1989-07-01",
      "title": "Cutting Class"
    },
    {
      "adult": false,
      "character": "Detective Frank Harris",
      "credit_id": "52fe45dd9251416c75065151",
      "id": 14239,
      "original_title": "Cool World",
      "poster_path": "/eSR3vFpgGQfYQYI2fMbwIZp70lp.jpg",
      "release_date": "1992-07-10",
      "title": "Cool World"
    },
    {
      "adult": false,
      "character": "Sinbad",
      "credit_id": "52fe45f09251416c75067ad5",
      "id": 14411,
      "original_title": "Sinbad: Legend of the Seven Seas",
      "poster_path": "/6LELf4ZzVBJwR9mNq86Mf5QVERS.jpg",
      "release_date": "2003-07-02",
      "title": "Sinbad: Legend of the Seven Seas"
    },
    {
      "adult": false,
      "character": "Billy Beane",
      "credit_id": "52fe461fc3a368484e0800bd",
      "id": 60308,
      "original_title": "Moneyball",
      "poster_path": "/9wr4yJKMjPCNPI9S8AZYHnUZdFu.jpg",
      "release_date": "2011-09-22",
      "title": "Moneyball"
    },
    {
      "adult": false,
      "character": "Metro Man (voice)",
      "credit_id": "52fe468e9251416c910583cd",
      "id": 38055,
      "original_title": "Megamind",
      "poster_path": "/b4mcRWB2TofqhiGDEuXJKkbthif.jpg",
      "release_date": "2010-11-04",
      "title": "Megamind"
    },
    {
      "adult": false,
      "character": "Himself",
      "credit_id": "52fe46acc3a368484e09d959",
      "id": 63472,
      "original_title": "His Way",
      "poster_path": "/wTQHzluHXnSzyh0hbsh2n2ky6kj.jpg",
      "release_date": "2011-03-04",
      "title": "His Way"
    },
    {
      "adult": false,
      "character": "Johnny Suede",
      "credit_id": "52fe46b2c3a36847f810d153",
      "id": 45145,
      "original_title": "Johnny Suede",
      "poster_path": "/3z4OZo5j1hkQ3AI8xTzuyqUD9Ru.jpg",
      "release_date": "1991-06-15",
      "title": "Johnny Suede"
    },
    {
      "adult": false,
      "character": "Jackie Cogan",
      "credit_id": "52fe46e4c3a368484e0a9a51",
      "id": 64689,
      "original_title": "Killing Them Softly",
      "poster_path": "/pIS0JWCYJYesGNAd6gWbtSwzgsF.jpg",
      "release_date": "2012-11-30",
      "title": "Killing Them Softly"
    },
    {
      "adult": false,
      "character": "Lt. Aldo Raine",
      "credit_id": "52fe46f29251416c75088c69",
      "id": 16869,
      "original_title": "Inglourious Basterds",
      "poster_path": "/vDwqPyhkzFPRDmwz9KbzN2ouEPe.jpg",
      "release_date": "2009-08-21",
      "title": "Inglourious Basterds"
    },
    {
      "adult": false,
      "character": "Will the Krill (voice)",
      "credit_id": "52fe4718c3a368484e0b4d23",
      "id": 65759,
      "original_title": "Happy Feet Two",
      "poster_path": "/gY8lWCObaGvcDsmeM8QHBF4AZVk.jpg",
      "release_date": "2011-11-17",
      "title": "Happy Feet Two"
    },
    {
      "adult": false,
      "character": "",
      "credit_id": "52fe4ef6c3a36847f82b3c95",
      "id": 244743,
      "original_title": "Contact",
      "poster_path": "/gAmyqdAlwzB8Et34ESMrl7tosn4.jpg",
      "release_date": "1992-01-01",
      "title": "Contact"
    },
    {
      "adult": false,
      "character": "Joe Maloney",
      "credit_id": "52fe4766c3a36847f81338e5",
      "id": 48448,
      "original_title": "Across the Tracks",
      "poster_path": "/o2Xvw7Wfzk1Q10yDNOgVx63CsIv.jpg",
      "release_date": "1991-02-14",
      "title": "Across the Tracks"
    },
    {
      "adult": false,
      "character": "Mickey O'Neil (Snatch) (archive footage)",
      "credit_id": "52fe47b4c3a368484e0d520b",
      "id": 68996,
      "original_title": "Ultimate Fights from the Movies",
      "poster_path": "/2KIKXjKoNTmpi22gsU3KUMv6wKA.jpg",
      "release_date": "2002-04-16",
      "title": "Ultimate Fights from the Movies"
    },
    {
      "adult": false,
      "character": "Elliott Fowler",
      "credit_id": "52fe47c8c3a36847f8147ff5",
      "id": 50463,
      "original_title": "The Favor",
      "poster_path": "/68a68qALehxulfbL2P56OKAJ6Ci.jpg",
      "release_date": "1994-04-29",
      "title": "The Favor"
    },
    {
      "adult": false,
      "character": "Gerry Lane",
      "credit_id": "52fe485dc3a368484e0f5061",
      "id": 72190,
      "original_title": "World War Z",
      "poster_path": "/gAt1PrsrFY1nX6UzebeiHP8njE9.jpg",
      "release_date": "2013-06-21",
      "title": "World War Z"
    },
    {
      "adult": false,
      "character": "Brian",
      "credit_id": "52fe48b9c3a36847f81761cb",
      "id": 55059,
      "original_title": "Happy Together",
      "poster_path": "/5H766FOKWY0GsbdMdQktqQYv3ku.jpg",
      "release_date": "1989-05-04",
      "title": "Happy Together"
    },
    {
      "adult": false,
      "character": "Steve Black",
      "credit_id": "52fe4900c3a368484e115b63",
      "id": 75451,
      "original_title": "The Image",
      "poster_path": "/xfiHVr42KCIu2FvKd5uD2iPbeie.jpg",
      "release_date": "1990-01-27",
      "title": "The Image"
    },
    {
      "adult": false,
      "character": "Bass",
      "credit_id": "52fe492cc3a368484e11dfa3",
      "id": 76203,
      "original_title": "12 Years a Slave",
      "poster_path": "/onpio7AWndkzH6mptb35cCuV9q1.jpg",
      "release_date": "2013-10-18",
      "title": "12 Years a Slave"
    },
    {
      "adult": false,
      "character": "Narrator (voice)",
      "credit_id": "52fe49a29251416c910b39e7",
      "id": 86822,
      "original_title": "Voyage of Time",
      "poster_path": null,
      "release_date": null,
      "title": "Voyage of Time"
    },
    {
      "adult": false,
      "character": "Westray",
      "credit_id": "52fe4aaac3a36847f81db47d",
      "id": 109091,
      "original_title": "The Counselor",
      "poster_path": "/aua3i7T6KiWwvh9rvawIIBRjn1b.jpg",
      "release_date": "2013-10-25",
      "title": "The Counselor"
    },
    {
      "adult": false,
      "character": "Chief Judge Vaughn R. Walker",
      "credit_id": "52fe4ab2c3a36847f81dcd13",
      "id": 109404,
      "original_title": "8",
      "poster_path": "/28fDtVBr6PyHsFFqyKJCeN3ysBP.jpg",
      "release_date": "2012-03-03",
      "title": "8"
    },
    {
      "adult": false,
      "character": "",
      "credit_id": "52fe4b529251416c750ff4b9",
      "id": 145206,
      "original_title": "20,000 Leagues Under the Sea: Captain Nemo",
      "poster_path": null,
      "release_date": null,
      "title": "20,000 Leagues Under the Sea: Captain Nemo"
    },
    {
      "adult": false,
      "character": "Yuri Trush",
      "credit_id": "52fe4b569251416c750ffb19",
      "id": 145266,
      "original_title": "The Tiger",
      "poster_path": null,
      "release_date": null,
      "title": "The Tiger"
    },
    {
      "adult": false,
      "character": "Wardaddy",
      "credit_id": "52fe4ec09251416c7516126f",
      "id": 228150,
      "original_title": "Fury",
      "poster_path": null,
      "release_date": "2014-11-14",
      "title": "Fury"
    },
    {
      "adult": false,
      "character": "Jerry Welbach",
      "credit_id": "52fe443bc3a36847f8089dd5",
      "id": 6073,
      "original_title": "The Mexican",
      "poster_path": "/a7PuqWv0ENFg8dt9k51AID6P1kh.jpg",
      "release_date": "2001-03-01",
      "title": "The Mexican"
    },
    {
      "adult": false,
      "character": "John Dean",
      "credit_id": "52fe44cac3a36847f80aa277",
      "id": 8947,
      "original_title": "Dirty Tricks",
      "poster_path": null,
      "release_date": "2013-12-31",
      "title": "Dirty Tricks"
    },
    {
      "adult": false,
      "character": "Mr. O'Brien",
      "credit_id": "52fe44ccc3a36847f80aa95b",
      "id": 8967,
      "original_title": "The Tree of Life",
      "poster_path": "/wt5Q6SQFeU2mW7UKF10mTwyyHog.jpg",
      "release_date": "2011-05-27",
      "title": "The Tree of Life"
    },
    {
      "adult": false,
      "character": "Rick",
      "credit_id": "52fe4510c3a368484e0467cb",
      "id": 26642,
      "original_title": "The Dark Side of the Sun",
      "poster_path": "/pofw83GCrfG2B6JxH53n8vX1pDv.jpg",
      "release_date": "1987-12-31",
      "title": "The Dark Side of the Sun"
    }
  ],
  "crew": [
    {
      "adult": false,
      "credit_id": "52fe4264c3a36847f801b07f",
      "department": "Production",
      "id": 652,
      "job": "Other",
      "original_title": "Troy",
      "poster_path": "/edMlij7nw2NMla32xskDnzMCFBM.jpg",
      "release_date": "2004-05-13",
      "title": "Troy"
    },
    {
      "adult": false,
      "credit_id": "52fe42f5c3a36847f802ff41",
      "department": "Production",
      "id": 1422,
      "job": "Producer",
      "original_title": "The Departed",
      "poster_path": "/tGLO9zw5ZtCeyyEWgbYGgsFxC6i.jpg",
      "release_date": "2006-10-05",
      "title": "The Departed"
    },
    {
      "adult": false,
      "credit_id": "52fe4329c3a36847f803ee3b",
      "department": "Production",
      "id": 1988,
      "job": "Producer",
      "original_title": "A Mighty Heart",
      "poster_path": "/8khwSri6av5MtP1BmyeN1gEHVJN.jpg",
      "release_date": "2007-05-21",
      "title": "A Mighty Heart"
    },
    {
      "adult": false,
      "credit_id": "52fe43c7c3a36847f806ef0b",
      "department": "Production",
      "id": 4512,
      "job": "Producer",
      "original_title": "The Assassination of Jesse James by the Coward Robert Ford",
      "poster_path": "/lSFYLoaL4eW7Q5VQ7SZQP4EHRCt.jpg",
      "release_date": "2007-09-20",
      "title": "The Assassination of Jesse James by the Coward Robert Ford"
    },
    {
      "adult": false,
      "credit_id": "52fe446ac3a368484e021e13",
      "department": "Production",
      "id": 23483,
      "job": "Producer",
      "original_title": "Kick-Ass",
      "poster_path": "/yYy7bJ0HuSudtHDksGODZohRQWo.jpg",
      "release_date": "2010-04-16",
      "title": "Kick-Ass"
    },
    {
      "adult": false,
      "credit_id": "52fe4481c3a36847f809a065",
      "department": "Production",
      "id": 7510,
      "job": "Producer",
      "original_title": "Running with Scissors",
      "poster_path": "/pYFF3iMWDPcwXKpRM0GLIsnPf22.jpg",
      "release_date": "2006-10-27",
      "title": "Running with Scissors"
    },
    {
      "adult": false,
      "credit_id": "52fe4b3fc3a36847f81f9f89",
      "department": "Production",
      "id": 113833,
      "job": "Producer",
      "original_title": "The Normal Heart",
      "poster_path": null,
      "release_date": "2014-04-01",
      "title": "The Normal Heart"
    },
    {
      "adult": false,
      "credit_id": "52fe4d49c3a36847f8258cf3",
      "department": "Production",
      "id": 174349,
      "job": "Executive Producer",
      "original_title": "Big Men",
      "poster_path": null,
      "release_date": null,
      "title": "Big Men"
    },
    {
      "adult": false,
      "credit_id": "52fe4e48c3a368484e2183d1",
      "department": "Production",
      "id": 218277,
      "job": "Executive Producer",
      "original_title": "Pretty/Handsome",
      "poster_path": "/hiASAaSle8sjUZ9BHs4XrA30shS.jpg",
      "release_date": "2008-06-01",
      "title": "Pretty/Handsome"
    },
    {
      "adult": false,
      "credit_id": "52fe4f0ac3a36847f82b9247",
      "department": "Production",
      "id": 245706,
      "job": "Producer",
      "original_title": "True Story",
      "poster_path": null,
      "release_date": null,
      "title": "True Story"
    },
    {
      "adult": false,
      "credit_id": "52fe469c9251416c91059ecf",
      "department": "Production",
      "id": 38167,
      "job": "Executive Producer",
      "original_title": "Eat Pray Love",
      "poster_path": "/s57E4AfPIU1fxwpGGRahk6A0DUl.jpg",
      "release_date": "2010-08-12",
      "title": "Eat Pray Love"
    },
    {
      "adult": false,
      "credit_id": "52fe46e4c3a368484e0a9aa7",
      "department": "Production",
      "id": 64689,
      "job": "Producer",
      "original_title": "Killing Them Softly",
      "poster_path": "/pIS0JWCYJYesGNAd6gWbtSwzgsF.jpg",
      "release_date": "2012-11-30",
      "title": "Killing Them Softly"
    },
    {
      "adult": false,
      "credit_id": "52fe485dc3a368484e0f50f9",
      "department": "Production",
      "id": 72190,
      "job": "Producer",
      "original_title": "World War Z",
      "poster_path": "/gAt1PrsrFY1nX6UzebeiHP8njE9.jpg",
      "release_date": "2013-06-21",
      "title": "World War Z"
    },
    {
      "adult": false,
      "credit_id": "52fe492cc3a368484e11dfe1",
      "department": "Production",
      "id": 76203,
      "job": "Producer",
      "original_title": "12 Years a Slave",
      "poster_path": "/onpio7AWndkzH6mptb35cCuV9q1.jpg",
      "release_date": "2013-10-18",
      "title": "12 Years a Slave"
    },
    {
      "adult": false,
      "credit_id": "52fe4495c3a368484e02af99",
      "department": "Production",
      "id": 24420,
      "job": "Executive Producer",
      "original_title": "The Time Traveler's Wife",
      "poster_path": "/gHupkXWAZocNHixV0dXh3h95OAq.jpg",
      "release_date": "2009-08-14",
      "title": "The Time Traveler's Wife"
    }
  ],
  "id": 287
}

Get the TV credits for a specific person id.

To get the expanded details for each record, call the [/credit](#credits) method with the provided `credit_id`. This will provide details about which episode and/or season the credit is for.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
<tr><td style="padding-left: 0;" colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-left: 0; width: 140px;">language</td><td>ISO 639-1 code.</td></tr>
<tr><td style="padding-left: 0; width: 140px;">append_to_response</td><td>Comma separated, any person method</td></tr>
</table>

GET /person/{id}/tv_credits
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "ed59e17b5c7940de30c6f597ebbf0200"
{
  "cast": [
    {
      "character": "",
      "credit_id": "525333fb19c295794002c720",
      "episode_count": 2,
      "first_air_date": "1985-09-24",
      "id": 54,
      "name": "Growing Pains",
      "original_name": "Growing Pains",
      "poster_path": "/eKyeUFwjc0LhPSp129IHpXniJVR.jpg"
    },
    {
      "character": "",
      "credit_id": "5257157c760ee3776a132ba8",
      "episode_count": 2,
      "first_air_date": "2000-04-12",
      "id": 1795,
      "name": "Jackass",
      "original_name": "Jackass",
      "poster_path": "/mz9PZo93dnIYHp1udcYsnBSLYTS.jpg"
    },
    {
      "character": "",
      "credit_id": "52571af119c29571140d5eda",
      "episode_count": 1,
      "first_air_date": "1983-04-04",
      "id": 1900,
      "name": "Live! with Kelly and Michael",
      "original_name": "Live! with Kelly and Michael",
      "poster_path": "/nuwhsprEgH31SROiJtIk0mxF82M.jpg"
    },
    {
      "character": "",
      "credit_id": "52572302760ee3776a22dc59",
      "episode_count": 1,
      "first_air_date": "1997-01-12",
      "id": 2122,
      "name": "King of the Hill",
      "original_name": "King of the Hill",
      "poster_path": "/r6dIrTjWLE3GfcTZzrDYikt6YDn.jpg"
    },
    {
      "character": "",
      "credit_id": "525763be760ee36aaa33f8e7",
      "episode_count": 1,
      "first_air_date": null,
      "id": 4325,
      "name": "Pet Star",
      "original_name": "Pet Star",
      "poster_path": null
    },
    {
      "character": "",
      "credit_id": "525764f9760ee36aaa357d18",
      "episode_count": 1,
      "first_air_date": "1988-10-08",
      "id": 4346,
      "name": "Freddy's Nightmares",
      "original_name": "Freddy's Nightmares",
      "poster_path": "/sMYfjEjK6rEF6FeiGum6g67Wor6.jpg"
    },
    {
      "character": "",
      "credit_id": "5257713f760ee36aaa496071",
      "episode_count": 1,
      "first_air_date": "1993-09-13",
      "id": 4573,
      "name": "Late Night with Conan O'Brien",
      "original_name": "Late Night with Conan O'Brien",
      "poster_path": "/v4N5l6JooObhXbjXoFaKbx92wB9.jpg"
    },
    {
      "character": "Albigence Waldo",
      "credit_id": "525782e7760ee36aaa605ae7",
      "episode_count": 0,
      "first_air_date": null,
      "id": 5741,
      "name": "Freedom: A History of Us",
      "original_name": "Freedom: A History of Us",
      "poster_path": null
    },
    {
      "character": "James K. Polk",
      "credit_id": "525782e7760ee36aaa605b0f",
      "episode_count": 0,
      "first_air_date": null,
      "id": 5741,
      "name": "Freedom: A History of Us",
      "original_name": "Freedom: A History of Us",
      "poster_path": null
    },
    {
      "character": "William Lloyd Garrison",
      "credit_id": "525782e7760ee36aaa605b19",
      "episode_count": 0,
      "first_air_date": null,
      "id": 5741,
      "name": "Freedom: A History of Us",
      "original_name": "Freedom: A History of Us",
      "poster_path": null
    },
    {
      "character": "George Hewes",
      "credit_id": "525782e7760ee36aaa605b23",
      "episode_count": 0,
      "first_air_date": null,
      "id": 5741,
      "name": "Freedom: A History of Us",
      "original_name": "Freedom: A History of Us",
      "poster_path": null
    },
    {
      "character": "John Russell Young",
      "credit_id": "525782e7760ee36aaa605b2d",
      "episode_count": 0,
      "first_air_date": null,
      "id": 5741,
      "name": "Freedom: A History of Us",
      "original_name": "Freedom: A History of Us",
      "poster_path": null
    },
    {
      "character": "",
      "credit_id": "5257fa0319c29531db2ed3bb",
      "episode_count": 1,
      "first_air_date": "1994-01-03",
      "id": 9937,
      "name": "Intimate Portrait",
      "original_name": "Intimate Portrait",
      "poster_path": null
    },
    {
      "character": "",
      "credit_id": "5258833a760ee346614043a6",
      "episode_count": 3,
      "first_air_date": "1929-05-16",
      "id": 27023,
      "name": "The Academy Awards",
      "original_name": "The Academy Awards",
      "poster_path": "/xxWK636MvogUp4Y5pX3ri69yMcy.jpg"
    },
    {
      "character": "Walker Lovejoy",
      "credit_id": "525895bf760ee3466158aa8e",
      "episode_count": 6,
      "first_air_date": "1990-07-25",
      "id": 29999,
      "name": "Glory Days",
      "original_name": "Glory Days",
      "poster_path": null
    },
    {
      "character": "",
      "credit_id": "525704eb760ee3776a008abe",
      "episode_count": 1,
      "first_air_date": "1987-09-29",
      "id": 1448,
      "name": "Thirtysomething",
      "original_name": "Thirtysomething",
      "poster_path": "/tlEWRrWe2SCd0cYfk3X5LEoiOpm.jpg"
    },
    {
      "character": "",
      "credit_id": "52570765760ee3776a03124d",
      "episode_count": 1,
      "first_air_date": "1987-04-12",
      "id": 1486,
      "name": "21 Jump Street",
      "original_name": "21 Jump Street",
      "poster_path": "/5zobhoPvdfqQOmvRRukhoJDiJ3M.jpg"
    },
    {
      "character": "Will Colbert",
      "credit_id": "5257107819c295731c02cf9b",
      "episode_count": 1,
      "first_air_date": "1994-09-22",
      "id": 1668,
      "name": "Friends",
      "original_name": "Friends",
      "poster_path": "/oQR7HXiCWpWvmUN1wo3asxrkAeZ.jpg"
    },
    {
      "character": "",
      "credit_id": "525713d5760ee3776a113c77",
      "episode_count": 1,
      "first_air_date": "1952-01-14",
      "id": 1709,
      "name": "Today",
      "original_name": "Today",
      "poster_path": "/o7bSMUPaIxYbbsgC6VTbrvOVDpN.jpg"
    },
    {
      "character": "",
      "credit_id": "52572b4e760ee3776a2d9c26",
      "episode_count": 1,
      "first_air_date": "1996-07-22",
      "id": 2224,
      "name": "The Daily Show",
      "original_name": "The Daily Show",
      "poster_path": "/dorWIToZ035gNAYORey8IzpFR1J.jpg"
    },
    {
      "character": "",
      "credit_id": "525734f6760ee3776a3977e7",
      "episode_count": 1,
      "first_air_date": "1989-06-10",
      "id": 2391,
      "name": "Tales from the Crypt",
      "original_name": "Tales from the Crypt",
      "poster_path": "/eN5mW5SVCtsu7AycQtZSTnOoLot.jpg"
    },
    {
      "character": "",
      "credit_id": "52573d96760ee36aaa047b7e",
      "episode_count": 1,
      "first_air_date": "1986-09-17",
      "id": 2589,
      "name": "Head of the Class",
      "original_name": "Head of the Class",
      "poster_path": "/Dyy3E7SqkeuYpiQET2rIwsGPxZ.jpg"
    },
    {
      "character": "",
      "credit_id": "525753c6760ee36aaa1f53c7",
      "episode_count": 1,
      "first_air_date": "2002-06-11",
      "id": 3626,
      "name": "American Idol",
      "original_name": "American Idol",
      "poster_path": "/2aqueDlNwOWrszV0hKsxVk5yOgK.jpg"
    },
    {
      "character": "",
      "credit_id": "5258094919c29531db3e2f4a",
      "episode_count": 6,
      "first_air_date": "2003-06-04",
      "id": 10946,
      "name": "Celebrities Uncensored",
      "original_name": "Celebrities Uncensored",
      "poster_path": null
    }
  ],
  "crew": [
    {
      "credit_id": "531cc64d92514177b7005d3a",
      "department": "Creator",
      "first_air_date": "2014-03-09",
      "id": 51370,
      "job": "Creator",
      "name": "Resurrection",
      "original_name": "Resurrection",
      "poster_path": "/oNiK8YaOGR995kLlNPhsM3mx506.jpg"
    }
  ],
  "id": 287
}

Get the combined (movie and TV) credits for a specific person id.

To get the expanded details for each TV record, call the [/credit](#credits) method with the provided `credit_id`. This will provide details about which episode and/or season the credit is for.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
<tr><td style="padding-left: 0;" colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-left: 0; width: 140px;">language</td><td>ISO 639-1 code.</td></tr>
<tr><td style="padding-left: 0; width: 140px;">append_to_response</td><td>Comma separated, any person method</td></tr>
</table>

GET /person/{id}/combined_credits
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "ed59e17b5c7940de30c6f597ebbf0200"
{
  "cast": [
    {
      "adult": false,
      "character": "Tristan Ludlow",
      "credit_id": "52fe43c4c3a36847f806e20d",
      "id": 4476,
      "original_title": "Legends of the Fall",
      "poster_path": "/uh0sJcx3SLtclJSuKAXl6Tt6AV0.jpg",
      "release_date": "1994-12-16",
      "title": "Legends of the Fall",
      "media_type": "movie"
    },
    {
      "adult": false,
      "character": "Frankie McGuire",
      "credit_id": "52fe43c4c3a36847f806e2b9",
      "id": 4477,
      "original_title": "The Devil's Own",
      "poster_path": "/1zusKhAtJ3YhbuA6J184Vm4INB3.jpg",
      "release_date": "1997-03-13",
      "title": "The Devil's Own",
      "media_type": "movie"
    },
    {
      "adult": false,
      "character": "Jesse James",
      "credit_id": "52fe43c7c3a36847f806eed5",
      "id": 4512,
      "original_title": "The Assassination of Jesse James by the Coward Robert Ford",
      "poster_path": "/lSFYLoaL4eW7Q5VQ7SZQP4EHRCt.jpg",
      "release_date": "2007-09-20",
      "title": "The Assassination of Jesse James by the Coward Robert Ford",
      "media_type": "movie"
    },
    {
      "adult": false,
      "character": "Early Grayce",
      "credit_id": "52fe43ce9251416c7501ef13",
      "id": 10909,
      "original_title": "Kalifornia",
      "poster_path": "/4gkWPWSumyMwRLwys2KYFEbXhf7.jpg",
      "release_date": "1993-09-03",
      "title": "Kalifornia",
      "media_type": "movie"
    },
    {
      "adult": false,
      "character": "Billy Canton",
      "credit_id": "52fe43d09251416c7501f331",
      "id": 10917,
      "original_title": "Too Young to Die?",
      "poster_path": "/4L0BapEctIHvneJurG6pUqwF2y2.jpg",
      "release_date": "1990-02-26",
      "title": "Too Young to Die?",
      "media_type": "movie"
    },
    {
      "adult": false,
      "character": "Brad, 1st Bachelor",
      "credit_id": "52fe43e2c3a36847f807610f",
      "id": 4912,
      "original_title": "Confessions of a Dangerous Mind",
      "poster_path": "/wD0R18CfNqOW5ViHdFwxIwDlTA7.jpg",
      "release_date": "2002-12-30",
      "title": "Confessions of a Dangerous Mind",
      "media_type": "movie"
    },
    {
      "adult": false,
      "character": "Benjamin Button",
      "credit_id": "52fe43e2c3a36847f807632f",
      "id": 4922,
      "original_title": "The Curious Case of Benjamin Button",
      "poster_path": "/4O4INOPtWTfHq3dd5vYTPV0TCwa.jpg",
      "release_date": "2008-11-24",
      "title": "The Curious Case of Benjamin Button",
      "media_type": "movie"
    },
    {
      "adult": false,
      "character": "Chad Feldheimer",
      "credit_id": "52fe43e6c3a36847f8076fe7",
      "id": 4944,
      "original_title": "Burn After Reading",
      "poster_path": "/kYXnwKcsMJlEBXwQbde1D20aNGe.jpg",
      "release_date": "2008-09-12",
      "title": "Burn After Reading",
      "media_type": "movie"
    },
    {
      "adult": false,
      "character": "Himself",
      "credit_id": "52fe44259251416c91006683",
      "id": 30565,
      "original_title": "The Hamster Factor and Other Tales of Twelve Monkeys",
      "poster_path": "/sfPxX29hVdBq8cP5839Dx91cCW9.jpg",
      "release_date": "1997-01-01",
      "title": "The Hamster Factor and Other Tales of Twelve Monkeys",
      "media_type": "movie"
    },
    {
      "adult": false,
      "character": "Dwight Ingalls",
      "credit_id": "52fe4428c3a368484e012c37",
      "id": 21799,
      "original_title": "Cutting Class",
      "poster_path": "/y8eBkmikGYZmhmZfNjgzWD5Lo1d.jpg",
      "release_date": "1989-07-01",
      "title": "Cutting Class",
      "media_type": "movie"
    },
    {
      "adult": false,
      "character": "Jerry Welbach",
      "credit_id": "52fe443bc3a36847f8089dd5",
      "id": 6073,
      "original_title": "The Mexican",
      "poster_path": "/a7PuqWv0ENFg8dt9k51AID6P1kh.jpg",
      "release_date": "2001-03-01",
      "title": "The Mexican",
      "media_type": "movie"
    },
    {
      "adult": false,
      "character": "John Dean",
      "credit_id": "52fe44cac3a36847f80aa277",
      "id": 8947,
      "original_title": "Dirty Tricks",
      "poster_path": null,
      "release_date": "2013-12-31",
      "title": "Dirty Tricks",
      "media_type": "movie"
    },
    {
      "adult": false,
      "character": "Mr. O'Brien",
      "credit_id": "52fe44ccc3a36847f80aa95b",
      "id": 8967,
      "original_title": "The Tree of Life",
      "poster_path": "/wt5Q6SQFeU2mW7UKF10mTwyyHog.jpg",
      "release_date": "2011-05-27",
      "title": "The Tree of Life",
      "media_type": "movie"
    },
    {
      "adult": false,
      "character": "Rick",
      "credit_id": "52fe4510c3a368484e0467cb",
      "id": 26642,
      "original_title": "The Dark Side of the Sun",
      "poster_path": "/pofw83GCrfG2B6JxH53n8vX1pDv.jpg",
      "release_date": "1987-12-31",
      "title": "The Dark Side of the Sun",
      "media_type": "movie"
    },
    {
      "adult": false,
      "character": "Detective Frank Harris",
      "credit_id": "52fe45dd9251416c75065151",
      "id": 14239,
      "original_title": "Cool World",
      "poster_path": "/eSR3vFpgGQfYQYI2fMbwIZp70lp.jpg",
      "release_date": "1992-07-10",
      "title": "Cool World",
      "media_type": "movie"
    },
    {
      "adult": false,
      "character": "Sinbad",
      "credit_id": "52fe45f09251416c75067ad5",
      "id": 14411,
      "original_title": "Sinbad: Legend of the Seven Seas",
      "poster_path": "/6LELf4ZzVBJwR9mNq86Mf5QVERS.jpg",
      "release_date": "2003-07-02",
      "title": "Sinbad: Legend of the Seven Seas",
      "media_type": "movie"
    },
    {
      "adult": false,
      "character": "Billy Beane",
      "credit_id": "52fe461fc3a368484e0800bd",
      "id": 60308,
      "original_title": "Moneyball",
      "poster_path": "/3oAa8mJJ97CH9AeGEY6vjAxqcvZ.jpg",
      "release_date": "2011-09-22",
      "title": "Moneyball",
      "media_type": "movie"
    },
    {
      "adult": false,
      "character": "Metro Man (voice)",
      "credit_id": "52fe468e9251416c910583cd",
      "id": 38055,
      "original_title": "Megamind",
      "poster_path": "/mws4hcv746iKsTv81sY393LY4YI.jpg",
      "release_date": "2010-11-04",
      "title": "Megamind",
      "media_type": "movie"
    },
    {
      "adult": false,
      "character": "Himself",
      "credit_id": "52fe46acc3a368484e09d959",
      "id": 63472,
      "original_title": "His Way",
      "poster_path": "/wTQHzluHXnSzyh0hbsh2n2ky6kj.jpg",
      "release_date": "2011-03-04",
      "title": "His Way",
      "media_type": "movie"
    },
    {
      "adult": false,
      "character": "Johnny Suede",
      "credit_id": "52fe46b2c3a36847f810d153",
      "id": 45145,
      "original_title": "Johnny Suede",
      "poster_path": "/3z4OZo5j1hkQ3AI8xTzuyqUD9Ru.jpg",
      "release_date": "1991-06-15",
      "title": "Johnny Suede",
      "media_type": "movie"
    },
    {
      "adult": false,
      "character": "Jackie Cogan",
      "credit_id": "52fe46e4c3a368484e0a9a51",
      "id": 64689,
      "original_title": "Killing Them Softly",
      "poster_path": "/s212M6rVyA0gcMB9IAqSdUGx5JT.jpg",
      "release_date": "2012-11-30",
      "title": "Killing Them Softly",
      "media_type": "movie"
    },
    {
      "adult": false,
      "character": "Lt. Aldo Raine",
      "credit_id": "52fe46f29251416c75088c69",
      "id": 16869,
      "original_title": "Inglourious Basterds",
      "poster_path": "/vDwqPyhkzFPRDmwz9KbzN2ouEPe.jpg",
      "release_date": "2009-08-21",
      "title": "Inglourious Basterds",
      "media_type": "movie"
    },
    {
      "adult": false,
      "character": "Will the Krill (voice)",
      "credit_id": "52fe4718c3a368484e0b4d23",
      "id": 65759,
      "original_title": "Happy Feet Two",
      "poster_path": "/gY8lWCObaGvcDsmeM8QHBF4AZVk.jpg",
      "release_date": "2011-11-17",
      "title": "Happy Feet Two",
      "media_type": "movie"
    },
    {
      "adult": false,
      "character": "Joe Maloney",
      "credit_id": "52fe4766c3a36847f81338e5",
      "id": 48448,
      "original_title": "Across the Tracks",
      "poster_path": "/o2Xvw7Wfzk1Q10yDNOgVx63CsIv.jpg",
      "release_date": "1991-02-14",
      "title": "Across the Tracks",
      "media_type": "movie"
    },
    {
      "adult": false,
      "character": "Mickey O'Neil (Snatch) (archive footage)",
      "credit_id": "52fe47b4c3a368484e0d520b",
      "id": 68996,
      "original_title": "Ultimate Fights from the Movies",
      "poster_path": "/2KIKXjKoNTmpi22gsU3KUMv6wKA.jpg",
      "release_date": "2002-04-16",
      "title": "Ultimate Fights from the Movies",
      "media_type": "movie"
    },
    {
      "adult": false,
      "character": "Elliott Fowler",
      "credit_id": "52fe47c8c3a36847f8147ff5",
      "id": 50463,
      "original_title": "The Favor",
      "poster_path": "/68a68qALehxulfbL2P56OKAJ6Ci.jpg",
      "release_date": "1994-04-29",
      "title": "The Favor",
      "media_type": "movie"
    },
    {
      "adult": false,
      "character": "Gerry Lane",
      "credit_id": "52fe485dc3a368484e0f5061",
      "id": 72190,
      "original_title": "World War Z",
      "poster_path": "/gAt1PrsrFY1nX6UzebeiHP8njE9.jpg",
      "release_date": "2013-06-21",
      "title": "World War Z",
      "media_type": "movie"
    },
    {
      "adult": false,
      "character": "Brian",
      "credit_id": "52fe48b9c3a36847f81761cb",
      "id": 55059,
      "original_title": "Happy Together",
      "poster_path": "/5H766FOKWY0GsbdMdQktqQYv3ku.jpg",
      "release_date": "1989-05-04",
      "title": "Happy Together",
      "media_type": "movie"
    },
    {
      "adult": false,
      "character": "Steve Black",
      "credit_id": "52fe4900c3a368484e115b63",
      "id": 75451,
      "original_title": "The Image",
      "poster_path": "/xfiHVr42KCIu2FvKd5uD2iPbeie.jpg",
      "release_date": "1990-01-27",
      "title": "The Image",
      "media_type": "movie"
    },
    {
      "adult": false,
      "character": "Samuel Bass",
      "credit_id": "52fe492cc3a368484e11dfa3",
      "id": 76203,
      "original_title": "12 Years a Slave",
      "poster_path": "/kb3X943WMIJYVg4SOAyK0pmWL5D.jpg",
      "release_date": "2013-10-30",
      "title": "12 Years a Slave",
      "media_type": "movie"
    },
    {
      "adult": false,
      "character": "Narrator (voice)",
      "credit_id": "52fe49a29251416c910b39e7",
      "id": 86822,
      "original_title": "Voyage of Time",
      "poster_path": null,
      "release_date": null,
      "title": "Voyage of Time",
      "media_type": "movie"
    },
    {
      "adult": false,
      "character": "Westray",
      "credit_id": "52fe4aaac3a36847f81db47d",
      "id": 109091,
      "original_title": "The Counselor",
      "poster_path": "/uxp6rHVBzUqZCyTaUI8xzUP5sOf.jpg",
      "release_date": "2013-10-25",
      "title": "The Counselor",
      "media_type": "movie"
    },
    {
      "adult": false,
      "character": "Chief Judge Vaughn R. Walker",
      "credit_id": "52fe4ab2c3a36847f81dcd13",
      "id": 109404,
      "original_title": "8",
      "poster_path": "/28fDtVBr6PyHsFFqyKJCeN3ysBP.jpg",
      "release_date": "2012-03-03",
      "title": "8",
      "media_type": "movie"
    },
    {
      "adult": false,
      "character": "Yuri Trush",
      "credit_id": "52fe4b569251416c750ffb19",
      "id": 145266,
      "original_title": "The Tiger",
      "poster_path": null,
      "release_date": null,
      "title": "The Tiger",
      "media_type": "movie"
    },
    {
      "adult": false,
      "character": "Mickey O'Neil",
      "credit_id": "52fe4218c3a36847f8003be5",
      "id": 107,
      "original_title": "Snatch",
      "poster_path": "/on9JlbGEccLsYkjeEph2Whm1DIp.jpg",
      "release_date": "2000-12-06",
      "title": "Snatch",
      "media_type": "movie"
    },
    {
      "adult": false,
      "character": "Rusty Ryan",
      "credit_id": "52fe4220c3a36847f800616b",
      "id": 161,
      "original_title": "Ocean's Eleven",
      "poster_path": "/5gkZ8PL3JZXXgq398xWzaxVB1Sc.jpg",
      "release_date": "2001-12-06",
      "title": "Ocean's Eleven",
      "media_type": "movie"
    },
    {
      "adult": false,
      "character": "Rusty Ryan",
      "credit_id": "52fe4221c3a36847f80062e5",
      "id": 163,
      "original_title": "Ocean's Twelve",
      "poster_path": "/4irBEiDC7SpYBuw5z1c7oLfF0EI.jpg",
      "release_date": "2004-12-09",
      "title": "Ocean's Twelve",
      "media_type": "movie"
    },
    {
      "adult": false,
      "character": "Paul Maclean",
      "credit_id": "52fe4233c3a36847f800bb79",
      "id": 293,
      "original_title": "A River Runs Through It",
      "poster_path": "/xX4H1hZG9IgSRkC0LANbPQ0StJi.jpg",
      "release_date": "1992-10-09",
      "title": "A River Runs Through It",
      "media_type": "movie"
    },
    {
      "adult": false,
      "character": "Joe Black",
      "credit_id": "52fe4234c3a36847f800bdbb",
      "id": 297,
      "original_title": "Meet Joe Black",
      "poster_path": "/nlxPnkZY3vY1iehJriKMQcT6eua.jpg",
      "release_date": "1998-11-12",
      "title": "Meet Joe Black",
      "media_type": "movie"
    },
    {
      "adult": false,
      "character": "Robert “Rusty” Charles Ryan",
      "credit_id": "52fe4234c3a36847f800bf0f",
      "id": 298,
      "original_title": "Ocean's Thirteen",
      "poster_path": "/k7jNRNN9UuUc2VQg21YXYn3JGVY.jpg",
      "release_date": "2007-06-07",
      "title": "Ocean's Thirteen",
      "media_type": "movie"
    },
    {
      "adult": false,
      "character": "Floyd",
      "credit_id": "52fe4237c3a36847f800cdd3",
      "id": 319,
      "original_title": "True Romance",
      "poster_path": "/xBO8R3CZfrJ9rrwrZoJ68PgJyAR.jpg",
      "release_date": "1993-09-09",
      "title": "True Romance",
      "media_type": "movie"
    },
    {
      "adult": false,
      "character": "Himself",
      "credit_id": "52fe4249c3a36847f801293b",
      "id": 492,
      "original_title": "Being John Malkovich",
      "poster_path": "/gLhl4MBEC6yHTInwV7TxV1D3FLp.jpg",
      "release_date": "1999-09-30",
      "title": "Being John Malkovich",
      "media_type": "movie"
    },
    {
      "adult": false,
      "character": "Tyler Durden",
      "credit_id": "52fe4250c3a36847f80149f7",
      "id": 550,
      "original_title": "Fight Club",
      "poster_path": "/2lECpi35Hnbpa4y46JX0aY3AWTy.jpg",
      "release_date": "1999-10-14",
      "title": "Fight Club",
      "media_type": "movie"
    },
    {
      "adult": false,
      "character": "Louis de Pointe du Lac",
      "credit_id": "52fe4260c3a36847f80199f9",
      "id": 628,
      "original_title": "Interview with the Vampire",
      "poster_path": "/hldXwwViSfHJS0kIJr07KBGmHJI.jpg",
      "release_date": "1994-11-11",
      "title": "Interview with the Vampire",
      "media_type": "movie"
    },
    {
      "adult": false,
      "character": "Achilles",
      "credit_id": "52fe4264c3a36847f801b083",
      "id": 652,
      "original_title": "Troy",
      "poster_path": "/edMlij7nw2NMla32xskDnzMCFBM.jpg",
      "release_date": "2004-05-13",
      "title": "Troy",
      "media_type": "movie"
    },
    {
      "adult": false,
      "character": "John Smith",
      "credit_id": "52fe4276c3a36847f80208cb",
      "id": 787,
      "original_title": "Mr. & Mrs. Smith",
      "poster_path": "/dqs5BmwSULtB28Kls3IB6khTQwp.jpg",
      "release_date": "2005-06-09",
      "title": "Mr. & Mrs. Smith",
      "media_type": "movie"
    },
    {
      "adult": false,
      "character": "Detective David Mills",
      "credit_id": "52fe4279c3a36847f802178b",
      "id": 807,
      "original_title": "Se7en",
      "poster_path": "/zgB9CCTDlXRv50Z70ZI4elJtNEk.jpg",
      "release_date": "1995-09-22",
      "title": "Se7en",
      "media_type": "movie"
    },
    {
      "adult": false,
      "character": "Michael Sullivan",
      "credit_id": "52fe427bc3a36847f80222a7",
      "id": 819,
      "original_title": "Sleepers",
      "poster_path": "/80LV2GvhvMEuOutxgQwbRYpo0Vv.jpg",
      "release_date": "1996-10-18",
      "title": "Sleepers",
      "media_type": "movie"
    },
    {
      "adult": false,
      "character": "Heinrich Harrer",
      "credit_id": "52fe4295c3a36847f802a10d",
      "id": 978,
      "original_title": "Seven Years in Tibet",
      "poster_path": "/cflSeFUVDCf73Tzh5sB204JbQ6j.jpg",
      "release_date": "1997-10-08",
      "title": "Seven Years in Tibet",
      "media_type": "movie"
    },
    {
      "adult": false,
      "character": "Richard",
      "credit_id": "52fe42e9c3a36847f802c221",
      "id": 1164,
      "original_title": "Babel",
      "poster_path": "/fxneN0EQZwTfAfhTGUvUuIn6PLi.jpg",
      "release_date": "2006-10-27",
      "title": "Babel",
      "media_type": "movie"
    },
    {
      "adult": false,
      "character": "Tom Bishop",
      "credit_id": "52fe42fbc3a36847f8031727",
      "id": 1535,
      "original_title": "Spy Game",
      "poster_path": "/hsb8hBeU3tkTX8SUYW6YYw6JPYD.jpg",
      "release_date": "2001-11-18",
      "title": "Spy Game",
      "media_type": "movie"
    },
    {
      "adult": false,
      "character": "J.D.",
      "credit_id": "52fe42fbc3a36847f8031a6d",
      "id": 1541,
      "original_title": "Thelma & Louise",
      "poster_path": "/29Do6f2m5h8tat9qt66OOMl2bsg.jpg",
      "release_date": "1991-05-24",
      "title": "Thelma & Louise",
      "media_type": "movie"
    },
    {
      "adult": false,
      "character": "Jeffrey Goines",
      "credit_id": "52fe4212c3a36847f8001b39",
      "id": 63,
      "original_title": "Twelve Monkeys",
      "poster_path": "/6Sj9wDu3YugthXsU0Vry5XFAZGg.jpg",
      "release_date": "1995-12-27",
      "title": "Twelve Monkeys",
      "media_type": "movie"
    },
    {
      "adult": false,
      "character": "Wardaddy",
      "credit_id": "52fe4ec09251416c7516126f",
      "id": 228150,
      "original_title": "Fury",
      "poster_path": "/fIHF63oznk2PXlYM6pPfhnOB3SD.jpg",
      "release_date": "2014-11-14",
      "title": "Fury",
      "media_type": "movie"
    },
    {
      "adult": false,
      "character": "",
      "credit_id": "52fe4ef6c3a36847f82b3c95",
      "id": 244743,
      "original_title": "Contact",
      "poster_path": "/gAmyqdAlwzB8Et34ESMrl7tosn4.jpg",
      "release_date": "1992-01-01",
      "title": "Contact",
      "media_type": "movie"
    },
    {
      "character": "",
      "credit_id": "525333fb19c295794002c720",
      "episode_count": 2,
      "first_air_date": "1985-09-24",
      "id": 54,
      "name": "Growing Pains",
      "original_name": "Growing Pains",
      "poster_path": "/eKyeUFwjc0LhPSp129IHpXniJVR.jpg",
      "media_type": "tv"
    },
    {
      "character": "",
      "credit_id": "5257157c760ee3776a132ba8",
      "episode_count": 2,
      "first_air_date": "2000-04-12",
      "id": 1795,
      "name": "Jackass",
      "original_name": "Jackass",
      "poster_path": "/mz9PZo93dnIYHp1udcYsnBSLYTS.jpg",
      "media_type": "tv"
    },
    {
      "character": "",
      "credit_id": "52571af119c29571140d5eda",
      "episode_count": 1,
      "first_air_date": "1983-04-04",
      "id": 1900,
      "name": "Live! with Kelly and Michael",
      "original_name": "Live! with Kelly and Michael",
      "poster_path": "/nuwhsprEgH31SROiJtIk0mxF82M.jpg",
      "media_type": "tv"
    },
    {
      "character": "",
      "credit_id": "52572302760ee3776a22dc59",
      "episode_count": 1,
      "first_air_date": "1997-01-12",
      "id": 2122,
      "name": "King of the Hill",
      "original_name": "King of the Hill",
      "poster_path": "/r6dIrTjWLE3GfcTZzrDYikt6YDn.jpg",
      "media_type": "tv"
    },
    {
      "character": "",
      "credit_id": "525763be760ee36aaa33f8e7",
      "episode_count": 1,
      "first_air_date": null,
      "id": 4325,
      "name": "Pet Star",
      "original_name": "Pet Star",
      "poster_path": null,
      "media_type": "tv"
    },
    {
      "character": "",
      "credit_id": "525764f9760ee36aaa357d18",
      "episode_count": 1,
      "first_air_date": "1988-10-08",
      "id": 4346,
      "name": "Freddy's Nightmares",
      "original_name": "Freddy's Nightmares",
      "poster_path": "/sMYfjEjK6rEF6FeiGum6g67Wor6.jpg",
      "media_type": "tv"
    },
    {
      "character": "",
      "credit_id": "5257713f760ee36aaa496071",
      "episode_count": 1,
      "first_air_date": "1993-09-13",
      "id": 4573,
      "name": "Late Night with Conan O'Brien",
      "original_name": "Late Night with Conan O'Brien",
      "poster_path": "/v4N5l6JooObhXbjXoFaKbx92wB9.jpg",
      "media_type": "tv"
    },
    {
      "character": "Albigence Waldo",
      "credit_id": "525782e7760ee36aaa605ae7",
      "episode_count": 0,
      "first_air_date": null,
      "id": 5741,
      "name": "Freedom: A History of Us",
      "original_name": "Freedom: A History of Us",
      "poster_path": null,
      "media_type": "tv"
    },
    {
      "character": "James K. Polk",
      "credit_id": "525782e7760ee36aaa605b0f",
      "episode_count": 0,
      "first_air_date": null,
      "id": 5741,
      "name": "Freedom: A History of Us",
      "original_name": "Freedom: A History of Us",
      "poster_path": null,
      "media_type": "tv"
    },
    {
      "character": "William Lloyd Garrison",
      "credit_id": "525782e7760ee36aaa605b19",
      "episode_count": 0,
      "first_air_date": null,
      "id": 5741,
      "name": "Freedom: A History of Us",
      "original_name": "Freedom: A History of Us",
      "poster_path": null,
      "media_type": "tv"
    },
    {
      "character": "George Hewes",
      "credit_id": "525782e7760ee36aaa605b23",
      "episode_count": 0,
      "first_air_date": null,
      "id": 5741,
      "name": "Freedom: A History of Us",
      "original_name": "Freedom: A History of Us",
      "poster_path": null,
      "media_type": "tv"
    },
    {
      "character": "John Russell Young",
      "credit_id": "525782e7760ee36aaa605b2d",
      "episode_count": 0,
      "first_air_date": null,
      "id": 5741,
      "name": "Freedom: A History of Us",
      "original_name": "Freedom: A History of Us",
      "poster_path": null,
      "media_type": "tv"
    },
    {
      "character": "",
      "credit_id": "5257fa0319c29531db2ed3bb",
      "episode_count": 1,
      "first_air_date": "1994-01-03",
      "id": 9937,
      "name": "Intimate Portrait",
      "original_name": "Intimate Portrait",
      "poster_path": null,
      "media_type": "tv"
    },
    {
      "character": "",
      "credit_id": "5258833a760ee346614043a6",
      "episode_count": 3,
      "first_air_date": "1929-05-16",
      "id": 27023,
      "name": "The Academy Awards",
      "original_name": "The Academy Awards",
      "poster_path": "/xxWK636MvogUp4Y5pX3ri69yMcy.jpg",
      "media_type": "tv"
    },
    {
      "character": "Walker Lovejoy",
      "credit_id": "525895bf760ee3466158aa8e",
      "episode_count": 6,
      "first_air_date": "1990-07-25",
      "id": 29999,
      "name": "Glory Days",
      "original_name": "Glory Days",
      "poster_path": null,
      "media_type": "tv"
    },
    {
      "character": "",
      "credit_id": "525704eb760ee3776a008abe",
      "episode_count": 1,
      "first_air_date": "1987-09-29",
      "id": 1448,
      "name": "Thirtysomething",
      "original_name": "Thirtysomething",
      "poster_path": "/tlEWRrWe2SCd0cYfk3X5LEoiOpm.jpg",
      "media_type": "tv"
    },
    {
      "character": "",
      "credit_id": "52570765760ee3776a03124d",
      "episode_count": 1,
      "first_air_date": "1987-04-12",
      "id": 1486,
      "name": "21 Jump Street",
      "original_name": "21 Jump Street",
      "poster_path": "/5zobhoPvdfqQOmvRRukhoJDiJ3M.jpg",
      "media_type": "tv"
    },
    {
      "character": "Will Colbert",
      "credit_id": "5257107819c295731c02cf9b",
      "episode_count": 1,
      "first_air_date": "1994-09-22",
      "id": 1668,
      "name": "Friends",
      "original_name": "Friends",
      "poster_path": "/oQR7HXiCWpWvmUN1wo3asxrkAeZ.jpg",
      "media_type": "tv"
    },
    {
      "character": "",
      "credit_id": "525713d5760ee3776a113c77",
      "episode_count": 1,
      "first_air_date": "1952-01-14",
      "id": 1709,
      "name": "Today",
      "original_name": "Today",
      "poster_path": "/o7bSMUPaIxYbbsgC6VTbrvOVDpN.jpg",
      "media_type": "tv"
    },
    {
      "character": "",
      "credit_id": "52572b4e760ee3776a2d9c26",
      "episode_count": 1,
      "first_air_date": "1996-07-22",
      "id": 2224,
      "name": "The Daily Show",
      "original_name": "The Daily Show",
      "poster_path": "/dorWIToZ035gNAYORey8IzpFR1J.jpg",
      "media_type": "tv"
    },
    {
      "character": "",
      "credit_id": "525734f6760ee3776a3977e7",
      "episode_count": 1,
      "first_air_date": "1989-06-10",
      "id": 2391,
      "name": "Tales from the Crypt",
      "original_name": "Tales from the Crypt",
      "poster_path": "/eN5mW5SVCtsu7AycQtZSTnOoLot.jpg",
      "media_type": "tv"
    },
    {
      "character": "",
      "credit_id": "52573d96760ee36aaa047b7e",
      "episode_count": 1,
      "first_air_date": "1986-09-17",
      "id": 2589,
      "name": "Head of the Class",
      "original_name": "Head of the Class",
      "poster_path": "/Dyy3E7SqkeuYpiQET2rIwsGPxZ.jpg",
      "media_type": "tv"
    },
    {
      "character": "",
      "credit_id": "525753c6760ee36aaa1f53c7",
      "episode_count": 1,
      "first_air_date": "2002-06-11",
      "id": 3626,
      "name": "American Idol",
      "original_name": "American Idol",
      "poster_path": "/2aqueDlNwOWrszV0hKsxVk5yOgK.jpg",
      "media_type": "tv"
    },
    {
      "character": "",
      "credit_id": "5258094919c29531db3e2f4a",
      "episode_count": 6,
      "first_air_date": "2003-06-04",
      "id": 10946,
      "name": "Celebrities Uncensored",
      "original_name": "Celebrities Uncensored",
      "poster_path": null,
      "media_type": "tv"
    }
  ],
  "crew": [
    {
      "adult": false,
      "credit_id": "52fe492cc3a368484e11dfe1",
      "department": "Production",
      "id": 76203,
      "job": "Producer",
      "original_title": "12 Years a Slave",
      "poster_path": "/kb3X943WMIJYVg4SOAyK0pmWL5D.jpg",
      "release_date": "2013-10-30",
      "title": "12 Years a Slave",
      "media_type": "movie"
    },
    {
      "adult": false,
      "credit_id": "52fe4b3fc3a36847f81f9f89",
      "department": "Production",
      "id": 113833,
      "job": "Producer",
      "original_title": "The Normal Heart",
      "poster_path": "/fIf4nLpWHK8BsbH76fPgMbLSjuU.jpg",
      "release_date": "2014-05-25",
      "title": "The Normal Heart",
      "media_type": "movie"
    },
    {
      "adult": false,
      "credit_id": "52fe4d49c3a36847f8258cf3",
      "department": "Production",
      "id": 174349,
      "job": "Executive Producer",
      "original_title": "Big Men",
      "poster_path": "/zBydsc77yMGXYIiHsbJNE9Vaf4i.jpg",
      "release_date": "2014-03-14",
      "title": "Big Men",
      "media_type": "movie"
    },
    {
      "adult": false,
      "credit_id": "52fe4e48c3a368484e2183d1",
      "department": "Production",
      "id": 218277,
      "job": "Executive Producer",
      "original_title": "Pretty/Handsome",
      "poster_path": "/hiASAaSle8sjUZ9BHs4XrA30shS.jpg",
      "release_date": "2008-06-01",
      "title": "Pretty/Handsome",
      "media_type": "movie"
    },
    {
      "adult": false,
      "credit_id": "52fe4f0ac3a36847f82b9247",
      "department": "Production",
      "id": 245706,
      "job": "Producer",
      "original_title": "True Story",
      "poster_path": null,
      "release_date": null,
      "title": "True Story",
      "media_type": "movie"
    },
    {
      "adult": false,
      "credit_id": "5383b2540e0a2624bd00d335",
      "department": "Production",
      "id": 60308,
      "job": "Producer",
      "original_title": "Moneyball",
      "poster_path": "/3oAa8mJJ97CH9AeGEY6vjAxqcvZ.jpg",
      "release_date": "2011-09-22",
      "title": "Moneyball",
      "media_type": "movie"
    },
    {
      "adult": false,
      "credit_id": "52fe42f5c3a36847f802ff41",
      "department": "Production",
      "id": 1422,
      "job": "Producer",
      "original_title": "The Departed",
      "poster_path": "/tGLO9zw5ZtCeyyEWgbYGgsFxC6i.jpg",
      "release_date": "2006-11-09",
      "title": "The Departed",
      "media_type": "movie"
    },
    {
      "adult": false,
      "credit_id": "52fe4329c3a36847f803ee3b",
      "department": "Production",
      "id": 1988,
      "job": "Producer",
      "original_title": "A Mighty Heart",
      "poster_path": "/eFhsNdOjLk5sAEaEMcvRpnKc19c.jpg",
      "release_date": "2007-05-21",
      "title": "A Mighty Heart",
      "media_type": "movie"
    },
    {
      "adult": false,
      "credit_id": "52fe43c7c3a36847f806ef0b",
      "department": "Production",
      "id": 4512,
      "job": "Producer",
      "original_title": "The Assassination of Jesse James by the Coward Robert Ford",
      "poster_path": "/lSFYLoaL4eW7Q5VQ7SZQP4EHRCt.jpg",
      "release_date": "2007-09-20",
      "title": "The Assassination of Jesse James by the Coward Robert Ford",
      "media_type": "movie"
    },
    {
      "adult": false,
      "credit_id": "52fe446ac3a368484e021e13",
      "department": "Production",
      "id": 23483,
      "job": "Producer",
      "original_title": "Kick-Ass",
      "poster_path": "/yYy7bJ0HuSudtHDksGODZohRQWo.jpg",
      "release_date": "2010-04-16",
      "title": "Kick-Ass",
      "media_type": "movie"
    },
    {
      "adult": false,
      "credit_id": "52fe4481c3a36847f809a065",
      "department": "Production",
      "id": 7510,
      "job": "Producer",
      "original_title": "Running with Scissors",
      "poster_path": "/pYFF3iMWDPcwXKpRM0GLIsnPf22.jpg",
      "release_date": "2006-10-27",
      "title": "Running with Scissors",
      "media_type": "movie"
    },
    {
      "adult": false,
      "credit_id": "52fe4495c3a368484e02af99",
      "department": "Production",
      "id": 24420,
      "job": "Executive Producer",
      "original_title": "The Time Traveler's Wife",
      "poster_path": "/ayGp00uS6XRrNfbR59XWrJh9jpC.jpg",
      "release_date": "2009-08-14",
      "title": "The Time Traveler's Wife",
      "media_type": "movie"
    },
    {
      "adult": false,
      "credit_id": "52fe469c9251416c91059ecf",
      "department": "Production",
      "id": 38167,
      "job": "Executive Producer",
      "original_title": "Eat Pray Love",
      "poster_path": "/s57E4AfPIU1fxwpGGRahk6A0DUl.jpg",
      "release_date": "2010-08-12",
      "title": "Eat Pray Love",
      "media_type": "movie"
    },
    {
      "adult": false,
      "credit_id": "52fe46e4c3a368484e0a9aa7",
      "department": "Production",
      "id": 64689,
      "job": "Producer",
      "original_title": "Killing Them Softly",
      "poster_path": "/s212M6rVyA0gcMB9IAqSdUGx5JT.jpg",
      "release_date": "2012-11-30",
      "title": "Killing Them Softly",
      "media_type": "movie"
    },
    {
      "adult": false,
      "credit_id": "52fe485dc3a368484e0f50f9",
      "department": "Production",
      "id": 72190,
      "job": "Producer",
      "original_title": "World War Z",
      "poster_path": "/gAt1PrsrFY1nX6UzebeiHP8njE9.jpg",
      "release_date": "2013-06-21",
      "title": "World War Z",
      "media_type": "movie"
    },
    {
      "credit_id": "531cc64d92514177b7005d3a",
      "department": "Creator",
      "first_air_date": "2014-03-09",
      "id": 51370,
      "job": "Creator",
      "name": "Resurrection",
      "original_name": "Resurrection",
      "poster_path": "/oNiK8YaOGR995kLlNPhsM3mx506.jpg",
      "media_type": "tv"
    }
  ],
  "id": 287
}

Get the external ids for a specific person id.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
</table>

GET /person/{id}/external_ids
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "aaddbc8ae6fdd1dd3e09c1f36b44a13e"
{
  "imdb_id": "nm0000093",
  "freebase_mid": "/m/0c6qh",
  "freebase_id": "/en/brad_pitt",
  "tvrage_id": 59436,
  "id": 287
}

Get the images for a specific person id.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
</table>

GET /person/{id}/images
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "daabfabaf8ea306c8043526a4cc8951f"
{
    "id": 287,
    "profiles": [
        {
            "file_path": "/w8zJQuN7tzlm6FY9mfGKihxp3Cb.jpg",
            "width": 1295,
            "height": 1969,
            "iso_639_1": null,
            "aspect_ratio": 0.66000000000000003
        },
        {
            "file_path": "/cLUacutO7dOMksQK8Zg0q2Gybsx.jpg",
            "width": 820,
            "height": 1230,
            "iso_639_1": null,
            "aspect_ratio": 0.67000000000000004
        },
        {
            "file_path": "/zEbgoayf0MfuSznehhXdaP2YkeH.jpg",
            "width": 700,
            "height": 983,
            "iso_639_1": null,
            "aspect_ratio": 0.70999999999999996
        }
    ]
}

Get the images that have been tagged with a specific person id. We return all of the image results with a `media` object mapped for each image.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
<tr><td style="padding-left: 0;" colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-right: 40px; width: 140px;">page</td><td>Minimum 1, maximum 1000.</td></tr>
<tr><td style="padding-right: 40px;">language</td><td>ISO 639-1 code.</td></tr>
</table>

GET /person/{id}/tagged_images
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "7916e89fb73baa0b2b6eed6d9f819af8"
{
  "id": 287,
  "page": 1,
  "results": [
    {
      "aspect_ratio": 1.78,
      "file_path": "/r9BCMru6cPtuXeIRKGGkf4NNRrU.jpg",
      "height": 1080,
      "id": "4ea5f7694f13c137cc00228b",
      "iso_639_1": null,
      "vote_average": 5.48299319727891,
      "vote_count": 7,
      "width": 1920,
      "image_type": "backdrop",
      "media": {
        "adult": false,
        "backdrop_path": "/r9BCMru6cPtuXeIRKGGkf4NNRrU.jpg",
        "id": 4944,
        "original_title": "Burn After Reading",
        "release_date": "2008-09-12",
        "poster_path": "/v8TXYDzCe9YRamiXMIUdksG8Hpy.jpg",
        "popularity": 2.235565339,
        "title": "Burn After Reading",
        "vote_average": 6.3,
        "vote_count": 202
      },
      "media_type": "movie"
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/ba4CpvnaxvAgff2jHiaqJrVpZJ5.jpg",
      "height": 1080,
      "id": "4f75feaa19c29542930005f1",
      "iso_639_1": null,
      "vote_average": 5.47619047619048,
      "vote_count": 5,
      "width": 1920,
      "image_type": "backdrop",
      "media": {
        "adult": false,
        "backdrop_path": "/y9LxwLkIYows2NcIU4oAAk3Xfgt.jpg",
        "id": 807,
        "original_title": "Se7en",
        "release_date": "1995-09-22",
        "poster_path": "/mhFXxB76LVgJBIfJM1sAuVBtVKv.jpg",
        "popularity": 5.8118368555,
        "title": "Se7en",
        "vote_average": 7.5,
        "vote_count": 1475
      },
      "media_type": "movie"
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/pxlaSPleGSNI8jJZYGhXH5LdI1B.jpg",
      "height": 1080,
      "id": "4ec2de3c5e73d648f4002d2b",
      "iso_639_1": null,
      "vote_average": 5.46258503401361,
      "vote_count": 7,
      "width": 1920,
      "image_type": "backdrop",
      "media": {
        "adult": false,
        "backdrop_path": "/pxlaSPleGSNI8jJZYGhXH5LdI1B.jpg",
        "id": 60308,
        "original_title": "Moneyball",
        "release_date": "2011-09-22",
        "poster_path": "/3oAa8mJJ97CH9AeGEY6vjAxqcvZ.jpg",
        "popularity": 4.028921845,
        "title": "Moneyball",
        "vote_average": 6.7,
        "vote_count": 355
      },
      "media_type": "movie"
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/uHx9E9xqSgOBoRvL4shmMNu8Ojc.jpg",
      "height": 1080,
      "id": "4ea5e2754f13c137cc001229",
      "iso_639_1": null,
      "vote_average": 5.45454545454546,
      "vote_count": 3,
      "width": 1920,
      "image_type": "backdrop",
      "media": {
        "adult": false,
        "backdrop_path": "/uHx9E9xqSgOBoRvL4shmMNu8Ojc.jpg",
        "id": 1164,
        "original_title": "Babel",
        "release_date": "2006-10-27",
        "poster_path": "/fxneN0EQZwTfAfhTGUvUuIn6PLi.jpg",
        "popularity": 2.888253175,
        "title": "Babel",
        "vote_average": 6.5,
        "vote_count": 166
      },
      "media_type": "movie"
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/7NhgUfvLRkE7rWWaAbeqihiQTQs.jpg",
      "height": 1080,
      "id": "51655f87760ee312c902821b",
      "iso_639_1": "xx",
      "vote_average": 5.45396825396825,
      "vote_count": 12,
      "width": 1920,
      "image_type": "backdrop",
      "media": {
        "adult": false,
        "backdrop_path": "/xMOQVYLeIKBXenJ9KMeasj7S64y.jpg",
        "id": 72190,
        "original_title": "World War Z",
        "release_date": "2013-06-21",
        "poster_path": "/gAt1PrsrFY1nX6UzebeiHP8njE9.jpg",
        "popularity": 6.313542341,
        "title": "World War Z",
        "vote_average": 6.8,
        "vote_count": 1271
      },
      "media_type": "movie"
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/T9a9Q6Pin6Pra7dNMwWzAibxkk.jpg",
      "height": 1080,
      "id": "4efc381d760ee30492008da9",
      "iso_639_1": null,
      "vote_average": 5.44117647058823,
      "vote_count": 5,
      "width": 1920,
      "image_type": "backdrop",
      "media": {
        "adult": false,
        "backdrop_path": "/pxlaSPleGSNI8jJZYGhXH5LdI1B.jpg",
        "id": 60308,
        "original_title": "Moneyball",
        "release_date": "2011-09-22",
        "poster_path": "/3oAa8mJJ97CH9AeGEY6vjAxqcvZ.jpg",
        "popularity": 4.028921845,
        "title": "Moneyball",
        "vote_average": 6.7,
        "vote_count": 355
      },
      "media_type": "movie"
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/bk0GylJLneaSbpQZXpgTwleYigq.jpg",
      "height": 1080,
      "id": "4ea637de4f13c137cc005f79",
      "iso_639_1": null,
      "vote_average": 5.41979949874687,
      "vote_count": 13,
      "width": 1920,
      "image_type": "backdrop",
      "media": {
        "adult": false,
        "backdrop_path": "/7nF6B9yCEq1ZCT82sGJVtNxOcl5.jpg",
        "id": 16869,
        "original_title": "Inglourious Basterds",
        "release_date": "2009-08-21",
        "poster_path": "/keAMrS08NxKvMxh8GRDhS9UCph4.jpg",
        "popularity": 8.9224393722,
        "title": "Inglourious Basterds",
        "vote_average": 7.2,
        "vote_count": 1374
      },
      "media_type": "movie"
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/9Vf8b0qeqiIvB5n2afnlHEg6VJp.jpg",
      "height": 768,
      "id": "4f09465319c2957a86002100",
      "iso_639_1": null,
      "vote_average": 5.41847041847042,
      "vote_count": 3,
      "width": 1366,
      "image_type": "backdrop",
      "media": {
        "adult": false,
        "backdrop_path": "/6KXbhaxkgExC5EdDqAzRinhmoZ8.jpg",
        "id": 63,
        "original_title": "Twelve Monkeys",
        "release_date": "1995-12-27",
        "poster_path": "/6Sj9wDu3YugthXsU0Vry5XFAZGg.jpg",
        "popularity": 3.5830834059,
        "title": "Twelve Monkeys",
        "vote_average": 6.9,
        "vote_count": 588
      },
      "media_type": "movie"
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/rdl3ywFMk6V1yC3Uen4EAF6XBPw.jpg",
      "height": 1080,
      "id": "4ea724a534f863633c005863",
      "iso_639_1": null,
      "vote_average": 5.41247484909457,
      "vote_count": 8,
      "width": 1920,
      "image_type": "backdrop",
      "media": {
        "adult": false,
        "backdrop_path": "/pxlaSPleGSNI8jJZYGhXH5LdI1B.jpg",
        "id": 60308,
        "original_title": "Moneyball",
        "release_date": "2011-09-22",
        "poster_path": "/3oAa8mJJ97CH9AeGEY6vjAxqcvZ.jpg",
        "popularity": 4.028921845,
        "title": "Moneyball",
        "vote_average": 6.7,
        "vote_count": 355
      },
      "media_type": "movie"
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/dy7jGcngbzgENCwYIHhLGWZ9S2H.jpg",
      "height": 1080,
      "id": "4f75fecc19c29542f7000616",
      "iso_639_1": null,
      "vote_average": 5.3968253968254,
      "vote_count": 3,
      "width": 1920,
      "image_type": "backdrop",
      "media": {
        "adult": false,
        "backdrop_path": "/y9LxwLkIYows2NcIU4oAAk3Xfgt.jpg",
        "id": 807,
        "original_title": "Se7en",
        "release_date": "1995-09-22",
        "poster_path": "/mhFXxB76LVgJBIfJM1sAuVBtVKv.jpg",
        "popularity": 5.8118368555,
        "title": "Se7en",
        "vote_average": 7.5,
        "vote_count": 1475
      },
      "media_type": "movie"
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/mMZRKb3NVo5ZeSPEIaNW9buLWQ0.jpg",
      "height": 1080,
      "id": "4ea5cca634f8633bdc000a73",
      "iso_639_1": "xx",
      "vote_average": 5.37414965986395,
      "vote_count": 14,
      "width": 1920,
      "image_type": "backdrop",
      "media": {
        "adult": false,
        "backdrop_path": "/hNFMawyNDWZKKHU4GYCBz1krsRM.jpg",
        "id": 550,
        "original_title": "Fight Club",
        "release_date": "1999-10-14",
        "poster_path": "/2lECpi35Hnbpa4y46JX0aY3AWTy.jpg",
        "popularity": 7.6505979119,
        "title": "Fight Club",
        "vote_average": 7.6,
        "vote_count": 2852
      },
      "media_type": "movie"
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/4H1dlgLNsaZlCBTupEB08VadI1D.jpg",
      "height": 1080,
      "id": "4ea724a634f863633c005865",
      "iso_639_1": null,
      "vote_average": 5.37114845938375,
      "vote_count": 5,
      "width": 1920,
      "image_type": "backdrop",
      "media": {
        "adult": false,
        "backdrop_path": "/pxlaSPleGSNI8jJZYGhXH5LdI1B.jpg",
        "id": 60308,
        "original_title": "Moneyball",
        "release_date": "2011-09-22",
        "poster_path": "/3oAa8mJJ97CH9AeGEY6vjAxqcvZ.jpg",
        "popularity": 4.028921845,
        "title": "Moneyball",
        "vote_average": 6.7,
        "vote_count": 355
      },
      "media_type": "movie"
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/pO32sfzXKf2yHd5A48UlIlDcndI.jpg",
      "height": 1080,
      "id": "4ea5f4652c058837cb0022c9",
      "iso_639_1": null,
      "vote_average": 5.34470504619758,
      "vote_count": 4,
      "width": 1920,
      "image_type": "backdrop",
      "media": {
        "adult": false,
        "backdrop_path": "/xOXT99TBUgROWqbDvIoCvqogTo1.jpg",
        "id": 4922,
        "original_title": "The Curious Case of Benjamin Button",
        "release_date": "2008-11-24",
        "poster_path": "/4O4INOPtWTfHq3dd5vYTPV0TCwa.jpg",
        "popularity": 3.448410102,
        "title": "The Curious Case of Benjamin Button",
        "vote_average": 6.7,
        "vote_count": 478
      },
      "media_type": "movie"
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/4PsQNjA2lK65j6ZE9sz88ml6owb.jpg",
      "height": 1080,
      "id": "4eecb3e319c29554de00053c",
      "iso_639_1": null,
      "vote_average": 5.31292517006803,
      "vote_count": 7,
      "width": 1920,
      "image_type": "backdrop",
      "media": {
        "adult": false,
        "backdrop_path": "/pxlaSPleGSNI8jJZYGhXH5LdI1B.jpg",
        "id": 60308,
        "original_title": "Moneyball",
        "release_date": "2011-09-22",
        "poster_path": "/3oAa8mJJ97CH9AeGEY6vjAxqcvZ.jpg",
        "popularity": 4.028921845,
        "title": "Moneyball",
        "vote_average": 6.7,
        "vote_count": 355
      },
      "media_type": "movie"
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/lIl7K5FslEOD4zUP8vfIK3IO2Tn.jpg",
      "height": 720,
      "id": "51eafe99760ee3364e045c3c",
      "iso_639_1": "xx",
      "vote_average": 5.3125,
      "vote_count": 1,
      "width": 1280,
      "image_type": "backdrop",
      "media": {
        "adult": false,
        "backdrop_path": "/hNFMawyNDWZKKHU4GYCBz1krsRM.jpg",
        "id": 550,
        "original_title": "Fight Club",
        "release_date": "1999-10-14",
        "poster_path": "/2lECpi35Hnbpa4y46JX0aY3AWTy.jpg",
        "popularity": 7.6505979119,
        "title": "Fight Club",
        "vote_average": 7.6,
        "vote_count": 2852
      },
      "media_type": "movie"
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/mTqQ7t3xMAsXtlLXPI3jOBasPJB.jpg",
      "height": 1080,
      "id": "4ea5f73c4f13c137cc002267",
      "iso_639_1": null,
      "vote_average": 5.31135531135531,
      "vote_count": 2,
      "width": 1920,
      "image_type": "backdrop",
      "media": {
        "adult": false,
        "backdrop_path": "/r9BCMru6cPtuXeIRKGGkf4NNRrU.jpg",
        "id": 4944,
        "original_title": "Burn After Reading",
        "release_date": "2008-09-12",
        "poster_path": "/v8TXYDzCe9YRamiXMIUdksG8Hpy.jpg",
        "popularity": 2.235565339,
        "title": "Burn After Reading",
        "vote_average": 6.3,
        "vote_count": 202
      },
      "media_type": "movie"
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/8T0hpqWsgzKhWGsXGD8ilkwRPkC.jpg",
      "height": 720,
      "id": "4ea5cc9934f8633bdc000a69",
      "iso_639_1": "xx",
      "vote_average": 5.28631705846896,
      "vote_count": 16,
      "width": 1280,
      "image_type": "backdrop",
      "media": {
        "adult": false,
        "backdrop_path": "/hNFMawyNDWZKKHU4GYCBz1krsRM.jpg",
        "id": 550,
        "original_title": "Fight Club",
        "release_date": "1999-10-14",
        "poster_path": "/2lECpi35Hnbpa4y46JX0aY3AWTy.jpg",
        "popularity": 7.6505979119,
        "title": "Fight Club",
        "vote_average": 7.6,
        "vote_count": 2852
      },
      "media_type": "movie"
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/6akRXcS3JjqfV4jguoLao6Z1cKm.jpg",
      "height": 1080,
      "id": "506d95fc760ee37d0a001046",
      "iso_639_1": null,
      "vote_average": 5.28273809523809,
      "vote_count": 1,
      "width": 1920,
      "image_type": "backdrop",
      "media": {
        "adult": false,
        "backdrop_path": "/y9LxwLkIYows2NcIU4oAAk3Xfgt.jpg",
        "id": 807,
        "original_title": "Se7en",
        "release_date": "1995-09-22",
        "poster_path": "/mhFXxB76LVgJBIfJM1sAuVBtVKv.jpg",
        "popularity": 5.8118368555,
        "title": "Se7en",
        "vote_average": 7.5,
        "vote_count": 1475
      },
      "media_type": "movie"
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/9Kr6UzouF674Smw3D9Hp2DlH1Vo.jpg",
      "height": 720,
      "id": "4ea5cc6534f8633bdc000a43",
      "iso_639_1": "xx",
      "vote_average": 5.24453024453024,
      "vote_count": 11,
      "width": 1280,
      "image_type": "backdrop",
      "media": {
        "adult": false,
        "backdrop_path": "/hNFMawyNDWZKKHU4GYCBz1krsRM.jpg",
        "id": 550,
        "original_title": "Fight Club",
        "release_date": "1999-10-14",
        "poster_path": "/2lECpi35Hnbpa4y46JX0aY3AWTy.jpg",
        "popularity": 7.6505979119,
        "title": "Fight Club",
        "vote_average": 7.6,
        "vote_count": 2852
      },
      "media_type": "movie"
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/8EfXLkttQGkCB6zXpHoGa5ZJUhe.jpg",
      "height": 1080,
      "id": "4ea5f7734f13c137cc002291",
      "iso_639_1": null,
      "vote_average": 5.21008403361345,
      "vote_count": 5,
      "width": 1920,
      "image_type": "backdrop",
      "media": {
        "adult": false,
        "backdrop_path": "/r9BCMru6cPtuXeIRKGGkf4NNRrU.jpg",
        "id": 4944,
        "original_title": "Burn After Reading",
        "release_date": "2008-09-12",
        "poster_path": "/v8TXYDzCe9YRamiXMIUdksG8Hpy.jpg",
        "popularity": 2.235565339,
        "title": "Burn After Reading",
        "vote_average": 6.3,
        "vote_count": 202
      },
      "media_type": "movie"
    }
  ],
  "total_pages": 2,
  "total_results": 24
}

Get the changes for a specific person id.

Changes are grouped by key, and ordered by date in descending order. By default, only the last 24 hours of changes are returned. The maximum number of days that can be returned in a single request is 14. The language is present on fields that are translatable.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
<tr><td style="padding-left: 0;" colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-left: 0; padding-right: 40px; width: 140px;">start_date</td><td>YYYY-MM-DD</td></tr>
<tr><td style="padding-left: 0; padding-right: 40px; width: 140px;">end_date</td><td>YYYY-MM-DD</td></tr>
</table>

GET /person/{id}/changes
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "a007a0175bcf31188293947721411fe6"
{
    "changes": [
        {
            "key": "general",
            "items": [
                {
                    "id": "50745cd3760ee35edd0001a4",
                    "action": "created",
                    "time": "2012-10-09 17:20:19 UTC"
                }
            ]
        },
        {
            "key": "name",
            "items": [
                {
                    "id": "50745cd3760ee35edd0001a5",
                    "action": "added",
                    "time": "2012-10-09 17:20:19 UTC",
                    "value": "Brad Pitt"
                }
            ]
        }
    ]
}

Get the list of popular people on The Movie Database. This list refreshes every day.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td colspan="2">api_key</td></tr>
<tr><td colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-right: 40px; width: 140px;">page</td><td>Minimum 1, maximum 1000.</td></tr>
</table>

GET /person/popular
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "e97fd97466d81dfca23cf648a016affd"
{
  "page": 1,
  "results": [
    {
      "adult": false,
      "id": 85,
      "known_for": [
        {
          "adult": false,
          "backdrop_path": "/jpRpigNQUjNlfx0gYRBJ30tQIOl.jpg",
          "id": 22,
          "original_title": "Pirates of the Caribbean: The Curse of the Black Pearl",
          "release_date": "2003-07-09",
          "poster_path": "/mL3mGWeiANcAeyJQDwEdwpxQKYv.jpg",
          "popularity": 3.97737662538628,
          "title": "Pirates of the Caribbean: The Curse of the Black Pearl",
          "vote_average": 6.9,
          "vote_count": 2191,
          "media_type": "movie"
        },
        {
          "adult": false,
          "backdrop_path": "/ddPXVUAeCBFMbtTajh8bg4uyBvv.jpg",
          "id": 1865,
          "original_title": "Pirates of the Caribbean: On Stranger Tides",
          "release_date": "2011-05-11",
          "poster_path": "/jUkGuSC9Kt29rW3x6UiB9zyZr1M.jpg",
          "popularity": 3.24218587513748,
          "title": "Pirates of the Caribbean: On Stranger Tides",
          "vote_average": 6.3,
          "vote_count": 1810,
          "media_type": "movie"
        },
        {
          "adult": false,
          "backdrop_path": "/hdHgIcljPHli4xaJGt0INz8Gn3J.jpg",
          "id": 58,
          "original_title": "Pirates of the Caribbean: Dead Man's Chest",
          "release_date": "2006-07-07",
          "poster_path": "/9pSUIBqkr89PdlJ6fNrXwNNbgp.jpg",
          "popularity": 3.56800816641277,
          "title": "Pirates of the Caribbean: Dead Man's Chest",
          "vote_average": 6.5,
          "vote_count": 1699,
          "media_type": "movie"
        }
      ],
      "name": "Johnny Depp",
      "popularity": 51.9316242947408,
      "profile_path": "/5sc2pu4YWItxCaXSvrFpg64zN9J.jpg"
    },
    {
      "adult": false,
      "id": 287,
      "known_for": [
        {
          "adult": false,
          "backdrop_path": "/hNFMawyNDWZKKHU4GYCBz1krsRM.jpg",
          "id": 550,
          "original_title": "Fight Club",
          "release_date": "1999-10-14",
          "poster_path": "/2lECpi35Hnbpa4y46JX0aY3AWTy.jpg",
          "popularity": 4.80985867465586,
          "title": "Fight Club",
          "vote_average": 7.7,
          "vote_count": 3011,
          "media_type": "movie"
        },
        {
          "adult": false,
          "backdrop_path": "/z2fiN0tgkgOcAFl5gxvQlYXCn3l.jpg",
          "id": 161,
          "original_title": "Ocean's Eleven",
          "release_date": "2001-12-06",
          "poster_path": "/5gkZ8PL3JZXXgq398xWzaxVB1Sc.jpg",
          "popularity": 2.71316059006952,
          "title": "Ocean's Eleven",
          "vote_average": 6.8,
          "vote_count": 1648,
          "media_type": "movie"
        },
        {
          "adult": false,
          "backdrop_path": "/xMOQVYLeIKBXenJ9KMeasj7S64y.jpg",
          "id": 72190,
          "original_title": "World War Z",
          "release_date": "2013-06-21",
          "poster_path": "/gAt1PrsrFY1nX6UzebeiHP8njE9.jpg",
          "popularity": 5.08625318186859,
          "title": "World War Z",
          "vote_average": 6.8,
          "vote_count": 1612,
          "media_type": "movie"
        }
      ],
      "name": "Brad Pitt",
      "popularity": 54.4720398191207,
      "profile_path": "/kc3M04QQAuZ9woUvH3Ju5T7ZqG5.jpg"
    },
    {
      "adult": false,
      "id": 31,
      "known_for": [
        {
          "adult": false,
          "backdrop_path": "/ctOEhQiFIHWkiaYp7b0ibSTe5IL.jpg",
          "id": 13,
          "original_title": "Forrest Gump",
          "release_date": "1994-06-22",
          "poster_path": "/y3EsNpMFwvpcucLmx4HiiRRhCXV.jpg",
          "popularity": 3.44425138567849,
          "title": "Forrest Gump",
          "vote_average": 7.6,
          "vote_count": 2239,
          "media_type": "movie"
        },
        {
          "adult": false,
          "backdrop_path": "/4y5TDUZlqUmWWtjTAznWb6CFpzt.jpg",
          "id": 857,
          "original_title": "Saving Private Ryan",
          "release_date": "1998-07-24",
          "poster_path": "/gc7IN6bWNaWXv4vI6cxSmeB7PeO.jpg",
          "popularity": 2.98171026475799,
          "title": "Saving Private Ryan",
          "vote_average": 7.3,
          "vote_count": 1782,
          "media_type": "movie"
        },
        {
          "adult": false,
          "backdrop_path": "/dji4Fm0gCDVb9DQQMRvAI8YNnTz.jpg",
          "id": 862,
          "original_title": "Toy Story",
          "release_date": "1995-11-21",
          "poster_path": "/agy8DheVu5zpQFbXfAdvYivF2FU.jpg",
          "popularity": 4.08406068625621,
          "title": "Toy Story",
          "vote_average": 7.2,
          "vote_count": 1743,
          "media_type": "movie"
        }
      ],
      "name": "Tom Hanks",
      "popularity": 59.424280524211,
      "profile_path": "/xxPMucou2wRDxLrud8i2D4dsywh.jpg"
    },
    {
      "adult": false,
      "id": 3896,
      "known_for": [
        {
          "adult": false,
          "backdrop_path": "/3bgtUfKQKNi3nJsAB5URpP2wdRt.jpg",
          "id": 49026,
          "original_title": "The Dark Knight Rises",
          "release_date": "2012-07-20",
          "poster_path": "/dEYnvnUfXrqvqeRSqvIEtmzhoA8.jpg",
          "popularity": 5.15224262409531,
          "title": "The Dark Knight Rises",
          "vote_average": 7.3,
          "vote_count": 4342,
          "media_type": "movie"
        },
        {
          "adult": false,
          "backdrop_path": "/v0p5dxVa9Giq6e0h31VifY0dWOX.jpg",
          "id": 272,
          "original_title": "Batman Begins",
          "release_date": "2005-06-15",
          "poster_path": "/zfVFOo2XCHbeA0mXbst42TAGhfC.jpg",
          "popularity": 4.53028273911481,
          "title": "Batman Begins",
          "vote_average": 7.1,
          "vote_count": 2702,
          "media_type": "movie"
        },
        {
          "adult": false,
          "backdrop_path": "/aI8luIcMpYZPixrEc70pARrHBwE.jpg",
          "id": 8681,
          "original_title": "Taken",
          "release_date": "2008-09-25",
          "poster_path": "/q2ufZ5zgbyvwSVR9J5wR6nEAEyy.jpg",
          "popularity": 5.76166783156055,
          "title": "Taken",
          "vote_average": 6.9,
          "vote_count": 1559,
          "media_type": "movie"
        }
      ],
      "name": "Liam Neeson",
      "popularity": 50.9446287827921,
      "profile_path": "/tBTGVtytdRr9qaDvKxcDUHxq6fn.jpg"
    },
    {
      "adult": false,
      "id": 12835,
      "known_for": [
        {
          "adult": false,
          "backdrop_path": "/qjfE7SkPXpqFs8FX8rIaG6eO2aK.jpg",
          "id": 82992,
          "original_title": "Fast & Furious 6",
          "release_date": "2013-05-22",
          "poster_path": "/bsH746t5bLZ8U1wIW9FmWXyyhrW.jpg",
          "popularity": 7.7609046199134,
          "title": "Fast & Furious 6",
          "vote_average": 6.5,
          "vote_count": 3316,
          "media_type": "movie"
        },
        {
          "adult": false,
          "backdrop_path": "/4y5TDUZlqUmWWtjTAznWb6CFpzt.jpg",
          "id": 857,
          "original_title": "Saving Private Ryan",
          "release_date": "1998-07-24",
          "poster_path": "/gc7IN6bWNaWXv4vI6cxSmeB7PeO.jpg",
          "popularity": 2.98171026475799,
          "title": "Saving Private Ryan",
          "vote_average": 7.3,
          "vote_count": 1782,
          "media_type": "movie"
        },
        {
          "adult": false,
          "backdrop_path": "/lmIqH8Qsv3IvDg0PTFUuVr89eBT.jpg",
          "id": 9799,
          "original_title": "The Fast and the Furious",
          "release_date": "2001-06-17",
          "poster_path": "/x4So4OkqnjfOSBCCNd5uosMmQiB.jpg",
          "popularity": 1.82850851338139,
          "title": "The Fast and the Furious",
          "vote_average": 6.2,
          "vote_count": 1721,
          "media_type": "movie"
        }
      ],
      "name": "Vin Diesel",
      "popularity": 42.7693179290989,
      "profile_path": "/qwyfzMKIhxJ7ols66FgEf7eGdcI.jpg"
    },
    {
      "adult": false,
      "id": 500,
      "known_for": [
        {
          "adult": false,
          "backdrop_path": "/8TE77jL2e4zdERpv8hnBAHUmFRx.jpg",
          "id": 75612,
          "original_title": "Oblivion",
          "release_date": "2013-04-18",
          "poster_path": "/hmOzkHlkGvi8x24fYpFSnXvjklv.jpg",
          "popularity": 5.88615472384954,
          "title": "Oblivion",
          "vote_average": 6.1,
          "vote_count": 2256,
          "media_type": "movie"
        },
        {
          "adult": false,
          "backdrop_path": "/wLRRjb0GEUCuBMx3YFksg2xyH4S.jpg",
          "id": 56292,
          "original_title": "Mission: Impossible - Ghost Protocol",
          "release_date": "2011-12-21",
          "poster_path": "/96k0SkL6bXqKyOj50hYu3fRRbmV.jpg",
          "popularity": 3.05614946375196,
          "title": "Mission: Impossible - Ghost Protocol",
          "vote_average": 6.4,
          "vote_count": 1830,
          "media_type": "movie"
        },
        {
          "adult": false,
          "backdrop_path": "/ezXodpP429qK0Av89pVNlaXWJkQ.jpg",
          "id": 75780,
          "original_title": "Jack Reacher",
          "release_date": "2012-12-21",
          "poster_path": "/38bmEXmuJuInLs9dwfgOGCHmZ7l.jpg",
          "popularity": 3.32484965926354,
          "title": "Jack Reacher",
          "vote_average": 6.0,
          "vote_count": 1360,
          "media_type": "movie"
        }
      ],
      "name": "Tom Cruise",
      "popularity": 50.3460750727771,
      "profile_path": "/3oWEuo0e8Nx8JvkqYCDec2iMY6K.jpg"
    },
    {
      "adult": false,
      "id": 228,
      "known_for": [
        {
          "adult": false,
          "backdrop_path": "/nhsi11CWxLvbrdVFmHwFj1BaLE9.jpg",
          "id": 49047,
          "original_title": "Gravity",
          "release_date": "2013-10-04",
          "poster_path": "/2gPjLWIyrWlAn2DgKMOKTBnZYyO.jpg",
          "popularity": 6.59532449821778,
          "title": "Gravity",
          "vote_average": 7.8,
          "vote_count": 1230,
          "media_type": "movie"
        },
        {
          "adult": false,
          "backdrop_path": "/3xJbBYJdbm55bxPlLbWEbtWOsfP.jpg",
          "id": 37165,
          "original_title": "The Truman Show",
          "release_date": "1998-06-04",
          "poster_path": "/EelZzudHRvJmjWccWscN1S5vmI.jpg",
          "popularity": 1.97623776868843,
          "title": "The Truman Show",
          "vote_average": 7.0,
          "vote_count": 692,
          "media_type": "movie"
        },
        {
          "adult": false,
          "backdrop_path": "/4E6fPI8VHCdttDve9vBk14rsuBW.jpg",
          "id": 453,
          "original_title": "A Beautiful Mind",
          "release_date": "2001-12-12",
          "poster_path": "/4SFqHDZ1NvWdysucWbgnYlobdxC.jpg",
          "popularity": 3.2431454821671,
          "title": "A Beautiful Mind",
          "vote_average": 7.0,
          "vote_count": 638,
          "media_type": "movie"
        }
      ],
      "name": "Ed Harris",
      "popularity": 52.6992333199219,
      "profile_path": "/a9ITc3shCAWjV4qKf3rgR0Opu3y.jpg"
    },
    {
      "adult": false,
      "id": 3,
      "known_for": [
        {
          "adult": false,
          "backdrop_path": "/4iJfYYoQzZcONB9hNzg0J0wWyPH.jpg",
          "id": 11,
          "original_title": "Star Wars: Episode IV - A New Hope",
          "release_date": "1977-05-25",
          "poster_path": "/tvSlBzAdRE29bZe5yYWrJ2ds137.jpg",
          "popularity": 3.70242055227017,
          "title": "Star Wars: Episode IV - A New Hope",
          "vote_average": 7.6,
          "vote_count": 2175,
          "media_type": "movie"
        },
        {
          "adult": false,
          "backdrop_path": "/AkE7LQs2hPMG5tpWYcum847Knre.jpg",
          "id": 1891,
          "original_title": "Star Wars: Episode V - The Empire Strikes Back",
          "release_date": "1980-05-21",
          "poster_path": "/6u1fYtxG5eqjhtCPDx04pJphQRW.jpg",
          "popularity": 3.55849134807068,
          "title": "Star Wars: Episode V - The Empire Strikes Back",
          "vote_average": 7.7,
          "vote_count": 1963,
          "media_type": "movie"
        },
        {
          "adult": false,
          "backdrop_path": "/bvJOpyHYWACDusvQvXxKEHFNjce.jpg",
          "id": 1892,
          "original_title": "Star Wars: Episode VI - Return of the Jedi",
          "release_date": "1983-05-25",
          "poster_path": "/jx5p0aHlbPXqe3AH9G15NvmWaqQ.jpg",
          "popularity": 3.03244662506575,
          "title": "Star Wars: Episode VI - Return of the Jedi",
          "vote_average": 7.5,
          "vote_count": 1587,
          "media_type": "movie"
        }
      ],
      "name": "Harrison Ford",
      "popularity": 41.6210510990901,
      "profile_path": "/7CcoVFTogQgex2kJkXKMe8qHZrC.jpg"
    },
    {
      "adult": false,
      "id": 1158,
      "known_for": [
        {
          "adult": false,
          "backdrop_path": "/6xKCYgH16UuwEGAyroLU6p8HLIn.jpg",
          "id": 238,
          "original_title": "The Godfather",
          "release_date": "1972-03-15",
          "poster_path": "/d4KNaTrltq6bpkFS01pYtyXa09m.jpg",
          "popularity": 6.65327800433174,
          "title": "The Godfather",
          "vote_average": 8.0,
          "vote_count": 2108,
          "media_type": "movie"
        },
        {
          "adult": false,
          "backdrop_path": "/xUU1melxrkb7IXl1F7PXrtZAWWl.jpg",
          "id": 240,
          "original_title": "The Godfather: Part II",
          "release_date": "1974-12-20",
          "poster_path": "/tHbMIIF51rguMNSastqoQwR0sBs.jpg",
          "popularity": 2.78936358061284,
          "title": "The Godfather: Part II",
          "vote_average": 7.9,
          "vote_count": 1066,
          "media_type": "movie"
        },
        {
          "adult": false,
          "backdrop_path": "/7ytb78OyijteFpFKKoZsYSvPw2u.jpg",
          "id": 298,
          "original_title": "Ocean's Thirteen",
          "release_date": "2007-06-07",
          "poster_path": "/k7jNRNN9UuUc2VQg21YXYn3JGVY.jpg",
          "popularity": 1.77460817504617,
          "title": "Ocean's Thirteen",
          "vote_average": 6.4,
          "vote_count": 717,
          "media_type": "movie"
        }
      ],
      "name": "Al Pacino",
      "popularity": 40.7432826715669,
      "profile_path": "/lk0nt14sH96SYHDkX7qsLlpqRjJ.jpg"
    },
    {
      "adult": false,
      "id": 16828,
      "known_for": [
        {
          "adult": false,
          "backdrop_path": "/hbn46fQaRmlpBuUrEiFqv0GDL6Y.jpg",
          "id": 24428,
          "original_title": "The Avengers",
          "release_date": "2012-05-04",
          "poster_path": "/cezWGskPY5x7GaglTTRN4Fugfb8.jpg",
          "popularity": 7.48281616156757,
          "title": "The Avengers",
          "vote_average": 7.1,
          "vote_count": 6116,
          "media_type": "movie"
        },
        {
          "adult": false,
          "backdrop_path": "/pmZtj1FKvQqISS6iQbkiLg5TAsr.jpg",
          "id": 1771,
          "original_title": "Captain America: The First Avenger",
          "release_date": "2011-07-22",
          "poster_path": "/sBZs1jSybBRBXDwcCR8IOyHLUMc.jpg",
          "popularity": 5.029714568253,
          "title": "Captain America: The First Avenger",
          "vote_average": 6.3,
          "vote_count": 2932,
          "media_type": "movie"
        },
        {
          "adult": false,
          "backdrop_path": "/3FweBee0xZoY77uO1bhUOlQorNH.jpg",
          "id": 76338,
          "original_title": "Thor: The Dark World",
          "release_date": "2013-11-08",
          "poster_path": "/aROh4ZwLfv9tmtOAsrnkYTbpujA.jpg",
          "popularity": 7.93600134067616,
          "title": "Thor: The Dark World",
          "vote_average": 7.1,
          "vote_count": 960,
          "media_type": "movie"
        }
      ],
      "name": "Chris Evans",
      "popularity": 43.5069936697849,
      "profile_path": "/y1OTC2CbAesg8mkuBbyIOc2jSKG.jpg"
    },
    {
      "adult": false,
      "id": 517,
      "known_for": [
        {
          "adult": false,
          "backdrop_path": "/pVXLoxvvFBwK6DUh677dZhcMGRm.jpg",
          "id": 32657,
          "original_title": "Percy Jackson & the Olympians: The Lightning Thief",
          "release_date": "2010-02-11",
          "poster_path": "/px2u1SbfH2rxlC7VEdJagMorarY.jpg",
          "popularity": 2.40519944994428,
          "title": "Percy Jackson & the Olympians: The Lightning Thief",
          "vote_average": 6.0,
          "vote_count": 420,
          "media_type": "movie"
        },
        {
          "adult": false,
          "backdrop_path": "/uTODVVo2EDocnrtvdlOvQw1bO2R.jpg",
          "id": 107985,
          "original_title": "The World's End",
          "release_date": "2013-07-19",
          "poster_path": "/7xZUJMqGBLvhPJqn23UM3yUC5k5.jpg",
          "popularity": 2.41803790308758,
          "title": "The World's End",
          "vote_average": 6.8,
          "vote_count": 362,
          "media_type": "movie"
        },
        {
          "adult": false,
          "backdrop_path": "/aneVpr70Uxoo6Nk2QpZnG6DwC9E.jpg",
          "id": 710,
          "original_title": "GoldenEye",
          "release_date": "1995-11-23",
          "poster_path": "/qUsIUglvfAIEsRhcRLBKXj2ZvSP.jpg",
          "popularity": 4.26363072304691,
          "title": "GoldenEye",
          "vote_average": 6.1,
          "vote_count": 343,
          "media_type": "movie"
        }
      ],
      "name": "Pierce Brosnan",
      "popularity": 42.0077319530199,
      "profile_path": "/ihpZeNwh3Lexcy7oIeihRiSz6LW.jpg"
    },
    {
      "adult": false,
      "id": 1248,
      "known_for": [
        {
          "adult": false,
          "backdrop_path": "/vACyyzY1VZnHEl5AnzjLgDf6SRM.jpg",
          "id": 2501,
          "original_title": "The Bourne Identity",
          "release_date": "2002-06-06",
          "poster_path": "/bXQIL36VQdzJ69lcjQR1WQzJqQR.jpg",
          "popularity": 3.07966544785629,
          "title": "The Bourne Identity",
          "vote_average": 7.0,
          "vote_count": 1549,
          "media_type": "movie"
        },
        {
          "adult": false,
          "backdrop_path": "/r38utnwaWYRkWinxDrRyPviX81E.jpg",
          "id": 39514,
          "original_title": "RED",
          "release_date": "2010-10-14",
          "poster_path": "/q2mwTRKrq1etP9S4SZVDIJq0wI2.jpg",
          "popularity": 4.83898560893311,
          "title": "RED",
          "vote_average": 6.4,
          "vote_count": 1303,
          "media_type": "movie"
        },
        {
          "adult": false,
          "backdrop_path": "/caIpcr9xhvRcWfOMRDq3F5iIcLB.jpg",
          "id": 61791,
          "original_title": "Rise of the Planet of the Apes",
          "release_date": "2011-08-04",
          "poster_path": "/ddWSWgAjAhksNhMeeBTSqY6otIA.jpg",
          "popularity": 2.71741142805648,
          "title": "Rise of the Planet of the Apes",
          "vote_average": 6.6,
          "vote_count": 1297,
          "media_type": "movie"
        }
      ],
      "name": "Brian Cox",
      "popularity": 37.0299285307469,
      "profile_path": "/m15C58NWii5WCIg57Llr7hejnfy.jpg"
    },
    {
      "adult": false,
      "id": 2888,
      "known_for": [
        {
          "adult": false,
          "backdrop_path": "/7u3UyejCbhM3jXcZ86xzA9JJxge.jpg",
          "id": 41154,
          "original_title": "Men in Black 3",
          "release_date": "2012-05-25",
          "poster_path": "/l9hrvXyGq19f6jPRZhSVRibTMwW.jpg",
          "popularity": 2.89381652836306,
          "title": "Men in Black 3",
          "vote_average": 6.0,
          "vote_count": 1915,
          "media_type": "movie"
        },
        {
          "adult": false,
          "backdrop_path": "/u6Qg7TH7Oh1IFWCQSRr4htFFt0A.jpg",
          "id": 6479,
          "original_title": "I Am Legend",
          "release_date": "2007-12-13",
          "poster_path": "/pfvQ3kkSbFsIPC5exKPUf5nOf60.jpg",
          "popularity": 3.03104778467511,
          "title": "I Am Legend",
          "vote_average": 6.6,
          "vote_count": 1490,
          "media_type": "movie"
        },
        {
          "adult": false,
          "backdrop_path": "/o17MKb2hyuqtb0P6xLdNY4HaxDi.jpg",
          "id": 607,
          "original_title": "Men in Black",
          "release_date": "1997-07-02",
          "poster_path": "/f24UVKq3UiQWLqGWdqjwkzgB8j8.jpg",
          "popularity": 2.60295653040062,
          "title": "Men in Black",
          "vote_average": 6.5,
          "vote_count": 1469,
          "media_type": "movie"
        }
      ],
      "name": "Will Smith",
      "popularity": 39.1128507880643,
      "profile_path": "/2iYXDlCvLyVO49louRyDDXagZ0G.jpg"
    },
    {
      "adult": false,
      "id": 8691,
      "known_for": [
        {
          "adult": false,
          "backdrop_path": "/5XPPB44RQGfkBrbJxmtdndKz05n.jpg",
          "id": 19995,
          "original_title": "Avatar",
          "release_date": "2009-12-18",
          "poster_path": "/tcqb9NHdw9SWs2a88KCDD4V8sVR.jpg",
          "popularity": 8.92594518437743,
          "title": "Avatar",
          "vote_average": 6.9,
          "vote_count": 5708,
          "media_type": "movie"
        },
        {
          "adult": false,
          "backdrop_path": "/jpRpigNQUjNlfx0gYRBJ30tQIOl.jpg",
          "id": 22,
          "original_title": "Pirates of the Caribbean: The Curse of the Black Pearl",
          "release_date": "2003-07-09",
          "poster_path": "/mL3mGWeiANcAeyJQDwEdwpxQKYv.jpg",
          "popularity": 3.97737662538628,
          "title": "Pirates of the Caribbean: The Curse of the Black Pearl",
          "vote_average": 6.9,
          "vote_count": 2191,
          "media_type": "movie"
        },
        {
          "adult": false,
          "backdrop_path": "/1XOSh6BFZbQ0xN75m4avqgzClyG.jpg",
          "id": 13475,
          "original_title": "Star Trek",
          "release_date": "2009-05-07",
          "poster_path": "/fM0LKKgP0dtjNXQnAGA8WC1qr8h.jpg",
          "popularity": 3.58478285621886,
          "title": "Star Trek",
          "vote_average": 7.2,
          "vote_count": 2182,
          "media_type": "movie"
        }
      ],
      "name": "Zoe Saldana",
      "popularity": 40.8711241311097,
      "profile_path": "/ofNrWiA2KDdqiNxFTLp51HcXUlp.jpg"
    },
    {
      "adult": false,
      "id": 10912,
      "known_for": [
        {
          "adult": false,
          "backdrop_path": "/xq6hXdBpDPIXWjtmvbFmtLvBFJt.jpg",
          "id": 36557,
          "original_title": "Casino Royale",
          "release_date": "2006-11-15",
          "poster_path": "/toEaueEfDxX57y4J5MRRA7qsy5K.jpg",
          "popularity": 2.78622356486528,
          "title": "Casino Royale",
          "vote_average": 6.8,
          "vote_count": 1582,
          "media_type": "movie"
        },
        {
          "adult": false,
          "backdrop_path": "/thzGA2cOW37jCxJ7sJeBNGVGZKj.jpg",
          "id": 62213,
          "original_title": "Dark Shadows",
          "release_date": "2012-05-11",
          "poster_path": "/6125p54Jnog3kFsY33oMQF3d1dJ.jpg",
          "popularity": 2.77164788585798,
          "title": "Dark Shadows",
          "vote_average": 5.5,
          "vote_count": 617,
          "media_type": "movie"
        },
        {
          "adult": false,
          "backdrop_path": "/mnxWdWTP3jbfxC4oaPrwevwvOZ2.jpg",
          "id": 53182,
          "original_title": "300: Rise of an Empire",
          "release_date": "2014-03-07",
          "poster_path": "/bRE7nmNuZzciep7MDkNfuTrM9zW.jpg",
          "popularity": 8.63162236986269,
          "title": "300: Rise of an Empire",
          "vote_average": 6.3,
          "vote_count": 540,
          "media_type": "movie"
        }
      ],
      "name": "Eva Green",
      "popularity": 35.9371301111388,
      "profile_path": "/qXlDuXpDlx7JTxtB78qG9DgKIzz.jpg"
    },
    {
      "adult": false,
      "id": 5530,
      "known_for": [
        {
          "adult": false,
          "backdrop_path": "/39nstYsfjR6ggyKTtB4Joga2fs8.jpg",
          "id": 49538,
          "original_title": "X-Men: First Class",
          "release_date": "2011-05-24",
          "poster_path": "/l2cBQTfbVEWUXW8nKWCgq0ct7WU.jpg",
          "popularity": 0.275574209261602,
          "title": "X-Men: First Class",
          "vote_average": 6.8,
          "vote_count": 2308,
          "media_type": "movie"
        },
        {
          "adult": false,
          "backdrop_path": "/aBkkrhQS4rO3u1OTahywtSXu3It.jpg",
          "id": 127585,
          "original_title": "X-Men: Days of Future Past",
          "release_date": "2014-05-23",
          "poster_path": "/qKkFk9HELmABpcPoc1HHZGIxQ5a.jpg",
          "popularity": 39.4809224514038,
          "title": "X-Men: Days of Future Past",
          "vote_average": 7.9,
          "vote_count": 841,
          "media_type": "movie"
        },
        {
          "adult": false,
          "backdrop_path": "/2mbQF4SfdbpqF8LmIxDfJ9RDqQQ.jpg",
          "id": 8909,
          "original_title": "Wanted",
          "release_date": "2008-06-26",
          "poster_path": "/mUrkppyahzk6koYFekxeu0tmcPd.jpg",
          "popularity": 4.65505451285522,
          "title": "Wanted",
          "vote_average": 6.0,
          "vote_count": 570,
          "media_type": "movie"
        }
      ],
      "name": "James McAvoy",
      "popularity": 44.3262572705673,
      "profile_path": "/vlFpTsXSmyaAXHTj2aA4bf0jQnu.jpg"
    },
    {
      "adult": false,
      "id": 62,
      "known_for": [
        {
          "adult": false,
          "backdrop_path": "/9rZg1J6vMQoDVSgRyWcpJa8IAGy.jpg",
          "id": 680,
          "original_title": "Pulp Fiction",
          "release_date": "1994-10-14",
          "poster_path": "/dM2w364MScsjFf8pfMbaWUcWrR.jpg",
          "popularity": 4.39425223156719,
          "title": "Pulp Fiction",
          "vote_average": 7.6,
          "vote_count": 2760,
          "media_type": "movie"
        },
        {
          "adult": false,
          "backdrop_path": "/17zArExB7ztm6fjUXZwQWgGMC9f.jpg",
          "id": 47964,
          "original_title": "A Good Day to Die Hard",
          "release_date": "2013-02-14",
          "poster_path": "/c2SQMd00CCGTiDxGXVqA2J9lmzF.jpg",
          "popularity": 2.6499082381844,
          "title": "A Good Day to Die Hard",
          "vote_average": 5.2,
          "vote_count": 2402,
          "media_type": "movie"
        },
        {
          "adult": false,
          "backdrop_path": "/cZkPJ0noQvcR3oCCZ4pwYZeWUYi.jpg",
          "id": 59967,
          "original_title": "Looper",
          "release_date": "2012-09-28",
          "poster_path": "/o4qXZMrZlfuTfwwNGMyTcx6p2uO.jpg",
          "popularity": 5.92838504157016,
          "title": "Looper",
          "vote_average": 6.3,
          "vote_count": 2222,
          "media_type": "movie"
        }
      ],
      "name": "Bruce Willis",
      "popularity": 45.6693689838814,
      "profile_path": "/kI1OluWhLJk3pnR19VjOfABpnTY.jpg"
    },
    {
      "adult": false,
      "id": 5049,
      "known_for": [
        {
          "adult": false,
          "backdrop_path": "/uD93T339xX1k3fnDUaeopZBiajY.jpg",
          "id": 671,
          "original_title": "Harry Potter and the Philosopher's Stone",
          "release_date": "2001-11-14",
          "poster_path": "/uLGaJ9FgPWf7EUgwjp9RTmHemw8.jpg",
          "popularity": 3.28736944898314,
          "title": "Harry Potter and the Philosopher's Stone",
          "vote_average": 6.8,
          "vote_count": 2257,
          "media_type": "movie"
        },
        {
          "adult": false,
          "backdrop_path": "/oPmZDHPkdmhuvxYGmwtKcQefeNr.jpg",
          "id": 12445,
          "original_title": "Harry Potter and the Deathly Hallows: Part 2",
          "release_date": "2011-07-07",
          "poster_path": "/7xmtxRc9nQnCuWINuTT4SMP5NJc.jpg",
          "popularity": 3.40833094169758,
          "title": "Harry Potter and the Deathly Hallows: Part 2",
          "vote_average": 7.3,
          "vote_count": 1929,
          "media_type": "movie"
        },
        {
          "adult": false,
          "backdrop_path": "/17Pf2aGFfehYyz9Ru2S6fzcXvEO.jpg",
          "id": 12444,
          "original_title": "Harry Potter and the Deathly Hallows: Part 1",
          "release_date": "2010-11-19",
          "poster_path": "/6aEW544qOWoW2h3eQm2zqEzPISO.jpg",
          "popularity": 2.91179361186525,
          "title": "Harry Potter and the Deathly Hallows: Part 1",
          "vote_average": 7.0,
          "vote_count": 1729,
          "media_type": "movie"
        }
      ],
      "name": "John Hurt",
      "popularity": 35.0946954230207,
      "profile_path": "/rpuH2YRLpxJjMxHq4T1QdOSVtlN.jpg"
    },
    {
      "adult": false,
      "id": 8783,
      "known_for": [
        {
          "adult": false,
          "backdrop_path": "/1XOSh6BFZbQ0xN75m4avqgzClyG.jpg",
          "id": 13475,
          "original_title": "Star Trek",
          "release_date": "2009-05-07",
          "poster_path": "/fM0LKKgP0dtjNXQnAGA8WC1qr8h.jpg",
          "popularity": 3.58478285621886,
          "title": "Star Trek",
          "vote_average": 7.2,
          "vote_count": 2182,
          "media_type": "movie"
        },
        {
          "adult": false,
          "backdrop_path": "/n2vIGWw4ezslXjlP0VNxkp9wqwU.jpg",
          "id": 12,
          "original_title": "Finding Nemo",
          "release_date": "2003-05-30",
          "poster_path": "/zjqInUwldOBa0q07fOyohYCWxWX.jpg",
          "popularity": 3.91731047831794,
          "title": "Finding Nemo",
          "vote_average": 7.0,
          "vote_count": 1987,
          "media_type": "movie"
        },
        {
          "adult": false,
          "backdrop_path": "/lIyNUZbIeEwWpaWXAO5gnciB8Dq.jpg",
          "id": 652,
          "original_title": "Troy",
          "release_date": "2004-05-13",
          "poster_path": "/edMlij7nw2NMla32xskDnzMCFBM.jpg",
          "popularity": 5.14792228247903,
          "title": "Troy",
          "vote_average": 6.5,
          "vote_count": 592,
          "media_type": "movie"
        }
      ],
      "name": "Eric Bana",
      "popularity": 39.9002863391912,
      "profile_path": "/36As49OkTNvT96CzzjXNuKMeuyx.jpg"
    },
    {
      "adult": false,
      "id": 9827,
      "known_for": [
        {
          "adult": false,
          "backdrop_path": "/39nstYsfjR6ggyKTtB4Joga2fs8.jpg",
          "id": 49538,
          "original_title": "X-Men: First Class",
          "release_date": "2011-05-24",
          "poster_path": "/l2cBQTfbVEWUXW8nKWCgq0ct7WU.jpg",
          "popularity": 0.275574209261602,
          "title": "X-Men: First Class",
          "vote_average": 6.8,
          "vote_count": 2308,
          "media_type": "movie"
        },
        {
          "adult": false,
          "backdrop_path": "/560F7BPaxRy8BsOfVU6cW4ivM46.jpg",
          "id": 1894,
          "original_title": "Star Wars: Episode II - Attack of the Clones",
          "release_date": "2002-05-16",
          "poster_path": "/2vcNFtrZXNwIcBgH5e2xXCmVR8t.jpg",
          "popularity": 2.33154542308356,
          "title": "Star Wars: Episode II - Attack of the Clones",
          "vote_average": 6.3,
          "vote_count": 1173,
          "media_type": "movie"
        },
        {
          "adult": false,
          "backdrop_path": "/lIyNUZbIeEwWpaWXAO5gnciB8Dq.jpg",
          "id": 652,
          "original_title": "Troy",
          "release_date": "2004-05-13",
          "poster_path": "/edMlij7nw2NMla32xskDnzMCFBM.jpg",
          "popularity": 5.14792228247903,
          "title": "Troy",
          "vote_average": 6.5,
          "vote_count": 592,
          "media_type": "movie"
        }
      ],
      "name": "Rose Byrne",
      "popularity": 36.3227150972673,
      "profile_path": "/aw9tVvSVBNDiAia2QuvnlJQh8m0.jpg"
    }
  ],
  "total_pages": 23991,
  "total_results": 479804
}

Get the latest person id.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
</table>

GET /person/latest
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "d0db9614fd3971f3928f9f044a7202e0"
{
    "adult": false,
    "also_known_as": [],
    "biography": null,
    "birthday": null,
    "deathday": null,
    "homepage": null,
    "id": 1108200,
    "name": "Ted Koland",
    "place_of_birth": null,
    "profile_path": null
}

--
Reviews
--

Get the full details of a review by ID.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
</table>

GET /review/{id}
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "deb66ac826fe2ad09ee8043df27ed198"
{
  "id": "5013bc76760ee372cb00253e",
  "author": "Chris",
  "content": "I personally thought this film is on par if not better than the Dark Knight.\r\n\r\nWhilst some think the film is too long for the story I didn't find this. The length of the film is longer than some (but doesn't feel it), I liked that the film took it's time rather than shoving more elements in it - I think this contributed to the dramatic ending (much like a classical piece of music will be relaxed and calm before the final crescendo).\r\n\r\nAt the end of the day whether you like this film will boil down to if you like films Christopher Nolan has directed and/or you like the Christopher Nolan Batman series so far.\r\n\r\nStupendously good film in my opinion.",
  "iso_639_1": "en",
  "media_id": 49026,
  "media_title": "The Dark Knight Rises",
  "media_type": "Movie",
  "url": "http://j.mp/P18dg1"
}

--
Search
--

Search for companies by name.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td colspan="2">api_key</td></tr>
<tr><td style="width: 140px;">query</td><td>CGI escaped string</td></tr>
<tr><td colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-right: 40px; width: 140px;">page</td><td>Minimum 1, maximum 1000.</td></tr>
</table>

GET /search/company
> Accept: application/json
< 200
< Content-Type: application/json
{
    "page": 1,
    "results": [
        {
            "id": 34,
            "logo_path": "/56VlAu08MIE926dQNfBcUwTY8np.png",
            "name": "Sony Pictures"
        },
        {
            "id": 2251,
            "logo_path": null,
            "name": "Sony Pictures Animation"
        },
        {
            "id": 58,
            "logo_path": null,
            "name": "Sony Pictures Classics"
        },
        {
            "id": 5752,
            "logo_path": null,
            "name": "Sony Pictures Entertainment"
        },
        {
            "id": 7431,
            "logo_path": null,
            "name": "Sony Pictures Entertainment (SPE)"
        },
        {
            "id": 5388,
            "logo_path": null,
            "name": "Sony Pictures Home Entertainment"
        },
        {
            "id": 3045,
            "logo_path": null,
            "name": "Sony Pictures Releasing"
        },
        {
            "id": 8285,
            "logo_path": null,
            "name": "Sony Pictures Studio"
        }
    ],
    "total_pages": 1,
    "total_results": 8
}

Search for collections by name.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td colspan="2">api_key</td></tr>
<tr><td style="width: 140px;">query</td><td>CGI escaped string</td></tr>
<tr><td colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-right: 40px; width: 140px;">page</td><td>Minimum 1, maximum 1000.</td></tr>
<tr><td style="padding-right: 40px;">language</td><td>ISO 639-1 code.</td></tr>
</table>

GET /search/collection
> Accept: application/json
< 200
< Content-Type: application/json
{
    "page": 1,
    "results": [
        {
            "id": 86311,
            "backdrop_path": "/zuW6fOiusv4X9nnW3paHGfXcSll.jpg",
            "name": "The Avengers Collection",
            "poster_path": "/fMSxlk2zPpcXBn4R3TK3uqYjOpa.jpg"
        },
        {
            "id": 138966,
            "backdrop_path": null,
            "name": "The Toxic Avenger Series",
            "poster_path": null
        },
        {
            "id": 32135,
            "backdrop_path": null,
            "name": "The Toxic Avenger Collection",
            "poster_path": null
        },
        {
            "id": 14706,
            "backdrop_path": "/zE4wyiYkgqu7qsNXlsjckWfimYo.jpg",
            "name": "Ultimate Avengers Collection",
            "poster_path": "/s77dIqIExEslhZevh5ek3NvMg80.jpg"
        },
        {
            "id": 142748,
            "backdrop_path": null,
            "name": "The Avengers Collection: Phase 1",
            "poster_path": null
        }
    ],
    "total_pages": 1,
    "total_results": 5
}

Search for keywords by name.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td colspan="2">api_key</td></tr>
<tr><td style="width: 140px;">query</td><td>CGI escaped string</td></tr>
<tr><td colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-right: 40px; width: 140px;">page</td><td>Minimum 1, maximum 1000.</td></tr>
</table>

GET /search/keyword
> Accept: application/json
< 200
< Content-Type: application/json
{
    "page": 1,
    "results": [
        {
            "id": 1721,
            "name": "fight"
        },
        {
            "id": 6211,
            "name": "fighter"
        },
        {
            "id": 9878,
            "name": "fighter jet"
        },
        {
            "id": 2533,
            "name": "fighter pilot"
        },
        {
            "id": 12985,
            "name": "fighter jet the man"
        }
    ],
    "total_pages": 1,
    "total_results": 5
}

Search for lists by name and description.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td colspan="2">api_key</td></tr>
<tr><td style="width: 140px;">query</td><td>CGI escaped string</td></tr>
<tr><td colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-right: 40px; width: 140px;">page</td><td>Minimum 1, maximum 1000.</td></tr>
<tr><td style="padding-right: 40px;">include_adult</td><td>Toggle the inclusion of adult lists.</td></tr>
</table>

GET /search/list
> Accept: application/json
< 200
< Content-Type: application/json
{
    "page": 1,
    "results": [
        {
            "description": "A list of the films that were nominated at the 2012 Oscars for best picture.",
            "favorite_count": 1,
            "id": "50941340760ee35da9000053",
            "item_count": 9,
            "iso_639_1": "en",
            "list_type": "movie",
            "name": "84th Academy Awards (2012) - Best Picture Nominations",
            "poster_path": "/zRqBleU93WncYnIwt8LAanQerZ7.jpg"
        },
        {
            "description": "A list of the films that were nominated at the 2011 Oscars for best picture.",
            "favorite_count": 1,
            "id": "509431ad19c2950b01000003",
            "item_count": 10,
            "iso_639_1": "en",
            "list_type": "movie",
            "name": "83rd Academy Awards (2011) - Best Picture Nominations",
            "poster_path": "/iAHDZPyLD3NW7GuNV6U1TVtb0hN.jpg"
        },
        {
            "description": "A list of the films that were nominated at the 2010 Oscars for best picture.",
            "favorite_count": 2,
            "id": "50956fd2760ee3698a001fb0",
            "item_count": 10,
            "iso_639_1": "en",
            "list_type": "movie",
            "name": "82nd Academy Awards (2010) - Best Picture Nominations",
            "poster_path": "/8iwe0iP49A6Gqcv31jBleZDZqI4.jpg"
        },
        {
            "description": "A list of the films that were nominated at the 2009 Oscars for best picture.",
            "favorite_count": 2,
            "id": "50957064760ee3698a001fc0",
            "item_count": 5,
            "iso_639_1": "en",
            "list_type": "movie",
            "name": "81st Academy Awards (2009) - Best Picture Nominations",
            "poster_path": "/gShtwIvdSHgCEQRZfymNkEUEwFm.jpg"
        },
        {
            "description": "A list of the films that were nominated at the 2008 Oscars for best picture.",
            "favorite_count": 1,
            "id": "509ec007760ee36f0c000916",
            "item_count": 5,
            "iso_639_1": "en",
            "list_type": "movie",
            "name": "80th Academy Awards (2008) - Best Picture Nominations",
            "poster_path": "/nWwFabqFaBbukXdkUCmtnrFxPNh.jpg"
        },
        {
            "description": "Here's my list of best picture winners for the Oscars. Thought it would be neat to see them all together. There's a lot of movies here I have never even heard of.",
            "favorite_count": 1,
            "id": "509ec17b19c2950a0600050d",
            "item_count": 84,
            "iso_639_1": "en",
            "list_type": "movie",
            "name": "Best Picture Winners - The Academy Awards",
            "poster_path": "/efBm2Nm2v5kQnO0w3hYcW6hVsJU.jpg"
        }
    ],
    "total_pages": 1,
    "total_results": 6
}

Search for movies by title.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td colspan="2">api_key</td></tr>
<tr><td style="width: 140px;">query</td><td>CGI escaped string</td></tr>
<tr><td colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-right: 40px; width: 140px;">page</td><td>Minimum 1, maximum 1000.</td></tr>
<tr><td style="padding-right: 40px;">language</td><td>ISO 639-1 code.</td></tr>
<tr><td style="padding-right: 40px;">include_adult</td><td>Toggle the inclusion of adult titles. Expected value is: true or false</td></tr>
<tr><td style="padding-right: 40px;">year</td><td>Filter the results release dates to matches that include this value.</td></tr>
<tr><td style="padding-right: 40px;">primary_release_year</td><td>Filter the results so that only the primary release dates have this value.</td></tr>
</table>

GET /search/movie
> Accept: application/json
< 200
< Content-Type: application/json
{
  "page": 1,
  "results": [
    {
      "adult": false,
      "backdrop_path": "/8uO0gUM8aNqYLs1OsTBQiXu0fEv.jpg",
      "genre_ids": [
        18
      ],
      "id": 550,
      "original_language": "en",
      "original_title": "Fight Club",
      "overview": "A ticking-time-bomb insomniac and a slippery soap salesman channel primal male aggression into a shocking new form of therapy. Their concept catches on, with underground \"fight clubs\" forming in every town, until an eccentric gets in the way and ignites an out-of-control spiral toward oblivion.",
      "release_date": "1999-10-14",
      "poster_path": "/811DjJTon9gD6hZ8nCjSitaIXFQ.jpg",
      "popularity": 4.39844,
      "title": "Fight Club",
      "video": false,
      "vote_average": 7.8,
      "vote_count": 3527
    },
    {
      "adult": false,
      "backdrop_path": "/qrZssI8koUdRxkYnrOKMRY3m5Fq.jpg",
      "genre_ids": [
        27
      ],
      "id": 289732,
      "original_language": "zh",
      "original_title": "Zombie Fight Club",
      "overview": "It's the end of the century at a corner of the city in a building riddled with crime - Everyone in the building has turned into zombies. After Jenny's boyfriend is killed in a zombie attack, she faces the challenge of surviving in the face of adversity. In order to stay alive, she struggles with Andy to flee danger.",
      "release_date": "2014-10-23",
      "poster_path": "/7k9db7pJyTaVbz3G4eshGltivR1.jpg",
      "popularity": 1.621746,
      "title": "Zombie Fight Club",
      "video": false,
      "vote_average": 3.5,
      "vote_count": 2
    },
    {
      "adult": false,
      "backdrop_path": "/uywB4ZS6iCuc4aCIsAOyhW4sFZJ.jpg",
      "genre_ids": [
        28,
        80,
        18,
        53
      ],
      "id": 14476,
      "original_language": "en",
      "original_title": "Clubbed",
      "overview": "An underworld drama set in the early 1980s, about a lonely factory worker whose life is transformed when he becomes a nightclub doorman.",
      "release_date": "2009-01-16",
      "poster_path": "/mGR71N18bh26uwtAYZuiKOc79m9.jpg",
      "popularity": 0.306603,
      "title": "Clubbed",
      "video": false,
      "vote_average": 7.1,
      "vote_count": 15
    },
    {
      "adult": false,
      "backdrop_path": null,
      "genre_ids": [
        99
      ],
      "id": 259016,
      "original_language": "en",
      "original_title": "Insane Fight Club",
      "overview": "A group of friends have created a brand new subculture that is taking over the streets of Glasgow. They've established their very own fight club, but this is no ordinary wrestling event - this is brutal, riotous chaos. Fights don't always stay inside the ring, people are bounced off the side of buses and thrown off balconies in pubs. They now plan the biggest show of their lives. The stakes are high, will it bring them the fame and recognition they need to survive?",
      "release_date": "2014-03-11",
      "poster_path": "/mLhwBQPV3iATe3L61kbpmxANwL8.jpg",
      "popularity": 1.005463,
      "title": "Insane Fight Club",
      "video": false,
      "vote_average": 0.0,
      "vote_count": 0
    },
    {
      "adult": false,
      "backdrop_path": "/5sWZRCsjzH1Dxo6gSBeX4RpAQ4p.jpg",
      "genre_ids": [
        28,
        18,
        80
      ],
      "id": 51021,
      "original_language": "en",
      "original_title": "Lure: Teen Fight Club",
      "overview": "A community is under siege as three Belmont Highschool coed students go missing with no trace of their whereabouts. The pressure is on the police to capture the culprits responsible. Scouring the school hallways in search of clues, undercover female detective Maggie Rawdon (Jessica Sonnerborn) enters Belmont High as a transfer student in an attempt to solve the hideous disappearance of the students. Maggie makes a few new friends, and gets invited to a private rave in the country. Just as the group begins to suspect that they've taken a wrong turn, however, the trap is sprung and Maggie finds out firsthand what fate has befallen the missing girls.",
      "release_date": "2010-11-16",
      "poster_path": "/aRTX5Y52yGbVL6TGnyI4E8jjtz4.jpg",
      "popularity": 1.002197,
      "title": "Lure: Teen Fight Club",
      "video": false,
      "vote_average": 2.0,
      "vote_count": 1
    },
    {
      "adult": false,
      "backdrop_path": "/tcoAGvTo96R7Y9ZGVCCz7BZvrvb.jpg",
      "genre_ids": [
        28,
        53
      ],
      "id": 104782,
      "original_language": "en",
      "original_title": "Florence Fight Club",
      "overview": "Four men decided to enter in the oldest Fight Club of the History, The Florentine Football tournament. A father and son, a black guy, an old champion and outsider clerk will enter in an arena of the time to win their fears, to go over their limits, to be heroes for a day.",
      "release_date": "2010-01-01",
      "poster_path": "/eQqqu0srTYcclWqylvgpLyU87hV.jpg",
      "popularity": 1.00115,
      "title": "Florence Fight Club",
      "video": false,
      "vote_average": 0.0,
      "vote_count": 0
    },
    {
      "adult": false,
      "backdrop_path": null,
      "genre_ids": [],
      "id": 289100,
      "original_language": "en",
      "original_title": "Girls Fight Club",
      "overview": "The best women's wrestling competition of all time...and if you think it's fake you're in for a big surprise See LEGENDARY Mixed Martial Arts fighters coach their teams to victory in the cage! aka Chuck Lidell's Girl's Fight Club",
      "release_date": "2009-06-18",
      "poster_path": null,
      "popularity": 1.000857,
      "title": "Girls Fight Club",
      "video": false,
      "vote_average": 0.0,
      "vote_count": 0
    },
    {
      "adult": false,
      "backdrop_path": null,
      "genre_ids": [
        99
      ],
      "id": 151912,
      "original_language": "de",
      "original_title": "Jurassic Fight Club",
      "overview": "Jurassic Fight Club, a paleontology-based miniseries that ran for 12 episodes, depicts how prehistoric beasts hunted their prey, dissecting these battles and uncovering a predatory world far more calculated and complex than originally thought. It was hosted by George Blasing, a self-taught paleontologist.",
      "release_date": "2008-10-22",
      "poster_path": "/AwECEjjen4eYSDZ3AETXnFG6dgu.jpg",
      "popularity": 1.000717,
      "title": "Jurassic Fight Club",
      "video": false,
      "vote_average": 0.0,
      "vote_count": 0
    },
    {
      "adult": false,
      "backdrop_path": null,
      "genre_ids": [],
      "id": 295477,
      "original_language": "da",
      "original_title": "Fight club camp kusse",
      "overview": null,
      "release_date": "2005-08-12",
      "poster_path": "/5itVi2OcKAkTUK0xtVxueKURb1W.jpg",
      "popularity": 1.000429,
      "title": "Fight club camp kusse",
      "video": false,
      "vote_average": 0.0,
      "vote_count": 0
    },
    {
      "adult": false,
      "backdrop_path": null,
      "genre_ids": [
        99
      ],
      "id": 209599,
      "original_language": "en",
      "original_title": "Brooklyn Girls Fight Club",
      "overview": "From the birthplace of boxing legend Mike Tyson, young women brawl in secret fight clubs to win $1000 and invaluable street cred.",
      "release_date": null,
      "poster_path": "/luWpP5WSw9JjbWS1J4BMnjkkJCX.jpg",
      "popularity": 1.0,
      "title": "Brooklyn Girls Fight Club",
      "video": false,
      "vote_average": 3.5,
      "vote_count": 1
    },
    {
      "adult": false,
      "backdrop_path": "/rsv46SYFz7SxWVbC48uV26Xu1x9.jpg",
      "genre_ids": [
        28,
        18,
        80
      ],
      "id": 219897,
      "original_language": "de",
      "original_title": "Barrio Brawler",
      "overview": "A martial arts instructor must enter the world of illegal pit fighting in order to save his family and his dojo.",
      "release_date": "2013-08-27",
      "poster_path": "/8AyCFlw1d856UAAN21fnWOpQu4l.jpg",
      "popularity": 1.000122,
      "title": "Barrio Brawler",
      "video": false,
      "vote_average": 1.5,
      "vote_count": 2
    },
    {
      "adult": false,
      "backdrop_path": null,
      "genre_ids": [
        99
      ],
      "id": 322772,
      "original_language": "en",
      "original_title": "Insane Fight Club II - This Time It’s Personal",
      "overview": "Insane Fight Club is back. This year the boys are taking their unique form of entertainment to England as they stage fight nights in Birmingham, Leeds, Liverpool and Newcastle.",
      "release_date": "2015-01-21",
      "poster_path": "/84mcV9ky4TSpD7dpHnsx7yBVs64.jpg",
      "popularity": 1.019699,
      "title": "Insane Fight Club II - This Time It’s Personal",
      "video": false,
      "vote_average": 0.0,
      "vote_count": 0
    },
    {
      "adult": false,
      "backdrop_path": null,
      "genre_ids": [],
      "id": 115584,
      "original_language": "en",
      "original_title": "Fight Club – The “I am Jack’s Laryngitis” Edit",
      "overview": "Edward Norton’s voiceover has been completely removed, making this a much more introspective and “arty” film. I believe it puts you even MORE inside the head of the main character. It’s kind of hard to explain, really. You’ll have to check it out.\r The narration is completely in the center channel of the 5.1 mix, so I simply faded out any instance of it. I had to use the soundtrack to fill in a few musical holes, had to do a bit of foley work (new audio for the Ron Popeil infomercial on TV, whistling as Jack is leaving his job with the shopping cart), and had to trim a second or two off a couple of shots (very minimal and unintrusive).",
      "release_date": null,
      "poster_path": null,
      "popularity": 1.0,
      "title": "Fight Club – The “I am Jack’s Laryngitis” Edit",
      "video": false,
      "vote_average": 0.0,
      "vote_count": 0
    }
  ],
  "total_pages": 1,
  "total_results": 13
}

Search the movie, tv show and person collections with a single query. Each item returned in the result array has a `media_type` field that maps to either movie, tv or person.

Each mapped result is the same response you would get from each independent search.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td colspan="2">api_key</td></tr>
<tr><td style="width: 140px;">query</td><td>CGI escaped string</td></tr>
<tr><td colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-right: 40px; width: 140px;">page</td><td>Minimum 1, maximum 1000.</td></tr>
<tr><td style="padding-right: 40px;">language</td><td>ISO 639-1 code.</td></tr>
<tr><td style="padding-right: 40px;">include_adult</td><td>Toggle the inclusion of adult titles. Expected value is: true or false</td></tr>
</table>

GET /search/multi
> Accept: application/json
< 200
< Content-Type: application/json
{
  "page": 1,
  "results": [
    {
      "backdrop_path": "/eSzpy96DwBujGFj0xMbXBcGcfxX.jpg",
      "first_air_date": "2008-01-19",
      "genre_ids": [
        18
      ],
      "id": 1396,
      "original_language": "en",
      "original_name": "Breaking Bad",
      "overview": "Breaking Bad is an American crime drama television series created and produced by Vince Gilligan. Set and produced in Albuquerque, New Mexico, Breaking Bad is the story of Walter White, a struggling high school chemistry teacher who is diagnosed with inoperable lung cancer at the beginning of the series. He turns to a life of crime, producing and selling methamphetamine, in order to secure his family's financial future before he dies, teaming with his former student, Jesse Pinkman. Heavily serialized, the series is known for positioning its characters in seemingly inextricable corners and has been labeled a contemporary western by its creator.",
      "origin_country": [
        "US"
      ],
      "poster_path": "/4yMXf3DW6oCL0lVPZaZM2GypgwE.jpg",
      "popularity": 18.095686,
      "name": "Breaking Bad",
      "vote_average": 8.9,
      "vote_count": 245,
      "media_type": "tv"
    },
    {
      "backdrop_path": "/amUqExzg5eutOHshr0mbfs3lgbj.jpg",
      "first_air_date": "2011-04-06",
      "genre_ids": [
        35,
        10759
      ],
      "id": 34541,
      "original_language": "en",
      "original_name": "Breaking In",
      "overview": "Breaking In is an American sitcom television series, which ran on Fox from April 6, 2011 to April 3, 2012. The series debuted as a midseason replacement following American Idol.\n\nInitially, Fox cancelled the series in May 2011, however three months later TV Guide announced that Breaking In had been renewed for a second season",
      "origin_country": [
        "US"
      ],
      "poster_path": "/m3SRLQjRgcdkgbLW02ZGerd9C9c.jpg",
      "popularity": 1.279647,
      "name": "Breaking In",
      "vote_average": 3.5,
      "vote_count": 2,
      "media_type": "tv"
    },
    {
      "adult": false,
      "backdrop_path": "/iIvEq8vYFK1edMeLMdJFHVYHoHY.jpg",
      "genre_ids": [
        27,
        35
      ],
      "id": 80384,
      "original_language": "en",
      "original_title": "Breaking Wind",
      "overview": "A comedic spoof based on the worldwide phenomenon \"The Twilight Saga.\"",
      "release_date": "2012-03-27",
      "poster_path": "/jFpKrtmK9zeYwC6Vc2AxV39E0Sb.jpg",
      "popularity": 1.590522,
      "title": "Breaking Wind",
      "video": false,
      "vote_average": 4.9,
      "vote_count": 22,
      "media_type": "movie"
    },
    {
      "adult": false,
      "backdrop_path": "/mMKahLSpwb9Yj2B0tB6vku3tkGy.jpg",
      "genre_ids": [
        99
      ],
      "id": 239459,
      "original_language": "en",
      "original_title": "No Half Measures: Creating the Final Season of Breaking Bad",
      "overview": "A documentary about the making of season five of the acclaimed AMC series Breaking Bad.",
      "release_date": "2013-11-26",
      "poster_path": "/8OixSR45U5dbqv8F0tlspmTbXxN.jpg",
      "popularity": 0.060672,
      "title": "No Half Measures: Creating the Final Season of Breaking Bad",
      "video": false,
      "vote_average": 8.1,
      "vote_count": 22,
      "media_type": "movie"
    },
    {
      "adult": false,
      "backdrop_path": null,
      "genre_ids": [
        18
      ],
      "id": 288285,
      "original_language": "en",
      "original_title": "Breaking Free",
      "overview": "When troubled, cynical teen Rick chooses service at a camp for the blind over serving time at a correctional facility, he thinks he's found the easy way out. Instead, it's the way to a new life. Friendship. Step by step, as Rick helps a blinded Gymnast rediscover the joys of competition through equestrian show jumping, they begin to take control of the most important journey of all... a journey called life.",
      "release_date": "1995-09-02",
      "poster_path": "/5uc3PJISEptFQ8wshzArI24QmKH.jpg",
      "popularity": 0.000942,
      "title": "Breaking Free",
      "video": false,
      "vote_average": 0.0,
      "vote_count": 0,
      "media_type": "movie"
    },
    {
      "adult": false,
      "backdrop_path": "/xzkuNJxaXaNxWnSAWpWxvrwRxEh.jpg",
      "genre_ids": [
        18,
        10749
      ],
      "id": 1253,
      "original_language": "en",
      "original_title": "Breaking and Entering",
      "overview": "Set in a blighted, inner-city neighbourhood of London, Breaking and Entering examines an affair which unfolds between a successful British landscape architect and Amira, a Bosnian woman – the mother of a troubled teen son – who was widowed by the war in Bosnia and Herzegovina.",
      "release_date": "2006-12-15",
      "poster_path": "/tH4pFx2e8oxTKTn1DrZv8eqJACn.jpg",
      "popularity": 0.402758,
      "title": "Breaking and Entering",
      "video": false,
      "vote_average": 6.3,
      "vote_count": 15,
      "media_type": "movie"
    },
    {
      "adult": false,
      "backdrop_path": "/wvuvEkNmxvDqCdJ21FSQIBYx4Jt.jpg",
      "genre_ids": [
        28,
        35,
        18,
        53,
        10769
      ],
      "id": 12543,
      "original_language": "en",
      "original_title": "Dai si gin",
      "overview": "When an ambulatory TV news unit live broadcasts the embarrassing defeat of a police battalion by five bank robbers in a ballistic showdown, the credibility of the police force drops to a nadir. While on a separate investigation in a run-down building, detective Cheung discovers the hideout of the robbers. Cheung and his men have also entered the building, getting ready to take their foes out any minute. Meanwhile, in order to beat the media at its own game, Inspector Rebecca decides to turn the stakeout into a breaking news show.",
      "release_date": "2004-01-01",
      "poster_path": "/e1QFV4vJJbPmpWuXqISySVjSAhB.jpg",
      "popularity": 0.207893,
      "title": "Breaking News",
      "video": false,
      "vote_average": 5.8,
      "vote_count": 6,
      "media_type": "movie"
    },
    {
      "backdrop_path": "/3lMh5BPOyI7ntGp6Qc4qQz5DhAm.jpg",
      "first_air_date": "2012-09-09",
      "genre_ids": [
        99
      ],
      "id": 45754,
      "original_language": "en",
      "original_name": "Breaking Amish",
      "overview": "Breaking Amish, the second season of which carries a subtitle as Breaking Amish: Los Angeles, is an American reality television series on TLC that debuted September 9, 2012. The series revolves around five young Anabaptist adults who move to New York City in order to experience a different life and to make a difficult decision regarding whether to return to their communities, or to remain outside their old communities and face potential ostracism by their family and friends. It follows the cast members as they experience life in New York and face new situations involving work, friendship, romance, and lifestyle, plus the drama that develops between cast members as they undergo various experiences.\n\nThe cast-members' move to New York City differs from Rumspringa, the rite of passage in which some 16-year-old Amish are allowed to experience the outside world and to decide whether or not they wish to remain with their home communities.\n\nIn February 2013, TLC ordered a second season of Breaking Amish. On April 3, 2013, TLC confirmed this was not a second season but a spinoff, Breaking Amish: Brave New World, starring the original cast members starting their lives in Sarasota, Florida, home to a large ex-Amish and Amish community. The second season, subtitled as Breaking Amish: Los Angeles, premiered on July 21, 2013.",
      "origin_country": [
        "US"
      ],
      "poster_path": "/ivpf0UixdgdLREbm3OBe8AIvQSp.jpg",
      "popularity": 1.056051,
      "name": "Breaking Amish",
      "vote_average": 0.0,
      "vote_count": 0,
      "media_type": "tv"
    },
    {
      "adult": false,
      "backdrop_path": "/ivb9YppjIFAokjjaTNpiglNLAGw.jpg",
      "genre_ids": [
        35,
        18,
        10749
      ],
      "id": 26393,
      "original_language": "en",
      "original_title": "Breaking Up",
      "overview": "An aloof, struggling food photographer thinks he has found true love with a fiery grade-school teacher. At first, the relationship is all wine and roses, but as they realize they have little in common besides great sex, the romance wanes, and they struggle through a succession of break-ups and reunions as they try to work things out.",
      "release_date": "1997-10-17",
      "poster_path": "/sHRjxdEBaqxBUaT7Jzfd39YNAxb.jpg",
      "popularity": 1.272862,
      "title": "Breaking Up",
      "video": false,
      "vote_average": 5.4,
      "vote_count": 2,
      "media_type": "movie"
    },
    {
      "backdrop_path": null,
      "first_air_date": "1963-09-16",
      "genre_ids": [
        18
      ],
      "id": 16377,
      "original_language": "en",
      "original_name": "Breaking Point",
      "overview": "Breaking Point is an American medical drama that aired on ABC from September 16, 1963, to April 27, 1964, continuing in rebroadcasts until September 7. The series, which was a spinoff of Ben Casey, stars Paul Richards and Eduard Franz. The series was created by Meta Rosenberg.",
      "origin_country": [
        "US"
      ],
      "poster_path": null,
      "popularity": 1.049332,
      "name": "Breaking Point",
      "vote_average": 0.0,
      "vote_count": 0,
      "media_type": "tv"
    },
    {
      "backdrop_path": null,
      "first_air_date": null,
      "genre_ids": [],
      "id": 50910,
      "original_language": "en",
      "original_name": "Breaking News",
      "overview": null,
      "origin_country": [],
      "poster_path": null,
      "popularity": 1.00002,
      "name": "Breaking News",
      "vote_average": 0.0,
      "vote_count": 0,
      "media_type": "tv"
    },
    {
      "adult": false,
      "backdrop_path": "/aH25DYXwoWMI2Wj9cQtkLk3Ax4T.jpg",
      "genre_ids": [
        35,
        18
      ],
      "id": 20283,
      "original_language": "en",
      "original_title": "Breaking Away",
      "overview": "Dave, nineteen, has just graduated high school, with his 3 friends, The comical Cyril, the warm hearted but short-tempered Moocher, and the athletic, spiteful but good-hearted Mike. Now, Dave enjoys racing bikes and hopes to race the Italians one day, and even takes up the Italian culture, much to his friends and parents annoyance.",
      "release_date": "1979-07-20",
      "poster_path": "/sCuhny2y7vbquI9uRESpKCBzfgM.jpg",
      "popularity": 0.184963,
      "title": "Breaking Away",
      "video": false,
      "vote_average": 7.3,
      "vote_count": 15,
      "media_type": "movie"
    },
    {
      "adult": false,
      "backdrop_path": "/v6mbkSCQe8m3LgQANlKXxsAacNX.jpg",
      "genre_ids": [
        28,
        12,
        18,
        53
      ],
      "id": 32005,
      "original_language": "en",
      "original_title": "Breaking Point",
      "overview": "\"Breaking Point\" is a dramatic tale of corruption and self-realization, in which one man has to overcome a deep seeded conspiracy and his own lingering past in order to gain the redemption he desires.",
      "release_date": "2009-01-01",
      "poster_path": "/MKYsuNDyzoAyy0nLRxkbvPJoVY.jpg",
      "popularity": 0.139665,
      "title": "Breaking Point",
      "video": false,
      "vote_average": 3.8,
      "vote_count": 3,
      "media_type": "movie"
    },
    {
      "adult": false,
      "backdrop_path": "/3o9s6CeNRNuJNsQ8dSKG0AeesYb.jpg",
      "genre_ids": [
        18
      ],
      "id": 65442,
      "original_language": "de",
      "original_title": "Auftauchen",
      "overview": "Having just landed a gig with a trendy magazine, young photographer Nadja celebrates at the local disco, where she bumps into Darius, a 20-year-old skateboarder. Although her frankness at first scares him, the two quickly become a couple, and as their intoxicating affair takes off, they seem like passionate soul mates. Soon, however, career demands and emotional intensity gradually put a strain on their rapport, and things turn even more complicated...",
      "release_date": "2006-10-25",
      "poster_path": "/lVGYuffa9GqvPddYIg6C1u62VKg.jpg",
      "popularity": 1.098577,
      "title": "Breaking the Surface",
      "video": false,
      "vote_average": 3.5,
      "vote_count": 1,
      "media_type": "movie"
    },
    {
      "adult": false,
      "backdrop_path": null,
      "genre_ids": [],
      "id": 214351,
      "original_language": "en",
      "original_title": "Breaking Loose",
      "overview": "Four best friends. They've been to war, they've seen life, but they are still passionate, young and willing to serve their country. They found their place at OMON - Russian Special Police Squad. They can count only on each other, and their real family is themselves. After working hours they become simple guys with ordinary dreams like family and comfort. But life changes when once in a night club the friends incidentally get in a fight with the local mafia. It turns out that they have no fear and they are ready to stand for each other whatever happens.",
      "release_date": "2014-05-08",
      "poster_path": "/5jZWisKO0K9GgWbMBIkW27K3ldv.jpg",
      "popularity": 1.013158,
      "title": "Breaking Loose",
      "video": false,
      "vote_average": 0.0,
      "vote_count": 0,
      "media_type": "movie"
    },
    {
      "adult": false,
      "backdrop_path": null,
      "genre_ids": [],
      "id": 306326,
      "original_language": "en",
      "original_title": "Breaking Barriers",
      "overview": "Breaking Barriers demonstrates what it takes to build and drive a vehicle faster than anyone on the planet. Follow American automobile entrepreneur John Hennessey's pursuit of the production vehicle land speed record and trace the roots of land speed racing from the early days of hot rodding to today. We discover that since the advent of the automobile, the pursuit of speed and the fight for the title of \"fastest car\" are intrinsically connected.",
      "release_date": "2014-05-07",
      "poster_path": null,
      "popularity": 1.005699,
      "title": "Breaking Barriers",
      "video": false,
      "vote_average": 0.0,
      "vote_count": 0,
      "media_type": "movie"
    },
    {
      "adult": false,
      "backdrop_path": "/2IFEvbvveOT5Xx15etROmMXAzyV.jpg",
      "genre_ids": [],
      "id": 303686,
      "original_language": "en",
      "original_title": "Breaking Trail",
      "overview": "Last season Utah’s Wasatch Mountains and most of North America experienced a record breaking doozie, making our never ending mission to flatten as many snowflakes as possible almost easy. We estimate the number of crystals crushed to be somewhere in the trillions. If you’ve had the\r extreme pleasure of viewing one of our previous films, then the obscenedisplay of exploding snow will be nothing new. On closer inspection you will notice that Powderwhore is no longer focusing primarily on the telemark turn. BREAKING TRAIL will highlight riders of all disciplines choosing their own backcountry adventures. Warning! There are no shots of helicopters filming other helicopters or hankie-clad 16-year-olds hepped up on energy drinks spinning to rap music. And you won’t win a Jeep if you come out to a premiere. You will find a mixed bag of highly talented and dedicated\r individuals who enjoy hiking out into the unknown in search of turns and adventure.",
      "release_date": "2011-11-10",
      "poster_path": "/qGtPVHThCZcsinhchTjdIAFQrrJ.jpg",
      "popularity": 1.002161,
      "title": "Breaking Trail",
      "video": false,
      "vote_average": 0.0,
      "vote_count": 0,
      "media_type": "movie"
    },
    {
      "adult": false,
      "backdrop_path": null,
      "genre_ids": [],
      "id": 149227,
      "original_language": "en",
      "original_title": "Breaking Man",
      "overview": "Pastor Sean seems to have everything, but inside his life is falling apart. His wife is leaving, he loses his job & has quit believing in God. As his life spirals out of control he is faced with the option of standing strong or breaking.",
      "release_date": "2012-02-07",
      "poster_path": "/hNtX67PNhhbfVDYDcTviDaEecRb.jpg",
      "popularity": 1.002087,
      "title": "Breaking Man",
      "video": false,
      "vote_average": 0.0,
      "vote_count": 0,
      "media_type": "movie"
    },
    {
      "adult": false,
      "backdrop_path": "/7L6wIguZewE9FhqOUoPPZ7jaMdV.jpg",
      "genre_ids": [
        53
      ],
      "id": 32707,
      "original_language": "en",
      "original_title": "Breaking Dawn",
      "overview": "Within the confines of an insane asylum languish many would-be seers and messiahs. Medical students meet them day in, day out. But when Eve meets Don Wake, she discovers he's not your run-of-the-mill paranoic. In fact, his visions of her life on the outside tread the line between fantasy and reality as a stalker becomes known to her. She could be in danger; it could all be in her head.",
      "release_date": "2005-02-02",
      "poster_path": "/cysO5EwGbNFJWzETY0u8YBBomWj.jpg",
      "popularity": 0.00179,
      "title": "Breaking Dawn",
      "video": false,
      "vote_average": 0.0,
      "vote_count": 1,
      "media_type": "movie"
    },
    {
      "adult": false,
      "backdrop_path": null,
      "genre_ids": [],
      "id": 332501,
      "original_language": "en",
      "original_title": "Breaking Night",
      "overview": "As night gives way to morning, GIRL breaks free becoming WOMAN. Yolonda Ross: director.",
      "release_date": "2012-01-01",
      "poster_path": null,
      "popularity": 1.001721,
      "title": "Breaking Night",
      "video": false,
      "vote_average": 0.0,
      "vote_count": 0,
      "media_type": "movie"
    }
  ],
  "total_pages": 7,
  "total_results": 134
}

Search for people by name.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td colspan="2">api_key</td></tr>
<tr><td style="width: 140px;">query</td><td>CGI escaped string</td></tr>
<tr><td colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-right: 40px; width: 140px;">page</td><td>Minimum 1, maximum 1000.</td></tr>
<tr><td style="padding-right: 40px;">include_adult</td><td>Toggle the inclusion of adult titles. Expected value is: true or false</td></tr>
<tr><td style="padding-right: 40px;">search_type</td><td>By default, the search type is 'phrase'. This is almost guaranteed the option you will want. It's a great all purpose search type and by far the most tuned for every day querying. For those wanting more of an "autocomplete" type search, set this option to 'ngram'.</td></tr>
</table>

GET /search/person
> Accept: application/json
< 200
< Content-Type: application/json
{
  "page": 1,
  "results": [
    {
      "adult": false,
      "id": 287,
      "known_for": [
        {
          "adult": false,
          "backdrop_path": "/8uO0gUM8aNqYLs1OsTBQiXu0fEv.jpg",
          "genre_ids": [
            18
          ],
          "id": 550,
          "original_language": "en",
          "original_title": "Fight Club",
          "overview": "A ticking-time-bomb insomniac and a slippery soap salesman channel primal male aggression into a shocking new form of therapy. Their concept catches on, with underground \"fight clubs\" forming in every town, until an eccentric gets in the way and ignites an out-of-control spiral toward oblivion.",
          "release_date": "1999-10-14",
          "poster_path": "/811DjJTon9gD6hZ8nCjSitaIXFQ.jpg",
          "popularity": 3.146334,
          "title": "Fight Club",
          "video": false,
          "vote_average": 7.8,
          "vote_count": 3526,
          "media_type": "movie"
        },
        {
          "adult": false,
          "backdrop_path": "/xMOQVYLeIKBXenJ9KMeasj7S64y.jpg",
          "genre_ids": [
            28,
            18,
            27,
            878,
            53
          ],
          "id": 72190,
          "original_language": "en",
          "original_title": "World War Z",
          "overview": "Life for former United Nations investigator Gerry Lane and his family seems content. Suddenly, the world is plagued by a mysterious infection turning whole human populations into rampaging mindless zombies. After barely escaping the chaos, Lane is persuaded to go on a mission to investigate this disease. What follows is a perilous trek around the world where Lane must brave horrific dangers and long odds to find answers before human civilization falls.",
          "release_date": "2013-06-21",
          "poster_path": "/gAt1PrsrFY1nX6UzebeiHP8njE9.jpg",
          "popularity": 2.613384,
          "title": "World War Z",
          "video": false,
          "vote_average": 6.8,
          "vote_count": 2236,
          "media_type": "movie"
        },
        {
          "adult": false,
          "backdrop_path": "/7nF6B9yCEq1ZCT82sGJVtNxOcl5.jpg",
          "genre_ids": [
            18,
            28,
            53,
            10752
          ],
          "id": 16869,
          "original_language": "en",
          "original_title": "Inglourious Basterds",
          "overview": "In Nazi-occupied France during World War II, a group of Jewish-American soldiers known as \"The Basterds\" are chosen specifically to spread fear throughout the Third Reich by scalping and brutally killing Nazis. The Basterds, lead by Lt. Aldo Raine soon cross paths with a French-Jewish teenage girl who runs a movie theater in Paris which is targeted by the soldiers.",
          "release_date": "2009-08-21",
          "poster_path": "/vDwqPyhkzFPRDmwz9KbzN2ouEPe.jpg",
          "popularity": 4.175311,
          "title": "Inglourious Basterds",
          "video": false,
          "vote_average": 7.5,
          "vote_count": 2087,
          "media_type": "movie"
        }
      ],
      "name": "Brad Pitt",
      "popularity": 8.357117,
      "profile_path": "/kc3M04QQAuZ9woUvH3Ju5T7ZqG5.jpg"
    },
    {
      "adult": false,
      "id": 1370,
      "known_for": [
        {
          "adult": false,
          "backdrop_path": "/dG4BmM32XJmKiwopLDQmvXEhuHB.jpg",
          "genre_ids": [
            12,
            14,
            28
          ],
          "id": 121,
          "original_language": "en",
          "original_title": "The Lord of the Rings: The Two Towers",
          "overview": "Frodo and Sam are trekking to Mordor to destroy the One Ring of Power while Gimli, Legolas and Aragorn search for the orc-captured Merry and Pippin. All along, nefarious wizard Saruman awaits the Fellowship members at the Orthanc Tower in Isengard.",
          "release_date": "2002-12-18",
          "poster_path": "/5o5fv1dHG7vWoH2hmqwihVPBoBm.jpg",
          "popularity": 4.383898,
          "title": "The Lord of the Rings: The Two Towers",
          "video": false,
          "vote_average": 7.6,
          "vote_count": 3440,
          "media_type": "movie"
        },
        {
          "adult": false,
          "backdrop_path": "/zFbKo9V1SVzndc8mydi92txe1jg.jpg",
          "genre_ids": [
            18
          ],
          "id": 510,
          "original_language": "en",
          "original_title": "One Flew Over the Cuckoo's Nest",
          "overview": "While serving time for insanity at a state mental hospital, implacable rabble-rouser Randle Patrick McMurphy inspires his fellow patients to rebel against the authoritarian rule of head nurse Mildred Ratched.",
          "release_date": "1975-11-18",
          "poster_path": "/2Sns5oMb356JNdBHgBETjIpRYy9.jpg",
          "popularity": 1.224917,
          "title": "One Flew Over the Cuckoo's Nest",
          "video": false,
          "vote_average": 7.8,
          "vote_count": 819,
          "media_type": "movie"
        },
        {
          "adult": false,
          "backdrop_path": "/kFslzsvxOQgjbuDfta3kzbOWQY5.jpg",
          "genre_ids": [
            28,
            27,
            878
          ],
          "id": 8078,
          "original_language": "en",
          "original_title": "Alien: Resurrection",
          "overview": "Two hundred years after Lt. Ripley died, a group of scientists clone her, hoping to breed the ultimate weapon. But the new Ripley is full of surprises … as are the new aliens. Ripley must team with a band of smugglers to keep the creatures from reaching Earth.",
          "release_date": "1997-11-26",
          "poster_path": "/ve2P64a9kzd7M78kfeaEzBEIEOR.jpg",
          "popularity": 1.751091,
          "title": "Alien: Resurrection",
          "video": false,
          "vote_average": 5.9,
          "vote_count": 419,
          "media_type": "movie"
        }
      ],
      "name": "Brad Dourif",
      "popularity": 4.265064,
      "profile_path": "/6pqeGxtWEdDjYsnQfUkmzXLlDvs.jpg"
    },
    {
      "adult": false,
      "id": 7087,
      "known_for": [
        {
          "adult": false,
          "backdrop_path": "/wLRRjb0GEUCuBMx3YFksg2xyH4S.jpg",
          "genre_ids": [
            28,
            53,
            12
          ],
          "id": 56292,
          "original_language": "en",
          "original_title": "Mission: Impossible - Ghost Protocol",
          "overview": "In the 4th installment of the Mission Impossible series, Ethan Hunt (Cruise) and his team are racing against time to track down a dangerous terrorist named Hendricks (Nyqvist), who has gained access to Russian nuclear launch codes and is planning a strike on the United States. An attempt to stop him ends in an explosion causing severe destruction to the Kremlin and the IMF to be implicated in the bombing, forcing the President to disavow them. No longer being aided by the government, Ethan and his team chase Hendricks around the globe, although they might still be too late to stop a disaster.",
          "release_date": "2011-12-21",
          "poster_path": "/96k0SkL6bXqKyOj50hYu3fRRbmV.jpg",
          "popularity": 3.985354,
          "title": "Mission: Impossible - Ghost Protocol",
          "video": false,
          "vote_average": 6.5,
          "vote_count": 2100,
          "media_type": "movie"
        },
        {
          "adult": false,
          "backdrop_path": "/iGknrkEyebPFmpQoLGy5L0utVxz.jpg",
          "genre_ids": [
            10751,
            16
          ],
          "id": 9806,
          "original_language": "en",
          "original_title": "The Incredibles",
          "overview": "Bob Parr has given up his superhero days to log in time as an insurance adjuster and raise his three children with his formerly heroic wife in suburbia. But when he receives a mysterious assignment, it's time to get back into costume.",
          "release_date": "2004-11-04",
          "poster_path": "/9k4sgKD79q0MDHSWIqNnHqOfOEV.jpg",
          "popularity": 2.862093,
          "title": "The Incredibles",
          "video": false,
          "vote_average": 6.9,
          "vote_count": 1636,
          "media_type": "movie"
        },
        {
          "adult": false,
          "backdrop_path": "/sFpGI08aeHIRKlLi9SxLyYrRyZ8.jpg",
          "genre_ids": [
            10751,
            14,
            16
          ],
          "id": 2062,
          "original_language": "en",
          "original_title": "Ratatouille",
          "overview": "In one of Paris' finest restaurants, Remy, a determined young rat, dreams of becoming a renowned French chef. Torn between his family's wishes and his true calling. Remy and his pal Linguini set in motion a hilarious chain of events that turns the City of Lights upside down.",
          "release_date": "2007-06-28",
          "poster_path": "/wbG9OoWlkZ7QRyxfvpjdWUbfREX.jpg",
          "popularity": 3.320039,
          "title": "Ratatouille",
          "video": false,
          "vote_average": 7.0,
          "vote_count": 1433,
          "media_type": "movie"
        }
      ],
      "name": "Brad Bird",
      "popularity": 1.995323,
      "profile_path": "/2XwJyYs6XNLaQuC1O2gbEHT3jxx.jpg"
    },
    {
      "adult": false,
      "id": 132876,
      "known_for": [
        {
          "adult": false,
          "backdrop_path": "/hFyKO3lkv6hY0goDa1Zv6MLtCD5.jpg",
          "genre_ids": [
            28,
            12,
            878
          ],
          "id": 72545,
          "original_language": "en",
          "original_title": "Journey 2: The Mysterious Island",
          "overview": "Sean Anderson partners with his mom's boyfriend on a mission to find his grandfather, who is thought to be missing on a mythical island.",
          "release_date": "2012-02-10",
          "poster_path": "/jMu45saMeqB2RGDqV3HoQ3XgZ2a.jpg",
          "popularity": 1.283355,
          "title": "Journey 2: The Mysterious Island",
          "video": false,
          "vote_average": 5.8,
          "vote_count": 311,
          "media_type": "movie"
        },
        {
          "adult": false,
          "backdrop_path": "/cUfGqafAVQkatQ7N4y08RNV3bgu.jpg",
          "genre_ids": [
            28,
            18,
            53
          ],
          "id": 254128,
          "original_language": "en",
          "original_title": "San Andreas",
          "overview": "In the aftermath of a massive earthquake in California, a rescue-chopper pilot makes a dangerous journey across the state in order to rescue his estranged daughter.",
          "release_date": "2015-05-29",
          "poster_path": "/6iQ4CMtYorKFfAmXEpAQZMnA0Qe.jpg",
          "popularity": 26.286682,
          "title": "San Andreas",
          "video": false,
          "vote_average": 6.2,
          "vote_count": 234,
          "media_type": "movie"
        },
        {
          "adult": false,
          "backdrop_path": "/8MJcCupnfyvh560FA3PmaHu1tMU.jpg",
          "genre_ids": [
            35,
            10751
          ],
          "id": 39691,
          "original_language": "en",
          "original_title": "Cats & Dogs: The Revenge of Kitty Galore",
          "overview": "The ongoing war between the canine and feline species is put on hold when they join forces to thwart a rogue cat spy with her own sinister plans for conquest.",
          "release_date": "2010-07-30",
          "poster_path": "/7NPz9uEJfNHjPPO6bo0Urid8lV.jpg",
          "popularity": 1.647681,
          "title": "Cats & Dogs: The Revenge of Kitty Galore",
          "video": false,
          "vote_average": 5.0,
          "vote_count": 22,
          "media_type": "movie"
        }
      ],
      "name": "Brad Peyton",
      "popularity": 1.527715,
      "profile_path": "/vahID4tRrJqTt5ZSsMD8DFvVo1W.jpg"
    },
    {
      "adult": false,
      "id": 60677,
      "known_for": [
        {
          "adult": false,
          "backdrop_path": "/6sLQFh6VFxU8nIkVaYg2yyJ6FL.jpg",
          "genre_ids": [
            80,
            18,
            53
          ],
          "id": 388,
          "original_language": "en",
          "original_title": "Inside Man",
          "overview": "An efficient gang enters a Manhattan bank, locks the doors, and takes hostages. They work deliberately, without haste. Detective Frazier is assigned to negotiate, but his mind is occupied with the corruption charges he is facing. With an army of police surrounding the bank, the thief, the cop, and the plutocrat's fixer enter high-stakes negotiations. Why are the robbers asking for a plane, if they are so competent and they know they won't get one? Why aren't they in more of a hurry?",
          "release_date": "2006-03-24",
          "poster_path": "/z6wYRuvk1lf60R4SmWETfdUvGsm.jpg",
          "popularity": 2.363864,
          "title": "Inside Man",
          "video": false,
          "vote_average": 6.9,
          "vote_count": 428,
          "media_type": "movie"
        },
        {
          "adult": false,
          "backdrop_path": "/jn4kOJHI7D45fOSg4KqHPsetdIB.jpg",
          "genre_ids": [
            35
          ],
          "id": 9927,
          "original_language": "en",
          "original_title": "The Ringer",
          "overview": "Pressured by a greedy uncle (Brian Cox) and a pile of debt, lovable loser Steve Barker (Knoxville) resorts to an unthinkable, contemptible, just-crazy-enough-to-work scheme. He pretends to be mentally challenged to rig the upcoming Special Olympics and bring home the gold. But when Steve's fellow competitors get wise to the con, they inspire him to rise to the greatest challenge of all: becoming a better person.",
          "release_date": "2005-12-23",
          "poster_path": "/AuqPA96KfZBjiFoBotMiDQ9mdOL.jpg",
          "popularity": 0.193714,
          "title": "The Ringer",
          "video": false,
          "vote_average": 5.2,
          "vote_count": 23,
          "media_type": "movie"
        },
        {
          "adult": false,
          "backdrop_path": "/1zzrSR7Xw85rqUjZbdpih6fSvso.jpg",
          "genre_ids": [
            18,
            53
          ],
          "id": 10093,
          "original_language": "en",
          "original_title": "The Return",
          "overview": "Joanna Mills has a successful career but feels her personal life is spinning out of control. She has few friends, an estranged father, and a crazy ex-boyfriend who is stalking her. Joanna begins having terrifying visions of a woman's murder, and it seems that she is the killer's next target. Determined to solve the mystery and escape her apparent fate, Joanna follows her visions to the victim's hometown and finds that some secrets just do not stay buried.",
          "release_date": "2006-11-10",
          "poster_path": "/qplM1tq6X1I1yMuANV2Cc8yeAuJ.jpg",
          "popularity": 0.486571,
          "title": "The Return",
          "video": false,
          "vote_average": 4.8,
          "vote_count": 11,
          "media_type": "movie"
        }
      ],
      "name": "Brad Leland",
      "popularity": 0.000001,
      "profile_path": "/gIXb73WT2fyoCChUyJgjcThqaA1.jpg"
    },
    {
      "adult": false,
      "id": 8131,
      "known_for": [
        {
          "adult": false,
          "backdrop_path": "/eKBUYeSgGVvztO2MZxD5YMcz6kv.jpg",
          "genre_ids": [
            14,
            16,
            10751,
            35
          ],
          "id": 585,
          "original_language": "en",
          "original_title": "Monsters, Inc.",
          "overview": "James Sullivan and Mike Wazowski are monsters, they earn their living scaring children and are the best in the business... even though they're more afraid of the children than they are of them. When a child accidentally enters their world, James and Mike suddenly find that kids are not to be afraid of and they uncover a conspiracy that could threaten all children across the world.",
          "release_date": "2001-11-01",
          "poster_path": "/93Y9BGx8blzmZOPSoivkFfaifqU.jpg",
          "popularity": 3.978067,
          "title": "Monsters, Inc.",
          "video": false,
          "vote_average": 7.1,
          "vote_count": 2769,
          "media_type": "movie"
        }
      ],
      "name": "Brad West",
      "popularity": 0.000001,
      "profile_path": null
    },
    {
      "adult": false,
      "id": 1018743,
      "known_for": [
        {
          "adult": false,
          "backdrop_path": "/oxWRnApCgJOjIoztmGe5rTc859I.jpg",
          "genre_ids": [
            18,
            10749
          ],
          "id": 98560,
          "original_language": "en",
          "original_title": "The Diary of Preston Plummer",
          "overview": "On the day of his college graduation, Preston Plummer cannot think of a single thing he really loves. Adrift, Preston follows a beautiful but troubled young woman to a small island town where he begins to fall for her, but it all threatens to fall apart when he uncovers her family’s dark past.",
          "release_date": "2012-03-05",
          "poster_path": "/zxKCF7zZeRfKAegTzx7pPX2uIaS.jpg",
          "popularity": 0.005169,
          "title": "The Diary of Preston Plummer",
          "video": false,
          "vote_average": 6.0,
          "vote_count": 1,
          "media_type": "movie"
        }
      ],
      "name": "Brad Champion",
      "popularity": 0.7,
      "profile_path": null
    },
    {
      "adult": false,
      "id": 1427931,
      "known_for": [
        {
          "adult": false,
          "backdrop_path": null,
          "genre_ids": [
            99
          ],
          "id": 325738,
          "original_language": "en",
          "original_title": "Viva Lucha Libre",
          "overview": "A documentary on Lucha Libre. Leap off the top rope into the world of Mexican wrestling.",
          "release_date": "2012-09-14",
          "poster_path": "/hytKbaUNNCAFibO3m1IMtsMUU73.jpg",
          "popularity": 1.0023,
          "title": "Viva Lucha Libre",
          "video": false,
          "vote_average": 0.0,
          "vote_count": 0,
          "media_type": "movie"
        }
      ],
      "name": "Brad Bemis",
      "popularity": 0.7,
      "profile_path": null
    },
    {
      "adult": false,
      "id": 938121,
      "known_for": [
        {
          "adult": false,
          "backdrop_path": "/9iz2DaLLvGnJJAPwawVEE1WFWIQ.jpg",
          "genre_ids": [
            35,
            27
          ],
          "id": 13009,
          "original_language": "en",
          "original_title": "Zombie Strippers!",
          "overview": "In the not too distant future a secret government re-animation chemo-virus gets released into conservative Sartre, Nebraska and lands in an underground strip club. As the virus begins to spread, turning the strippers into \"Super Zombie Strippers\" the girls struggle with whether or not to conform to the new \"fad\" even if it means there's no turning back.",
          "release_date": "2008-04-18",
          "poster_path": "/wmuidysXkosHFgkxTtghR04M16x.jpg",
          "popularity": 1.393503,
          "title": "Zombie Strippers!",
          "video": false,
          "vote_average": 5.0,
          "vote_count": 24,
          "media_type": "movie"
        },
        {
          "adult": false,
          "backdrop_path": "/jV8e1NGdMddde0MWCqZGurv19RP.jpg",
          "genre_ids": [
            27
          ],
          "id": 40238,
          "original_language": "en",
          "original_title": "The Slaughter",
          "overview": "A turn of the century ritual to raise the feminine evil goes horribly awry and leaves a she demon dormant on the ceremonial grounds. Sixty years later a young couple moves into a house built on the ancient grounds and their daughter's murder leaves the house abandoned for yet another forty years. Now, six fun loving college students are hired to clean up the house for a greedy real estate mogul who plans to sell it for a large profit. They unwittingly awake the demon and only the ultimate sacrifice can save the world from a future of unspeakable evil.",
          "release_date": "2006-05-01",
          "poster_path": "/47ZCG7azoqMFaeEjbYxxAQdy1gx.jpg",
          "popularity": 0.000845,
          "title": "The Slaughter",
          "video": false,
          "vote_average": 6.0,
          "vote_count": 1,
          "media_type": "movie"
        },
        {
          "adult": false,
          "backdrop_path": null,
          "genre_ids": [
            28,
            18
          ],
          "id": 145834,
          "original_language": "en",
          "original_title": "Crisis",
          "overview": "Alex reluctantly collects debts for the mob. Alex has led his brother Tony to believe that he is a shoe-salesman. However Alex's real talents will be desperately needed as the story unfolds. Tony has aligned himself with a group of mercenaries lead by Simon, who have agreed to deter a local scientist from using a faulty filtering system to neutralize chemical warheads. As tension mounts, Alex is forced into action against Simon and his treacherous band of terrorists. Who will live, who will die... only a killer can save the day.",
          "release_date": "1997-03-19",
          "poster_path": "/aIj8xMb4PNupiUzdUe0JMIUx669.jpg",
          "popularity": 0.000233,
          "title": "Crisis",
          "video": false,
          "vote_average": 5.0,
          "vote_count": 1,
          "media_type": "movie"
        }
      ],
      "name": "Brad Milne",
      "popularity": 0.0,
      "profile_path": null
    },
    {
      "adult": false,
      "id": 219548,
      "known_for": [
        {
          "adult": false,
          "backdrop_path": "/pzPaaVzfSX3RuGAhrFy4JoGpv53.jpg",
          "genre_ids": [
            28,
            80,
            18,
            37
          ],
          "id": 11509,
          "original_language": "en",
          "original_title": "Silverado",
          "overview": "A misfit bunch of friends come together to right the injustices that exist in a small town.",
          "release_date": "1985-07-10",
          "poster_path": "/9erBmGHvj7v5w9C4gj81DV9QvxC.jpg",
          "popularity": 0.580742,
          "title": "Silverado",
          "video": false,
          "vote_average": 6.7,
          "vote_count": 39,
          "media_type": "movie"
        }
      ],
      "name": "Brad Williams",
      "popularity": 0.7,
      "profile_path": null
    },
    {
      "adult": false,
      "id": 575647,
      "known_for": [
        {
          "adult": false,
          "backdrop_path": "/iYpN7lFCOGwQav2c6eJZiofhAgM.jpg",
          "genre_ids": [
            16
          ],
          "id": 67598,
          "original_language": "en",
          "original_title": "The Plastics Inventor",
          "overview": "Donald is listening to a radio program that tells how to build an airplane from plastic, in a process much like baking a cake, cookies, and making toast. He takes it out for a test flight, still guided by the radio, and it works wonderfully. Until the radio interviewer asks if there's any problems: yes, it melts when it gets wet. Of course, Donald instantly flies into a rain cloud, and has to battle his plane as it disintegrates.",
          "release_date": "1944-09-01",
          "poster_path": "/ciZMNIfLXeTdjHqwQeDdBCEUQc3.jpg",
          "popularity": 0.00015,
          "title": "The Plastics Inventor",
          "video": false,
          "vote_average": 2.8,
          "vote_count": 2,
          "media_type": "movie"
        },
        {
          "adult": false,
          "backdrop_path": null,
          "genre_ids": [],
          "id": 281953,
          "original_language": "en",
          "original_title": "Supermarket Pink",
          "overview": "The final Pink Panther theatrical cartoon.",
          "release_date": "1980-02-01",
          "poster_path": null,
          "popularity": 1.00135,
          "title": "Supermarket Pink",
          "video": false,
          "vote_average": 0.0,
          "vote_count": 0,
          "media_type": "movie"
        }
      ],
      "name": "Brad Case",
      "popularity": 0.7,
      "profile_path": null
    },
    {
      "adult": false,
      "id": 89342,
      "known_for": [
        {
          "adult": false,
          "backdrop_path": null,
          "genre_ids": [],
          "id": 17528,
          "original_language": "en",
          "original_title": "UFC 24: First Defense",
          "overview": "UFC 24: First Defense was a mixed martial arts event held by the Ultimate Fighting Championship on March 10, 2000 at the Lake Charles Civic Center in Lake Charles, Louisiana.",
          "release_date": "2000-03-10",
          "poster_path": "/fLvQ9Vs7G2m57pQOUe2VCQIcgK3.jpg",
          "popularity": 0.000174,
          "title": "UFC 24: First Defense",
          "video": false,
          "vote_average": 5.3,
          "vote_count": 2,
          "media_type": "movie"
        },
        {
          "adult": false,
          "backdrop_path": null,
          "genre_ids": [],
          "id": 17534,
          "original_language": "en",
          "original_title": "UFC 27: Ultimate Bad Boyz",
          "overview": "UFC 27: Ultimate Bad Boyz was a mixed martial arts event held by the Ultimate Fighting Championship on September 22, 2000 at Lake Front Arena in New Orleans, Louisiana.",
          "release_date": "2000-09-22",
          "poster_path": "/b6WMEQE6cxBwT5QeTIlhDqgvqyk.jpg",
          "popularity": 0.000357,
          "title": "UFC 27: Ultimate Bad Boyz",
          "video": false,
          "vote_average": 5.3,
          "vote_count": 2,
          "media_type": "movie"
        }
      ],
      "name": "Brad Gumm",
      "popularity": 0.7,
      "profile_path": null
    },
    {
      "adult": false,
      "id": 68574,
      "known_for": [
        {
          "adult": false,
          "backdrop_path": "/pyllS8OfE5ld539TToT2qSC6iw7.jpg",
          "genre_ids": [
            28,
            12,
            35
          ],
          "id": 11199,
          "original_language": "en",
          "original_title": "Wild Hogs",
          "overview": "Restless and ready for adventure, four suburban bikers leave the safety of their subdivision and head out on the open road. But complications ensue when they cross paths with an intimidating band of New Mexico bikers known as the Del Fuegos.",
          "release_date": "2007-03-02",
          "poster_path": "/auKcnTuYtB7v1t4yLC4y29ZwyTq.jpg",
          "popularity": 2.113437,
          "title": "Wild Hogs",
          "video": false,
          "vote_average": 5.6,
          "vote_count": 178,
          "media_type": "movie"
        },
        {
          "adult": false,
          "backdrop_path": "/hac0ngx7NLbLzJ3zWD2Xyb3ch7n.jpg",
          "genre_ids": [
            35
          ],
          "id": 198062,
          "original_language": "en",
          "original_title": "Coffee Town",
          "overview": "Three thirty-something friends band together when their carefree existence is threatened.",
          "release_date": "2013-07-09",
          "poster_path": "/m8qNXU2eDgZ1rDJhjRM2MinM2VK.jpg",
          "popularity": 1.04062,
          "title": "Coffee Town",
          "video": false,
          "vote_average": 6.9,
          "vote_count": 25,
          "media_type": "movie"
        },
        {
          "backdrop_path": "/bCZgRAqv4pjNUjZbA4hGuaFSJ1h.jpg",
          "first_air_date": "2007-08-30",
          "genre_ids": [
            35
          ],
          "id": 2317,
          "original_language": "en",
          "original_name": "My Name Is Earl",
          "overview": "My Name Is Earl is an American television comedy series created by Greg Garcia that aired on the NBC television network from September 20, 2005, to May 14, 2009, in the United States. It was produced by 20th Century Fox Television and starred Jason Lee as the title character, Earl Hickey. The series also stars Ethan Suplee, Jaime Pressly, Eddie Steeples, and Nadine Velazquez.",
          "origin_country": [
            "US"
          ],
          "poster_path": "/wqkMrw0zUGqfy5emVc1SPTt2Y4m.jpg",
          "popularity": 1.495457,
          "name": "My Name Is Earl",
          "vote_average": 7.7,
          "vote_count": 24,
          "media_type": "tv"
        }
      ],
      "name": "Brad Copeland",
      "popularity": 0.000001,
      "profile_path": null
    },
    {
      "adult": false,
      "id": 132623,
      "known_for": [
        {
          "adult": false,
          "backdrop_path": "/tBkgUdkli6uhfeeXl7fKT2ti2LP.jpg",
          "genre_ids": [
            18,
            53
          ],
          "id": 39889,
          "original_language": "en",
          "original_title": "The Candy Snatchers",
          "overview": "An abused autistic boy is the sole witness to the kidnapping of a teenage heiress.",
          "release_date": "1973-01-01",
          "poster_path": "/5fkVeonY3IDt4juvFpUjAY3m3qd.jpg",
          "popularity": 1.000005,
          "title": "The Candy Snatchers",
          "video": false,
          "vote_average": 5.5,
          "vote_count": 3,
          "media_type": "movie"
        },
        {
          "adult": false,
          "backdrop_path": "/hfQ9BEJNO7SZa3pv7chj2aiIWRJ.jpg",
          "genre_ids": [
            28
          ],
          "id": 45232,
          "original_language": "en",
          "original_title": "Eat My Dust",
          "overview": "Hoover Nielbold is a car-crazy teenager who, in order to impress the hottest girl in school, takes her for a ride in a souped-up race car owned by local racer Big Bubba Jones. Hoover's father Harry, who's also the local sheriff is furious at the situation and orders his bumbling deputies to go after him. With the Sheriff's office overflowing with concerned parents and citizens and his deputies failing to catch him. He enlists the help of Jones and fellow racers to capture him. It culminates in a thrilling car chase finale through the rural countryside.",
          "release_date": "1976-04-01",
          "poster_path": "/p9vS8qZLLkpsNyOzOvTDzWNv05h.jpg",
          "popularity": 0.000189,
          "title": "Eat My Dust",
          "video": false,
          "vote_average": 0.0,
          "vote_count": 0,
          "media_type": "movie"
        }
      ],
      "name": "Brad David",
      "popularity": 0.21,
      "profile_path": null
    },
    {
      "adult": false,
      "id": 1044820,
      "known_for": [
        {
          "adult": false,
          "backdrop_path": "/f4Sk0PXFqaoE9SYuFBtt73kXuam.jpg",
          "genre_ids": [
            99
          ],
          "id": 108713,
          "original_language": "en",
          "original_title": "A Player to Be Named Later",
          "overview": "This inspiring true story of four minor league baseball players charts the course of one season with the Indianapolis Indians, the Triple-A affiliate of the Milwaukee Brewers.",
          "release_date": "",
          "poster_path": "/w4B6QxpXCErWqroeKzW4806JApQ.jpg",
          "popularity": 0.0045,
          "title": "A Player to Be Named Later",
          "video": false,
          "vote_average": 6.0,
          "vote_count": 1,
          "media_type": "movie"
        }
      ],
      "name": "Brad Tyler",
      "popularity": 0.21,
      "profile_path": null
    },
    {
      "adult": false,
      "id": 1197440,
      "known_for": [
        {
          "adult": false,
          "backdrop_path": "/mr8e91QeIJTjlCQVLokEMfABcFx.jpg",
          "genre_ids": [
            53
          ],
          "id": 211381,
          "original_language": "de",
          "original_title": "Machine Head",
          "overview": "Three college girls staying at an isolated home for spring break suddenly find themselves terrorized by a maniac on the road.",
          "release_date": "2013-08-01",
          "poster_path": "/3SNAb6ED3i50P9Ej7Qm9RaMRNO3.jpg",
          "popularity": 1.04746,
          "title": "Machine Head",
          "video": false,
          "vote_average": 3.5,
          "vote_count": 3,
          "media_type": "movie"
        }
      ],
      "name": "Brad LaFave",
      "popularity": 0.21,
      "profile_path": null
    },
    {
      "adult": false,
      "id": 1413905,
      "known_for": [
        {
          "adult": false,
          "backdrop_path": "/ki7xrOPMcFp98TTY87NO0MTrpEw.jpg",
          "genre_ids": [
            18
          ],
          "id": 49014,
          "original_language": "en",
          "original_title": "Cosmopolis",
          "overview": "Riding across Manhattan in a stretch limo in order to get a haircut, a 28-year-old billionaire asset manager's day devolves into an odyssey with a cast of characters that start to tear his world apart.",
          "release_date": "2012-05-25",
          "poster_path": "/1L7Ise1Zjj2zbpbfrk7zHFiYrTt.jpg",
          "popularity": 1.614885,
          "title": "Cosmopolis",
          "video": false,
          "vote_average": 4.6,
          "vote_count": 95,
          "media_type": "movie"
        }
      ],
      "name": "Brad Milburn",
      "popularity": 0.21,
      "profile_path": null
    },
    {
      "adult": false,
      "id": 931652,
      "known_for": [
        {
          "adult": false,
          "backdrop_path": "/el3jzC6Hp1ZkMxmBQF3tdxYY5cU.jpg",
          "genre_ids": [
            27,
            878
          ],
          "id": 27358,
          "original_language": "en",
          "original_title": "Monsturd",
          "overview": "A serial killer mutates with a chemical inside a sewer, to become a monster made of human waste just as the FBI and police are onto him.",
          "release_date": "2003-04-08",
          "poster_path": "/ujore1y4A1lk5g9xCsVs6Q4NDLK.jpg",
          "popularity": 0.000286,
          "title": "Monsturd",
          "video": false,
          "vote_average": 0.0,
          "vote_count": 0,
          "media_type": "movie"
        }
      ],
      "name": "Brad Dosland",
      "popularity": 0.0,
      "profile_path": null
    },
    {
      "adult": false,
      "id": 1460898,
      "known_for": [
        {
          "adult": false,
          "backdrop_path": null,
          "genre_ids": [
            18
          ],
          "id": 337874,
          "original_language": "en",
          "original_title": "Goat",
          "overview": "Reeling from a terrifying assault, a nineteen year old enrolls into college with his brother and pledges the same fraternity. What happens there, in the name of 'brotherhood,' tests him and his loyalty in brutal ways.",
          "release_date": null,
          "poster_path": null,
          "popularity": 1.0,
          "title": "Goat",
          "video": false,
          "vote_average": 0.0,
          "vote_count": 0,
          "media_type": "movie"
        }
      ],
      "name": "Brad Land",
      "popularity": 0.0,
      "profile_path": null
    },
    {
      "adult": false,
      "id": 78684,
      "known_for": [
        {
          "adult": false,
          "backdrop_path": null,
          "genre_ids": [
            10749
          ],
          "id": 123864,
          "original_language": "en",
          "original_title": "Leaving Metropolis",
          "overview": "David is a painter with painter's block who takes a job as a waiter to get some inspiration. He falls for hunky diner owner Matt, who falls just as hard back. But Violet, Matt's wife is a complicating factor! Toss in David's best friends a dying pre-op transsexual best friend and an aging, bitter, fag-hag journalist. Will David break up Matt's marriage? Will Violet learn the truth? Will David or Matt learn the true meaning of love?",
          "release_date": "2002-08-31",
          "poster_path": "/pQgESDcsSbbIztyTWjLpiGiAFBx.jpg",
          "popularity": 0.000286,
          "title": "Leaving Metropolis",
          "video": false,
          "vote_average": 6.5,
          "vote_count": 1,
          "media_type": "movie"
        },
        {
          "adult": false,
          "backdrop_path": "/z40fZxif0agJhcbbLG3CglJbxKR.jpg",
          "genre_ids": [
            35,
            18
          ],
          "id": 15730,
          "original_language": "en",
          "original_title": "Love & Human Remains",
          "overview": "Dreary urban landscape of an anonymous Canadian city. Dark comedy about a group of twentysomethings looking for love and meaning in the '90s. The film focuses on roommates David, a gay waiter who has has given up on his acting career, and Candy, a book reviewer who is also David's ex-lover. David and Candy's lives are entangled with those of David's friends and Candy's dates.",
          "release_date": "1993-05-20",
          "poster_path": "/Aq5RgnkiDUkXyp4HWckPOBxaUcJ.jpg",
          "popularity": 0.073577,
          "title": "Love & Human Remains",
          "video": false,
          "vote_average": 0.0,
          "vote_count": 0,
          "media_type": "movie"
        }
      ],
      "name": "Brad Fraser",
      "popularity": 0.21,
      "profile_path": null
    }
  ],
  "total_pages": 59,
  "total_results": 1165
}

Search for TV shows by title.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td colspan="2">api_key</td></tr>
<tr><td style="width: 140px;">query</td><td>CGI escaped string</td></tr>
<tr><td colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-right: 40px; width: 140px;">page</td><td>Minimum 1, maximum 1000.</td></tr>
<tr><td style="padding-right: 40px;">language</td><td>ISO 639-1 code.</td></tr>
<tr><td style="padding-right: 40px;">first_air_date_year</td><td>Filter the results to only match shows that have a air date with with value.</td></tr>
<tr><td style="padding-right: 40px;">search_type</td><td>By default, the search type is 'phrase'. This is almost guaranteed the option you will want. It's a great all purpose search type and by far the most tuned for every day querying. For those wanting more of an "autocomplete" type search, set this option to 'ngram'.</td></tr>
</table>

GET /search/tv
> Accept: application/json
< 200
< Content-Type: application/json
{
  "page": 1,
  "results": [
    {
      "backdrop_path": "/eSzpy96DwBujGFj0xMbXBcGcfxX.jpg",
      "first_air_date": "2008-01-19",
      "genre_ids": [
        18
      ],
      "id": 1396,
      "original_language": "en",
      "original_name": "Breaking Bad",
      "overview": "Breaking Bad is an American crime drama television series created and produced by Vince Gilligan. Set and produced in Albuquerque, New Mexico, Breaking Bad is the story of Walter White, a struggling high school chemistry teacher who is diagnosed with inoperable lung cancer at the beginning of the series. He turns to a life of crime, producing and selling methamphetamine, in order to secure his family's financial future before he dies, teaming with his former student, Jesse Pinkman. Heavily serialized, the series is known for positioning its characters in seemingly inextricable corners and has been labeled a contemporary western by its creator.",
      "origin_country": [
        "US"
      ],
      "poster_path": "/4yMXf3DW6oCL0lVPZaZM2GypgwE.jpg",
      "popularity": 18.095686,
      "name": "Breaking Bad",
      "vote_average": 8.9,
      "vote_count": 245
    }
  ],
  "total_pages": 1,
  "total_results": 1
}

--
Timezones
--

Get the list of supported timezones for the API methods that support them.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
</table>

GET /timezones/list
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "c1da77064f42cc3d4f97f085f634b215"
[
  {
    "AD": [
      "Europe/Andorra"
    ]
  },
  {
    "AE": [
      "Asia/Dubai"
    ]
  },
  {
    "AF": [
      "Asia/Kabul"
    ]
  },
  {
    "AG": [
      "America/Antigua"
    ]
  },
  {
    "AI": [
      "America/Anguilla"
    ]
  },
  {
    "AL": [
      "Europe/Tirane"
    ]
  },
  {
    "AM": [
      "Asia/Yerevan"
    ]
  },
  {
    "AO": [
      "Africa/Luanda"
    ]
  },
  {
    "AQ": [
      "Antarctica/McMurdo",
      "Antarctica/Rothera",
      "Antarctica/Palmer",
      "Antarctica/Mawson",
      "Antarctica/Davis",
      "Antarctica/Casey",
      "Antarctica/Vostok",
      "Antarctica/DumontDUrville",
      "Antarctica/Syowa"
    ]
  },
  {
    "AR": [
      "America/Argentina/Buenos_Aires",
      "America/Argentina/Cordoba",
      "America/Argentina/Salta",
      "America/Argentina/Jujuy",
      "America/Argentina/Tucuman",
      "America/Argentina/Catamarca",
      "America/Argentina/La_Rioja",
      "America/Argentina/San_Juan",
      "America/Argentina/Mendoza",
      "America/Argentina/San_Luis",
      "America/Argentina/Rio_Gallegos",
      "America/Argentina/Ushuaia"
    ]
  },
  {
    "AS": [
      "Pacific/Pago_Pago"
    ]
  },
  {
    "AT": [
      "Europe/Vienna"
    ]
  },
  {
    "AU": [
      "Australia/Lord_Howe",
      "Antarctica/Macquarie",
      "Australia/Hobart",
      "Australia/Currie",
      "Australia/Melbourne",
      "Australia/Sydney",
      "Australia/Broken_Hill",
      "Australia/Brisbane",
      "Australia/Lindeman",
      "Australia/Adelaide",
      "Australia/Darwin",
      "Australia/Perth",
      "Australia/Eucla"
    ]
  },
  {
    "AW": [
      "America/Aruba"
    ]
  },
  {
    "AX": [
      "Europe/Mariehamn"
    ]
  },
  {
    "AZ": [
      "Asia/Baku"
    ]
  },
  {
    "BA": [
      "Europe/Sarajevo"
    ]
  },
  {
    "BB": [
      "America/Barbados"
    ]
  },
  {
    "BD": [
      "Asia/Dhaka"
    ]
  },
  {
    "BE": [
      "Europe/Brussels"
    ]
  },
  {
    "BF": [
      "Africa/Ouagadougou"
    ]
  },
  {
    "BG": [
      "Europe/Sofia"
    ]
  },
  {
    "BH": [
      "Asia/Bahrain"
    ]
  },
  {
    "BI": [
      "Africa/Bujumbura"
    ]
  },
  {
    "BJ": [
      "Africa/Porto-Novo"
    ]
  },
  {
    "BL": [
      "America/St_Barthelemy"
    ]
  },
  {
    "BM": [
      "Atlantic/Bermuda"
    ]
  },
  {
    "BN": [
      "Asia/Brunei"
    ]
  },
  {
    "BO": [
      "America/La_Paz"
    ]
  },
  {
    "BQ": [
      "America/Kralendijk"
    ]
  },
  {
    "BR": [
      "America/Noronha",
      "America/Belem",
      "America/Fortaleza",
      "America/Recife",
      "America/Araguaina",
      "America/Maceio",
      "America/Bahia",
      "America/Sao_Paulo",
      "America/Campo_Grande",
      "America/Cuiaba",
      "America/Santarem",
      "America/Porto_Velho",
      "America/Boa_Vista",
      "America/Manaus",
      "America/Eirunepe",
      "America/Rio_Branco"
    ]
  },
  {
    "BS": [
      "America/Nassau"
    ]
  },
  {
    "BT": [
      "Asia/Thimphu"
    ]
  },
  {
    "BV": []
  },
  {
    "BW": [
      "Africa/Gaborone"
    ]
  },
  {
    "BY": [
      "Europe/Minsk"
    ]
  },
  {
    "BZ": [
      "America/Belize"
    ]
  },
  {
    "CA": [
      "America/St_Johns",
      "America/Halifax",
      "America/Glace_Bay",
      "America/Moncton",
      "America/Goose_Bay",
      "America/Blanc-Sablon",
      "America/Toronto",
      "America/Nipigon",
      "America/Thunder_Bay",
      "America/Iqaluit",
      "America/Pangnirtung",
      "America/Resolute",
      "America/Atikokan",
      "America/Rankin_Inlet",
      "America/Winnipeg",
      "America/Rainy_River",
      "America/Regina",
      "America/Swift_Current",
      "America/Edmonton",
      "America/Cambridge_Bay",
      "America/Yellowknife",
      "America/Inuvik",
      "America/Creston",
      "America/Dawson_Creek",
      "America/Vancouver",
      "America/Whitehorse",
      "America/Dawson"
    ]
  },
  {
    "CC": [
      "Indian/Cocos"
    ]
  },
  {
    "CD": [
      "Africa/Kinshasa",
      "Africa/Lubumbashi"
    ]
  },
  {
    "CF": [
      "Africa/Bangui"
    ]
  },
  {
    "CG": [
      "Africa/Brazzaville"
    ]
  },
  {
    "CH": [
      "Europe/Zurich"
    ]
  },
  {
    "CI": [
      "Africa/Abidjan"
    ]
  },
  {
    "CK": [
      "Pacific/Rarotonga"
    ]
  },
  {
    "CL": [
      "America/Santiago",
      "Pacific/Easter"
    ]
  },
  {
    "CM": [
      "Africa/Douala"
    ]
  },
  {
    "CN": [
      "Asia/Shanghai",
      "Asia/Harbin",
      "Asia/Chongqing",
      "Asia/Urumqi",
      "Asia/Kashgar"
    ]
  },
  {
    "CO": [
      "America/Bogota"
    ]
  },
  {
    "CR": [
      "America/Costa_Rica"
    ]
  },
  {
    "CU": [
      "America/Havana"
    ]
  },
  {
    "CV": [
      "Atlantic/Cape_Verde"
    ]
  },
  {
    "CW": [
      "America/Curacao"
    ]
  },
  {
    "CX": [
      "Indian/Christmas"
    ]
  },
  {
    "CY": [
      "Asia/Nicosia"
    ]
  },
  {
    "CZ": [
      "Europe/Prague"
    ]
  },
  {
    "DE": [
      "Europe/Berlin",
      "Europe/Busingen"
    ]
  },
  {
    "DJ": [
      "Africa/Djibouti"
    ]
  },
  {
    "DK": [
      "Europe/Copenhagen"
    ]
  },
  {
    "DM": [
      "America/Dominica"
    ]
  },
  {
    "DO": [
      "America/Santo_Domingo"
    ]
  },
  {
    "DZ": [
      "Africa/Algiers"
    ]
  },
  {
    "EC": [
      "America/Guayaquil",
      "Pacific/Galapagos"
    ]
  },
  {
    "EE": [
      "Europe/Tallinn"
    ]
  },
  {
    "EG": [
      "Africa/Cairo"
    ]
  },
  {
    "EH": [
      "Africa/El_Aaiun"
    ]
  },
  {
    "ER": [
      "Africa/Asmara"
    ]
  },
  {
    "ES": [
      "Europe/Madrid",
      "Africa/Ceuta",
      "Atlantic/Canary"
    ]
  },
  {
    "ET": [
      "Africa/Addis_Ababa"
    ]
  },
  {
    "FI": [
      "Europe/Helsinki"
    ]
  },
  {
    "FJ": [
      "Pacific/Fiji"
    ]
  },
  {
    "FK": [
      "Atlantic/Stanley"
    ]
  },
  {
    "FM": [
      "Pacific/Chuuk",
      "Pacific/Pohnpei",
      "Pacific/Kosrae"
    ]
  },
  {
    "FO": [
      "Atlantic/Faroe"
    ]
  },
  {
    "FR": [
      "Europe/Paris"
    ]
  },
  {
    "GA": [
      "Africa/Libreville"
    ]
  },
  {
    "GB": [
      "Europe/London"
    ]
  },
  {
    "GD": [
      "America/Grenada"
    ]
  },
  {
    "GE": [
      "Asia/Tbilisi"
    ]
  },
  {
    "GF": [
      "America/Cayenne"
    ]
  },
  {
    "GG": [
      "Europe/Guernsey"
    ]
  },
  {
    "GH": [
      "Africa/Accra"
    ]
  },
  {
    "GI": [
      "Europe/Gibraltar"
    ]
  },
  {
    "GL": [
      "America/Godthab",
      "America/Danmarkshavn",
      "America/Scoresbysund",
      "America/Thule"
    ]
  },
  {
    "GM": [
      "Africa/Banjul"
    ]
  },
  {
    "GN": [
      "Africa/Conakry"
    ]
  },
  {
    "GP": [
      "America/Guadeloupe"
    ]
  },
  {
    "GQ": [
      "Africa/Malabo"
    ]
  },
  {
    "GR": [
      "Europe/Athens"
    ]
  },
  {
    "GS": [
      "Atlantic/South_Georgia"
    ]
  },
  {
    "GT": [
      "America/Guatemala"
    ]
  },
  {
    "GU": [
      "Pacific/Guam"
    ]
  },
  {
    "GW": [
      "Africa/Bissau"
    ]
  },
  {
    "GY": [
      "America/Guyana"
    ]
  },
  {
    "HK": [
      "Asia/Hong_Kong"
    ]
  },
  {
    "HM": []
  },
  {
    "HN": [
      "America/Tegucigalpa"
    ]
  },
  {
    "HR": [
      "Europe/Zagreb"
    ]
  },
  {
    "HT": [
      "America/Port-au-Prince"
    ]
  },
  {
    "HU": [
      "Europe/Budapest"
    ]
  },
  {
    "ID": [
      "Asia/Jakarta",
      "Asia/Pontianak",
      "Asia/Makassar",
      "Asia/Jayapura"
    ]
  },
  {
    "IE": [
      "Europe/Dublin"
    ]
  },
  {
    "IL": [
      "Asia/Jerusalem"
    ]
  },
  {
    "IM": [
      "Europe/Isle_of_Man"
    ]
  },
  {
    "IN": [
      "Asia/Kolkata"
    ]
  },
  {
    "IO": [
      "Indian/Chagos"
    ]
  },
  {
    "IQ": [
      "Asia/Baghdad"
    ]
  },
  {
    "IR": [
      "Asia/Tehran"
    ]
  },
  {
    "IS": [
      "Atlantic/Reykjavik"
    ]
  },
  {
    "IT": [
      "Europe/Rome"
    ]
  },
  {
    "JE": [
      "Europe/Jersey"
    ]
  },
  {
    "JM": [
      "America/Jamaica"
    ]
  },
  {
    "JO": [
      "Asia/Amman"
    ]
  },
  {
    "JP": [
      "Asia/Tokyo"
    ]
  },
  {
    "KE": [
      "Africa/Nairobi"
    ]
  },
  {
    "KG": [
      "Asia/Bishkek"
    ]
  },
  {
    "KH": [
      "Asia/Phnom_Penh"
    ]
  },
  {
    "KI": [
      "Pacific/Tarawa",
      "Pacific/Enderbury",
      "Pacific/Kiritimati"
    ]
  },
  {
    "KM": [
      "Indian/Comoro"
    ]
  },
  {
    "KN": [
      "America/St_Kitts"
    ]
  },
  {
    "KP": [
      "Asia/Pyongyang"
    ]
  },
  {
    "KR": [
      "Asia/Seoul"
    ]
  },
  {
    "KW": [
      "Asia/Kuwait"
    ]
  },
  {
    "KY": [
      "America/Cayman"
    ]
  },
  {
    "KZ": [
      "Asia/Almaty",
      "Asia/Qyzylorda",
      "Asia/Aqtobe",
      "Asia/Aqtau",
      "Asia/Oral"
    ]
  },
  {
    "LA": [
      "Asia/Vientiane"
    ]
  },
  {
    "LB": [
      "Asia/Beirut"
    ]
  },
  {
    "LC": [
      "America/St_Lucia"
    ]
  },
  {
    "LI": [
      "Europe/Vaduz"
    ]
  },
  {
    "LK": [
      "Asia/Colombo"
    ]
  },
  {
    "LR": [
      "Africa/Monrovia"
    ]
  },
  {
    "LS": [
      "Africa/Maseru"
    ]
  },
  {
    "LT": [
      "Europe/Vilnius"
    ]
  },
  {
    "LU": [
      "Europe/Luxembourg"
    ]
  },
  {
    "LV": [
      "Europe/Riga"
    ]
  },
  {
    "LY": [
      "Africa/Tripoli"
    ]
  },
  {
    "MA": [
      "Africa/Casablanca"
    ]
  },
  {
    "MC": [
      "Europe/Monaco"
    ]
  },
  {
    "MD": [
      "Europe/Chisinau"
    ]
  },
  {
    "ME": [
      "Europe/Podgorica"
    ]
  },
  {
    "MF": [
      "America/Marigot"
    ]
  },
  {
    "MG": [
      "Indian/Antananarivo"
    ]
  },
  {
    "MH": [
      "Pacific/Majuro",
      "Pacific/Kwajalein"
    ]
  },
  {
    "MK": [
      "Europe/Skopje"
    ]
  },
  {
    "ML": [
      "Africa/Bamako"
    ]
  },
  {
    "MM": [
      "Asia/Rangoon"
    ]
  },
  {
    "MN": [
      "Asia/Ulaanbaatar",
      "Asia/Hovd",
      "Asia/Choibalsan"
    ]
  },
  {
    "MO": [
      "Asia/Macau"
    ]
  },
  {
    "MP": [
      "Pacific/Saipan"
    ]
  },
  {
    "MQ": [
      "America/Martinique"
    ]
  },
  {
    "MR": [
      "Africa/Nouakchott"
    ]
  },
  {
    "MS": [
      "America/Montserrat"
    ]
  },
  {
    "MT": [
      "Europe/Malta"
    ]
  },
  {
    "MU": [
      "Indian/Mauritius"
    ]
  },
  {
    "MV": [
      "Indian/Maldives"
    ]
  },
  {
    "MW": [
      "Africa/Blantyre"
    ]
  },
  {
    "MX": [
      "America/Mexico_City",
      "America/Cancun",
      "America/Merida",
      "America/Monterrey",
      "America/Matamoros",
      "America/Mazatlan",
      "America/Chihuahua",
      "America/Ojinaga",
      "America/Hermosillo",
      "America/Tijuana",
      "America/Santa_Isabel",
      "America/Bahia_Banderas"
    ]
  },
  {
    "MY": [
      "Asia/Kuala_Lumpur",
      "Asia/Kuching"
    ]
  },
  {
    "MZ": [
      "Africa/Maputo"
    ]
  },
  {
    "NA": [
      "Africa/Windhoek"
    ]
  },
  {
    "NC": [
      "Pacific/Noumea"
    ]
  },
  {
    "NE": [
      "Africa/Niamey"
    ]
  },
  {
    "NF": [
      "Pacific/Norfolk"
    ]
  },
  {
    "NG": [
      "Africa/Lagos"
    ]
  },
  {
    "NI": [
      "America/Managua"
    ]
  },
  {
    "NL": [
      "Europe/Amsterdam"
    ]
  },
  {
    "NO": [
      "Europe/Oslo"
    ]
  },
  {
    "NP": [
      "Asia/Kathmandu"
    ]
  },
  {
    "NR": [
      "Pacific/Nauru"
    ]
  },
  {
    "NU": [
      "Pacific/Niue"
    ]
  },
  {
    "NZ": [
      "Pacific/Auckland",
      "Pacific/Chatham"
    ]
  },
  {
    "OM": [
      "Asia/Muscat"
    ]
  },
  {
    "PA": [
      "America/Panama"
    ]
  },
  {
    "PE": [
      "America/Lima"
    ]
  },
  {
    "PF": [
      "Pacific/Tahiti",
      "Pacific/Marquesas",
      "Pacific/Gambier"
    ]
  },
  {
    "PG": [
      "Pacific/Port_Moresby"
    ]
  },
  {
    "PH": [
      "Asia/Manila"
    ]
  },
  {
    "PK": [
      "Asia/Karachi"
    ]
  },
  {
    "PL": [
      "Europe/Warsaw"
    ]
  },
  {
    "PM": [
      "America/Miquelon"
    ]
  },
  {
    "PN": [
      "Pacific/Pitcairn"
    ]
  },
  {
    "PR": [
      "America/Puerto_Rico"
    ]
  },
  {
    "PS": [
      "Asia/Gaza",
      "Asia/Hebron"
    ]
  },
  {
    "PT": [
      "Europe/Lisbon",
      "Atlantic/Madeira",
      "Atlantic/Azores"
    ]
  },
  {
    "PW": [
      "Pacific/Palau"
    ]
  },
  {
    "PY": [
      "America/Asuncion"
    ]
  },
  {
    "QA": [
      "Asia/Qatar"
    ]
  },
  {
    "RE": [
      "Indian/Reunion"
    ]
  },
  {
    "RO": [
      "Europe/Bucharest"
    ]
  },
  {
    "RS": [
      "Europe/Belgrade"
    ]
  },
  {
    "RU": [
      "Europe/Kaliningrad",
      "Europe/Moscow",
      "Europe/Volgograd",
      "Europe/Samara",
      "Asia/Yekaterinburg",
      "Asia/Omsk",
      "Asia/Novosibirsk",
      "Asia/Novokuznetsk",
      "Asia/Krasnoyarsk",
      "Asia/Irkutsk",
      "Asia/Yakutsk",
      "Asia/Khandyga",
      "Asia/Vladivostok",
      "Asia/Sakhalin",
      "Asia/Ust-Nera",
      "Asia/Magadan",
      "Asia/Kamchatka",
      "Asia/Anadyr"
    ]
  },
  {
    "RW": [
      "Africa/Kigali"
    ]
  },
  {
    "SA": [
      "Asia/Riyadh"
    ]
  },
  {
    "SB": [
      "Pacific/Guadalcanal"
    ]
  },
  {
    "SC": [
      "Indian/Mahe"
    ]
  },
  {
    "SD": [
      "Africa/Khartoum"
    ]
  },
  {
    "SE": [
      "Europe/Stockholm"
    ]
  },
  {
    "SG": [
      "Asia/Singapore"
    ]
  },
  {
    "SH": [
      "Atlantic/St_Helena"
    ]
  },
  {
    "SI": [
      "Europe/Ljubljana"
    ]
  },
  {
    "SJ": [
      "Arctic/Longyearbyen"
    ]
  },
  {
    "SK": [
      "Europe/Bratislava"
    ]
  },
  {
    "SL": [
      "Africa/Freetown"
    ]
  },
  {
    "SM": [
      "Europe/San_Marino"
    ]
  },
  {
    "SN": [
      "Africa/Dakar"
    ]
  },
  {
    "SO": [
      "Africa/Mogadishu"
    ]
  },
  {
    "SR": [
      "America/Paramaribo"
    ]
  },
  {
    "SS": [
      "Africa/Juba"
    ]
  },
  {
    "ST": [
      "Africa/Sao_Tome"
    ]
  },
  {
    "SV": [
      "America/El_Salvador"
    ]
  },
  {
    "SX": [
      "America/Lower_Princes"
    ]
  },
  {
    "SY": [
      "Asia/Damascus"
    ]
  },
  {
    "SZ": [
      "Africa/Mbabane"
    ]
  },
  {
    "TC": [
      "America/Grand_Turk"
    ]
  },
  {
    "TD": [
      "Africa/Ndjamena"
    ]
  },
  {
    "TF": [
      "Indian/Kerguelen"
    ]
  },
  {
    "TG": [
      "Africa/Lome"
    ]
  },
  {
    "TH": [
      "Asia/Bangkok"
    ]
  },
  {
    "TJ": [
      "Asia/Dushanbe"
    ]
  },
  {
    "TK": [
      "Pacific/Fakaofo"
    ]
  },
  {
    "TL": [
      "Asia/Dili"
    ]
  },
  {
    "TM": [
      "Asia/Ashgabat"
    ]
  },
  {
    "TN": [
      "Africa/Tunis"
    ]
  },
  {
    "TO": [
      "Pacific/Tongatapu"
    ]
  },
  {
    "TR": [
      "Europe/Istanbul"
    ]
  },
  {
    "TT": [
      "America/Port_of_Spain"
    ]
  },
  {
    "TV": [
      "Pacific/Funafuti"
    ]
  },
  {
    "TW": [
      "Asia/Taipei"
    ]
  },
  {
    "TZ": [
      "Africa/Dar_es_Salaam"
    ]
  },
  {
    "UA": [
      "Europe/Kiev",
      "Europe/Uzhgorod",
      "Europe/Zaporozhye",
      "Europe/Simferopol"
    ]
  },
  {
    "UG": [
      "Africa/Kampala"
    ]
  },
  {
    "UM": [
      "Pacific/Johnston",
      "Pacific/Midway",
      "Pacific/Wake"
    ]
  },
  {
    "US": [
      "America/New_York",
      "America/Detroit",
      "America/Kentucky/Louisville",
      "America/Kentucky/Monticello",
      "America/Indiana/Indianapolis",
      "America/Indiana/Vincennes",
      "America/Indiana/Winamac",
      "America/Indiana/Marengo",
      "America/Indiana/Petersburg",
      "America/Indiana/Vevay",
      "America/Chicago",
      "America/Indiana/Tell_City",
      "America/Indiana/Knox",
      "America/Menominee",
      "America/North_Dakota/Center",
      "America/North_Dakota/New_Salem",
      "America/North_Dakota/Beulah",
      "America/Denver",
      "America/Boise",
      "America/Phoenix",
      "America/Los_Angeles",
      "America/Anchorage",
      "America/Juneau",
      "America/Sitka",
      "America/Yakutat",
      "America/Nome",
      "America/Adak",
      "America/Metlakatla",
      "Pacific/Honolulu"
    ]
  },
  {
    "UY": [
      "America/Montevideo"
    ]
  },
  {
    "UZ": [
      "Asia/Samarkand",
      "Asia/Tashkent"
    ]
  },
  {
    "VA": [
      "Europe/Vatican"
    ]
  },
  {
    "VC": [
      "America/St_Vincent"
    ]
  },
  {
    "VE": [
      "America/Caracas"
    ]
  },
  {
    "VG": [
      "America/Tortola"
    ]
  },
  {
    "VI": [
      "America/St_Thomas"
    ]
  },
  {
    "VN": [
      "Asia/Ho_Chi_Minh"
    ]
  },
  {
    "VU": [
      "Pacific/Efate"
    ]
  },
  {
    "WF": [
      "Pacific/Wallis"
    ]
  },
  {
    "WS": [
      "Pacific/Apia"
    ]
  },
  {
    "YE": [
      "Asia/Aden"
    ]
  },
  {
    "YT": [
      "Indian/Mayotte"
    ]
  },
  {
    "ZA": [
      "Africa/Johannesburg"
    ]
  },
  {
    "ZM": [
      "Africa/Lusaka"
    ]
  },
  {
    "ZW": [
      "Africa/Harare"
    ]
  }
]

--
TV
--

Get the primary information about a TV series by id.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
<tr><td style="padding-left: 0;" colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-left: 0; width: 140px;">language</td><td>ISO 639-1 code.</td></tr>
<tr><td style="padding-left: 0; width: 140px;">append_to_response</td><td>Comma separated, any tv series method</td></tr>
</table>

GET /tv/{id}
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "acea50fb474c5d1f9b60cb14d7171ada"
{
  "backdrop_path": "/bzoZjhbpriBT2N5kwgK0weUfVOX.jpg",
  "created_by": [
    {
      "id": 66633,
      "name": "Vince Gilligan",
      "profile_path": "/rLSUjr725ez1cK7SKVxC9udO03Y.jpg"
    }
  ],
  "episode_run_time": [
    45,
    47
  ],
  "first_air_date": "2008-01-19",
  "genres": [
    {
      "id": 18,
      "name": "Drama"
    }
  ],
  "homepage": "http://www.amctv.com/shows/breaking-bad",
  "id": 1396,
  "in_production": false,
  "languages": [
    "en",
    "de",
    "ro",
    "es",
    "fa"
  ],
  "last_air_date": "2013-09-29",
  "name": "Breaking Bad",
  "networks": [
    {
      "id": 174,
      "name": "AMC"
    }
  ],
  "number_of_episodes": 62,
  "number_of_seasons": 5,
  "origin_country": [
    "US"
  ],
  "original_language": "en",
  "original_name": "Breaking Bad",
  "overview": "Breaking Bad is an American crime drama television series created and produced by Vince Gilligan. Set and produced in Albuquerque, New Mexico, Breaking Bad is the story of Walter White, a struggling high school chemistry teacher who is diagnosed with inoperable lung cancer at the beginning of the series. He turns to a life of crime, producing and selling methamphetamine, in order to secure his family's financial future before he dies, teaming with his former student, Jesse Pinkman. Heavily serialized, the series is known for positioning its characters in seemingly inextricable corners and has been labeled a contemporary western by its creator.",
  "popularity": 7.39820387093879,
  "poster_path": "/4yMXf3DW6oCL0lVPZaZM2GypgwE.jpg",
  "production_companies": [
    {
      "name": "Gran Via Productions",
      "id": 2605
    },
    {
      "name": "Sony Pictures Television",
      "id": 11073
    },
    {
      "name": "High Bridge Entertainment",
      "id": 33742
    }
  ],
  "seasons": [
    {
      "air_date": "2009-02-17",
      "episode_count": 6,
      "id": 3577,
      "poster_path": "/spPmYZAq2xLKQOEIdBPkhiRxrb9.jpg",
      "season_number": 0
    },
    {
      "air_date": "2008-01-19",
      "episode_count": 7,
      "id": 3572,
      "poster_path": "/dHCYpEoHEjAV6Xt3eyNthkdLRl3.jpg",
      "season_number": 1
    },
    {
      "air_date": "2009-03-08",
      "episode_count": 13,
      "id": 3573,
      "poster_path": "/ww6cDy0dhrVEdMqielNEsYz96mg.jpg",
      "season_number": 2
    },
    {
      "air_date": "2010-03-21",
      "episode_count": 13,
      "id": 3575,
      "poster_path": "/rINvcsYHUprsx9L8zNr5JltALda.jpg",
      "season_number": 3
    },
    {
      "air_date": "2011-07-17",
      "episode_count": 13,
      "id": 3576,
      "poster_path": "/ngnE7FFQqrrLgK3yVsv3kjwtQMZ.jpg",
      "season_number": 4
    },
    {
      "air_date": "2012-07-15",
      "episode_count": 16,
      "id": 3578,
      "poster_path": "/ih1JKNxEzW56azeFpEQmdu4poA4.jpg",
      "season_number": 5
    }
  ],
  "status": "Ended",
  "type": "Scripted",
  "vote_average": 9.2,
  "vote_count": 152
}

This method lets users get the status of whether or not the TV show has been rated or added to their favourite or watch lists. A [valid session](#authentication) id is required.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
<tr><td style="padding-left: 0;" colspan="2">session_id</td></tr>
</table>

GET /tv/{id}/account_states
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "81d02d846cfb9ed331681f6a9701aa5b"
{
  "id": 1396,
  "favorite": false,
  "rated": {
    "value": 8.5
  },
  "watchlist": false
}

Get the alternative titles for a specific show ID.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
</table>

GET /tv/{id}/alternative_titles
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "384570976cbf92e28714f8e5d6e46fa8"
{
  "id": 1396,
  "results": [
    {
      "title": "Totál szívás",
      "iso_3166_1": "HU"
    },
    {
      "title": "Bręstantis blogis",
      "iso_3166_1": "LT"
    }
  ]
}

Get the changes for a specific TV show id.

Changes are grouped by key, and ordered by date in descending order. By default, only the last 24 hours of changes are returned. The maximum number of days that can be returned in a single request is 14. The language is present on fields that are translatable.

TV changes are different than movie changes in that there are some edits on seasons and episodes that will create a change entry at the show level. They can be found under the `season` and `episode` keys. These keys will contain a `series_id` and `episode_id`. You can use the [/tv/season/{id}/changes](#get-%2F3%2Ftv%2Fseason%2F%7Bid%7D%2Fchanges) and [/tv/episode/{id}/changes](#get-%2F3%2Ftv%2Fepisode%2F%7Bid%7D%2Fchanges) methods to look up these specific changes.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
<tr><td style="padding-left: 0;" colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-left: 0; padding-right: 40px; width: 140px;">start_date</td><td>YYYY-MM-DD</td></tr>
<tr><td style="padding-left: 0; padding-right: 40px; width: 140px;">end_date</td><td>YYYY-MM-DD</td></tr>
</table>

GET /tv/{id}/changes
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "691e0c90d7869a78ead95cb777b870dd"
{
  "changes": [
    {
      "key": "translations",
      "items": [
        {
          "id": "537276630e0a2672e8002268",
          "action": "added",
          "time": "2014-05-13 19:45:39 UTC",
          "value": "de"
        }
      ]
    },
    {
      "key": "season",
      "items": [
        {
          "id": "5372775c0e0a2672eb0022fa",
          "action": "created",
          "time": "2014-05-13 19:49:48 UTC",
          "value": {
            "season_number": 1,
            "season_id": 60476
          }
        }
      ]
    },
    {
      "key": "languages",
      "items": [
        {
          "id": "537276850e0a2672ee0022aa",
          "action": "updated",
          "time": "2014-05-13 19:46:13 UTC",
          "value": [
            "de"
          ],
          "original_value": [
            "en"
          ]
        }
      ]
    },
    {
      "key": "overview",
      "items": [
        {
          "id": "537276850e0a2672ee0022ab",
          "action": "added",
          "time": "2014-05-13 19:46:13 UTC",
          "iso_639_1": "de",
          "value": "Alles was zählt ger ist nach Gute Zeiten, schlechte Zeiten und Unter uns die dritte Dailysoap von RTL"
        },
        {
          "id": "537277280e0a2672f1002254",
          "action": "updated",
          "time": "2014-05-13 19:48:56 UTC",
          "iso_639_1": "de",
          "value": "Alles was zählt ist nach Gute Zeiten, schlechte Zeiten und Unter uns die dritte Dailysoap von RTL",
          "original_value": "Alles was zählt ger ist nach Gute Zeiten, schlechte Zeiten und Unter uns die dritte Dailysoap von RTL"
        }
      ]
    },
    {
      "key": "status",
      "items": [
        {
          "id": "537276a40e0a2672e800226d",
          "action": "updated",
          "time": "2014-05-13 19:46:44 UTC",
          "value": "In Production",
          "original_value": "Ended"
        }
      ]
    },
    {
      "key": "production_companies",
      "items": [
        {
          "id": "537276d50e0a2672fb002352",
          "action": "added",
          "time": "2014-05-13 19:47:33 UTC",
          "value": {
            "id": 2018,
            "name": "Grundy UFA TV Produktions"
          }
        }
      ]
    },
    {
      "key": "season_regular",
      "items": [
        {
          "id": "537277680e0a2672eb002300",
          "action": "added",
          "time": "2014-05-13 19:50:00 UTC",
          "value": {
            "credit_id": "52583487760ee36aaa91fe7e",
            "character": "Simone Steinkamp",
            "add_to_every_season": true
          }
        },
        {
          "id": "537277820e0a2672f4002331",
          "action": "added",
          "time": "2014-05-13 19:50:26 UTC",
          "value": {
            "credit_id": "52583488760ee36aaa91fe95",
            "character": "Ingo Zadek",
            "add_to_every_season": true
          }
        }
      ]
    }
  ]
}

Get the content ratings for a specific TV show id.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
</table>

GET /tv/{id}/content_ratings
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "2709fe3f55707aaf5bc6c308d1f2b01c"
{
  "results": [
    {
      "iso_3166_1": "US",
      "rating": "TV-MA"
    }
  ],
  "id": 1396
}

Get the cast & crew information about a TV series. Just like the website, we pull this information from the last season of the series.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
<tr><td style="padding-left: 0;" colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-left: 0; width: 140px;">language</td><td>ISO 639-1 code.</td></tr>
<tr><td style="padding-left: 0; width: 140px;">append_to_response</td><td>Comma separated, any collection method</td></tr>
</table>

GET /tv/{id}/credits
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "751ddafde032eeed659e5a4f858431f0"
{
  "cast": [
    {
      "character": "Walter White",
      "credit_id": "52542282760ee313280017f9",
      "id": 17419,
      "name": "Bryan Cranston",
      "profile_path": "/vr3YaG9ehqQY82fsxOFZvlqncX7.jpg",
      "order": 0
    },
    {
      "character": "Skyler White",
      "credit_id": "52542282760ee3132800181b",
      "id": 134531,
      "name": "Anna Gunn",
      "profile_path": "/lKlGjfmu9mJcF4upNrhtG3X9uyq.jpg",
      "order": 1
    },
    {
      "character": "Jesse Pinkman",
      "credit_id": "52542282760ee31328001845",
      "id": 84497,
      "name": "Aaron Paul",
      "profile_path": "/oTceEUb6A9Bg6DeUTJBTETUOEAy.jpg",
      "order": 2
    },
    {
      "character": "Hank Schrader",
      "credit_id": "52542283760ee3132800187b",
      "id": 14329,
      "name": "Dean Norris",
      "profile_path": "/500eNhWneDTXQuEXg6BR269IjHr.jpg",
      "order": 3
    },
    {
      "character": "Marie Schrader",
      "credit_id": "52542283760ee31328001891",
      "id": 1217934,
      "name": "Betsy Brandt",
      "profile_path": "/zpmsca1HCVqYrtWXV9xdmsECDTI.jpg",
      "order": 4
    },
    {
      "character": "Walter White Jr.",
      "credit_id": "52542284760ee313280018a9",
      "id": 1223196,
      "name": "RJ Mitte",
      "profile_path": "/hO5HJKM6p6SQjpdV8Fs64BJWzmT.jpg",
      "order": 5
    },
    {
      "character": "Saul Goodman",
      "credit_id": "5271b180760ee35afc09bb8d",
      "id": 59410,
      "name": "Bob Odenkirk",
      "profile_path": "/p905eCTyeda8xqMSKoRY14ZvdiH.jpg",
      "order": 11
    }
  ],
  "crew": [
    {
      "department": "Production",
      "id": 29779,
      "name": "Michelle MacLaren",
      "job": "Executive Producer",
      "profile_path": null
    },
    {
      "department": "Production",
      "id": 5162,
      "name": "Mark Johnson",
      "job": "Executive Producer",
      "profile_path": "/yKGF6cbzyP03Gl1QhVLCu1gWSW6.jpg"
    },
    {
      "department": "Production",
      "id": 66633,
      "name": "Vince Gilligan",
      "job": "Executive Producer",
      "profile_path": "/wSTvJGz7QbJf1HK2Mv1Cev6W9TV.jpg"
    }
  ],
  "id": 1396
}

Get the external ids that we have stored for a TV series.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
<tr><td style="padding-left: 0;" colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-left: 0; width: 140px;">language</td><td>ISO 639-1 code.</td></tr>
</table>

GET /tv/{id}/external_ids
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "a25c7f77e4d2fa0bca41a8a0e956d7b4"
{
  "imdb_id": "tt0903747",
  "freebase_id": "/en/breaking_bad",
  "freebase_mid": "/m/03d34x8",
  "id": 1396,
  "tvdb_id": 81189,
  "tvrage_id": 18164
}

Get the images (posters and backdrops) for a TV series.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
<tr><td style="padding-left: 0;" colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-left: 0; width: 140px;">language</td><td>ISO 639-1 code.</td></tr>
<tr><td style="padding-left: 0; width: 140px;">include_image_language</td><td>Comma separated, a valid ISO 69-1. Maximum 5 per request.</td></tr>
</table>

GET /tv/{id}/images
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "ae542c5a1816a477563ccfa97b68634d"
{
  "backdrops": [
    {
      "aspect_ratio": 1.78,
      "file_path": "/dRaV8HGx7Z9xmw77qSs8prp5OuI.jpg",
      "height": 720.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 1280.0
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/mojM8NJQlpTWpiD68rcOWnzcEVR.jpg",
      "height": 720.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 1280.0
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/nIh1ygw5kVRaEFqXcj8CTi3pKK5.jpg",
      "height": 720.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 1280.0
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/bbFaLaLv8TwR7KKnZ3reHGS4VQO.jpg",
      "height": 720.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 1280.0
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/bFWW5vVZS1YW0a0Cq0liYakj6vt.jpg",
      "height": 1080.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 1920.0
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/siv4GtEMtrqljl0qiTGCvzolXWH.jpg",
      "height": 1080.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 1920.0
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/wvFckSZAG1OVPEwM8YUtLC3DOWk.jpg",
      "height": 720.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 1280.0
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/162VmHV23N1FqQhYjZiZCH9GkQA.jpg",
      "height": 720.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 1280.0
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/yG8QDHyrVMBe5H7Q7cIPKq45Iq0.jpg",
      "height": 720.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 1280.0
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/gzqjlgcYIHZSnUS1bnxVDxQwCR0.jpg",
      "height": 720.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 1280.0
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/eIOpMbnMqiAgBS8cZocAATRsb28.jpg",
      "height": 1080.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 1920.0
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/9V1yohvMOePVd6b1WiUItPEN8sK.jpg",
      "height": 720.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 1280.0
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/bzoZjhbpriBT2N5kwgK0weUfVOX.jpg",
      "height": 1080.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 1920.0
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/n5kIjzyYNdDfJbfNkG68j3qDm6n.jpg",
      "height": 1080.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 1920.0
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/qfWveejMEeA4zGgscX8YUUTLuTB.jpg",
      "height": 1080.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 1920.0
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/6tRuCy7VXidZVRXIR4ThBx2omK3.jpg",
      "height": 1080.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 1920.0
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/3FnSPz1nqkaW65dj4BWRN185adp.jpg",
      "height": 1080.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 1920.0
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/va7jNaQUsIdWADokQSdPXeJYk1O.jpg",
      "height": 720.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 1280.0
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/nXlTbIrgroOCCKQSsMizmqQ4NAG.jpg",
      "height": 720.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 1280.0
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/bZ23G5JiNmeZG9t6hRMLhY9W3gP.jpg",
      "height": 720.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 1280.0
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/eSzpy96DwBujGFj0xMbXBcGcfxX.jpg",
      "height": 1080.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 1920.0
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/mrPw1gAXXV8OYQTr8ipKuYTuaa6.jpg",
      "height": 1080.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 1920.0
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/gVqPqjyiDmpoNJOFjvg0b8WCC3R.jpg",
      "height": 1080.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 1920.0
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/bagJdiz7JzB1bb87x24qKwrgWwK.jpg",
      "height": 1080.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 1920.0
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/an69IYogpnY2spo1dGVjmTy0m49.jpg",
      "height": 1080.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 1920.0
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/eCaT3wx244KPmnMchvag3kSGGw5.jpg",
      "height": 1080.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 1920.0
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/u3TcKJ1qqgd8D3Dd73FnIMVVsur.jpg",
      "height": 1080.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 1920.0
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/yFVUqiugxQLLrYxwsIcGQ3P8FWO.jpg",
      "height": 1080.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 1920.0
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/qamG9LeH42UombW1bfZlIlzO5Y1.jpg",
      "height": 1080.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 1920.0
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/6u4YG0oNzhFaFJs080ljlpSgpfD.jpg",
      "height": 1080.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 1920.0
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/ewRICWQvNXsViiUouM3M3jm5VyI.jpg",
      "height": 1080.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 1920.0
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/qgSmYpFBZdfHbjssjJN1XOrb2Us.jpg",
      "height": 1080.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 1920.0
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/tJAeZ3OoWDC1LVe7IBmGnBSOYZX.jpg",
      "height": 1080.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 1920.0
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/zzSjSRC04z7dc4xQopWTJYmXb9g.jpg",
      "height": 1080.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 1920.0
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/vIgu5fsJH50QcSz2hmfaqJHovUo.jpg",
      "height": 1080.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 1920.0
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/upeNbWF5wKZUOUcUW4EnVfYYjRo.jpg",
      "height": 1080.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 1920.0
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/oyOGhA8G74rd8c6VdeU3Mi7v8n7.jpg",
      "height": 1080.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 1920.0
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/2X0RK6uztO7J5XBSRtRcNGlzGml.jpg",
      "height": 1080.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 1920.0
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/gN14vIo5T2G3kwtdS1lHuFeJXJ9.jpg",
      "height": 1080.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 1920.0
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/2cxsSpTNb8SFczKLrnXlhw2p7hW.jpg",
      "height": 1080.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 1920.0
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/yjcJtXWrQfU96l3b46iMvNNpg3Z.jpg",
      "height": 1080.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 1920.0
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/5sHew9rjOsALuDVxwu0pJexOXWA.jpg",
      "height": 1080.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 1920.0
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/61nBiocMvOxvPtJgxk3GLCePvcf.jpg",
      "height": 1080.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 1920.0
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/aIOpF9Fp1SbYpSusJ3scDTYrqod.jpg",
      "height": 1080.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 1920.0
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/bTJLGUbqcGAuZnNvp59zjqqLzOR.jpg",
      "height": 1080.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 1920.0
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/y3Roit8o1v7EYXGqH7AQMNUBZQR.jpg",
      "height": 1080.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 1920.0
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/mpfWjvSmNaYCzfUmfnGe1A1wkbu.jpg",
      "height": 1080.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 1920.0
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/77q8qOwsEJyZGDf3nzhq13bg98X.jpg",
      "height": 1080.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 1920.0
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/zintOUBVjVapaNN3vAmm1rcfY1p.jpg",
      "height": 1080.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 1920.0
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/rmOmfel02YAgVRUdb9tlRyLW8mm.jpg",
      "height": 1080.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 1920.0
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/23RzDwwd78gCffMoBXoOKKSRlcr.jpg",
      "height": 1080.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 1920.0
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/m5apfrjjgc7jbOwRTXqDF5EXXfO.jpg",
      "height": 1080.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 1920.0
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/nxVC9a6TjeoKr6Ezkfz1jRSlf6L.jpg",
      "height": 1080.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 1920.0
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/qnnW0CY89lQZPgNrwpARJOBQtI1.jpg",
      "height": 1080.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 1920.0
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/k2N2DpK7kDcM0rXLKuTV6bUTIZN.jpg",
      "height": 1080.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 1920.0
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/ebQo1jOfx9UR9cPzBa6bHpxMWhq.jpg",
      "height": 1080.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 1920.0
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/rMvlRVBngldwWCoGqxQoPdJRnwC.jpg",
      "height": 1080.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 1920.0
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/abFs25RWEvn8WkLholkqFAUC8OF.jpg",
      "height": 1080.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 1920.0
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/lPQn4F48YL5IwwrjPx6QRRuDyjR.jpg",
      "height": 1080.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 1920.0
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/v5F4chUYiRnH9cI2JCpWrrK3q6D.jpg",
      "height": 1080.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 1920.0
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/ymrPqhHl8v9tf3eK3f9byggZ0n7.jpg",
      "height": 1080.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 1920.0
    },
    {
      "aspect_ratio": 1.78,
      "file_path": "/99PL1OnOT9PVZx09qerR0s1g0Gx.jpg",
      "height": 1080.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 1920.0
    }
  ],
  "id": 1396,
  "posters": [
    {
      "aspect_ratio": 0.68,
      "file_path": "/iRDNn9EHKuBhGa77UBteazvsZa1.jpg",
      "height": 1000.0,
      "iso_639_1": null,
      "vote_average": 5.29761904761905,
      "vote_count": 0,
      "width": 680.0
    },
    {
      "aspect_ratio": 0.68,
      "file_path": "/51H0ifZ0TM5sAp4yBvolUpFaPaC.jpg",
      "height": 1000.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 680.0
    },
    {
      "aspect_ratio": 0.68,
      "file_path": "/xkbl9GV6p9JwDPXIYwODVcunlRT.jpg",
      "height": 1000.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 680.0
    },
    {
      "aspect_ratio": 0.68,
      "file_path": "/u69b0G6HlfrwkchQdn8lzEEjFxt.jpg",
      "height": 1000.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 680.0
    },
    {
      "aspect_ratio": 0.68,
      "file_path": "/8VxH7FSiCDmSZyfxsqz25ZWbP7O.jpg",
      "height": 1000.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 680.0
    },
    {
      "aspect_ratio": 0.68,
      "file_path": "/hyWz3p2KCxw3T3OTyFzQgYvkhJf.jpg",
      "height": 1000.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 680.0
    },
    {
      "aspect_ratio": 0.68,
      "file_path": "/7pFpIXpn2DOst3bOMQIqCEHeAWS.jpg",
      "height": 1000.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 680.0
    },
    {
      "aspect_ratio": 0.68,
      "file_path": "/mmtUaqRpsWcqb4ISF3CaoK0ptO7.jpg",
      "height": 1000.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 680.0
    },
    {
      "aspect_ratio": 0.68,
      "file_path": "/i5bfplXbyLm7BvofUBVj7Zaan4w.jpg",
      "height": 1000.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 680.0
    },
    {
      "aspect_ratio": 0.68,
      "file_path": "/mVIvcV9IEsfnTdbNXPNWkiU6N7t.jpg",
      "height": 1000.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 680.0
    },
    {
      "aspect_ratio": 0.68,
      "file_path": "/e6Ydx9xHuiQa6qsx20LR4x71DCS.jpg",
      "height": 1000.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 680.0
    },
    {
      "aspect_ratio": 0.68,
      "file_path": "/3bHLOzAOY2Rn4c5YrvALzZVE6tg.jpg",
      "height": 1000.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 680.0
    },
    {
      "aspect_ratio": 0.68,
      "file_path": "/t9r2UYuRppdZKmeYHSgdx09uHzl.jpg",
      "height": 1000.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 680.0
    },
    {
      "aspect_ratio": 0.68,
      "file_path": "/3feoWsQPtBdUhrOCVPB1iazSb2W.jpg",
      "height": 1000.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 680.0
    },
    {
      "aspect_ratio": 0.68,
      "file_path": "/fO3OqQQEIZN44K9jcs0Q4JuFHOl.jpg",
      "height": 1000.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 680.0
    },
    {
      "aspect_ratio": 0.68,
      "file_path": "/uL7s6lRsApvFuxQFqT5FA1SuSdX.jpg",
      "height": 1000.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 680.0
    },
    {
      "aspect_ratio": 0.68,
      "file_path": "/6uiT7QidRjVVywFgOAUQoY7iPup.jpg",
      "height": 1000.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 680.0
    },
    {
      "aspect_ratio": 0.68,
      "file_path": "/1gNZDkSc1kb9azLYVQHS9DCBpAs.jpg",
      "height": 1000.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 680.0
    },
    {
      "aspect_ratio": 0.68,
      "file_path": "/zdipQZmcmaCUNGsXP6OdBEjDCSP.jpg",
      "height": 1000.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 680.0
    },
    {
      "aspect_ratio": 0.68,
      "file_path": "/xd8tk9ox8QvQClvD48bmQc2t6dx.jpg",
      "height": 1000.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 680.0
    },
    {
      "aspect_ratio": 0.68,
      "file_path": "/pOLOC2tzz70N1RngVZtfhJuB5Fp.jpg",
      "height": 1000.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 680.0
    },
    {
      "aspect_ratio": 0.68,
      "file_path": "/85fOzuORUuQlDPSNkFCDnaUXZtv.jpg",
      "height": 1000.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 680.0
    },
    {
      "aspect_ratio": 0.68,
      "file_path": "/21YJ4mM7C9V5e2esqeB4AoBjKPy.jpg",
      "height": 1000.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 680.0
    },
    {
      "aspect_ratio": 0.68,
      "file_path": "/xMvrBjxu2EskHhiZ2sNgufBhGzF.jpg",
      "height": 1000.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 680.0
    },
    {
      "aspect_ratio": 0.68,
      "file_path": "/wnoMOVO06oI55fWrR0mQUbquERh.jpg",
      "height": 1000.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 680.0
    },
    {
      "aspect_ratio": 0.68,
      "file_path": "/nXXSNb8qXn66DeZL0BIG79Omyo2.jpg",
      "height": 1000.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 680.0
    },
    {
      "aspect_ratio": 0.68,
      "file_path": "/biPjf15logCBAIptiM1YYCOeTxE.jpg",
      "height": 1000.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 680.0
    },
    {
      "aspect_ratio": 0.68,
      "file_path": "/ccxTzJHWYhAGg3MFaTzGfpFAXGi.jpg",
      "height": 1000.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 680.0
    },
    {
      "aspect_ratio": 0.68,
      "file_path": "/oAJvOW85rXKs8GlWoT5jssd7qNU.jpg",
      "height": 1000.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 680.0
    },
    {
      "aspect_ratio": 0.68,
      "file_path": "/fzNaunYfNFvVesiIvcEgr8h4qix.jpg",
      "height": 1000.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 680.0
    }
  ]
}

Get the plot keywords for a specific TV show id.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
<tr><td style="padding-left: 0;" colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-left: 0; width: 140px;">append_to_response</td><td>Comma separated, any TV method</td></tr>
</table>

GET /tv/{id}/keywords
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "06ed999e72a65a881228dda2f93186ae"
{
  "id": 1396,
  "results": [
    {
      "id": 41525,
      "name": "high school teacher"
    },
    {
      "id": 195308,
      "name": "crystal method"
    },
    {
      "id": 2231,
      "name": "drug dealer"
    }
  ]
}

This method lets users create (or delete) a rating on a TV series. A [valid session](#authentication) id **or** [guest session](#get-%2F3%2Fauthentication%2Fguest_session%2Fnew) id is required.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
<tr><td style="padding-left: 0;" colspan="2">session_id</td></tr>
<tr><td style="padding-left: 0;" colspan="2">guest_session_id</td></tr>
<tr><td style="padding-left: 0;" colspan="2"><strong>Required JSON Body (when POST-ing)</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">value</td></tr>
</table>

POST /tv/{id}/rating
> Accept: application/json
> Content-Type: application/json
{
    "value": 7.5
}
< 200
< Content-Type: application/json
{
    "status_code": 1,
    "status_message": "Success"
}

DELETE /tv/{id}/rating
> Accept: application/json
> Content-Type: application/json
< 200
< Content-Type: application/json
{
  "status_code": 13,
  "status_message": "The item/record was deleted successfully."
}

Get the similar TV shows for a specific tv id.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td colspan="2">api_key</td></tr>
<tr><td colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-right: 40px; width: 140px;">page</td><td>Minimum 1, maximum 1000.</td></tr>
<tr><td style="padding-right: 40px; width: 140px;">language</td><td>ISO 639-1 code.</td></tr>
<tr><td style="width: 140px;">append_to_response</td><td>Comma separated, any TV method</td></tr>
</table>

GET /tv/{id}/similar
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "ed6d3600ec1cf6d4ab30c24f6f33625e"
{
  "page": 1,
  "results": [
    {
      "backdrop_path": null,
      "id": 15985,
      "original_name": "Tom Brown's Schooldays",
      "first_air_date": "1971-11-14",
      "poster_path": "/7IDcPPyw0DYSH7xS583ufVZBhnd.jpg",
      "popularity": 0.3,
      "name": "Tom Brown's Schooldays",
      "vote_average": 0.0,
      "vote_count": 0
    },
    {
      "backdrop_path": "/2Fg9H4TBJlXoZzZ4ixnAg0YV9xS.jpg",
      "id": 47141,
      "original_name": "The Bridge",
      "first_air_date": "2013-07-10",
      "poster_path": "/zBHarUl03wPbJsPOMCiKqtgsjmQ.jpg",
      "popularity": 0.39,
      "name": "The Bridge",
      "vote_average": 8.3,
      "vote_count": 2
    },
    {
      "backdrop_path": null,
      "id": 16092,
      "original_name": "Tom Brown's Schooldays",
      "first_air_date": null,
      "poster_path": null,
      "popularity": 0.4,
      "name": "Tom Brown's Schooldays",
      "vote_average": 0.0,
      "vote_count": 0
    },
    {
      "backdrop_path": "/5uButYLL7IHp26NrWKjn3PzK4Ls.jpg",
      "id": 1413,
      "original_name": "American Horror Story",
      "first_air_date": "2011-10-05",
      "poster_path": "/aq7GRQMSWrVeSU9P1xrlEibjdP5.jpg",
      "popularity": 1.85595210803436,
      "name": "American Horror Story",
      "vote_average": 8.2,
      "vote_count": 11
    },
    {
      "backdrop_path": null,
      "id": 60671,
      "original_name": "Sivi dom",
      "first_air_date": "1986-01-01",
      "poster_path": "/8kZ0dLA4tF6dpUpyg0buHvdDKTe.jpg",
      "popularity": 0.4,
      "name": "Grey Home",
      "vote_average": 9.0,
      "vote_count": 1
    },
    {
      "backdrop_path": "/A8FJKwNKCZbrUdNSADzj9VMOTAN.jpg",
      "id": 45238,
      "original_name": "Stephen Hawking's Grand Design",
      "first_air_date": "2012-06-02",
      "poster_path": "/7tdNpS9JcDmk1ZUCVsUq8y72SkL.jpg",
      "popularity": 0.26,
      "name": "Stephen Hawking's Grand Design",
      "vote_average": 10.0,
      "vote_count": 1
    },
    {
      "backdrop_path": null,
      "id": 60593,
      "original_name": "Ai Shimai",
      "first_air_date": "2001-04-27",
      "poster_path": "/e6Ncxj1yLNU6MbpKCfqg76eGZhQ.jpg",
      "popularity": 0.4,
      "name": "Ai Shimai",
      "vote_average": 8.0,
      "vote_count": 1
    },
    {
      "backdrop_path": null,
      "id": 14918,
      "original_name": "Me Too!",
      "first_air_date": null,
      "poster_path": null,
      "popularity": 0.4,
      "name": "Me Too!",
      "vote_average": 0.0,
      "vote_count": 0
    },
    {
      "backdrop_path": null,
      "id": 14921,
      "original_name": "General Motors Theatre",
      "first_air_date": null,
      "poster_path": null,
      "popularity": 0.4,
      "name": "General Motors Theatre",
      "vote_average": 0.0,
      "vote_count": 0
    },
    {
      "backdrop_path": null,
      "id": 14922,
      "original_name": "Head Start",
      "first_air_date": null,
      "poster_path": null,
      "popularity": 0.4,
      "name": "Head Start",
      "vote_average": 0.0,
      "vote_count": 0
    },
    {
      "backdrop_path": "/uASK0WiXxLzADDR5JldRTs01a3s.jpg",
      "id": 14929,
      "original_name": "Heartland",
      "first_air_date": "2007-10-14",
      "poster_path": "/7OT7XGz0bJJb7ZNaxBGcFygWi3s.jpg",
      "popularity": 0.4,
      "name": "Heartland",
      "vote_average": 0.0,
      "vote_count": 0
    },
    {
      "backdrop_path": null,
      "id": 14940,
      "original_name": "Wind on Water",
      "first_air_date": "1998-10-17",
      "poster_path": null,
      "popularity": 0.4,
      "name": "Wind on Water",
      "vote_average": 0.0,
      "vote_count": 0
    },
    {
      "backdrop_path": null,
      "id": 14944,
      "original_name": "Robert Montgomery Presents",
      "first_air_date": "1950-01-30",
      "poster_path": null,
      "popularity": 0.762553466886709,
      "name": "Robert Montgomery Presents",
      "vote_average": 0.0,
      "vote_count": 0
    },
    {
      "backdrop_path": "/gqbg7pdHwtJguo7m9g9NQRiqURA.jpg",
      "id": 14968,
      "original_name": "Empire Falls",
      "first_air_date": null,
      "poster_path": "/4cozaIrgo9irgfX88Z2ghDBfvFL.jpg",
      "popularity": 0.2,
      "name": "Empire Falls",
      "vote_average": 0.0,
      "vote_count": 0
    },
    {
      "backdrop_path": null,
      "id": 14977,
      "original_name": "Seaway",
      "first_air_date": "1965-09-16",
      "poster_path": null,
      "popularity": 0.48,
      "name": "Seaway",
      "vote_average": 0.0,
      "vote_count": 0
    },
    {
      "backdrop_path": null,
      "id": 14980,
      "original_name": "The Richard Boone Show",
      "first_air_date": "1963-09-24",
      "poster_path": null,
      "popularity": 0.278,
      "name": "The Richard Boone Show",
      "vote_average": 0.0,
      "vote_count": 0
    },
    {
      "backdrop_path": null,
      "id": 14983,
      "original_name": "413 Hope St.",
      "first_air_date": "1997-09-11",
      "poster_path": null,
      "popularity": 0.2,
      "name": "413 Hope St.",
      "vote_average": 0.0,
      "vote_count": 0
    },
    {
      "backdrop_path": null,
      "id": 14986,
      "original_name": "Laurel Avenue",
      "first_air_date": null,
      "poster_path": null,
      "popularity": 0.4,
      "name": "Laurel Avenue",
      "vote_average": 0.0,
      "vote_count": 0
    },
    {
      "backdrop_path": "/sms3KTHY1u3hqLTtcIGQmbcnNMh.jpg",
      "id": 15003,
      "original_name": "Tell Me You Love Me",
      "first_air_date": "2007-09-09",
      "poster_path": "/7VHVNdqWIvKTLupN4SXeH4xrXX2.jpg",
      "popularity": 0.4577518425,
      "name": "Tell Me You Love Me",
      "vote_average": 0.0,
      "vote_count": 0
    },
    {
      "backdrop_path": null,
      "id": 15008,
      "original_name": "Something in the Air",
      "first_air_date": "2000-01-17",
      "poster_path": null,
      "popularity": 0.500518,
      "name": "Something in the Air",
      "vote_average": 0.0,
      "vote_count": 0
    }
  ],
  "total_pages": 339,
  "total_results": 6763
}

Get the list of translations that exist for a TV series. These translations cascade down to the episode level.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
<tr><td style="padding-left: 0;" colspan="2"><strong>Optional Parameters</strong></td></tr>
</table>

GET /tv/{id}/translations
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "07deaef7646538ee3c4e46b9f8e5f26c"
{
  "id": 1396,
  "translations": [
    {
      "iso_639_1": "en",
      "name": "English",
      "english_name": "English"
    },
    {
      "iso_639_1": "nl",
      "name": "Nederlands",
      "english_name": "Dutch"
    },
    {
      "iso_639_1": "tr",
      "name": "Türkçe",
      "english_name": "Turkish"
    },
    {
      "iso_639_1": "sk",
      "name": "Slovenščina",
      "english_name": "Slovak"
    },
    {
      "iso_639_1": "de",
      "name": "Deutsch",
      "english_name": "German"
    },
    {
      "iso_639_1": "ru",
      "name": "Pусский",
      "english_name": "Russian"
    },
    {
      "iso_639_1": "fr",
      "name": "Français",
      "english_name": "French"
    },
    {
      "iso_639_1": "hu",
      "name": "Magyar",
      "english_name": "Hungarian"
    },
    {
      "iso_639_1": "zh",
      "name": "中国",
      "english_name": "Mandarin"
    }
  ]
}

Get the videos that have been added to a TV series (trailers, opening credits, etc...)

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
<tr><td style="padding-left: 0;" colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-left: 0; width: 140px;">language</td><td>ISO 639-1 code.</td></tr>
</table>

GET /tv/{id}/videos
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "ae542c5a1816a477563ccfa97b68634d"
{
  "id": 1396,
  "results": [
    {
      "id": "5335e212c3a368266100002e",
      "iso_639_1": "en",
      "key": "TDFAYRtrYuk",
      "name": "Opening Credits (Full)",
      "site": "YouTube",
      "size": 720,
      "type": "Opening Credits"
    },
    {
      "id": "5335e299c3a368265000001d",
      "iso_639_1": "en",
      "key": "6OdIbPMU720",
      "name": "Opening Credits (Short)",
      "site": "YouTube",
      "size": 480,
      "type": "Opening Credits"
    }
  ]
}

Get the latest TV show id.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
</table>

GET /tv/latest
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "1e8786816109333f1360dd6b3a176823"
{
  "backdrop_path": null,
  "created_by": [
    {
      "id": 1323358,
      "name": "Willy Purucker",
      "profile_path": null
    }
  ],
  "episode_run_time": [
    60
  ],
  "first_air_date": "1989-11-14",
  "genres": [
    {
      "id": 18,
      "name": "Drama"
    }
  ],
  "homepage": "",
  "id": 60968,
  "in_production": false,
  "languages": [
    "de"
  ],
  "last_air_date": "1989-11-14",
  "name": "Löwengrube",
  "networks": [
    {
      "id": 871,
      "name": "Bayerischer Rundfunk"
    }
  ],
  "number_of_episodes": null,
  "number_of_seasons": 1,
  "original_name": "Löwengrube",
  "origin_country": [
    "DE"
  ],
  "overview": "",
  "popularity": 0.0,
  "poster_path": null,
  "seasons": [
    {
      "air_date": "1989-11-14",
      "id": 61443,
      "poster_path": null,
      "season_number": 1
    }
  ],
  "status": "Ended",
  "vote_average": 0.0,
  "vote_count": 0
}

Get the list of TV shows that are currently on the air. This query looks for any TV show that has an episode with an air date in the next 7 days.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td colspan="2">api_key</td></tr>
<tr><td colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-right: 40px; width: 140px;">page</td><td>Minimum 1, maximum 1000.</td></tr>
<tr><td style="padding-right: 40px; width: 140px;">language</td><td>ISO 639-1 code.</td></tr>
</table>

GET /tv/on_the_air
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "d8207977649efbe7d07766cc2a5d2adb"
{
  "page": 1,
  "results": [
    {
      "backdrop_path": "/kohPYEYHuQLWX3gjchmrWWOEycD.jpg",
      "first_air_date": "2015-06-12",
      "genre_ids": [
        878
      ],
      "id": 62425,
      "original_language": "en",
      "original_name": "Dark Matter",
      "overview": "The six-person crew of a derelict spaceship awakens from stasis in the farthest reaches of space. Their memories wiped clean, they have no recollection of who they are or how they got on board. The only clue to their identities is a cargo bay full of weaponry and a destination: a remote mining colony that is about to become a war zone. With no idea whose side they are on, they face a deadly decision. Will these amnesiacs turn their backs on history, or will their pasts catch up with them?",
      "origin_country": [
        "CA"
      ],
      "poster_path": "/iDSXueb3hjerXMq5w92rBP16LWY.jpg",
      "popularity": 27.373853,
      "name": "Dark Matter",
      "vote_average": 6.4,
      "vote_count": 4
    },
    {
      "backdrop_path": "/jXpndJTekLFYcx3xX0H3sDqFnJU.jpg",
      "first_air_date": "2015-06-02",
      "genre_ids": [
        878
      ],
      "id": 62196,
      "original_language": "en",
      "original_name": "Stitchers",
      "overview": "A young woman is recruited into a secret government agency to be “stitched” into the minds of the recently deceased, using their memories to investigate murders.",
      "origin_country": [
        "US"
      ],
      "poster_path": "/2Ni5y120OSt6lSHGDavU1nDrHHP.jpg",
      "popularity": 19.064832,
      "name": "Stitchers",
      "vote_average": 7.5,
      "vote_count": 4
    },
    {
      "backdrop_path": "/t6JB8bgobNr7qNnO1vXBOYayo5T.jpg",
      "first_air_date": "2015-06-14",
      "genre_ids": [
        18,
        878
      ],
      "id": 62822,
      "original_language": "en",
      "original_name": "Humans",
      "overview": "In a parallel present where the latest must-have gadget for any busy family is a 'Synth' - a highly-developed robotic servant that's so similar to a real human it's transforming the way we live.",
      "origin_country": [
        "GB",
        "US"
      ],
      "poster_path": "/gJCyS65ieDT827F2NR9Nx9ZLuw5.jpg",
      "popularity": 18.931591,
      "name": "Humans",
      "vote_average": 9.2,
      "vote_count": 3
    },
    {
      "backdrop_path": "/tacmO3UpPX26bb1EQz8c11lQezR.jpg",
      "first_air_date": "2015-06-16",
      "genre_ids": [
        35
      ],
      "id": 62771,
      "original_language": "en",
      "original_name": "Clipped",
      "overview": "Clipped centers on a group of barbershop coworkers who all went to high school together but ran in very different crowds. Now they find themselves working together at Buzzy's, a barbershop in Charlestown, Massachusetts.",
      "origin_country": [
        "US"
      ],
      "poster_path": "/a7Tp2hVM3aMINT9L5blINV6AweY.jpg",
      "popularity": 18.628063,
      "name": "Clipped",
      "vote_average": 2.3,
      "vote_count": 2
    },
    {
      "backdrop_path": "/bYjc2HolyI2HWGhftWsyo5HgqTC.jpg",
      "first_air_date": "2014-06-27",
      "genre_ids": [
        10751,
        35
      ],
      "id": 46028,
      "original_language": "en",
      "original_name": "Girl Meets World",
      "overview": "Based on ABC's hugely popular 1993-2000 sitcom, this comedy, set in New York City, tells the wonderfully funny, heartfelt stories that \"Boy Meets World\" is renowned for – only this time from a tween girl's perspective – as the curious and bright 7th grader Riley Matthews and her quick-witted friend Maya Fox embark on an unforgettable middle school experience. But their plans for a carefree year will be adjusted slightly under the watchful eyes of Riley's parents – dad Cory, who's also a faculty member (and their new History teacher), and mom Topanga, who owns a trendy afterschool hangout that specializes in pudding.",
      "origin_country": [
        "US"
      ],
      "poster_path": "/yeRT4hAVoCz6RjLUiwdZ8Of34Th.jpg",
      "popularity": 18.319562,
      "name": "Girl Meets World",
      "vote_average": 7.5,
      "vote_count": 1
    },
    {
      "backdrop_path": "/trypBo0ldWAZ4ULeQDjkzzW7To3.jpg",
      "first_air_date": "2010-06-08",
      "genre_ids": [
        9648,
        18
      ],
      "id": 31917,
      "original_language": "en",
      "original_name": "Pretty Little Liars",
      "overview": "Based on the Pretty Little Liars series of young adult novels by Sara Shepard, the series follows the lives of four girls — Spencer, Hanna, Aria, and Emily — whose clique falls apart after the disappearance of their queen bee, Alison. One year later, they begin receiving messages from someone using the name \"A\" who threatens to expose their secrets — including long-hidden ones they thought only Alison knew. ",
      "origin_country": [
        "US"
      ],
      "poster_path": "/2rOLNdUPahiAq6VuoA1I6sA45cs.jpg",
      "popularity": 15.188609,
      "name": "Pretty Little Liars",
      "vote_average": 6.8,
      "vote_count": 27
    },
    {
      "backdrop_path": "/lkQ4fEbAkrHeKWq8GAbFqnwXweo.jpg",
      "first_air_date": "2013-10-10",
      "genre_ids": [
        18
      ],
      "id": 40008,
      "original_language": "en",
      "original_name": "Hannibal",
      "overview": "Hannibal is an American thriller television series developed by Bryan Fuller for NBC. The series is based on characters and elements appearing in the novel Red Dragon by Thomas Harris and focuses on the budding relationship between FBI special investigator Will Graham and Dr. Hannibal Lecter, a forensic psychiatrist destined to become Graham's most cunning enemy.",
      "origin_country": [
        "US"
      ],
      "poster_path": "/kArcGHvTb7m1v6RFHdU40jpiGZD.jpg",
      "popularity": 14.592192,
      "name": "Hannibal",
      "vote_average": 8.4,
      "vote_count": 52
    },
    {
      "backdrop_path": "/bzFDAvhN6dcUOTbXduHl9pMvdX0.jpg",
      "first_air_date": "2015-06-18",
      "genre_ids": [
        18
      ],
      "id": 62458,
      "original_language": "en",
      "original_name": "The Astronaut Wives Club",
      "overview": "Based on the book by Lily Koppel, this 10 episode limited series tells the story of the women who were key players behind some of the biggest events in American history. As America’s astronauts were launched on death-defying missions, Life Magazine documented the astronauts’ families, capturing the behind-the-scenes lives of their young wives. Overnight, these women were transformed from military spouses into American royalty. As their celebrity rose and tragedy began to touch their lives, they rallied together.",
      "origin_country": [
        "US"
      ],
      "poster_path": "/cCg0WE4tTC20gW034rQ9mm1uJvW.jpg",
      "popularity": 14.565064,
      "name": "The Astronaut Wives Club",
      "vote_average": 5.5,
      "vote_count": 1
    },
    {
      "backdrop_path": "/heXGzDx18UCQALqUKSLwasp5j18.jpg",
      "first_air_date": "2015-06-19",
      "genre_ids": [
        878
      ],
      "id": 62413,
      "original_language": "en",
      "original_name": "Killjoys",
      "overview": "An action-packed adventure series following a trio of interplanetary bounty hunters as they navigate through political turmoil, questionable morality and complicated relationships.",
      "origin_country": [
        "CA"
      ],
      "poster_path": "/ziPB638w1DGOJ0wbFhmHaew4Rz.jpg",
      "popularity": 14.522814,
      "name": "Killjoys",
      "vote_average": 0.0,
      "vote_count": 0
    },
    {
      "backdrop_path": "/v0VWoRbWpwlNwvI7TJgNcSa0NcV.jpg",
      "first_air_date": "2015-06-16",
      "genre_ids": [
        10765,
        18
      ],
      "id": 62812,
      "original_language": "en",
      "original_name": "Proof",
      "overview": "A brilliant surgeon searches for proof of life after death.",
      "origin_country": [],
      "poster_path": "/iHIdJMlle0bSAHbpWMDY0iS7q7M.jpg",
      "popularity": 14.267329,
      "name": "Proof",
      "vote_average": 7.0,
      "vote_count": 1
    },
    {
      "backdrop_path": "/gyuV7OHhsvXgVdMznBo9MTBCaGG.jpg",
      "first_air_date": "2015-06-17",
      "genre_ids": [
        18
      ],
      "id": 62681,
      "original_language": "de",
      "original_name": "Deutschland 83",
      "overview": "A gripping coming-of-age story set against the real culture wars and political events of Germany in the 1980s. The drama follows Martin Rauch as the 24 year-old East Germany native is pulled from the world as he knows it and sent to the West as an undercover spy for the Stasi foreign service. Hiding in plain sight in the West German army, he must gather the secrets of NATO military strategy. Everything is new, nothing is quite what it seems and everyone he encounters is harboring secrets, both political and personal.",
      "origin_country": [
        "DE"
      ],
      "poster_path": "/yL7XdYrbjwPQg9Xt4NUspKLyM1K.jpg",
      "popularity": 14.227498,
      "name": "Deutschland 83",
      "vote_average": 0.0,
      "vote_count": 0
    },
    {
      "backdrop_path": "/7QewmjzcCQDct2FAQC4sNolJWYL.jpg",
      "first_air_date": "2015-06-18",
      "genre_ids": [
        18
      ],
      "id": 62817,
      "original_language": "en",
      "original_name": "Complications",
      "overview": "John Ellis, a disillusioned suburban ER doctor, who finds his existence transformed when he intervenes in a drive-by shooting, saving a young boy's life and killing one of his attackers. When he learns the boy is still marked for death he finds himself compelled to save him at any cost and discovers that his life and his outlook on medicine may never be the same.",
      "origin_country": [
        "US"
      ],
      "poster_path": "/vSz47YjZ9qDDLaouyYA4s6m5aXW.jpg",
      "popularity": 13.889071,
      "name": "Complications",
      "vote_average": 0.0,
      "vote_count": 0
    },
    {
      "backdrop_path": "/z1A83plDjsHxbiwOtLTwq1zHJs5.jpg",
      "first_air_date": "2015-06-15",
      "genre_ids": [
        18,
        99
      ],
      "id": 62336,
      "original_language": "en",
      "original_name": "The Making of The Mob: New York",
      "overview": "An eight-part series which begins in 1905 and spans over 50 years to trace the rise of Charles “Lucky” Luciano, Meyer Lansky, Benjamin “Bugsy” Siegel and other notorious gangsters from their beginnings as a neighborhood gang of teenagers to murderous entrepreneurs and bootleggers who organized the criminal underworld, turning the Mafia into an American institution.",
      "origin_country": [
        "US"
      ],
      "poster_path": "/nPzEMTPbinHvIH6Yw7NjaS0qJyF.jpg",
      "popularity": 12.462114,
      "name": "The Making of The Mob: New York",
      "vote_average": 8.0,
      "vote_count": 1
    },
    {
      "backdrop_path": "/qddgk4kIcbcQR9jvZQLRJQCloB6.jpg",
      "first_air_date": "2015-05-28",
      "genre_ids": [
        18,
        80
      ],
      "id": 61239,
      "original_language": "en",
      "original_name": "Aquarius",
      "overview": "In the late 1960s, a Los Angeles police sergeant with a complicated personal life starts tracking a small-time criminal and budding cult leader seeking out vulnerable women to join his “cause.” The name of that man is Charles Manson.",
      "origin_country": [
        "US"
      ],
      "poster_path": "/nF3KKABpIEPMYs7SnkuKXKphoR3.jpg",
      "popularity": 11.184313,
      "name": "Aquarius",
      "vote_average": 8.7,
      "vote_count": 5
    },
    {
      "backdrop_path": "/4EFVV3ZsIRFGxvHJXGyTQLzThQK.jpg",
      "first_air_date": "2014-01-12",
      "genre_ids": [
        18
      ],
      "id": 46648,
      "original_language": "en",
      "original_name": "True Detective",
      "overview": "An American anthology police detective series utilizing multiple timelines in which investigations seem to unearth both personal and professional secrets of those involved, both within or outside the law.",
      "origin_country": [
        "US"
      ],
      "poster_path": "/qKrk5sXTDWncg7m7aT25lgVSNqi.jpg",
      "popularity": 10.485979,
      "name": "True Detective",
      "vote_average": 8.6,
      "vote_count": 89
    },
    {
      "backdrop_path": "/unEuJTz88F0Bft99RNi0KAgtK27.jpg",
      "first_air_date": "2015-05-14",
      "genre_ids": [
        18,
        9648,
        53
      ],
      "id": 53425,
      "original_language": "en",
      "original_name": "Wayward Pines",
      "overview": "Imagine the perfect American town... beautiful homes, manicured lawns, children playing safely in the streets. Now imagine never being able to leave. You have no communication with the outside world. You think you're going insane. You must be in Wayward Pines.\n\nBased on the best-selling novel “Pines” by Blake Crouch and brought to life by suspenseful storyteller M. Night Shyamalan, “Wayward Pines” is the intense new mind-bending 10-episode event thriller evocative of the classic hit “Twin Peaks.”",
      "origin_country": [
        "US"
      ],
      "poster_path": "/dlGyl2HuB1RFHhMtHOI8WKnR5qY.jpg",
      "popularity": 10.356223,
      "name": "Wayward Pines",
      "vote_average": 8.9,
      "vote_count": 4
    },
    {
      "backdrop_path": "/bmtdLpAgrB8ZC1CSMs7JkHig1r1.jpg",
      "first_air_date": "2012-10-11",
      "genre_ids": [
        18,
        10749,
        53,
        10765
      ],
      "id": 44606,
      "original_language": "en",
      "original_name": "Beauty and the Beast",
      "overview": "Detective Catherine Chandler is a smart, no-nonsense homicide detective. When she was a teenager, she witnessed the murder of her mother at the hands of two gunmen and herself was saved by someone – or something. Years have passed and while investigating a murder, Catherine discovers a clue that leads her to Vincent Keller, who was reportedly killed in 2002. Catherine learns that Vincent is actually still alive and that it was he who saved her many years before. For mysterious reasons that have forced him to live outside of traditional society, Vincent has been in hiding for the past 10 years to guard his secret – when he is enraged, he becomes a terrifying beast, unable to control his super-strength and heightened senses.",
      "origin_country": [
        "US"
      ],
      "poster_path": "/e6dreBifK0pqBlXP2uILpOWCJwG.jpg",
      "popularity": 9.97213,
      "name": "Beauty and the Beast",
      "vote_average": 6.0,
      "vote_count": 11
    },
    {
      "backdrop_path": "/wPFSIYusujghtjxalc8rosMkVj0.jpg",
      "first_air_date": "2014-06-07",
      "genre_ids": [
        18
      ],
      "id": 54650,
      "original_language": "en",
      "original_name": "Power",
      "overview": "James “Ghost” St. Patrick has it all: a beautiful wife, a gorgeous Manhattan penthouse, and the hottest, up-and-coming new nightclub in New York. His club, Truth, caters to the elite: the famous and infamous boldface names that run the city that never sleeps. As its success grows, so do Ghost’s plans to build an empire. However, Truth hides an ugly reality. It’s a front for Ghost’s criminal underworld; a lucrative drug network, serving only the wealthy and powerful. \n\nAs Ghost is seduced by the prospect of a legitimate life, everything precious to him becomes unknowingly threatened. Once you're in, can you ever get out?",
      "origin_country": [
        "US"
      ],
      "poster_path": "/cmTOOVIE7LlQIF08QNTYOE0MRra.jpg",
      "popularity": 9.781836,
      "name": "Power",
      "vote_average": 7.9,
      "vote_count": 5
    },
    {
      "backdrop_path": "/zaZLBPFZnEfUTjBey1tIAP3wALJ.jpg",
      "first_air_date": "2015-06-21",
      "genre_ids": [
        18,
        35
      ],
      "id": 62704,
      "original_language": "en",
      "original_name": "Ballers",
      "overview": "A look at the lives of athletes after they step off the field.",
      "origin_country": [
        "US"
      ],
      "poster_path": "/7ZPZA4x7vcvdAFpuXrPYZP5iTyT.jpg",
      "popularity": 9.266809,
      "name": "Ballers",
      "vote_average": 0.0,
      "vote_count": 0
    },
    {
      "backdrop_path": "/hPu4btEVP0E4ztAU8mxEhg1KM6I.jpg",
      "first_air_date": "2015-06-01",
      "genre_ids": [
        9648,
        878
      ],
      "id": 62195,
      "original_language": "en",
      "original_name": "The Whispers",
      "overview": "We love to play games with our children. But what happens when someone else starts to play with them too? Someone we don't know. Can't see. Can't hear. In The Whispers, someone or something -- is manipulating the ones we love most to accomplish the unthinkable. An unseen alien force has figured this human weakness out. They have invaded earth and are using our most unlikely resource to achieve world domination – our children.",
      "origin_country": [
        "US"
      ],
      "poster_path": "/fcByTtMWtdABZTDT9WbFJYm772O.jpg",
      "popularity": 8.620542,
      "name": "The Whispers",
      "vote_average": 8.0,
      "vote_count": 1
    }
  ],
  "total_pages": 10,
  "total_results": 184
}

Get the list of TV shows that air today. Without a specified timezone, this query defaults to EST (Eastern Time UTC-05:00).

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td colspan="2">api_key</td></tr>
<tr><td colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-right: 40px; width: 140px;">page</td><td>Minimum 1, maximum 1000.</td></tr>
<tr><td style="padding-right: 40px; width: 140px;">language</td><td>ISO 639-1 code.</td></tr>
<tr><td style="padding-right: 40px; width: 140px;">timezone</td><td>Valid value from the list of timezones.</td></tr>
</table>

GET /tv/airing_today
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "4620cf6a1b1df7d25d52a7958f0942d6"
{
  "page": 1,
  "results": [
    {
      "backdrop_path": "/kohPYEYHuQLWX3gjchmrWWOEycD.jpg",
      "first_air_date": "2015-06-12",
      "genre_ids": [
        878
      ],
      "id": 62425,
      "original_language": "en",
      "original_name": "Dark Matter",
      "overview": "The six-person crew of a derelict spaceship awakens from stasis in the farthest reaches of space. Their memories wiped clean, they have no recollection of who they are or how they got on board. The only clue to their identities is a cargo bay full of weaponry and a destination: a remote mining colony that is about to become a war zone. With no idea whose side they are on, they face a deadly decision. Will these amnesiacs turn their backs on history, or will their pasts catch up with them?",
      "origin_country": [
        "CA"
      ],
      "poster_path": "/iDSXueb3hjerXMq5w92rBP16LWY.jpg",
      "popularity": 27.373853,
      "name": "Dark Matter",
      "vote_average": 6.4,
      "vote_count": 4
    },
    {
      "backdrop_path": "/bYjc2HolyI2HWGhftWsyo5HgqTC.jpg",
      "first_air_date": "2014-06-27",
      "genre_ids": [
        10751,
        35
      ],
      "id": 46028,
      "original_language": "en",
      "original_name": "Girl Meets World",
      "overview": "Based on ABC's hugely popular 1993-2000 sitcom, this comedy, set in New York City, tells the wonderfully funny, heartfelt stories that \"Boy Meets World\" is renowned for – only this time from a tween girl's perspective – as the curious and bright 7th grader Riley Matthews and her quick-witted friend Maya Fox embark on an unforgettable middle school experience. But their plans for a carefree year will be adjusted slightly under the watchful eyes of Riley's parents – dad Cory, who's also a faculty member (and their new History teacher), and mom Topanga, who owns a trendy afterschool hangout that specializes in pudding.",
      "origin_country": [
        "US"
      ],
      "poster_path": "/yeRT4hAVoCz6RjLUiwdZ8Of34Th.jpg",
      "popularity": 18.319562,
      "name": "Girl Meets World",
      "vote_average": 7.5,
      "vote_count": 1
    },
    {
      "backdrop_path": "/heXGzDx18UCQALqUKSLwasp5j18.jpg",
      "first_air_date": "2015-06-19",
      "genre_ids": [
        878
      ],
      "id": 62413,
      "original_language": "en",
      "original_name": "Killjoys",
      "overview": "An action-packed adventure series following a trio of interplanetary bounty hunters as they navigate through political turmoil, questionable morality and complicated relationships.",
      "origin_country": [
        "CA"
      ],
      "poster_path": "/ziPB638w1DGOJ0wbFhmHaew4Rz.jpg",
      "popularity": 14.522814,
      "name": "Killjoys",
      "vote_average": 0.0,
      "vote_count": 0
    },
    {
      "backdrop_path": "/rYAhN4199PUJVEqcqEFKYCGQz3H.jpg",
      "first_air_date": "2013-04-15",
      "genre_ids": [
        18,
        878
      ],
      "id": 51704,
      "original_language": "en",
      "original_name": "Defiance",
      "overview": "Defiance is an American science fiction television show that takes place in the future on a radically transformed Earth containing new species arriving from space. In the show, Joshua Nolan works as the lawman for the town of Defiance, a community where humans and intelligent extraterrestrial species coexist. The show follows Nolan, his adopted alien daughter Irisa, and the town's new mayor Amanda Rosewater.",
      "origin_country": [
        "US"
      ],
      "poster_path": "/gX419PowKcLvzHky9XRA360yfbe.jpg",
      "popularity": 7.248747,
      "name": "Defiance",
      "vote_average": 5.8,
      "vote_count": 28
    },
    {
      "backdrop_path": "/fPysI2bEqSjAffmyXzm2y7uJuE5.jpg",
      "first_air_date": "2000-07-14",
      "genre_ids": [
        10764
      ],
      "id": 11366,
      "original_language": "en",
      "original_name": "Big Brother",
      "overview": "Big Brother is a BAFTA Award-winning British reality television game show in which a number of contestants live in an isolated house for several weeks, trying to avoid being evicted by the public with the aim of winning a large cash prize at the end of the run. It is the British version of the Dutch Big Brother television format, which takes its name from the character in George Orwell's 1949 novel Nineteen Eighty-Four.\n\nBig Brother, along with its spin-off series Celebrity Big Brother, was originally broadcast on Channel 4 from 18 July 2000 until 10 September 2010, after which it was dropped from Channel 4's schedules due to declining ratings. The rights to the programme were acquired by Channel 5 in a two-year contract with Endemol to air on the main channel and subsidiary channel 5*. The re-launched version premiered on 18 August 2011 with a back-to-back series of Celebrity Big Brother and Big Brother.\n\nThe host of Big Brother for the duration of its run on Channel 4 was Davina McCall, who was replaced by Brian Dowling when the show moved to Channel 5. Marcus Bentley has narrated the show since its inception on Channel 4. Spin-off shows that co-existed with the Channel 4 series were presented by Dermot O'Leary, Russell Brand, George Lamb and Emma Willis, who returned to present the only spin-off show Big Brother's Bit on the Side alongside Jamie East and Alice Levine when the show began on Channel 5. On 2 April 2013, it was announced that Willis would be the main Big Brother presenter.",
      "origin_country": [
        "GB"
      ],
      "poster_path": "/u5WlsOMrk7m4cHBwFtbn18u6lgS.jpg",
      "popularity": 6.991886,
      "name": "Big Brother",
      "vote_average": 10.0,
      "vote_count": 1
    },
    {
      "backdrop_path": "/5UUZN7WXGrOK6LaBp2PvXe4w6VH.jpg",
      "first_air_date": "2014-02-17",
      "genre_ids": [
        10767,
        35
      ],
      "id": 59941,
      "original_language": "en",
      "original_name": "The Tonight Show Starring Jimmy Fallon",
      "overview": "After Jay Leno's second retirement from the program, Jimmy Fallon stepped in as his permanent replacement. After 42 years in Los Angeles the program was brought back to New York.",
      "origin_country": [
        "US"
      ],
      "poster_path": "/3KOSUIuALoQq1JBSBSpNO8fhS4t.jpg",
      "popularity": 5.87069,
      "name": "The Tonight Show Starring Jimmy Fallon",
      "vote_average": 10.0,
      "vote_count": 2
    },
    {
      "backdrop_path": "/whzT5q3xivbaZr49yhsxyDYbwst.jpg",
      "first_air_date": "2015-04-17",
      "genre_ids": [
        10765,
        18,
        10759,
        9648
      ],
      "id": 60864,
      "original_language": "en",
      "original_name": "The Messengers",
      "overview": "When a mysterious object crashes to Earth, a group of seemingly unconnected strangers die from the energy pulse, but then awaken to learn that they have assigned the task of preventing the impending Apocalypse.",
      "origin_country": [
        "US"
      ],
      "poster_path": "/qSN2VB5DsrCegPU4eR6dpCxr9DQ.jpg",
      "popularity": 4.822799,
      "name": "The Messengers",
      "vote_average": 7.4,
      "vote_count": 4
    },
    {
      "backdrop_path": "/p2go1rc53e6yqmmp6qLt7On6mFz.jpg",
      "first_air_date": "2012-02-24",
      "genre_ids": [
        80,
        18,
        9648
      ],
      "id": 43078,
      "original_language": "en",
      "original_name": "Miss Fisher's Murder Mysteries",
      "overview": "Our lady sleuth sashays through the back lanes and jazz clubs of late 1920’s Melbourne, fighting injustice with her pearl handled pistol and her dagger sharp wit. Leaving a trail of admirers in her wake, our thoroughly modern heroine makes sure she enjoys every moment of her lucky life. Based on author Kerry Greenwood's Phryne Fisher Murder Mystery novels.",
      "origin_country": [
        "AU"
      ],
      "poster_path": "/kyTHUx8qC0Bn26zU3sADpWK8YLv.jpg",
      "popularity": 4.469917,
      "name": "Miss Fisher's Murder Mysteries",
      "vote_average": 9.2,
      "vote_count": 3
    },
    {
      "backdrop_path": null,
      "first_air_date": "2015-05-29",
      "genre_ids": [
        18,
        10749
      ],
      "id": 62838,
      "original_language": "ko",
      "original_name": "사랑하는 은동아",
      "overview": "Top actor Ji Eun-ho hires ghostwriter Seo Jung-eun to write his autobiography. Eun-ho is tense, irritable and difficult to work with, but Jung-eun finds her assignment fascinating because Eun-ho claims he began acting not because he wanted to become a star but because he thought being in the limelight would help him find his first love, Ji Eun-dong. Eun-ho and Eun-dong's complicated romantic history has spanned two decades, and he's convinced that he can never love anyone else. As Jung-eun helps him remember Eun-dong and why he lost her, Eun-ho (whose birth name is Hyun Soo) looks back on his memories of her, from when he met when he was seventeen.",
      "origin_country": [
        "KR"
      ],
      "poster_path": "/tY3qHR9sh1ZdfZNAxHMOFk8BjJp.jpg",
      "popularity": 3.104819,
      "name": "My Love Eun Dong",
      "vote_average": 8.0,
      "vote_count": 1
    },
    {
      "backdrop_path": "/gQjfIHNpNHLcqLqFQd0lihtiVk9.jpg",
      "first_air_date": "2015-05-27",
      "genre_ids": [],
      "id": 62790,
      "original_language": "en",
      "original_name": "The Briefcase",
      "overview": "THE BRIEFCASE features hard-working American families experiencing financial setbacks who are presented with a briefcase containing a large sum of money and a potentially life-altering decision: they can keep all of the money for themselves, or give all or part of it to another family in need.",
      "origin_country": [
        "US"
      ],
      "poster_path": null,
      "popularity": 2.823387,
      "name": "The Briefcase",
      "vote_average": 0.0,
      "vote_count": 0
    },
    {
      "backdrop_path": "/qNc0bT76vg99ThLZzEMgHAXyzHR.jpg",
      "first_air_date": "2013-04-05",
      "genre_ids": [
        10763,
        99
      ],
      "id": 48552,
      "original_language": "en",
      "original_name": "VICE",
      "overview": "Vice is a documentary TV-series created and hosted by Shane Smith of Vice magazine. Produced by Bill Maher, it uses CNN journalist Fareed Zakaria as a consultant, and covers topics such as political assassinations, young weapons manufacturers, and child suicide bombers using an immersionist style of documentary filmmaking. It aired on HBO in April 2013. Rolling Stone wrote that the show \"feels a little like your buddy from the bar just happened to be wandering through eastern Afghanistan with a camera crew.\"",
      "origin_country": [
        "US"
      ],
      "poster_path": "/wsuZVbmfgiFMffimIySp7lLK8pu.jpg",
      "popularity": 2.758192,
      "name": "VICE",
      "vote_average": 8.8,
      "vote_count": 3
    },
    {
      "backdrop_path": "/m5WsvyZ3k3RR16QDFFxEtsZxkia.jpg",
      "first_air_date": "2003-02-21",
      "genre_ids": [
        10767,
        35
      ],
      "id": 4419,
      "original_language": "en",
      "original_name": "Real Time with Bill Maher",
      "overview": "Real Time with Bill Maher is a talk show that airs weekly on HBO, hosted by comedian and political satirist Bill Maher.\n\nMuch like his previous series Politically Incorrect on ABC, Real Time features a panel of guests who discuss current events in politics and the media. Unlike the previous show, however, guests are usually more well-versed in the subject matter: more experts such as journalists, professors and politicians participate in the panel, and there are fewer actors and celebrities included in it. Additionally, many guests appear via satellite. Also, Politically Incorrect was produced four days a week and was pre-recorded, while Real Time only produces one episode a week which is broadcast live.\n\nReal Time is an hour-long program with a studio audience, airing live on Friday nights at 10:00 PM. It originates from Studio 33 at CBS Television City in Los Angeles. Prior to 2009, approximately 12 new weekly episodes aired in the spring, followed by another such set of new episodes in the fall. In 2009, the show began airing as one continuous season. Because of the live, current-events nature of the show, HBO does not re-air old episodes between breaks, though occasionally a repeat will be shown when the program takes a week off during the season.",
      "origin_country": [
        "US"
      ],
      "poster_path": "/nx4vS7Oq0OGjAaINSyGZRkKuosf.jpg",
      "popularity": 2.638909,
      "name": "Real Time with Bill Maher",
      "vote_average": 9.7,
      "vote_count": 3
    },
    {
      "backdrop_path": "/cknJ1o12Xg0AxEyLVclWCtOIcTW.jpg",
      "first_air_date": "2012-10-12",
      "genre_ids": [
        35,
        10751
      ],
      "id": 43775,
      "original_language": "en",
      "original_name": "Dog With a Blog",
      "overview": "Avery Jennings and Tyler James are step-siblings who are complete opposites. The family faces an even bigger adjustment when their new dog, Stan, can talk and also has a blog, unbeknownst to the family. Stan uses his blog to discuss the happenings in the Jennings-James household. Avery and Tyler later learn of Stan's talking ability and agree to keep it a secret from their parents.",
      "origin_country": [
        "US"
      ],
      "poster_path": "/pYTiWMCprilsNdnbtUcFF4XkwIf.jpg",
      "popularity": 2.449821,
      "name": "Dog With a Blog",
      "vote_average": 5.0,
      "vote_count": 1
    },
    {
      "backdrop_path": "/5Wng0MWUnGvWtH3fiuXFOzDkWhU.jpg",
      "first_air_date": "2015-05-15",
      "genre_ids": [
        14,
        10749
      ],
      "id": 62669,
      "original_language": "ko",
      "original_name": "오렌지 마말레이드",
      "overview": "Despite a 200-year-old treaty between humans and vampires, both races still don't get along. Amidst the looming tension, Jung Jae Min, a posh yet kindhearted high school student, quickly falls for the mysterious new girl Baek Ma Ri. But can Ma Ri conceal her true vampire identity and give love an honest chance? If these star-crossed lovers can find the courage to make the leap, Jae Min and Ma Ri may just be the key to a final resolution between humans and vampires...",
      "origin_country": [
        "KR"
      ],
      "poster_path": "/m9OGKEZWTuefEn91IKDUE7ALnJR.jpg",
      "popularity": 2.095246,
      "name": "Orange Marmalade",
      "vote_average": 0.0,
      "vote_count": 0
    },
    {
      "backdrop_path": "/axwGRNYpKxlSRdD6WHyQPQryOso.jpg",
      "first_air_date": "2015-04-10",
      "genre_ids": [
        16
      ],
      "id": 62428,
      "original_language": "ja",
      "original_name": "聖闘士星矢 黄金魂",
      "overview": "Transcending eternity, the 12 Gold Saints return to protect love and peace on Earth! They gave their lives to destroy the Wailing Wall to break the way for Seiya and the Saints in their battle against Hades in the Underworld! Though presumed to have perished, Aiolia and the other Gold Saints return to the beautiful earthly world of radiating luminescence! Why have these lost souls been brought back to life? Shrouded in this deep mystery, Aiolia becomes embroiled in a duel. When he burns his Cosmo to its limit…the Cloth of Leo transforms!",
      "origin_country": [
        "JP"
      ],
      "poster_path": "/bm84DVJivv8kcmlg0wF8toUfhrl.jpg",
      "popularity": 2.081701,
      "name": "Saint Seiya: Soul of Gold",
      "vote_average": 9.0,
      "vote_count": 2
    },
    {
      "backdrop_path": "/uZMcmHDpW7HowkLxk1GA1BsSO7a.jpg",
      "first_air_date": "1985-02-19",
      "genre_ids": [
        18
      ],
      "id": 1871,
      "original_language": "en",
      "original_name": "EastEnders",
      "overview": "EastEnders is a British television soap opera, first broadcast in the United Kingdom on BBC One on 19 February 1985. EastEnders storylines examine the domestic and professional lives of the people who live and work in the fictional London Borough of Walford in the East End of London. The series primarily centres on the residents of Albert Square, a Victorian square of terraced houses, and its neighbouring streets, namely Bridge Street, Turpin Road and George Street. The Square encompasses a pub, street market, night club, community centre, charity shop, cafe and various small businesses, in addition to a park and allotments.\n\nThe series was originally screened as two half-hour episodes per week. Since August 2001, four episodes are broadcast each week on BBC One, with each episode being repeated on BBC Three at 10.30pm and an omnibus edition on BBC One at weekends. From 1985 to 2012 the omnibus aired on Sunday afternoons, but was moved to a Friday night/Saturday morning slot from 6 April 2012, before being moved back to a Sunday afternoon slot in January 2013 on BBC Two.\n\nIt is one of the UK's highest-rated programmes, often appearing near or at the top of the week's BARB ratings. Within eight months of its launch, it reached the number-one spot in the ratings, and has consistently remained among the top-rated TV programmes in Britain. As of July 2013, the average audience share for an episode is around 30 percent. Created by producer Julia Smith and script editor Tony Holland, EastEnders has remained a significant programme in terms of the BBC's success and audience share, and also in the history of British television drama, tackling many controversial and taboo issues previously unseen on mainstream television in the UK.",
      "origin_country": [
        "GB"
      ],
      "poster_path": "/drANC7RsL0YoK2cqPEpP9SsY1n8.jpg",
      "popularity": 2.044063,
      "name": "EastEnders",
      "vote_average": 0.0,
      "vote_count": 0
    },
    {
      "backdrop_path": "/fk1isMjNb8whxnIoSvI3CW4WKrT.jpg",
      "first_air_date": "2014-05-06",
      "genre_ids": [
        10764
      ],
      "id": 61789,
      "original_language": "en",
      "original_name": "Alaskan Bush People",
      "overview": "Deep in the Alaskan wilderness lives a newly discovered family who was born and raised wild. Billy Brown, his wife Ami and their seven grown children – 5 boys and 2 girls – are so far removed from civilization that they often go six to nine months of the year without seeing an outsider. They’ve developed their own accent and dialect, refer to themselves as a \"wolf pack,\" and at night, all nine sleep together in a one-room cabin. Simply put, they are unlike any other family in America. Recently, according to the Browns, the cabin where they lived for years was seized and burned to the ground for being in the wrong location on public land.",
      "origin_country": [
        "US"
      ],
      "poster_path": "/qIFYyQhNp1fbSw18JejEC0B9i0i.jpg",
      "popularity": 1.979603,
      "name": "Alaskan Bush People",
      "vote_average": 5.3,
      "vote_count": 2
    },
    {
      "backdrop_path": "/eCbmUk12PWGfu1MuNinZ0qVdPFk.jpg",
      "first_air_date": "2007-02-22",
      "genre_ids": [
        10767
      ],
      "id": 1220,
      "original_language": "en",
      "original_name": "The Graham Norton Show",
      "overview": "The Graham Norton Show is a British comedy chat show broadcast on BBC One in the United Kingdom. It was originally shown on BBC Two from February 2007 to May 2009 until it moved to BBC One from October 2009. Presented by Irish comedian Graham Norton, the show's format is very similar to his previous Channel 4 programmes, V Graham Norton and So Graham Norton, both of which were also produced by So Television.",
      "origin_country": [
        "GB"
      ],
      "poster_path": "/irOmnAKLi1elTmPylng4M1IkoyF.jpg",
      "popularity": 1.961686,
      "name": "The Graham Norton Show",
      "vote_average": 9.0,
      "vote_count": 1
    },
    {
      "backdrop_path": "/bhGO8DqistlswRyQboYiI7oFAhA.jpg",
      "first_air_date": "2015-05-15",
      "genre_ids": [
        35
      ],
      "id": 62644,
      "original_language": "ko",
      "original_name": "프로듀사",
      "overview": "The drama is set in the backstage world of broadcasting industry. The story revolves around the dynamic work and life of people who work in entertainment division of television network.",
      "origin_country": [
        "KR"
      ],
      "poster_path": "/wRDtDzBVig9PTBejiXTAXHX0Qcn.jpg",
      "popularity": 1.938203,
      "name": "The Producers",
      "vote_average": 0.0,
      "vote_count": 0
    },
    {
      "backdrop_path": "/bHrzj9FeXWDZajF2UUmNIHSMNkD.jpg",
      "first_air_date": "1972-10-16",
      "genre_ids": [
        10766,
        18
      ],
      "id": 2527,
      "original_language": "en",
      "original_name": "Emmerdale",
      "overview": "Emmerdale, known as Emmerdale Farm until 1989, is a long-running British soap opera set in Emmerdale, a fictional village in the Yorkshire Dales. Created by Kevin Laffan, Emmerdale Farm was first broadcast on 16 October 1972. It is produced by Yorkshire Television, now part of ITV Studios, and has been filmed at their Yorkshire Studios since its inception. It has since been shown in all regions of ITV almost throughout its existence. It is the UK's second oldest soap opera and one of the 'Top 4' alongside Coronation Street, EastEnders and Hollyoaks.\n\nIt was originally broadcast as a daytime programme in an afternoon slot, becoming an early evening programme in 1978 in most ITV regions, but excluding London and Anglia, both of which followed in the mid-1980s. Until Christmas 1988, Emmerdale took seasonal breaks; since then it has been broadcast year-round.\n\nEmmerdale half-hourly episodes are shown every weekday at 7pm with an extra Thursday episode being aired at 8pm. Episodes are first broadcast on the ITV Network. On 10 October 2011, Emmerdale began broadcasting in high definition on ITV HD.",
      "origin_country": [
        "GB"
      ],
      "poster_path": "/4IMptN7S7VJs6tyLOx5dOTPsOx8.jpg",
      "popularity": 1.906783,
      "name": "Emmerdale",
      "vote_average": 0.0,
      "vote_count": 0
    }
  ],
  "total_pages": 2,
  "total_results": 33
}

Get the list of top rated TV shows. By default, this list will only include TV shows that have 2 or more votes. This list refreshes every day.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td colspan="2">api_key</td></tr>
<tr><td colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-right: 40px; width: 140px;">page</td><td>Minimum 1, maximum 1000.</td></tr>
<tr><td style="padding-right: 40px; width: 140px;">language</td><td>ISO 639-1 code.</td></tr>
</table>

GET /tv/top_rated
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "673e90c5bb02e543d616459fbb43a5ed"
{
  "page": 1,
  "results": [
    {
      "backdrop_path": "/55Vuc2uYHUsZzP8rYYUyeowGbJ3.jpg",
      "first_air_date": "2013-10-21",
      "genre_ids": [
        35
      ],
      "id": 62718,
      "original_language": "en",
      "original_name": "Upstairs",
      "overview": "Joe, Matt, Sarah and Francine are four college students. The girls are roommates downstairs while the boys are roommates... Upstairs. This bunch could not be more different. You'll find out not only their choice of majors, but by their drastically different cultural backgrounds too. It can cause friction to say the least, but hilarity will always ensue. Will opposites attract?",
      "origin_country": [
        "IT"
      ],
      "poster_path": "/4XFBM8FlA7FidB3ZxMLNeM6KJH1.jpg",
      "popularity": 1.200313,
      "name": "Upstairs",
      "vote_average": 9.5,
      "vote_count": 30
    },
    {
      "backdrop_path": "/nZ9JxNFLrygdQE8TT2csBHch6Yc.jpg",
      "first_air_date": "2002-06-02",
      "genre_ids": [
        9648,
        80,
        18
      ],
      "id": 1438,
      "original_language": "en",
      "original_name": "The Wire",
      "overview": "The Wire is an American television crime drama series set and produced in and around Baltimore, Maryland. Each season of The Wire introduces a different facet of the city of Baltimore. In chronological order they are: the illegal drug trade, the seaport system, the city government and bureaucracy, the school system, and the print news media.\n\nDespite only receiving average ratings and never winning major television awards, The Wire has been described by many critics and fans as one of the greatest TV dramas of all time. The show is recognized for its realistic portrayal of urban life, its literary ambitions, and its uncommonly deep exploration of sociopolitical themes.",
      "origin_country": [
        "US"
      ],
      "poster_path": "/dg7NuKDjmS6OzuNy33qt8kSkPA1.jpg",
      "popularity": 5.576837,
      "name": "The Wire",
      "vote_average": 9.5,
      "vote_count": 36
    },
    {
      "backdrop_path": "/pfTlxypaLQ8KHecKrD7EFcAzEUs.jpg",
      "first_air_date": "2013-12-02",
      "genre_ids": [
        35,
        12,
        10765
      ],
      "id": 60625,
      "original_language": "en",
      "original_name": "Rick and Morty",
      "overview": "Rick is a mentally-unbalanced but scientifically-gifted old man who has recently reconnected with his family. He spends most of his time involving his young grandson Morty in dangerous, outlandish adventures throughout space and alternate universes. Compounded with Morty's already unstable family life, these events cause Morty much distress at home and school.",
      "origin_country": [],
      "poster_path": "/qJdfO3ahgAMf2rcmhoqngjBBZW1.jpg",
      "popularity": 3.574103,
      "name": "Rick and Morty",
      "vote_average": 9.4,
      "vote_count": 6
    },
    {
      "backdrop_path": "/gCWhn0HqQcxCaw1TDypja7CPXTJ.jpg",
      "first_air_date": "2007-01-07",
      "genre_ids": [
        80,
        9648,
        18
      ],
      "id": 32368,
      "original_language": "da",
      "original_name": "Forbrydelsen",
      "overview": "The Killing is a Danish police set in the Copenhagen main police department and revolves around Detective Inspector Sarah Lund and her team, with each season series following a different murder case day-by-day and a one-hour episode covering twenty-four hours of the investigation. The series is noted for its plot twists, season-long storylines, dark tone and for giving equal emphasis to the story of the murdered victim's family alongside the police investigation. It has also been singled out for the photography of its Danish setting, and for the acting ability of its cast.",
      "origin_country": [
        "DK"
      ],
      "poster_path": "/Al172OkeZmC7dy4PfobC7cFGVOV.jpg",
      "popularity": 3.776574,
      "name": "The Killing",
      "vote_average": 9.4,
      "vote_count": 10
    },
    {
      "backdrop_path": "/4AJnC8L5CXdqb6tMeUBL5jdHfuG.jpg",
      "first_air_date": "2006-03-05",
      "genre_ids": [
        99
      ],
      "id": 1044,
      "original_language": "en",
      "original_name": "Planet Earth",
      "overview": "Planet Earth is a 2006 television series produced by the BBC Natural History Unit. The Emmy-award-winning program was five years in the making. It was the most expensive nature documentary series ever commissioned by the BBC and was described by its makers as \"the definitive look at the diversity of our planet\".",
      "origin_country": [
        "GB",
        "US",
        "CA",
        "JP"
      ],
      "poster_path": "/tI5hLHgrfOTOhPjX22vlo9YsdgE.jpg",
      "popularity": 3.569406,
      "name": "Planet Earth",
      "vote_average": 9.3,
      "vote_count": 5
    },
    {
      "backdrop_path": "/9RUwWb6ejWcX1agqJiiFfZw9PlU.jpg",
      "first_air_date": "2010-06-09",
      "genre_ids": [
        99
      ],
      "id": 32900,
      "original_language": "en",
      "original_name": "Through the Wormhole",
      "overview": "Hosted by Morgan Freeman, Through the Wormhole explores the deepest mysteries of existence - the questions that have puzzled mankind for eternity. What are we made of? What was there before the beginning? Are we really alone? Is there a creator? These questions have been pondered by the most exquisite minds of the human race. Now, science has evolved to the point where hard facts and evidence may be able to provide us with answers instead of philosophical theories. Through the Wormhole brings together the brightest minds and best ideas from the very edges of science - Astrophysics, Astrobiology, Quantum Mechanics, String Theory, and more - to reveal the extraordinary truth of our Universe.",
      "origin_country": [
        "US"
      ],
      "poster_path": "/yzlGw8yFbVwoHS52g8C0DFYDak9.jpg",
      "popularity": 3.128265,
      "name": "Through the Wormhole",
      "vote_average": 9.3,
      "vote_count": 8
    },
    {
      "backdrop_path": "/sJjrJWsOCUUeVMexxANNC6s3dWV.jpg",
      "first_air_date": "2012-06-15",
      "genre_ids": [
        9648,
        16,
        10765,
        35
      ],
      "id": 40075,
      "original_language": "en",
      "original_name": "Gravity Falls",
      "overview": "Gravity Falls is an American animated television series produced by Disney Television Animation for Disney Channel. The series was created by Alex Hirsch, a former writer for The Marvelous Misadventures of Flapjack and Fish Hooks. The first episode aired as a preview on June 15, 2012 and the series officially debuted on June 29, 2012. On July 29, 2013, the show was officially renewed for a second season by Disney Channel.",
      "origin_country": [
        "US"
      ],
      "poster_path": "/oGsgxjeZ9rd20eCJsGSMGgWvl4P.jpg",
      "popularity": 2.927891,
      "name": "Gravity Falls",
      "vote_average": 9.2,
      "vote_count": 7
    },
    {
      "backdrop_path": "/1LrtAhWPSEetJLjblXvnaYtl7eA.jpg",
      "first_air_date": "2001-09-08",
      "genre_ids": [
        28,
        10752,
        18,
        10759,
        10768
      ],
      "id": 4613,
      "original_language": "en",
      "original_name": "Band of Brothers",
      "overview": "Drawn from interviews with survivors of Easy Company, as well as their journals and letters, Band of Brothers chronicles the experiences of these men from paratrooper training in Georgia through the end of the war. As an elite rifle company parachuting into Normandy early on D-Day morning, participants in the Battle of the Bulge, and witness to the horrors of war, the men of Easy knew extraordinary bravery and extraordinary fear - and became the stuff of legend. Based on Stephen E. Ambrose's acclaimed book of the same name.",
      "origin_country": [
        "GB",
        "US"
      ],
      "poster_path": "/s9qU8HULDrwDv7dWna2vqTXzbRP.jpg",
      "popularity": 4.713002,
      "name": "Band of Brothers",
      "vote_average": 9.2,
      "vote_count": 23
    },
    {
      "backdrop_path": "/aKz3lXU71wqdslC1IYRC3yHD6yw.jpg",
      "first_air_date": "2011-04-17",
      "genre_ids": [
        10765,
        18
      ],
      "id": 1399,
      "original_language": "en",
      "original_name": "Game of Thrones",
      "overview": "Seven noble families fight for control of the mythical land of Westeros. Friction between the houses leads to full-scale war. All while a very ancient evil awakens in the farthest north. Amidst the war, a neglected military order of misfits, the Night's Watch, is all that stands between the realms of men and icy horrors beyond.\n\n",
      "origin_country": [
        "US"
      ],
      "poster_path": "/jIhL6mlT7AblhbHJgEoiBIOUVl1.jpg",
      "popularity": 36.072708,
      "name": "Game of Thrones",
      "vote_average": 9.1,
      "vote_count": 273
    },
    {
      "backdrop_path": "/6ecdbcPoLojEPwPX1xSfNGSbYOR.jpg",
      "first_air_date": "2004-09-03",
      "genre_ids": [
        10759,
        18
      ],
      "id": 1414,
      "original_language": "zh",
      "original_name": "The Shield",
      "overview": "The story of an inner-city Los Angeles police precinct where some of the cops aren't above breaking the rules or working against their associates to both keep the streets safe and their self-interests intact.",
      "origin_country": [
        "US"
      ],
      "poster_path": "/Akl157JPKHgZBNE7viv8F1kCAcd.jpg",
      "popularity": 3.25005,
      "name": "The Shield",
      "vote_average": 9.0,
      "vote_count": 11
    },
    {
      "backdrop_path": "/1maVcORgJbbbKj2FZ1xSXKqOWzs.jpg",
      "first_air_date": "2006-10-03",
      "genre_ids": [
        18
      ],
      "id": 4278,
      "original_language": "en",
      "original_name": "Friday Night Lights",
      "overview": "Friday Night Lights is an American drama television series based around a high school football team situated in Texas. It was developed by Peter Berg, and executive produced by Brian Grazer, David Nevins, Sarah Aubrey, and Jason Katims, based on the book and film of the same name. The series takes place in the fictional town of Dillon: a small, close-knit community in rural Texas. Particular focus is given to team coach Eric Taylor and his family, Tammy and Julie. The show uses this small town backdrop to address many issues facing contemporary American culture, including family values, child development, life lessons, school funding, racism, drugs, abortion and lack of economic opportunities.\n\nProduced by NBCUniversal, Friday Night Lights premiered on October 3, 2006, airing for two seasons on the National Broadcasting Company. Although the show had garnered critical acclaim and passionate fans, the series suffered low ratings and was in danger of cancellation after the second season. To save the series, NBC struck a deal with DirecTV to co-produce three more seasons of the show with each subsequent season premiering on DirecTV's 101 Network after which NBC rebroadcast the series a few months later. The series ended its run on The 101 Network after five seasons on February 9, 2011.",
      "origin_country": [
        "US"
      ],
      "poster_path": "/f72UEMY8Jv1WrwJbaKRO8NggUj0.jpg",
      "popularity": 3.477767,
      "name": "Friday Night Lights",
      "vote_average": 9.0,
      "vote_count": 9
    },
    {
      "backdrop_path": "/qkKHndnjcb8Wxg0eEtRRFVqtkCS.jpg",
      "first_air_date": "1989-01-08",
      "genre_ids": [
        18,
        9648,
        80,
        10770
      ],
      "id": 790,
      "original_language": "en",
      "original_name": "Agatha Christie's Poirot",
      "overview": "Agatha Christie's Poirot is a British television drama that premiered on ITV in 1989, where it has remained throughout its airing. David Suchet stars as the titular detective, Agatha Christie's fictional Hercule Poirot. Initially produced by LWT, the current production company is ITV Studios. In the United States, PBS and A&E have aired it as Poirot, which was the title prior to 2004. Series 13 premiered June 9, 2013 and will end with the finale, Curtain, based on the final novel Christie wrote featuring Poirot. At the programs' conclusion, every major literary work by Christie that featured the title character will have been adapted.",
      "origin_country": [
        "GB"
      ],
      "poster_path": "/5shIDhTIfRnmUAXMS4wF2GF0NFO.jpg",
      "popularity": 2.611875,
      "name": "Agatha Christie's Poirot",
      "vote_average": 9.0,
      "vote_count": 6
    },
    {
      "backdrop_path": "/30iomnEyQWtJ9aT2vsbzWUgjrzV.jpg",
      "first_air_date": "2000-09-29",
      "genre_ids": [
        35
      ],
      "id": 903,
      "original_language": "en",
      "original_name": "Black Books",
      "overview": "Black Books is a British sitcom created by Dylan Moran and Graham Linehan that was broadcast on Channel 4 from 2000 to 2004. Starring Moran, Bill Bailey and Tamsin Greig, the series is set in the eponymous London bookshop Black Books and follows the lives of its owner Bernard Black, his assistant Manny Bianco and their friend Fran Katzenjammer. The series was produced by Big Talk Productions, in association with Channel 4.\n\nBlack Books was produced in a multiple-camera setup, and was primarily filmed at Teddington Studios in Teddington, London, with exterior scenes filmed on location on Leigh Street and the surrounding areas in Bloomsbury. The debut episode premiered on 29 September 2000 and three seasons followed, with the final episode airing on 15 April 2004.\n\nBlack Books was a critical success, winning a number of awards‒including two BAFTA awards for Best Situation Comedy in 2001 and 2005 and a Bronze Rose at the Festival Rose d'Or‒and being called \"a hugely popular series\" by The Times.",
      "origin_country": [
        "GB"
      ],
      "poster_path": "/5I3xoBsEOzdBIRoPOWncDdRhx4r.jpg",
      "popularity": 2.423177,
      "name": "Black Books",
      "vote_average": 9.0,
      "vote_count": 12
    },
    {
      "backdrop_path": "/w0LSB0POet7MR91nOQCXTOetia2.jpg",
      "first_air_date": "2004-03-21",
      "genre_ids": [
        37,
        18
      ],
      "id": 1406,
      "original_language": "zh",
      "original_name": "Deadwood",
      "overview": "Deadwood is an American western television series that was created, produced and largely written by David Milch and aired on the premium cable network HBO from March 21, 2004, to August 27, 2006, spanning three 12-episode seasons. The show is set in the 1870s in Deadwood, South Dakota, before and after the area's annexation by the Dakota Territory. The series charts Deadwood's growth from camp to town, incorporating themes ranging from the formation of communities to western capitalism.",
      "origin_country": [
        "US"
      ],
      "poster_path": "/M8HjXeXQy4Ta4MOrCmMHLr8pBl.jpg",
      "popularity": 3.265569,
      "name": "Deadwood",
      "vote_average": 9.0,
      "vote_count": 11
    },
    {
      "backdrop_path": "/7RRPju9LQHBBMqdggUUL16unChG.jpg",
      "first_air_date": "1996-01-08",
      "genre_ids": [
        16,
        10759,
        10765,
        35,
        9648
      ],
      "id": 30983,
      "original_language": "ja",
      "original_name": "名探偵コナン",
      "overview": "Jimmy Kudo is a high school detective who helps the police solve cases. During an investigation, he is attacked by Gin and Vodka who belong to a syndicate known as the Black Organization. They force him to ingest an experimental poison called APTX 4869 to kill him without leaving evidence. A rare side-effect of the poison, however, transforms him into a child instead of killing him. Adopting the pseudonym Conan Edogawa, Kudo hides his identity to investigate the Black Organization. Later, Shiho Miyano, a member of the Black Organization and creator of APTX 4869, tries to leave the syndicate after her sister's death but is captured. She attempts suicide by ingesting APTX 4869, and like Kudo, is transformed into a child. She escapes and enrolls in Conan's school under a pseudonym, Anita Hailey. During a rare encounter with the Black Organization, Conan helps the FBI plant a CIA agent, Kir, inside the Black Organization as an undercover spy.",
      "origin_country": [
        "JP"
      ],
      "poster_path": "/7V2Ey3Owy7a7zp2ZvQze8xSFSoe.jpg",
      "popularity": 3.591045,
      "name": "Detective Conan",
      "vote_average": 8.9,
      "vote_count": 5
    },
    {
      "backdrop_path": "/eSzpy96DwBujGFj0xMbXBcGcfxX.jpg",
      "first_air_date": "2008-01-19",
      "genre_ids": [
        18
      ],
      "id": 1396,
      "original_language": "en",
      "original_name": "Breaking Bad",
      "overview": "Breaking Bad is an American crime drama television series created and produced by Vince Gilligan. Set and produced in Albuquerque, New Mexico, Breaking Bad is the story of Walter White, a struggling high school chemistry teacher who is diagnosed with inoperable lung cancer at the beginning of the series. He turns to a life of crime, producing and selling methamphetamine, in order to secure his family's financial future before he dies, teaming with his former student, Jesse Pinkman. Heavily serialized, the series is known for positioning its characters in seemingly inextricable corners and has been labeled a contemporary western by its creator.",
      "origin_country": [
        "US"
      ],
      "poster_path": "/4yMXf3DW6oCL0lVPZaZM2GypgwE.jpg",
      "popularity": 18.095686,
      "name": "Breaking Bad",
      "vote_average": 8.9,
      "vote_count": 245
    },
    {
      "backdrop_path": "/8GZ91vtbYOMp05qruAGPezWC0Ja.jpg",
      "first_air_date": "1999-01-10",
      "genre_ids": [
        18
      ],
      "id": 1398,
      "original_language": "en",
      "original_name": "The Sopranos",
      "overview": "The Sopranos is an American television drama created by David Chase. The series revolves around the New Jersey-based Italian-American mobster Tony Soprano and the difficulties he faces as he tries to balance the conflicting requirements of his home life and the criminal organization he heads. Those difficulties are often highlighted through his ongoing professional relationship with psychiatrist Jennifer Melfi. The show features Tony's family members and Mafia associates in prominent roles and story arcs, most notably his wife Carmela and his cousin and protégé Christopher Moltisanti.",
      "origin_country": [
        "US"
      ],
      "poster_path": "/u0cLcBQITrYqfHsn06fxnQwtqiE.jpg",
      "popularity": 3.669163,
      "name": "The Sopranos",
      "vote_average": 8.8,
      "vote_count": 38
    },
    {
      "backdrop_path": "/qcLVonGvPUBUmSi5hKxF3FDjdA9.jpg",
      "first_air_date": "2014-04-27",
      "genre_ids": [
        10767,
        35
      ],
      "id": 60694,
      "original_language": "en",
      "original_name": "Last Week Tonight with John Oliver",
      "overview": "'Last Week Tonight with John Oliver' presents a half-hour satirical look at the week in news, politics and current events.",
      "origin_country": [
        "US"
      ],
      "poster_path": "/xo6mfa37ZdCGPCdsqJRVDBwxBlQ.jpg",
      "popularity": 3.570515,
      "name": "Last Week Tonight with John Oliver",
      "vote_average": 8.8,
      "vote_count": 9
    },
    {
      "backdrop_path": "/knwLWYzTmUtr3h3bsz4rKPGVf4z.jpg",
      "first_air_date": "2014-07-27",
      "genre_ids": [
        18
      ],
      "id": 61112,
      "original_language": "en",
      "original_name": "Manhattan",
      "overview": "Set against the backdrop of the greatest clandestine race against time in the history of science with the mission to build the world's first atomic bomb in Los Alamos, New Mexico. Flawed scientists and their families attempt to co-exist in a world where secrets and lies infiltrate every aspect of their lives.",
      "origin_country": [
        "US"
      ],
      "poster_path": "/mT0jljRMRRBnsGHqxlxVVf09yLb.jpg",
      "popularity": 2.684439,
      "name": "Manhattan",
      "vote_average": 8.8,
      "vote_count": 9
    },
    {
      "backdrop_path": "/rrPe1tzyPHLqR474ylSrBMCNhv6.jpg",
      "first_air_date": "2010-04-05",
      "genre_ids": [
        14,
        16,
        35
      ],
      "id": 15260,
      "original_language": "en",
      "original_name": "Adventure Time",
      "overview": "Adventure Time is an American animated television series created by Pendleton Ward for Cartoon Network. The series follows the adventures of Finn, a human boy, and his best friend and adoptive brother Jake, a dog with magical powers to change shape and grow and shrink at will. Finn and Jake live in the post-apocalyptic Land of Ooo. Along the way, they interact with the other main characters of the show: Princess Bubblegum, The Ice King, and Marceline the Vampire Queen.",
      "origin_country": [
        "US"
      ],
      "poster_path": "/bZpF5g6u0np0AMYXkug4aJSH3xI.jpg",
      "popularity": 4.477858,
      "name": "Adventure Time",
      "vote_average": 8.8,
      "vote_count": 23
    }
  ],
  "total_pages": 25,
  "total_results": 497
}

Get the list of popular TV shows. This list refreshes every day.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td colspan="2">api_key</td></tr>
<tr><td colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-right: 40px; width: 140px;">page</td><td>Minimum 1, maximum 1000.</td></tr>
<tr><td style="padding-right: 40px; width: 140px;">language</td><td>ISO 639-1 code.</td></tr>
</table>

GET /tv/popular
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "99be0948e83ee582d0706f13b3b7a3fb"
{
  "page": 1,
  "results": [
    {
      "backdrop_path": "/aKz3lXU71wqdslC1IYRC3yHD6yw.jpg",
      "first_air_date": "2011-04-17",
      "genre_ids": [
        10765,
        18
      ],
      "id": 1399,
      "original_language": "en",
      "original_name": "Game of Thrones",
      "overview": "Seven noble families fight for control of the mythical land of Westeros. Friction between the houses leads to full-scale war. All while a very ancient evil awakens in the farthest north. Amidst the war, a neglected military order of misfits, the Night's Watch, is all that stands between the realms of men and icy horrors beyond.\n\n",
      "origin_country": [
        "US"
      ],
      "poster_path": "/jIhL6mlT7AblhbHJgEoiBIOUVl1.jpg",
      "popularity": 36.072708,
      "name": "Game of Thrones",
      "vote_average": 9.1,
      "vote_count": 273
    },
    {
      "backdrop_path": "/kohPYEYHuQLWX3gjchmrWWOEycD.jpg",
      "first_air_date": "2015-06-12",
      "genre_ids": [
        878
      ],
      "id": 62425,
      "original_language": "en",
      "original_name": "Dark Matter",
      "overview": "The six-person crew of a derelict spaceship awakens from stasis in the farthest reaches of space. Their memories wiped clean, they have no recollection of who they are or how they got on board. The only clue to their identities is a cargo bay full of weaponry and a destination: a remote mining colony that is about to become a war zone. With no idea whose side they are on, they face a deadly decision. Will these amnesiacs turn their backs on history, or will their pasts catch up with them?",
      "origin_country": [
        "CA"
      ],
      "poster_path": "/iDSXueb3hjerXMq5w92rBP16LWY.jpg",
      "popularity": 27.373853,
      "name": "Dark Matter",
      "vote_average": 6.4,
      "vote_count": 4
    },
    {
      "backdrop_path": "/nGsNruW3W27V6r4gkyc3iiEGsKR.jpg",
      "first_air_date": "2007-09-24",
      "genre_ids": [
        35
      ],
      "id": 1418,
      "original_language": "en",
      "original_name": "The Big Bang Theory",
      "overview": "The Big Bang Theory is centered on five characters living in Pasadena, California: roommates Leonard Hofstadter and Sheldon Cooper; Penny, a waitress and aspiring actress who lives across the hall; and Leonard and Sheldon's equally geeky and socially awkward friends and co-workers, mechanical engineer Howard Wolowitz and astrophysicist Raj Koothrappali. The geekiness and intellect of the four guys is contrasted for comic effect with Penny's social skills and common sense.",
      "origin_country": [
        "US"
      ],
      "poster_path": "/8SUIoe1ENMHWLZ0fe2sMqMP3eZD.jpg",
      "popularity": 21.903248,
      "name": "The Big Bang Theory",
      "vote_average": 8.0,
      "vote_count": 181
    },
    {
      "backdrop_path": "/jXpndJTekLFYcx3xX0H3sDqFnJU.jpg",
      "first_air_date": "2015-06-02",
      "genre_ids": [
        878
      ],
      "id": 62196,
      "original_language": "en",
      "original_name": "Stitchers",
      "overview": "A young woman is recruited into a secret government agency to be “stitched” into the minds of the recently deceased, using their memories to investigate murders.",
      "origin_country": [
        "US"
      ],
      "poster_path": "/2Ni5y120OSt6lSHGDavU1nDrHHP.jpg",
      "popularity": 19.064832,
      "name": "Stitchers",
      "vote_average": 7.5,
      "vote_count": 4
    },
    {
      "backdrop_path": "/t6JB8bgobNr7qNnO1vXBOYayo5T.jpg",
      "first_air_date": "2015-06-14",
      "genre_ids": [
        18,
        878
      ],
      "id": 62822,
      "original_language": "en",
      "original_name": "Humans",
      "overview": "In a parallel present where the latest must-have gadget for any busy family is a 'Synth' - a highly-developed robotic servant that's so similar to a real human it's transforming the way we live.",
      "origin_country": [
        "GB",
        "US"
      ],
      "poster_path": "/gJCyS65ieDT827F2NR9Nx9ZLuw5.jpg",
      "popularity": 18.931591,
      "name": "Humans",
      "vote_average": 9.2,
      "vote_count": 3
    },
    {
      "backdrop_path": "/tacmO3UpPX26bb1EQz8c11lQezR.jpg",
      "first_air_date": "2015-06-16",
      "genre_ids": [
        35
      ],
      "id": 62771,
      "original_language": "en",
      "original_name": "Clipped",
      "overview": "Clipped centers on a group of barbershop coworkers who all went to high school together but ran in very different crowds. Now they find themselves working together at Buzzy's, a barbershop in Charlestown, Massachusetts.",
      "origin_country": [
        "US"
      ],
      "poster_path": "/a7Tp2hVM3aMINT9L5blINV6AweY.jpg",
      "popularity": 18.628063,
      "name": "Clipped",
      "vote_average": 2.3,
      "vote_count": 2
    },
    {
      "backdrop_path": "/bYjc2HolyI2HWGhftWsyo5HgqTC.jpg",
      "first_air_date": "2014-06-27",
      "genre_ids": [
        10751,
        35
      ],
      "id": 46028,
      "original_language": "en",
      "original_name": "Girl Meets World",
      "overview": "Based on ABC's hugely popular 1993-2000 sitcom, this comedy, set in New York City, tells the wonderfully funny, heartfelt stories that \"Boy Meets World\" is renowned for – only this time from a tween girl's perspective – as the curious and bright 7th grader Riley Matthews and her quick-witted friend Maya Fox embark on an unforgettable middle school experience. But their plans for a carefree year will be adjusted slightly under the watchful eyes of Riley's parents – dad Cory, who's also a faculty member (and their new History teacher), and mom Topanga, who owns a trendy afterschool hangout that specializes in pudding.",
      "origin_country": [
        "US"
      ],
      "poster_path": "/yeRT4hAVoCz6RjLUiwdZ8Of34Th.jpg",
      "popularity": 18.319562,
      "name": "Girl Meets World",
      "vote_average": 7.5,
      "vote_count": 1
    },
    {
      "backdrop_path": "/eSzpy96DwBujGFj0xMbXBcGcfxX.jpg",
      "first_air_date": "2008-01-19",
      "genre_ids": [
        18
      ],
      "id": 1396,
      "original_language": "en",
      "original_name": "Breaking Bad",
      "overview": "Breaking Bad is an American crime drama television series created and produced by Vince Gilligan. Set and produced in Albuquerque, New Mexico, Breaking Bad is the story of Walter White, a struggling high school chemistry teacher who is diagnosed with inoperable lung cancer at the beginning of the series. He turns to a life of crime, producing and selling methamphetamine, in order to secure his family's financial future before he dies, teaming with his former student, Jesse Pinkman. Heavily serialized, the series is known for positioning its characters in seemingly inextricable corners and has been labeled a contemporary western by its creator.",
      "origin_country": [
        "US"
      ],
      "poster_path": "/4yMXf3DW6oCL0lVPZaZM2GypgwE.jpg",
      "popularity": 18.095686,
      "name": "Breaking Bad",
      "vote_average": 8.9,
      "vote_count": 245
    },
    {
      "backdrop_path": "/sCx2r2bw49OqxnjWaU9DsPwdR0C.jpg",
      "first_air_date": "1994-09-22",
      "genre_ids": [
        35
      ],
      "id": 1668,
      "original_language": "en",
      "original_name": "Friends",
      "overview": "Friends is an American sitcom created by David Crane and Marta Kauffman, which aired on NBC from September 22, 1994 to May 6, 2004. The series revolves around a group of friends in the New York City borough of Manhattan. The series was produced by Bright/Kauffman/Crane Productions, in association with Warner Bros. Television. The original executive producers were Crane, Kauffman, and Kevin S. Bright, with numerous others being promoted in later seasons.\n\nKauffman and Crane began developing Friends under the title Insomnia Cafe in November/December 1993. They presented the idea to Bright, with whom they had previously worked, and together they pitched a seven-page treatment of the series to NBC. After several script rewrites and changes, including a second title change to Friends Like Us, the series was finally named Friends and premiered on NBC's coveted Thursday 8:30 pm timeslot. Filming for the series took place at Warner Bros. Studios in Burbank, California in front of a live studio audience. The series finale, airing on May 6, 2004, was watched by 52.5 million American viewers, making it the fourth most watched series finale in television history and the most watched episode of the decade.",
      "origin_country": [
        "US"
      ],
      "poster_path": "/7buCWBTpiPrCF5Lt023dSC60rgS.jpg",
      "popularity": 17.348013,
      "name": "Friends",
      "vote_average": 8.0,
      "vote_count": 83
    },
    {
      "backdrop_path": "/lnnrirKFGwFW18GiH3AmuYy40cz.jpg",
      "first_air_date": "1989-12-16",
      "genre_ids": [
        16,
        35,
        10751
      ],
      "id": 456,
      "original_language": "en",
      "original_name": "The Simpsons",
      "overview": "The Simpsons is an American animated sitcom created by Matt Groening for the Fox Broadcasting Company. The series is a satirical parody of a middle class American lifestyle epitomized by its family of the same name, which consists of Homer, Marge, Bart, Lisa, and Maggie. The show is set in the fictional town of Springfield and parodies American culture, society, television, and many aspects of the human condition.",
      "origin_country": [
        "US"
      ],
      "poster_path": "/yTZQkSsxUFJZJe67IenRM0AEklc.jpg",
      "popularity": 15.360667,
      "name": "The Simpsons",
      "vote_average": 8.6,
      "vote_count": 94
    },
    {
      "backdrop_path": "/trypBo0ldWAZ4ULeQDjkzzW7To3.jpg",
      "first_air_date": "2010-06-08",
      "genre_ids": [
        9648,
        18
      ],
      "id": 31917,
      "original_language": "en",
      "original_name": "Pretty Little Liars",
      "overview": "Based on the Pretty Little Liars series of young adult novels by Sara Shepard, the series follows the lives of four girls — Spencer, Hanna, Aria, and Emily — whose clique falls apart after the disappearance of their queen bee, Alison. One year later, they begin receiving messages from someone using the name \"A\" who threatens to expose their secrets — including long-hidden ones they thought only Alison knew. ",
      "origin_country": [
        "US"
      ],
      "poster_path": "/2rOLNdUPahiAq6VuoA1I6sA45cs.jpg",
      "popularity": 15.188609,
      "name": "Pretty Little Liars",
      "vote_average": 6.8,
      "vote_count": 27
    },
    {
      "backdrop_path": "/lkQ4fEbAkrHeKWq8GAbFqnwXweo.jpg",
      "first_air_date": "2013-10-10",
      "genre_ids": [
        18
      ],
      "id": 40008,
      "original_language": "en",
      "original_name": "Hannibal",
      "overview": "Hannibal is an American thriller television series developed by Bryan Fuller for NBC. The series is based on characters and elements appearing in the novel Red Dragon by Thomas Harris and focuses on the budding relationship between FBI special investigator Will Graham and Dr. Hannibal Lecter, a forensic psychiatrist destined to become Graham's most cunning enemy.",
      "origin_country": [
        "US"
      ],
      "poster_path": "/kArcGHvTb7m1v6RFHdU40jpiGZD.jpg",
      "popularity": 14.592192,
      "name": "Hannibal",
      "vote_average": 8.4,
      "vote_count": 52
    },
    {
      "backdrop_path": "/bzFDAvhN6dcUOTbXduHl9pMvdX0.jpg",
      "first_air_date": "2015-06-18",
      "genre_ids": [
        18
      ],
      "id": 62458,
      "original_language": "en",
      "original_name": "The Astronaut Wives Club",
      "overview": "Based on the book by Lily Koppel, this 10 episode limited series tells the story of the women who were key players behind some of the biggest events in American history. As America’s astronauts were launched on death-defying missions, Life Magazine documented the astronauts’ families, capturing the behind-the-scenes lives of their young wives. Overnight, these women were transformed from military spouses into American royalty. As their celebrity rose and tragedy began to touch their lives, they rallied together.",
      "origin_country": [
        "US"
      ],
      "poster_path": "/cCg0WE4tTC20gW034rQ9mm1uJvW.jpg",
      "popularity": 14.565064,
      "name": "The Astronaut Wives Club",
      "vote_average": 5.5,
      "vote_count": 1
    },
    {
      "backdrop_path": "/heXGzDx18UCQALqUKSLwasp5j18.jpg",
      "first_air_date": "2015-06-19",
      "genre_ids": [
        878
      ],
      "id": 62413,
      "original_language": "en",
      "original_name": "Killjoys",
      "overview": "An action-packed adventure series following a trio of interplanetary bounty hunters as they navigate through political turmoil, questionable morality and complicated relationships.",
      "origin_country": [
        "CA"
      ],
      "poster_path": "/ziPB638w1DGOJ0wbFhmHaew4Rz.jpg",
      "popularity": 14.522814,
      "name": "Killjoys",
      "vote_average": 0.0,
      "vote_count": 0
    },
    {
      "backdrop_path": "/v0VWoRbWpwlNwvI7TJgNcSa0NcV.jpg",
      "first_air_date": "2015-06-16",
      "genre_ids": [
        10765,
        18
      ],
      "id": 62812,
      "original_language": "en",
      "original_name": "Proof",
      "overview": "A brilliant surgeon searches for proof of life after death.",
      "origin_country": [],
      "poster_path": "/iHIdJMlle0bSAHbpWMDY0iS7q7M.jpg",
      "popularity": 14.267329,
      "name": "Proof",
      "vote_average": 7.0,
      "vote_count": 1
    },
    {
      "backdrop_path": "/gyuV7OHhsvXgVdMznBo9MTBCaGG.jpg",
      "first_air_date": "2015-06-17",
      "genre_ids": [
        18
      ],
      "id": 62681,
      "original_language": "de",
      "original_name": "Deutschland 83",
      "overview": "A gripping coming-of-age story set against the real culture wars and political events of Germany in the 1980s. The drama follows Martin Rauch as the 24 year-old East Germany native is pulled from the world as he knows it and sent to the West as an undercover spy for the Stasi foreign service. Hiding in plain sight in the West German army, he must gather the secrets of NATO military strategy. Everything is new, nothing is quite what it seems and everyone he encounters is harboring secrets, both political and personal.",
      "origin_country": [
        "DE"
      ],
      "poster_path": "/yL7XdYrbjwPQg9Xt4NUspKLyM1K.jpg",
      "popularity": 14.227498,
      "name": "Deutschland 83",
      "vote_average": 0.0,
      "vote_count": 0
    },
    {
      "backdrop_path": "/zYFQM9G5j9cRsMNMuZAX64nmUMf.jpg",
      "first_air_date": "2010-10-31",
      "genre_ids": [
        18,
        27,
        53
      ],
      "id": 1402,
      "original_language": "en",
      "original_name": "The Walking Dead",
      "overview": "The Walking Dead is an American horror drama television series developed by Frank Darabont. It is based on the comic book series of the same name by Robert Kirkman, Tony Moore, and Charlie Adlard. The series stars Andrew Lincoln as sheriff's deputy Rick Grimes, who awakens from a coma to find a post-apocalyptic world dominated by flesh-eating zombies. He sets out to find his family and encounters many other survivors along the way.",
      "origin_country": [
        "US"
      ],
      "poster_path": "/vxuoMW6YBt6UsxvMfRNwRl9LtWS.jpg",
      "popularity": 14.044885,
      "name": "The Walking Dead",
      "vote_average": 7.9,
      "vote_count": 160
    },
    {
      "backdrop_path": "/7QewmjzcCQDct2FAQC4sNolJWYL.jpg",
      "first_air_date": "2015-06-18",
      "genre_ids": [
        18
      ],
      "id": 62817,
      "original_language": "en",
      "original_name": "Complications",
      "overview": "John Ellis, a disillusioned suburban ER doctor, who finds his existence transformed when he intervenes in a drive-by shooting, saving a young boy's life and killing one of his attackers. When he learns the boy is still marked for death he finds himself compelled to save him at any cost and discovers that his life and his outlook on medicine may never be the same.",
      "origin_country": [
        "US"
      ],
      "poster_path": "/vSz47YjZ9qDDLaouyYA4s6m5aXW.jpg",
      "popularity": 13.889071,
      "name": "Complications",
      "vote_average": 0.0,
      "vote_count": 0
    },
    {
      "backdrop_path": "/dpNeXLEnuKzAvbNwveJhNEiQvXZ.jpg",
      "first_air_date": "2015-04-10",
      "genre_ids": [
        28,
        80
      ],
      "id": 61889,
      "original_language": "en",
      "original_name": "Marvel's Daredevil",
      "overview": "Lawyer-by-day Matt Murdock uses his heightened senses from being blinded as a young boy to fight crime at night on the streets of Hell’s Kitchen as Daredevil.",
      "origin_country": [
        "US"
      ],
      "poster_path": "/itrCovJkuH7I8SJ98jxrInnwm2y.jpg",
      "popularity": 13.805904,
      "name": "Marvel's Daredevil",
      "vote_average": 7.9,
      "vote_count": 33
    },
    {
      "backdrop_path": "/g3Cke7hDiIq80HFv6agwxTx8AfT.jpg",
      "first_air_date": "2009-09-23",
      "genre_ids": [
        35,
        10751
      ],
      "id": 1421,
      "original_language": "en",
      "original_name": "Modern Family",
      "overview": "Modern Family is an American comedy series which debuted on ABC on September 23, 2009. Presented in mockumentary style, the fictional characters frequently talk directly into the camera. The program tells of Jay Pritchett, his second wife, their infant son and his stepson, and his two children and their families. Christopher Lloyd and Steven Levitan conceived the series while sharing stories of their own \"modern families\".",
      "origin_country": [
        "US"
      ],
      "poster_path": "/vmokjFmPtDZySnNTQd6uqYcTjNF.jpg",
      "popularity": 13.643311,
      "name": "Modern Family",
      "vote_average": 8.4,
      "vote_count": 52
    }
  ],
  "total_pages": 3089,
  "total_results": 61761
}

--
TV Seasons
--

Get the primary information about a TV season by its season number.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
<tr><td style="padding-left: 0;" colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-left: 0; width: 140px;">language</td><td>ISO 639-1 code.</td></tr>
<tr><td style="padding-left: 0; width: 140px;">append_to_response</td><td>Comma separated, any tv season method</td></tr>
</table>

GET /tv/{id}/season/{season_number}
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "156dd9d458e4a713a12f6a078caa078a"
{
  "air_date": "2008-01-19",
  "episodes": [
    {
      "air_date": "2008-01-19",
      "crew": [
        {
          "id": 2483,
          "credit_id": "52b7029219c29533d00dd2c1",
          "name": "John Toll",
          "department": "Camera",
          "job": "Director of Photography",
          "profile_path": null
        },
        {
          "id": 66633,
          "credit_id": "52542275760ee313280006ce",
          "name": "Vince Gilligan",
          "department": "Writing",
          "job": "Writer",
          "profile_path": "/wSTvJGz7QbJf1HK2Mv1Cev6W9TV.jpg"
        },
        {
          "id": 66633,
          "credit_id": "52542275760ee313280006e8",
          "name": "Vince Gilligan",
          "department": "Directing",
          "job": "Director",
          "profile_path": "/wSTvJGz7QbJf1HK2Mv1Cev6W9TV.jpg"
        },
        {
          "id": 1280071,
          "credit_id": "52b702ea19c2955402183a66",
          "name": "Lynne Willingham",
          "department": "Editing",
          "job": "Editor",
          "profile_path": null
        }
      ],
      "episode_number": 1,
      "guest_stars": [
        {
          "id": 92495,
          "name": "John Koyama",
          "credit_id": "52542273760ee3132800068e",
          "character": "Emilio Koyama",
          "order": 1,
          "profile_path": "/uh4g85qbQGZZ0HH6IQI9fM9VUGS.jpg"
        },
        {
          "id": 1223192,
          "name": "Roberta Marquez Seret",
          "credit_id": "52542275760ee313280006a2",
          "character": "Chad's Girlfriend",
          "order": 2,
          "profile_path": null
        },
        {
          "id": 1216132,
          "name": "Aaron Hill",
          "credit_id": "52542275760ee313280006b4",
          "character": "Jock",
          "order": 3,
          "profile_path": "/vTqz2FIutFJPG73j7gFDTWomTbb.jpg"
        },
        {
          "id": 161591,
          "name": "Gregory Chase",
          "credit_id": "52725cb1760ee3044d0b9984",
          "character": "Dr. Belknap (as Greg Chase)",
          "order": 33,
          "profile_path": null
        },
        {
          "id": 1046460,
          "name": "Max Arciniega",
          "credit_id": "52725845760ee3046b09426e",
          "character": "Krazy-8",
          "order": 38,
          "profile_path": "/hRyw2cglnWU42vHZUag7b9dIf4u.jpg"
        },
        {
          "id": 1223197,
          "name": "Marius Stan",
          "credit_id": "5272587a760ee3045009ddec",
          "character": "Bogdan Wolynetz",
          "order": 112,
          "profile_path": null
        },
        {
          "id": 61535,
          "name": "Steven Michael Quezada",
          "credit_id": "5271b489760ee35b3e0881a7",
          "character": "Steven Gomez",
          "order": 173,
          "profile_path": "/dRJsuGKUWlAooFnYEq85SN9C4Nf.jpg"
        },
        {
          "id": 115688,
          "name": "Carmen Serano",
          "credit_id": "52542273760ee31328000676",
          "character": "Carmen Molina",
          "order": 176,
          "profile_path": "/gJOeg4jtRZR5xRBizYX2IibByVj.jpg"
        }
      ],
      "name": "Pilot",
      "overview": "When an unassuming high school chemistry teacher discovers he has a rare form of lung cancer, he decides to team up with a former student and create a top of the line crystal meth in a used RV, to provide for his family once he is gone.",
      "id": 62085,
      "production_code": null,
      "season_number": 1,
      "still_path": "/88Z0fMP8a88EpQWMCs1593G0ngu.jpg",
      "vote_average": 8.5,
      "vote_count": 2
    },
    {
      "air_date": "2008-01-26",
      "crew": [
        {
          "id": 66633,
          "credit_id": "52542275760ee313280006ce",
          "name": "Vince Gilligan",
          "department": "Writing",
          "job": "Writer",
          "profile_path": "/wSTvJGz7QbJf1HK2Mv1Cev6W9TV.jpg"
        },
        {
          "id": 1212611,
          "credit_id": "52542275760ee31328000725",
          "name": "Adam Bernstein",
          "department": "Directing",
          "job": "Director",
          "profile_path": null
        },
        {
          "id": 1280071,
          "credit_id": "52b702ea19c2955402183a66",
          "name": "Lynne Willingham",
          "department": "Editing",
          "job": "Editor",
          "profile_path": null
        },
        {
          "id": 17948,
          "credit_id": "52b7049c19c2953b63015013",
          "name": "Reynaldo Villalobos",
          "department": "Camera",
          "job": "Director of Photography",
          "profile_path": null
        }
      ],
      "episode_number": 2,
      "guest_stars": [
        {
          "id": 92495,
          "name": "John Koyama",
          "credit_id": "52542273760ee3132800068e",
          "character": "Emilio Koyama",
          "order": 1,
          "profile_path": "/uh4g85qbQGZZ0HH6IQI9fM9VUGS.jpg"
        },
        {
          "id": 1046460,
          "name": "Max Arciniega",
          "credit_id": "52725845760ee3046b09426e",
          "character": "Krazy-8",
          "order": 38,
          "profile_path": "/hRyw2cglnWU42vHZUag7b9dIf4u.jpg"
        }
      ],
      "name": "The Cat's in the Bag",
      "overview": "Walt and Jesse attempt to tie up loose ends. The desperate situation gets more complicated with the flip of a coin. Walt's wife, Skyler, becomes suspicious of Walt's strange behavior.",
      "id": 62086,
      "production_code": null,
      "season_number": 1,
      "still_path": "/2v0kPL1ARB9YCegUmT57cjT1KPm.jpg",
      "vote_average": 9.0,
      "vote_count": 1
    },
    {
      "air_date": "2008-02-09",
      "crew": [
        {
          "id": 66633,
          "credit_id": "52542275760ee313280006ce",
          "name": "Vince Gilligan",
          "department": "Writing",
          "job": "Writer",
          "profile_path": "/wSTvJGz7QbJf1HK2Mv1Cev6W9TV.jpg"
        },
        {
          "id": 1212611,
          "credit_id": "52542275760ee31328000725",
          "name": "Adam Bernstein",
          "department": "Directing",
          "job": "Director",
          "profile_path": null
        },
        {
          "id": 17948,
          "credit_id": "52b7049c19c2953b63015013",
          "name": "Reynaldo Villalobos",
          "department": "Camera",
          "job": "Director of Photography",
          "profile_path": null
        },
        {
          "id": 1280074,
          "credit_id": "52b7051619c29533d00e8c79",
          "name": "Kelley Dixon",
          "department": "Editing",
          "job": "Editor",
          "profile_path": null
        }
      ],
      "episode_number": 3,
      "guest_stars": [
        {
          "id": 1046460,
          "name": "Max Arciniega",
          "credit_id": "52725845760ee3046b09426e",
          "character": "Krazy-8",
          "order": 38,
          "profile_path": "/hRyw2cglnWU42vHZUag7b9dIf4u.jpg"
        },
        {
          "id": 61535,
          "name": "Steven Michael Quezada",
          "credit_id": "5271b489760ee35b3e0881a7",
          "character": "Steven Gomez",
          "order": 173,
          "profile_path": "/dRJsuGKUWlAooFnYEq85SN9C4Nf.jpg"
        },
        {
          "id": 115688,
          "name": "Carmen Serano",
          "credit_id": "52542273760ee31328000676",
          "character": "Carmen Molina",
          "order": 176,
          "profile_path": "/gJOeg4jtRZR5xRBizYX2IibByVj.jpg"
        },
        {
          "id": 14984,
          "name": "Jessica Hecht",
          "credit_id": "52542275760ee31328000768",
          "character": "Gretchen Schwartz",
          "order": 182,
          "profile_path": "/jT5k2BLOEXaRF94ZV9plQBV6mAL.jpg"
        }
      ],
      "name": "...and the Bag's in the River",
      "overview": "Walter fights with Jesse over his drug use, causing him to leave Walter alone with their captive, Krazy-8. Meanwhile, Hank has a scared straight moment with Walter Jr. after his aunt discovers he has been smoking pot. Also, Skylar is upset when Walter stays away from home.",
      "id": 62087,
      "production_code": null,
      "season_number": 1,
      "still_path": "/mn73tWx7ivuMmdBdiHFK7KRHjDy.jpg",
      "vote_average": 0.0,
      "vote_count": 0
    },
    {
      "air_date": "2008-02-16",
      "crew": [
        {
          "id": 66633,
          "credit_id": "52542275760ee313280006ce",
          "name": "Vince Gilligan",
          "department": "Writing",
          "job": "Writer",
          "profile_path": "/wSTvJGz7QbJf1HK2Mv1Cev6W9TV.jpg"
        },
        {
          "id": 1218239,
          "credit_id": "52542278760ee31328000a9b",
          "name": "Jim McKay",
          "department": "Directing",
          "job": "Director",
          "profile_path": null
        },
        {
          "id": 1280071,
          "credit_id": "52b702ea19c2955402183a66",
          "name": "Lynne Willingham",
          "department": "Editing",
          "job": "Editor",
          "profile_path": null
        },
        {
          "id": 17948,
          "credit_id": "52b7049c19c2953b63015013",
          "name": "Reynaldo Villalobos",
          "department": "Camera",
          "job": "Director of Photography",
          "profile_path": null
        }
      ],
      "episode_number": 4,
      "guest_stars": [
        {
          "id": 1215836,
          "name": "Kyle Bornheimer",
          "credit_id": "52743e4d760ee35a69055194",
          "character": "Ken Wins",
          "order": 37,
          "profile_path": "/ucMZNXfcG8NlVIPBGujt15fFPcX.jpg"
        },
        {
          "id": 220118,
          "name": "Benjamin Petry",
          "credit_id": "527442eb760ee3572b078715",
          "character": "Jake Pinkman (as Ben Petry)",
          "order": 40,
          "profile_path": "/s0CaWbrxL9bhuiQ4F1XN6dWNPaW.jpg"
        },
        {
          "id": 79211,
          "name": "David House",
          "credit_id": "5271b65b760ee35b0c090f74",
          "character": "Dr. Delcavoli",
          "order": 71,
          "profile_path": null
        },
        {
          "id": 41249,
          "name": "Tess Harper",
          "credit_id": "52542277760ee31328000a61",
          "character": "Mrs. Pinkman",
          "order": 75,
          "profile_path": "/gJT9fBX04LjmbOzOzYcNbA2ZLF1.jpg"
        },
        {
          "id": 95195,
          "name": "Michael Bofshever",
          "credit_id": "527440ce760ee3570906ada3",
          "character": "Mr. Pinkman",
          "order": 75,
          "profile_path": "/wd4CATP5RQTq5iAyTJEC74qkStV.jpg"
        },
        {
          "id": 61535,
          "name": "Steven Michael Quezada",
          "credit_id": "5271b489760ee35b3e0881a7",
          "character": "Steven Gomez",
          "order": 173,
          "profile_path": "/dRJsuGKUWlAooFnYEq85SN9C4Nf.jpg"
        },
        {
          "id": 82945,
          "name": "Charles Baker",
          "credit_id": "52744007760ee356f6076365",
          "character": "Skinny Pete",
          "order": 182,
          "profile_path": "/lZ5fOVbTVvHkpSX9PUXNfkYLPUt.jpg"
        }
      ],
      "name": "Cancer Man",
      "overview": "Walter finally tells his family that he has been stricken with cancer. Meanwhile, the DEA believes Albuquerque has a new, big time player to worry about. Meanwhile, a worthy recipient is the target of a depressed Walter's anger, and Jesse makes a surprise visit to his parents home.",
      "id": 62088,
      "production_code": null,
      "season_number": 1,
      "still_path": "/m1CCrmD47ag6r8zQCDpd54ryksw.jpg",
      "vote_average": 0.0,
      "vote_count": 0
    },
    {
      "air_date": "2008-02-23",
      "crew": [
        {
          "id": 1218856,
          "credit_id": "52542278760ee31328000aea",
          "name": "Patty Lin",
          "department": "Writing",
          "job": "Writer",
          "profile_path": null
        },
        {
          "id": 1215145,
          "credit_id": "52542278760ee31328000afe",
          "name": "Tricia Brock",
          "department": "Directing",
          "job": "Director",
          "profile_path": null
        },
        {
          "id": 17948,
          "credit_id": "52b7049c19c2953b63015013",
          "name": "Reynaldo Villalobos",
          "department": "Camera",
          "job": "Director of Photography",
          "profile_path": null
        },
        {
          "id": 1280074,
          "credit_id": "52b7051619c29533d00e8c79",
          "name": "Kelley Dixon",
          "department": "Editing",
          "job": "Editor",
          "profile_path": null
        }
      ],
      "episode_number": 5,
      "guest_stars": [
        {
          "id": 202830,
          "name": "William Sterchi",
          "credit_id": "527445a9760ee356ff077e53",
          "character": "Manager",
          "order": 43,
          "profile_path": null
        },
        {
          "id": 59303,
          "name": "Marc Mouchet",
          "credit_id": "527445f5760ee357130849b9",
          "character": "Farley",
          "order": 44,
          "profile_path": null
        },
        {
          "id": 14984,
          "name": "Jessica Hecht",
          "credit_id": "52542275760ee31328000768",
          "character": "Gretchen Schwartz",
          "order": 182,
          "profile_path": "/jT5k2BLOEXaRF94ZV9plQBV6mAL.jpg"
        },
        {
          "id": 191202,
          "name": "Matt Jones",
          "credit_id": "52744535760ee3572209100e",
          "character": "Badger",
          "order": 182,
          "profile_path": "/wcB788lpiyc58s0F3nzqZnspISA.jpg"
        },
        {
          "id": 23429,
          "name": "Adam Godley",
          "credit_id": "527444c1760ee3572208fbc2",
          "character": "Elliott Schwartz",
          "order": 182,
          "profile_path": "/dUvq9pDPdXK36yICkuWYNfuAZpq.jpg"
        }
      ],
      "name": "Gray Matter",
      "overview": "Walter and Skyler attend a former colleague's party. Jesse tries to free himself from the drugs, while Skyler organizes an intervention.",
      "id": 62089,
      "production_code": null,
      "season_number": 1,
      "still_path": "/aR1BLarVxcMSemL6Q0C796nZfC2.jpg",
      "vote_average": 0.0,
      "vote_count": 0
    },
    {
      "air_date": "2008-03-01",
      "crew": [
        {
          "id": 1223193,
          "credit_id": "52542279760ee31328000b45",
          "name": "George Mastras",
          "department": "Writing",
          "job": "Writer",
          "profile_path": null
        },
        {
          "id": 18320,
          "credit_id": "52542279760ee31328000b61",
          "name": "Bronwen Hughes",
          "department": "Directing",
          "job": "Director",
          "profile_path": "/eOCUyOpDGsliiWA3c5qc2CGS5he.jpg"
        },
        {
          "id": 17948,
          "credit_id": "52b7049c19c2953b63015013",
          "name": "Reynaldo Villalobos",
          "department": "Camera",
          "job": "Director of Photography",
          "profile_path": null
        },
        {
          "id": 1123195,
          "credit_id": "52b705e619c2955a1f0c895b",
          "name": "Skip MacDonald",
          "department": "Editing",
          "job": "Editor",
          "profile_path": null
        }
      ],
      "episode_number": 6,
      "guest_stars": [
        {
          "id": 58650,
          "name": "Raymond Cruz",
          "credit_id": "5254227b760ee31328000cd6",
          "character": "Tuco Salamanca",
          "order": 44,
          "profile_path": "/BAGS8xnQC4rJLywn9fM4BDfPI8.jpg"
        },
        {
          "id": 210056,
          "name": "Pierre Barrera",
          "credit_id": "52744776760ee356ea0892f3",
          "character": "Hugo Archuleta",
          "order": 44,
          "profile_path": null
        },
        {
          "id": 187505,
          "name": "Vivian Nesbitt",
          "credit_id": "527447f4760ee37c3e00f81f",
          "character": "Mrs. Pope",
          "order": 47,
          "profile_path": null
        },
        {
          "id": 1089016,
          "name": "Seri DeYoung",
          "credit_id": "52744908760ee35a690730ed",
          "character": "Student (as Seraphine DeYoung)",
          "order": 49,
          "profile_path": "/72e1Ka1679fEKNxpYlshJXJhoXh.jpg"
        },
        {
          "id": 1221121,
          "name": "Dennis Keiffer",
          "credit_id": "52744a4a760ee35a69077c8c",
          "character": "Lookout",
          "order": 50,
          "profile_path": null
        },
        {
          "id": 53255,
          "name": "Cesar Garcia",
          "credit_id": "527447ca760ee356ea08e165",
          "character": "Gonzo",
          "order": 52,
          "profile_path": "/6KkknahgOIBaZfz3ip3UV89iVFO.jpg"
        },
        {
          "id": 1260529,
          "name": "Jesus Jr.",
          "credit_id": "527447b3760ee3571308a638",
          "character": "No-Doze (as Jesus Payan)",
          "order": 52,
          "profile_path": null
        },
        {
          "id": 210057,
          "name": "Judith Rane",
          "credit_id": "52744834760ee37c3e010cc7",
          "character": "Office Manager",
          "order": 61,
          "profile_path": null
        },
        {
          "id": 61535,
          "name": "Steven Michael Quezada",
          "credit_id": "5271b489760ee35b3e0881a7",
          "character": "Steven Gomez",
          "order": 173,
          "profile_path": "/dRJsuGKUWlAooFnYEq85SN9C4Nf.jpg"
        },
        {
          "id": 115688,
          "name": "Carmen Serano",
          "credit_id": "52542273760ee31328000676",
          "character": "Carmen Molina",
          "order": 176,
          "profile_path": "/gJOeg4jtRZR5xRBizYX2IibByVj.jpg"
        },
        {
          "id": 82945,
          "name": "Charles Baker",
          "credit_id": "52744007760ee356f6076365",
          "character": "Skinny Pete",
          "order": 182,
          "profile_path": "/lZ5fOVbTVvHkpSX9PUXNfkYLPUt.jpg"
        }
      ],
      "name": "Crazy Handful of Nothin'",
      "overview": "The side effects of chemo begin to plague Walt. Meanwhile, the DEA rounds up suspected dealers.",
      "id": 62090,
      "production_code": null,
      "season_number": 1,
      "still_path": "/fZST9WpwrYl2qHq3WHdUFDrPc2U.jpg",
      "vote_average": 0.0,
      "vote_count": 0
    },
    {
      "air_date": "2008-03-08",
      "crew": [
        {
          "id": 24951,
          "credit_id": "52542286760ee31328001ab9",
          "name": "Peter Gould",
          "department": "Writing",
          "job": "Writer",
          "profile_path": null
        },
        {
          "id": 15858,
          "credit_id": "5254227b760ee31328000d0c",
          "name": "Tim Hunter",
          "department": "Directing",
          "job": "Director",
          "profile_path": "/wEho8OQCrEJx94Q91fvSY203BgI.jpg"
        },
        {
          "id": 1280071,
          "credit_id": "52b702ea19c2955402183a66",
          "name": "Lynne Willingham",
          "department": "Editing",
          "job": "Editor",
          "profile_path": null
        },
        {
          "id": 17948,
          "credit_id": "52b7049c19c2953b63015013",
          "name": "Reynaldo Villalobos",
          "department": "Camera",
          "job": "Director of Photography",
          "profile_path": null
        }
      ],
      "episode_number": 7,
      "guest_stars": [
        {
          "id": 58650,
          "name": "Raymond Cruz",
          "credit_id": "5254227b760ee31328000cd6",
          "character": "Tuco Salamanca",
          "order": 44,
          "profile_path": "/afRJntGec0KE1uIYn9FFyp5lyO8.jpg"
        },
        {
          "id": 967071,
          "name": "Beth Bailey",
          "credit_id": "52744bd7760ee356ff08c286",
          "character": "Realtor",
          "order": 50,
          "profile_path": "/6KuevI6K8VcKJFB6vyuMZ0cPvM7.jpg"
        },
        {
          "id": 58651,
          "name": "Geoffrey Rivas",
          "credit_id": "52744c32760ee37c3e01cba4",
          "character": "Police Officer",
          "order": 51,
          "profile_path": null
        },
        {
          "id": 210154,
          "name": "Mike Miller",
          "credit_id": "52744cd0760ee357130a126e",
          "character": "Jewelry Store Owner",
          "order": 52,
          "profile_path": null
        },
        {
          "id": 53255,
          "name": "Cesar Garcia",
          "credit_id": "527447ca760ee356ea08e165",
          "character": "Gonzo",
          "order": 52,
          "profile_path": "/6KkknahgOIBaZfz3ip3UV89iVFO.jpg"
        },
        {
          "id": 1260529,
          "name": "Jesus Jr.",
          "credit_id": "527447b3760ee3571308a638",
          "character": "No-Doze (as Jesus Payan)",
          "order": 52,
          "profile_path": null
        },
        {
          "id": 79211,
          "name": "David House",
          "credit_id": "5271b65b760ee35b0c090f74",
          "character": "Dr. Delcavoli",
          "order": 71,
          "profile_path": null
        },
        {
          "id": 115688,
          "name": "Carmen Serano",
          "credit_id": "52542273760ee31328000676",
          "character": "Carmen Molina",
          "order": 176,
          "profile_path": "/gJOeg4jtRZR5xRBizYX2IibByVj.jpg"
        }
      ],
      "name": "A No-Rough-Stuff Type Deal",
      "overview": "Walter accepts his new identity as a drug dealer after a PTA meeting. Elsewhere, Jesse decides to put his aunt's house on the market and Skyler is the recipient of a baby shower.",
      "id": 62091,
      "production_code": null,
      "season_number": 1,
      "still_path": "/gyRMAdWZHhWpXpOvA1ctwoIuG4e.jpg",
      "vote_average": 0.0,
      "vote_count": 0
    }
  ],
  "name": "Season 1",
  "overview": "The first season of the American television drama series Breaking Bad premiered on January 20, 2008 and concluded on March 9, 2008. It consisted of seven episodes, each running approximately 47 minutes in length, except the pilot episode which runs approximately 57 minutes. AMC broadcast the first season on Sundays at 10:00 pm in the United States. Season one was to consist of nine episodes, which was reduced to seven by the writer's strike. The complete first season was released on Region 1 DVD on February 24, 2009 and Region A Blu-ray on March 16, 2010.",
  "id": 3572,
  "poster_path": "/dHCYpEoHEjAV6Xt3eyNthkdLRl3.jpg",
  "season_number": 1
}

Look up a TV season's changes by season ID. This method is used in conjunction with the [/tv/{id}/changes](#get-%2F3%2Ftv%2F%7Bid%7D%2Fchanges) method. This method uses the `season_id` value found in the change entries.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
<tr><td style="padding-left: 0;" colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-left: 0; padding-right: 40px; width: 140px;">start_date</td><td>YYYY-MM-DD</td></tr>
<tr><td style="padding-left: 0; padding-right: 40px; width: 140px;">end_date</td><td>YYYY-MM-DD</td></tr>
</table>

GET /tv/season/{id}/changes
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "691e0c90d7869a78ead95cb777b870dd"
{
  "changes": [
    {
      "key": "episode",
      "items": [
        {
          "id": "537320ce0e0a267c590011ae",
          "action": "updated",
          "time": "2014-05-14 07:52:46 UTC",
          "value": {
            "episode_number": 4,
            "episode_id": 63099
          }
        }
      ]
    }
  ]
}

This method lets users get the status of whether or not the TV episodes of a season have been rated. A [valid session](#authentication) id is required.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
<tr><td style="padding-left: 0;" colspan="2">session_id</td></tr>
</table>

GET /tv/{id}/season/{season_number}/account_states
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "81d02d846cfb9ed331681f6a9701aa5b"
{
  "id": 3578,
  "results": [
    {
      "id": 62147,
      "episode_number": 1,
      "rated": {
        "value": 8.0
      }
    },
    {
      "id": 62148,
      "episode_number": 2,
      "rated": false
    },
    {
      "id": 62149,
      "episode_number": 3,
      "rated": false
    },
    {
      "id": 62150,
      "episode_number": 4,
      "rated": false
    },
    {
      "id": 62151,
      "episode_number": 5,
      "rated": false
    },
    {
      "id": 62152,
      "episode_number": 6,
      "rated": false
    },
    {
      "id": 62153,
      "episode_number": 7,
      "rated": {
        "value": 8.5
      }
    },
    {
      "id": 62154,
      "episode_number": 8,
      "rated": false
    },
    {
      "id": 62155,
      "episode_number": 9,
      "rated": false
    },
    {
      "id": 62156,
      "episode_number": 10,
      "rated": false
    },
    {
      "id": 62159,
      "episode_number": 11,
      "rated": false
    },
    {
      "id": 62158,
      "episode_number": 12,
      "rated": false
    },
    {
      "id": 62157,
      "episode_number": 13,
      "rated": false
    },
    {
      "id": 62162,
      "episode_number": 14,
      "rated": false
    },
    {
      "id": 62160,
      "episode_number": 15,
      "rated": false
    },
    {
      "id": 62161,
      "episode_number": 16,
      "rated": {
        "value": 9.0
      }
    }
  ]
}

Get the cast & crew credits for a TV season by season number.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
</table>

GET /tv/{id}/season/{season_number}/credits
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "aed32cd2c6d824a451e7272474559df9"
{
  "cast": [
    {
      "character": "Walter White",
      "credit_id": "52542282760ee313280017f9",
      "id": 17419,
      "name": "Bryan Cranston",
      "profile_path": "/vr3YaG9ehqQY82fsxOFZvlqncX7.jpg",
      "order": 0
    },
    {
      "character": "Skyler White",
      "credit_id": "52542282760ee3132800181b",
      "id": 134531,
      "name": "Anna Gunn",
      "profile_path": "/lKlGjfmu9mJcF4upNrhtG3X9uyq.jpg",
      "order": 1
    },
    {
      "character": "Jesse Pinkman",
      "credit_id": "52542282760ee31328001845",
      "id": 84497,
      "name": "Aaron Paul",
      "profile_path": "/oTceEUb6A9Bg6DeUTJBTETUOEAy.jpg",
      "order": 2
    },
    {
      "character": "Hank Schrader",
      "credit_id": "52542283760ee3132800187b",
      "id": 14329,
      "name": "Dean Norris",
      "profile_path": "/500eNhWneDTXQuEXg6BR269IjHr.jpg",
      "order": 3
    },
    {
      "character": "Marie Schrader",
      "credit_id": "52542283760ee31328001891",
      "id": 1217934,
      "name": "Betsy Brandt",
      "profile_path": "/zpmsca1HCVqYrtWXV9xdmsECDTI.jpg",
      "order": 4
    },
    {
      "character": "Walter White Jr.",
      "credit_id": "52542284760ee313280018a9",
      "id": 1223196,
      "name": "RJ Mitte",
      "profile_path": "/hO5HJKM6p6SQjpdV8Fs64BJWzmT.jpg",
      "order": 5
    }
  ],
  "crew": [
    {
      "department": "Production",
      "id": 1223202,
      "name": "Diane Mercer",
      "job": "Producer",
      "profile_path": null
    },
    {
      "department": "Production",
      "id": 17419,
      "name": "Bryan Cranston",
      "job": "Producer",
      "profile_path": "/vr3YaG9ehqQY82fsxOFZvlqncX7.jpg"
    },
    {
      "department": "Production",
      "id": 1223198,
      "name": "Moira Walley-Beckett",
      "job": "Producer",
      "profile_path": null
    },
    {
      "department": "Writing",
      "id": 24951,
      "name": "Peter Gould",
      "job": "Writer",
      "profile_path": null
    },
    {
      "department": "Writing",
      "id": 1223198,
      "name": "Moira Walley-Beckett",
      "job": "Writer",
      "profile_path": null
    },
    {
      "department": "Writing",
      "id": 103009,
      "name": "Thomas Schnauz",
      "job": "Writer",
      "profile_path": null
    },
    {
      "department": "Production",
      "id": 1218856,
      "name": "Patty Lin",
      "job": "Producer",
      "profile_path": null
    },
    {
      "department": "Production",
      "id": 29924,
      "name": "John Shiban",
      "job": "Producer",
      "profile_path": null
    },
    {
      "department": "Production",
      "id": 1223199,
      "name": "Melissa Bernstein",
      "job": "Producer",
      "profile_path": null
    },
    {
      "department": "Production",
      "id": 1223201,
      "name": "Karen Moore",
      "job": "Producer",
      "profile_path": null
    },
    {
      "department": "Production",
      "id": 24951,
      "name": "Peter Gould",
      "job": "Producer",
      "profile_path": null
    },
    {
      "department": "Production",
      "id": 1223193,
      "name": "George Mastras",
      "job": "Producer",
      "profile_path": null
    },
    {
      "department": "Production",
      "id": 103009,
      "name": "Thomas Schnauz",
      "job": "Producer",
      "profile_path": null
    },
    {
      "department": "Production",
      "id": 1223200,
      "name": "Stewart Lyons",
      "job": "Producer",
      "profile_path": null
    },
    {
      "department": "Production",
      "id": 1223194,
      "name": "Sam Catlin",
      "job": "Producer",
      "profile_path": null
    },
    {
      "department": "Production",
      "id": 66633,
      "name": "Vince Gilligan",
      "job": "Executive Producer",
      "profile_path": "/wSTvJGz7QbJf1HK2Mv1Cev6W9TV.jpg"
    },
    {
      "department": "Production",
      "id": 5162,
      "name": "Mark Johnson",
      "job": "Executive Producer",
      "profile_path": "/yKGF6cbzyP03Gl1QhVLCu1gWSW6.jpg"
    },
    {
      "department": "Production",
      "id": 29779,
      "name": "Michelle MacLaren",
      "job": "Executive Producer",
      "profile_path": null
    }
  ],
  "id": 3572
}

Get the external ids that we have stored for a TV season by season number.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
<tr><td style="padding-left: 0;" colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-left: 0; width: 140px;">language</td><td>ISO 639-1 code.</td></tr>
</table>

GET /tv/{id}/season/{season_number}/external_ids
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "597e78ed2bcf00905893e1451ffc37be"
{
  "freebase_id": "/en/breaking_bad_season_1",
  "freebase_mid": "/m/05yy27m",
  "id": 3572,
  "tvdb_id": 30272,
  "tvrage_id": null
}

Get the images (posters) that we have stored for a TV season by season number.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
<tr><td style="padding-left: 0;" colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-left: 0; width: 140px;">language</td><td>ISO 639-1 code.</td></tr>
<tr><td style="padding-left: 0; width: 140px;">include_image_language</td><td>Comma separated, a valid ISO 69-1. Maximum 5 per request.</td></tr>
</table>

GET /tv/{id}/season/{season_number}/images
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "ce92657001b8893bb305c5ea0d075c91"
{
  "id": 3572,
  "posters": [
    {
      "aspect_ratio": 0.69,
      "file_path": "/6lP6ac4wNmWgtfVvrsl53bMqrKS.jpg",
      "height": 578.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 400.0
    },
    {
      "aspect_ratio": 0.69,
      "file_path": "/wr0pPDfwn4gNQxjNWpkazhN13tZ.jpg",
      "height": 578.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 400.0
    },
    {
      "aspect_ratio": 0.69,
      "file_path": "/zPrE7NvhW6kk9acm8yXjdgDcXZJ.jpg",
      "height": 578.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 400.0
    },
    {
      "aspect_ratio": 0.69,
      "file_path": "/8yHDd0EVpNpWk1VBa9QP4iUmw0v.jpg",
      "height": 578.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 400.0
    },
    {
      "aspect_ratio": 0.69,
      "file_path": "/zJV0m4IFtsmF2QsCk4Yrwv5frNn.jpg",
      "height": 578.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 400.0
    },
    {
      "aspect_ratio": 0.69,
      "file_path": "/z6eMdUp2mF2rsmOnZH2N0S9QGFY.jpg",
      "height": 578.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 400.0
    },
    {
      "aspect_ratio": 0.69,
      "file_path": "/tFnhmMscpXZxiceNOSC6ooxtn51.jpg",
      "height": 578.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 400.0
    },
    {
      "aspect_ratio": 0.69,
      "file_path": "/spPmYZAq2xLKQOEIdBPkhiRxrb9.jpg",
      "height": 578.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 400.0
    },
    {
      "aspect_ratio": 0.69,
      "file_path": "/ryreluyqCf6CxlEFbZSceVGw8NL.jpg",
      "height": 578.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 400.0
    },
    {
      "aspect_ratio": 0.69,
      "file_path": "/m8V6Fm2dV7tNvjCmBpROSU3Zrq4.jpg",
      "height": 578.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 400.0
    },
    {
      "aspect_ratio": 0.69,
      "file_path": "/oW0G4MKWcyNaRGckZf8WRFyO5cH.jpg",
      "height": 578.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 400.0
    },
    {
      "aspect_ratio": 0.69,
      "file_path": "/tkZ9bXm9iFNEdZmblSl8rxgpstT.jpg",
      "height": 578.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 400.0
    },
    {
      "aspect_ratio": 0.69,
      "file_path": "/l5s5lfqpdP4uigrZtu5Sg8hfWqG.jpg",
      "height": 578.0,
      "iso_639_1": "en",
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 400.0
    },
    {
      "aspect_ratio": 0.69,
      "file_path": "/2lhO5zd1nnf7PjC7dGCUo45Volz.jpg",
      "height": 578.0,
      "iso_639_1": "en",
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 400.0
    }
  ]
}

Get the videos that have been added to a TV season (trailers, teasers, etc...)

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
<tr><td style="padding-left: 0;" colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-left: 0; width: 140px;">language</td><td>ISO 639-1 code.</td></tr>
</table>

GET /tv/{id}/season/{season_number}/videos
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "ae542c5a1816a477563ccfa97b68634d"
{
  "id": 3578,
  "results": [
    {
      "id": "5336d65dc3a3680e7f0012f3",
      "iso_639_1": "en",
      "key": "ccuuThlb5Vg",
      "name": "Trailer",
      "site": "YouTube",
      "size": 480,
      "type": "Trailer"
    },
    {
      "id": "5336d677c3a3680e8f0013d9",
      "iso_639_1": "en",
      "key": "bXN2skxH5qU",
      "name": "Promo #1 - We Are Done",
      "site": "YouTube",
      "size": 480,
      "type": "Teaser"
    },
    {
      "id": "5336d6b6c3a3680e8f0013de",
      "iso_639_1": "en",
      "key": "oNPfvEseRdY",
      "name": "Promo #2 - Scared",
      "site": "YouTube",
      "size": 480,
      "type": "Teaser"
    },
    {
      "id": "5336d703c3a3680e70001241",
      "iso_639_1": "en",
      "key": "Z-gURIpg5f4",
      "name": "Teaser 1",
      "site": "YouTube",
      "size": 480,
      "type": "Trailer"
    },
    {
      "id": "5336d71fc3a3680e70001244",
      "iso_639_1": "en",
      "key": "I3An9mDuf7E",
      "name": "Teaser 2",
      "site": "YouTube",
      "size": 480,
      "type": "Teaser"
    },
    {
      "id": "5336d73ac3a3680e87001313",
      "iso_639_1": "en",
      "key": "gAIL5SQ3QFs",
      "name": "Teaser 3",
      "site": "YouTube",
      "size": 480,
      "type": "Teaser"
    }
  ]
}

--
TV Episodes
--

Get the primary information about a TV episode by combination of a season and episode number.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
<tr><td style="padding-left: 0;" colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-left: 0; width: 140px;">language</td><td>ISO 639-1 code.</td></tr>
<tr><td style="padding-left: 0; width: 140px;">append_to_response</td><td>Comma separated, any tv series method</td></tr>
</table>

GET /tv/{id}/season/{season_number}/episode/{episode_number}
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "750a4f3aa57dea5ebdafcc3109c80f5d"
{
  "air_date": "2008-01-20",
  "episode_number": 1,
  "name": "Pilot",
  "overview": "When an unassuming high school chemistry teacher discovers he has a rare form of lung cancer, he decides to team up with a former student and create a top of the line crystal meth in a used RV, to provide for his family once he is gone.",
  "id": 62085,
  "production_code": null,
  "season_number": 1,
  "still_path": "/2IksiG9RPQ4lxYxgiKEhns0N4Zx.jpg",
  "vote_average": 0.0,
  "vote_count": 0
}

Look up a TV episode's changes by episode ID. This method is used in conjunction with the [/tv/{id}/changes](#get-%2F3%2Ftv%2F%7Bid%7D%2Fchanges) method. This method uses the `episode_id` value found in the change entries.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
<tr><td style="padding-left: 0;" colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-left: 0; padding-right: 40px; width: 140px;">start_date</td><td>YYYY-MM-DD</td></tr>
<tr><td style="padding-left: 0; padding-right: 40px; width: 140px;">end_date</td><td>YYYY-MM-DD</td></tr>
</table>

GET /tv/episode/{id}/changes
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "691e0c90d7869a78ead95cb777b870dd"
{
  "changes": [
    {
      "key": "videos",
      "items": [
        {
          "id": "537320ce0e0a267c590011af",
          "action": "added",
          "time": "2014-05-14 07:52:46 UTC",
          "iso_639_1": "en",
          "value": {
            "id": "537320ce0e0a267c590011ac",
            "name": "Anatomy of a Scene - Tyrion's Trial",
            "key": "JVVr4_h8X4g",
            "size": 720,
            "type": "Featurette"
          }
        },
        {
          "id": "537320f50e0a267c590011b6",
          "action": "added",
          "time": "2014-05-14 07:53:25 UTC",
          "iso_639_1": "en",
          "value": {
            "id": "537320f50e0a267c590011b5",
            "name": "Tyrion's Breakdown",
            "key": "uvX4k_3Cmvs",
            "size": 720,
            "type": "Clip"
          }
        }
      ]
    }
  ]
}

This method lets users get the status of whether or not the TV episode has been rated. A [valid session](#authentication) id is required.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
<tr><td style="padding-left: 0;" colspan="2">session_id</td></tr>
</table>

GET /tv/{id}/season/{season_number}/episode/{episode_number}/account_states
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "b9a88e1b9a56794518c435dda774718b"
{
  "id": 62085,
  "rated": false
}

Get the TV episode credits by combination of season and episode number.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
</table>

GET /tv/{id}/season/{season_number}/episode/{episode_number}/credits
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "1407081051e6f9b38a14b4356aff4e11"
{
  "cast": [
    {
      "character": "Walter White",
      "credit_id": "52542282760ee313280017f9",
      "id": 17419,
      "name": "Bryan Cranston",
      "profile_path": "/vr3YaG9ehqQY82fsxOFZvlqncX7.jpg",
      "order": 0
    },
    {
      "character": "Skyler White",
      "credit_id": "52542282760ee3132800181b",
      "id": 134531,
      "name": "Anna Gunn",
      "profile_path": "/lKlGjfmu9mJcF4upNrhtG3X9uyq.jpg",
      "order": 1
    },
    {
      "character": "Jesse Pinkman",
      "credit_id": "52542282760ee31328001845",
      "id": 84497,
      "name": "Aaron Paul",
      "profile_path": "/oTceEUb6A9Bg6DeUTJBTETUOEAy.jpg",
      "order": 2
    },
    {
      "character": "Hank Schrader",
      "credit_id": "52542283760ee3132800187b",
      "id": 14329,
      "name": "Dean Norris",
      "profile_path": "/500eNhWneDTXQuEXg6BR269IjHr.jpg",
      "order": 3
    },
    {
      "character": "Marie Schrader",
      "credit_id": "52542283760ee31328001891",
      "id": 1217934,
      "name": "Betsy Brandt",
      "profile_path": "/zpmsca1HCVqYrtWXV9xdmsECDTI.jpg",
      "order": 4
    },
    {
      "character": "Walter White Jr.",
      "credit_id": "52542284760ee313280018a9",
      "id": 1223196,
      "name": "RJ Mitte",
      "profile_path": "/hO5HJKM6p6SQjpdV8Fs64BJWzmT.jpg",
      "order": 5
    }
  ],
  "crew": [
    {
      "id": 66633,
      "name": "Vince Gilligan",
      "department": "Writing",
      "job": "Writer",
      "profile_path": "/wSTvJGz7QbJf1HK2Mv1Cev6W9TV.jpg"
    },
    {
      "id": 66633,
      "name": "Vince Gilligan",
      "department": "Directing",
      "job": "Director",
      "profile_path": "/wSTvJGz7QbJf1HK2Mv1Cev6W9TV.jpg"
    }
  ],
  "guest_stars": [
    {
      "id": 115688,
      "name": "Carmen Serano",
      "credit_id": "52542273760ee31328000676",
      "character": "Carmen Molina",
      "order": 0,
      "profile_path": "/gJOeg4jtRZR5xRBizYX2IibByVj.jpg"
    },
    {
      "id": 92495,
      "name": "John Koyama",
      "credit_id": "52542273760ee3132800068e",
      "character": "Emilio Koyama",
      "order": 1,
      "profile_path": null
    },
    {
      "id": 1223192,
      "name": "Roberta Marquez Seret",
      "credit_id": "52542275760ee313280006a2",
      "character": "Chad's Girlfriend",
      "order": 2,
      "profile_path": null
    },
    {
      "id": 1216132,
      "name": "Aaron Hill",
      "credit_id": "52542275760ee313280006b4",
      "character": "Jock",
      "order": 3,
      "profile_path": "/vTqz2FIutFJPG73j7gFDTWomTbb.jpg"
    },
    {
      "id": 1046460,
      "name": "Max Arciniega",
      "credit_id": "52725845760ee3046b09426e",
      "character": "Krazy-8",
      "order": 31,
      "profile_path": "/hRyw2cglnWU42vHZUag7b9dIf4u.jpg"
    },
    {
      "id": 1223197,
      "name": "Marius Stan",
      "credit_id": "5272587a760ee3045009ddec",
      "character": "Bogdan Wolynetz",
      "order": 32,
      "profile_path": null
    },
    {
      "id": 161591,
      "name": "Gregory Chase",
      "credit_id": "52725cb1760ee3044d0b9984",
      "character": "Dr. Belknap (as Greg Chase)",
      "order": 33,
      "profile_path": null
    },
    {
      "id": 61535,
      "name": "Steven Michael Quezada",
      "credit_id": "5271b489760ee35b3e0881a7",
      "character": "Steven Gomez",
      "order": 34,
      "profile_path": null
    }
  ],
  "id": 62085
}

Get the external ids for a TV episode by comabination of a season and episode number.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
<tr><td style="padding-left: 0;" colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-left: 0; width: 140px;">language</td><td>ISO 639-1 code.</td></tr>
</table>

GET /tv/{id}/season/{season_number}/episode/{episode_number}/external_ids
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "8b413cfeba24aca5be89df985cdf0fa2"
{
  "imdb_id": "tt0959621",
  "freebase_id": null,
  "freebase_mid": "/m/03mb620",
  "id": 62085,
  "tvdb_id": 349232,
  "tvrage_id": 637041
}

Get the images (episode stills) for a TV episode by combination of a season and episode number. Since episode stills don't have a language, this call will always return all images.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
</table>

GET /tv/{id}/season/{season_number}/episode/{episode_number}/images
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "dd9a7fa69c35bbdf55b63b08bfe7ab52"
{
  "id": 62085,
  "stills": [
    {
      "aspect_ratio": 1.48,
      "file_path": "/2IksiG9RPQ4lxYxgiKEhns0N4Zx.jpg",
      "height": 271.0,
      "iso_639_1": null,
      "vote_average": 0.0,
      "vote_count": 0,
      "width": 400.0
    }
  ]
}

This method lets users create (or delete) a rating on a TV episode. A [valid session](#authentication) id **or** [guest session](#get-%2F3%2Fauthentication%2Fguest_session%2Fnew) id is required.

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
<tr><td style="padding-left: 0;" colspan="2">session_id</td></tr>
<tr><td style="padding-left: 0;" colspan="2">guest_session_id</td></tr>
<tr><td style="padding-left: 0;" colspan="2"><strong>Required JSON Body (when POST-ing)</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">value</td></tr>
</table>

POST /tv/{id}/season/{season_number}/episode/{episode_number}/rating
> Accept: application/json
> Content-Type: application/json
{
    "value": 7.5
}
< 200
< Content-Type: application/json
{
    "status_code": 1,
    "status_message": "Success"
}

DELETE /tv/{id}/season/{season_number}/episode/{episode_number}/rating
> Accept: application/json
> Content-Type: application/json
< 200
< Content-Type: application/json
{
  "status_code": 13,
  "status_message": "The item/record was deleted successfully."
}

Get the videos that have been added to a TV episode (teasers, clips, etc...)

<table style="width: 100%; margin: 12px 0 0 0;">
<tr><td style="padding-left: 0;" colspan="2"><strong>Required Parameters</strong></td></tr>
<tr><td style="padding-left: 0;" colspan="2">api_key</td></tr>
<tr><td style="padding-left: 0;" colspan="2"><strong>Optional Parameters</strong></td></tr>
<tr><td style="padding-left: 0; width: 140px;">language</td><td>ISO 639-1 code.</td></tr>
</table>

GET /tv/{id}/season/{season_number}/episode/{episode_number}/videos
> Accept: application/json
< 200
< Content-Type: application/json
< ETag: "ae542c5a1816a477563ccfa97b68634d"
{
  "id": 62161,
  "results": [
    {
      "id": "5336d65dc3a3680e7f0012f3",
      "iso_639_1": "en",
      "key": "ccuuThlb5Vg",
      "name": "Trailer",
      "site": "YouTube",
      "size": 480,
      "type": "Trailer"
    }
  ]
}
