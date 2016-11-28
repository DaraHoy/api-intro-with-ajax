# Intro to APIs & AJAX

## Learning Objectives

- Describe what an API is and why we might use one.
- Understand the format of API data.
- Explain the common role of JSON on the web.
- Use jQuery's `$.ajax()` method to make GET requests for data.
- Use jQuery's "promise-like" methods to handle AJAX responses.
- Render new HTML content using data loaded from an AJAX request.
- Perform POST, PUT and DELETE requests to an API to modify data.

## Turn & Talk

[Open up Google Maps in your browser](https://www.google.com/maps). As you interact with the web app, consider the following questions...

<details>
  <summary><strong>What is happening asynchronously?</strong></summary>

  > Pretty much everything.

</details>

<details>
  <summary><strong>How would Google Maps work if everything happened synchronously?</strong></summary>

  > The page would need to re-load whenever the map is updated. Consider how http requests work within your rails application.

</details>

<details>
  <summary><strong>Why might synchronous programming not be effective for the front end?</strong></summary>

  > We don't want to sit around and wait for code to execute before we load the rest of our script. It would be really nice if we could just describe what we want to happen when the code finally does execute.

</details>

## Framing

**What is an API?**

> Basically, an API is a service that provides raw data for public use.

API stands for "Application Program Interface" and technically applies to all of software design. The DOM and jQuery are actually examples of APIs! Since the explosion of information technology, however, the term now commonly refers to web URLs that can be accessed for raw data.

APIs publish data for public use. As third-party software developers, we can access an organization's API and use their data within our own applications.

<details>
  <summary><strong>Q: Why do we care?</strong></summary>

  > Why recreate data when we don't have to? Think about projects or ideas that would be easier to build if you could pull in data already gathered elsewhere.

  > APIs can provide us with data that would we would otherwise not be able to create ourselves.

</details>

Remember this for when we move into building single page applications. Now, however, is the perfect time to start understanding how to obtain data on the client side and then render it on the browser.

## API Exploration

**Check out these stock quotes...**

* [http://dev.markitondemand.com/Api/Quote/json?symbol=AAPL](http://dev.markitondemand.com/Api/Quote/json?symbol=AAPL)
* [http://dev.markitondemand.com/Api/Quote/json?symbol=GOOGL](http://dev.markitondemand.com/Api/Quote/json?symbol=GOOGL)

> Does the JSON look unreadable in the browser? If you're using Chrome, install the [JSON Formatter plugin](https://chrome.google.com/webstore/detail/json-formatter/bcjindcccaagfpapjjmafapmmgkkhgoa?hl=en).

### Think, Pair, Share

Form pairs and explore the API links in the below table. Record any observations that come to mind. In particular, think about what you see in the URL and the API response itself.

| API | Sample URL |
|-----|------------|
| **[This for That](http://itsthisforthat.com/)** | http://itsthisforthat.com/api.php?json |
| **[Giphy](https://github.com/Giphy/GiphyAPI)** | http://api.giphy.com/v1/gifs/search?q=funny+cat&api_key=dc6zaTOxFJmzC |
| **[OMDB API](http://www.omdbapi.com/)** | http://www.omdbapi.com/?t=Game%20of%20Thrones&Season=1 |
| **[StarWars](http://swapi.co/)** | http://swapi.co/api/people/3 |
| **[Stocks](http://dev.markitondemand.com/MODApis/)** | http://dev.markitondemand.com/Api/Quote/xml?symbol=AAPL |


### Why Just Data?

Sometimes thats's all we need. All this information, from all these browsers and all these servers, has to travel through the network. That's almost certainly the slowest part of the request cycle. We want to minimize the bits. There are times when we just need the data. For those times, we want a concise format.   

### What is Serialized Data?

All data sent via HTTP are strings. Unfortunately, what we really want to pass between web applications is **structured data** (i.e., arrays and hashes). Thus, native data structures can be **serialized** into a string representation of the data. This string can be transmitted and then parsed back into data by another web agent.  

There are **two** major serialized data formats...  

#### JSON

**JSON** stands for "JavaScript Object Notation" and has become a universal standard for serializing native data structures for transmission. It is light-weight, easy to read and quick to parse.

```json
{
  "users": [
    {"name": "Bob", "id": 23},
    {"name": "Tim", "id": 72}
  ]
}
```
> Remember, JSON is a serialized format. While it may look like an object, it needs to be parsed so we can interact with it as a true Javascript object.

#### XML

**XML** stands for "eXtensible Markup Language" and is the granddaddy of serialized data formats (itself based on HTML). XML is fat, ugly and cumbersome to parse. It remains a major format, however, due to its legacy usage across the web. You'll probably always favor using a JSON API, if available.

```
<users>
  <user id="23">
    <name><![CDATA[Bob]]></name>
  </user>
  <user id="72">
    <name><![CDATA[Tim]]></name>
  </user>
</users>
```

**Many APIs publish data in multiple formats, for example...**

* [http://dev.markitondemand.com/Api/Quote/json?symbol=AAPL](http://dev.markitondemand.com/Api/Quote/json?symbol=AAPL)
* [http://dev.markitondemand.com/Api/Quote/xml?symbol=AAPL](http://dev.markitondemand.com/Api/Quote/xml?symbol=AAPL)

## Where Do We Find APIs?

APIs are published everywhere. Chances are good that most major content sources you follow online publish their data in some type of serialized format. Heck, [even Marvel publishes an API](http://developer.marvel.com/documentation/getting_started). Look around for a "Developers" section on major websites.

**That sounds hard. Can't you just give me a freebie?**

Try the [Programmable Web API Directory](http://www.programmableweb.com/apis/directory) or the [Public APIs Directory](http://www.publicapis.com/).

## What Is An API Key?

While the majority of APIs are free to use, many of them require an API "key" that identifies the developer requesting data access. This is done to regulate usage and prevent abuse. Some APIs also rate-limit developers, meaning they have caps on the free data allowed during a given time period.

**Try hitting the [Giphy](https://api.giphy.com/) API...**

* No key: [http://api.giphy.com/v1/gifs/search?q=funny+cat](http://api.giphy.com/v1/gifs/search?q=funny+cat)

* With key: [http://api.giphy.com/v1/gifs/search?q=funny+cat&api_key=dc6zaTOxFJmzC](http://api.giphy.com/v1/gifs/search?q=funny+cat&api_key=dc6zaTOxFJmzC)

**It is very important that you not push your API keys to a public Github repo.**

> This is especially true when working with [Amazon Web Services (AWS)](https://aws.amazon.com/). Here's an example of a [stolen key horror story](https://wptavern.com/ryan-hellyers-aws-nightmare-leaked-access-keys-result-in-a-6000-bill-overnight).

## AJAX

**So we know what an API is. Now what?**

How can we use an API to dynamically manipulate the DOM with the given data? **AJAX**. As you'll come to learn, this allows us to build single page applications that do not require refreshes.

**AJAX**, which stands for "Asynchronous Javascript and XML," is the method through which we are able to make HTTP requests. The standard requests we will be making are `GET` `POST` `PUT` `PATCH` and `DELETE`.

| Type of Request | What's It Do? | An Example URL |
|-----------------|---------------|----------------|
| `GET`  | Read | [http://swapi.co/api/planets/](http://swapi.co/api/planets/) |
| `POST` | Create | [http://swapi.co/api/planets/](http://swapi.co/api/planets/) |
| `PUT` | Update | [http://swapi.co/api/planets/2/](http://swapi.co/api/planets/2) |
| `PATCH` | Update | [http://swapi.co/api/planets/2/](http://swapi.co/api/planets/2) |
| `DELETE` | Delete | [http://swapi.co/api/planets/2/](http://swapi.co/api/planets/2) |

<details>
  <summary><strong>Q: Why do you think there is a "2" at the end of the URLs in the last three rows? (Hint: you could replace it with any number)</strong></summary>

  > You'll notice that the URLs for `PUT` `PATCH` and `DELETE` end with a number. That's because these actions are updating or deleting a *particular* planet. That number is a unique identifier for a particular planet defined on the back-end. We'll learn more about this later in the course...

</details>


## GET Data From an API Using AJAX

### I Do: GET From OMDB

> **Do not follow along for this portion of the lesson.** You will have the opportunity to do it yourself afterwards.

Let's build a very simple app that posts a movie title and poster after searching for it. We'll do this using the [OMDB API](http://www.omdbapi.com/).

**[OMDB movie search](https://github.com/ga-wdi-exercises/omdb-api)**

The starter code is linked above. It contains a basic HTML/CSS/JS setup. If you open up the HTML in the browser, you will notice that searching for something returns no results.

Let's go ahead and add in the AJAX request...

```js
// Get value from search input field.
var keyword = $("input[name='keyword']").val();
var url = "https://www.omdbapi.com/?t="+keyword
$.ajax({
  url: url,
  type: "GET",
  dataType: "json"
})
```

`$.ajax` takes an object as an argument with at least three key-value pairs...
  1. The URL endpoint for the JSON object.
  2. Type of HTTP request.
  3. Datatype. Usually JSON.

<details>

  <summary><strong>Q: How did we know which URL to use?</strong></summary>

  > The [OMDB API documentation](http://www.omdbapi.com/)

</details>

<details>

  <summary><strong>Q: What does it mean to set `type` to `GET`?</strong></summary>

  > We are **reading** the response sent back to us. To `GET` means to "read."

</details>

<details>

  <summary><strong>Q: So our application can make an AJAX call. Why aren't we seeing anything after clicking "Search"?</strong></summary>

  > We haven't told our application what to do once it receives a response.

</details>

### Promises

Now we need to tell our AJAX method what to do next. We can do this by doing something with the its return value, which comes in the form of a **promise**. We can use **promise methods** to tell the `$.ajax` request what to do if the request is successful or not. In this case, we are going to add three...

#### `.done`

A promise method for when the AJAX call is successful...

```js
.done(function(response){
  console.log(response);
});
```

<details>
  <summary><strong>Q: What are we passing into the `.done` promise method? Does this remind you of anything we've done previously in class?</strong></summary>

  > `.done` requires a callback that determines what we do after a successful AJAX call.

</details>

#### `.fail`

A promise method for when the AJAX call fails...

```js
.fail(function (){
  console.log("fail");
});
```

> `.fail` requires a callback that determines what we do after an unsuccessful AJAX call.

#### `.always`

A promise method that is executed regardless of whether the AJAX call succeeds or fails...

```js
.always(function(){
  console.log("Something happens");
});
```
> `.always` requires a callback that determines what we do regardless of a successful or unsuccessful call. While technically not necessary, it certainly doesn't hurt to include.

### You Do: GET From OMDB

Now it's your turn to do the same. Start by cloning down the starter code...

`$ git clone git@github.com:ga-wdi-exercises/omdb-api.git`

Then you may either walk through the specific steps outlined in the previous section or try to do it yourself using this pseudocode...

```
When the user clicks the search button.
  Store the search term.
  Create and store the URL.
  Make an AJAX call, indicating the proper URL, type and data type.
    Indicate what should be done after a successful API call.
    Indicate what should be done after a failed API call.
```

#### Bonus I

You'll notice we're only receiving one movie in the API response. Use the OMDB Documentation to figure out how to return **multiple** movies.

#### Bonus II

Instead of just logging the API response to the console, append it to the page!


## You Do: The Weather Underground API
Fork and clone [this repo](https://github.com/ga-wdi-exercises/weather_teller) and follow the directions.

## Conclusion

- What is an API?
- Why are APIs useful/important?
- What is AJAX?
- What information might we need to pass into an AJAX call?
- How do we go about interacting with the response of an AJAX call?


## Resources
* [GA DC AJAX ](https://github.com/ga-wdi-lessons/api-intro-with-ajax)
* [GA DC AJAX (Updated) ](https://github.com/ga-wdi-lessons/js-ajax)
* [Andy Kim's blog](http://andrewsunglaekim.github.io/Server-side-api-calls-wrapped-in-ruby-classes/)
* [Postman](https://www.getpostman.com/)
* [Intro to APIs](https://zapier.com/learn/apis/chapter-1-introduction-to-apis/)
* [Practice with APIs](https://github.com/ga-dc/weather_teller)
* [Beautify your JSON in Chrome](https://chrome.google.com/webstore/detail/jsonview/chklaanhfefbnpoihckbnefhakgolnmc?hl=en)
