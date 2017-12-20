# Working with APIs

## 1: What's An API?

We've worked with data sets pretty extensively so far. While they're popular resources, there are many cases where it's impractical to use one.

Here are a few situations where data sets don't work well:

- The data change frequently. It doesn't really make sense to regenerate a data set of stock prices, for example, and download it every minute. This approach would require a lot of bandwidth, and be very slow.
- You only want a small piece of a much larger data set. [Reddit](https://www.reddit.com/) comments are one example. What if you want to pull just your own comments from reddit? It doesn't make much sense to download the entire reddit database, then filter it for a few items.
- It involves repeated computation. For example, Spotify has an API that can tell you the genre of a piece of music. You could theoretically create your own classifier and use it to categorize music, but you'll never have as much data as Spotify does.

In cases like these, an **application program interface (API)** is the right solution. An API is a set of methods and tools that allows different applications to interact with each other. Programmers use APIs to query and retrieve data dynamically (which they can then integrate with their own apps). A client can retrieve information quickly and effectively through an API.

Reddit, Spotify, Twitter, Facebook, and many other companies provide free APIs that enable developers to access the information they store on their servers; others charge for access to their APIs.

In this mission, we'll query a basic API to retrieve data about the [International Space Station](https://en.wikipedia.org/wiki/International_Space_Station) (ISS). Using an API will save us time and effort, instead of doing all the computation ourselves.

## 2: Introduction To API Requests

Organizations host their APIs on **Web servers**. When you type `www.google.com` in your browser's address bar, your computer is actually asking the `www.google.com` server for a Web page, which it then returns to your browser.

APIs work much the same way, except instead of your Web browser asking for a Web page, your program asks for data. The API usually returns this data in [JavaScript Object Notation](http://json.org/) (JSON) format. We'll discuss JSON more later on in this mission.

We make an API request to the Web server we want to get data from. The server then replies and sends it to us. In Python, we use the [requests library](http://www.python-requests.org/en/latest/) to do this.

## 3: Types Of Requests

There are many different types of requests. The most common is a *GET* request, which we use to retrieve data. We'll explore the other types in later missions.

We can use a simple *GET* request to retrieve information from the [OpenNotify](http://open-notify.org/) API.

OpenNotify has several API **endpoints**. An endpoint is a server route for retrieving specific data from an API. For example, the `/comments` endpoint on the reddit API might retrieve information about comments, while the `/users` endpoint might retrieve data about users.

The first endpoint we'll look at on OpenNotify is the `iss-now.json` endpoint. This endpoint gets the current latitude and longitude position of the ISS. A data set wouldn't be a great fit for this task because the information changes often, and involves some calculation on the server.

[Check out the complete list](http://open-notify.org/Open-Notify-API/) of OpenNotify endpoints.

### Instructions

- The server will send a status code indicating the success or failure of your request. You can get the status code of the response from `response.status_code`.Assign the status code to the variable `status_code`.

```
# Make a get request to get the latest position of the ISS from the OpenNotify API.
response = requests.get("http://api.open-notify.org/iss-now.json")
status_code = response.status_code
```

## 4: Understanding Status Codes

The request we just made returned a **status code** of `200`. Web servers return status codes every time they receive an API request. A status code provides information about what happened with a request. Here are some codes that are relevant to *GET* requests:

- `200` - Everything went okay, and the server returned a result (if any).
- `301` - The server is redirecting you to a different endpoint. This can happen when a company switches domain names, or an endpoint's name has changed.
- `401` - The server thinks you're not authenticated. This happens when you don't send the right credentials to access an API (we'll talk about this in a later mission).
- `400` - The server thinks you made a bad request. This can happen when you don't send the information the API requires to process your request, among other things.
- `403` - The resource you're trying to access is forbidden; you don't have the right permissions to see it.
- `404` - The server didn't find the resource you tried to access.

### Instructions

- Make a *GET* request to `http://api.open-notify.org/iss-pass`.Assign the status code of the response to `status_code`.

```
# Enter your answer below.
response = requests.get("http://api.open-notify.org/iss-pass")
status_code = response.status_code
```

## 5: Hitting The Right Endpoint

`iss-pass` wasn't a valid endpoint, so the API's server sent us a `404` status code in response. We forgot to add `.json` at the end, like the [API documentation](http://open-notify.org/Open-Notify-API/) tells us to do.

### Instructions

- Make a *GET* request to `http://api.open-notify.org/iss-pass.json`.Assign the status code of the response to `status_code`.

```
# Enter your answer below.
response = requests.get("http://api.open-notify.org/iss-pass.json")
status_code = response.status_code
```

## 6: Adding Query Parameters

You'll see that in the last example, we got a `400` status code, which indicates a bad request. If you look at the documentation for the OpenNotify API, we see that the [ISS Pass](http://open-notify.org/Open-Notify-API/ISS-Pass-Times/) endpoint requires two *parameters*.

This endpoint returns the next time the ISS will pass over a given location on the Earth.

To request this information, we'll need to pass the coordinates for a specific location to the API. We do this by passing in two parameters, latitude and longitude.

To accomplish this, we can add an optional keyword argument, `params`, to our request. In this case, we need to pass in two parameters:

- `lat` - The latitude of the location
- `lon` - The longitude of the location

We can make a dictionary that contains these parameters, and then pass them into the function.

We can also do the same thing directly by adding the query parameters to the url, like this:

`http://api.open-notify.org/iss-pass.json?lat=40.71&lon=-74`

It's almost always preferable to set up the parameters as a dictionary, because the `requests` library we mentioned earlier takes care of certain issues, like properly formatting the query parameters.

### Instructions

- Use a dictionary and the `parameters` argument to get a response for the latitude `37.78` and the longitude `-122.41`(the coordinates of San Francisco).Retrieve the content of the response with `response.content`.Assign the content to the variable `content`.

```
# Set up the parameters we want to pass to the API.
# This is the latitude and longitude of New York City.
parameters = {"lat": 40.71, "lon": -74}

# Make a get request with the parameters.
response = requests.get("http://api.open-notify.org/iss-pass.json", params=parameters)

# Print the content of the response (the data the server returned)
print(response.content)

# This gets the same data as the command above
response = requests.get("http://api.open-notify.org/iss-pass.json?lat=40.71&lon=-74")
print(response.content)
parameters = {"lat": 37.78, "lon": -122.41}
response = requests.get("http://api.open-notify.org/iss-pass.json", params=parameters)
content = response.content
```

## 7: JSON Format

You may have noticed that the content of the API response we received earlier was a `string`. Strings are the way we pass information back and forth through APIs, but it's hard to get the information we want out of them. How do we know how to decode the string we receive and work with it in Python?

Luckily, there's a format we call JSON. We mentioned it earlier in the mission. This format encodes data structures like lists and dictionaries as strings to ensure that machines can read them easily. JSON is the primary format for sending and receiving data through APIs.

Python offers great support for JSON through its `json` library. We can convert *lists* and *dictionaries* to JSON, and vice versa. Our ISS Pass data, for example, is a dictionary encoded as a string in JSON format.

The JSON library has two main methods:

- `dumps` -- Takes in a Python object, and converts it to a string
- `loads` -- Takes a JSON string, and converts it to a Python object

### Instructions

- Use the JSON function `loads` to convert `fast_food_franchise_string` to a Python object.Assign the resulting Python object to `fast_food_franchise_2`.

```
# Make a list of fast food chains.
best_food_chains = ["Taco Bell", "Shake Shack", "Chipotle"]
print(type(best_food_chains))

# Import the JSON library.
import json

# Use json.dumps to convert best_food_chains to a string.
best_food_chains_string = json.dumps(best_food_chains)
print(type(best_food_chains_string))

# Convert best_food_chains_string back to a list.
print(type(json.loads(best_food_chains_string)))

# Make a dictionary
fast_food_franchise = {
    "Subway": 24722,
    "McDonalds": 14098,
    "Starbucks": 10821,
    "Pizza Hut": 7600
}

# We can also dump a dictionary to a string and load it.
fast_food_franchise_string = json.dumps(fast_food_franchise)
print(type(fast_food_franchise_string))
fast_food_franchise_2 = json.loads(fast_food_franchise_string)
```

## 8: Getting JSON From A Request

We can get the content of a response as a Python object by using the `.json()`method on the response.

### Instructions

- Get the `duration` value of the ISS' first pass over *San Francisco* (this is the `duration` key of the first dictionary in the `response` list).Assign the value to `first_pass_duration`.

```
8: Getting JSON From A Request
We can get the content of a response as a Python object by using the .json() method on the response.

Instructions
Get the duration value of the ISS' first pass over San Francisco (this is the duration key of the first dictionary in the response list).
Assign the value to first_pass_duration.
```

## 9: Content Type

The server sends more than a status code and the data when it generates a response. It also sends metadata containing information on how it generated the data and how to decode it. This information appears in the *response headers*. We can access it using the `.headers` property that responses have.

The headers will appear as a dictionary. For now, the `content-type` within the headers is the most important key. It tells us the format of the response, and how to decode it. For the OpenNotify API, the format is JSON, which is why we could decode it with JSON earlier.

### Instructions

- Get `content-type` from `response.headers`.Assign the content type to the `content_type`variable.

```
# Headers is a dictionary
print(response.headers)
content_type = response.headers["content-type"]
```

## 10: Finding The Number Of People In Space

OpenNotify has one more API endpoint, `astros.json`. It tells us how many people are currently in space. You can find the format of the responses [here](http://open-notify.org/Open-Notify-API/People-In-Space/).

### Instructions

- Find how many people are currently in space.
  - Assign the result to `in_space_count`.

```
# Call the API here.
response = requests.get("http://api.open-notify.org/astros.json")
json_data = response.json()

in_space_count = json_data["number"]
```

# Intermediate APIs

## 1: Introduction

We looked at a basic API in the last mission. That API didn't require authentication, but most do. Imagine that you're using the reddit API to pull a list of your private messages. It would be a huge privacy breach for reddit to give that information to anyone, so requiring authentication makes sense.

APIs also use authentication to perform **rate limiting**. Developers typically use APIs to build interesting applications or services. In order to ensure that it remains available and responsive for all users, an API will prevent you from making too many requests in too short a time. We call this restriction rate limiting. It ensures that one user can't overload the API server by making too many requests too fast.

In this mission, we'll explore the GitHub API and use it to pull some interesting data on repositories and users. GitHub is a site for hosting code. If you haven't looked at it, you should - it's a great place to share a portfolio.

GitHub has user accounts ([example](https://github.com/torvalds)), repositories that contain code ([example](https://github.com/torvalds/linux)), and organizations that companies can create ([example](https://github.com/dataquestio)).

Take a look at the [documentation for the GitHub API](https://developer.github.com/v3/), and specifically the [authentication](https://developer.github.com/v3/#authentication) section.

## 2: API Authentication

To authenticate with the GitHub API, we'll need to use an access token. An access token is a credential we can [generate on GitHub's website](https://github.com/settings/tokens). The token is a string that the API can read and associate with your account.

Using a token is preferable to a username and password for a few reasons:

- Typically, you'll be accessing an API from a script. If you put your username and password in the script and someone manages to get their hands on it, they can take over your account. In contrast, you can revoke an access token to cancel an unauthorized person's access if there's a security breach.
- Access tokens can have scopes and specific permissions. For instance, you can make a token that has permission to write to your GitHub repositories and make new ones. Or, you can make a token that can only read from your repositories. Using read-access-only tokens in potentially insecure or shared scripts gives you more control over security.

You'll need to pass your token to the GitHub API through an *Authorization header*. Just like the server sends headers in response to our request, we can send the server headers when we make a request. Headers contain metadata about the request. We can use Python's `requests` library to make a dictionary of headers, and then pass it into our request.

We need to include the word `token` in the Authorization header, followed by our access token. Here's an example of an Authorization header:

```

```

```
{"Authorization": "token 1f36137fbbe1602f779300dad26e4c1b7fbab631"}
```

In this case, our access token is `1f36137fbbe1602f779300dad26e4c1b7fbab631`. GitHub generated this token and associated it with the account of `Vik Paruchuri`.

You should **never** share your token with anyone you don't want to have access to your account. We've revoked the token you'll be using throughout this mission, so it isn't valid anymore. Consider a token somewhat equivalent to a password, and store it securely.

### Instructions

- Make an authenticated request to `https://api.github.com/users/VikParuchuri/orgs`. This will give us a list of the organizations a GitHub user belongs to.Assign the JSON content of the response to `orgs` (you can get this with `response.json()`).

```
# Create a dictionary of headers containing our Authorization header.
headers = {"Authorization": "token 1f36137fbbe1602f779300dad26e4c1b7fbab631"}

# Make a GET request to the GitHub API with our headers.
# This API endpoint will give us details about Vik Paruchuri.
response = requests.get("https://api.github.com/users/VikParuchuri", headers=headers)

# Print the content of the response.  As you can see, this token corresponds to the account of Vik Paruchuri.
print(response.json())
response = requests.get("https://api.github.com/users/VikParuchuri/orgs", headers=headers)
orgs = response.json()
```

## 3: Endpoints And Objects

APIs usually let us retrieve information about specific objects in a database. On the previous screen, for example, we retrieved information about a specific user object, `VikParuchuri`. We could also retrieve information about other GitHub users through the same endpoint. For example, `https://api.github.com/users/torvalds`would get us information about [Linus Torvalds](https://en.wikipedia.org/wiki/Linus_Torvalds).

### Instructions

- Use the endpoint `https://api.github.com/users/torvalds` with the same headers from before to get information about Linus Torvalds.Use the `response.json()` method to get the JSON of the response.Assign the result to `torvalds`.

```
# We've loaded headers in.
response = requests.get("https://api.github.com/users/torvalds", headers=headers)
torvalds = response.json()
```

## 4: Other Objects

In addition to users, the GitHub API has a few other types of objects. For example, `https://api.github.com/orgs/dataquestio`will retrieve information about the Dataquest organization on GitHub.`https://api.github.com/repos/octocat/Hello-World` will give us information about the [`Hello-World`](https://github.com/octocat/Hello-World) repository that the user `octocat` owns.

GitHub offers full [documentation](https://developer.github.com/v3/) for all of the API's endpoints.

### Instructions

- Make a *GET* request to the `https://api.github.com/repos/octocat/Hello-World`endpoint.Assign the JSON result to `hello_world`.

```
# Enter your answer here.
response = requests.get("https://api.github.com/repos/octocat/Hello-World", headers=headers)
hello_world = response.json()
```

## 5: Pagination

Sometimes, a request can return a lot of objects. This might happen when you're doing something like listing out all of a user's repositories, for example. Returning too much data will take a long time and slow the server down. For example, if a user has 1,000+ repositories, requesting all of them might take 10+ seconds. This isn't a great user experience, so it's typical for API providers to implement **pagination**. This means that the API provider will only return a certain number of records per page. You can specify the page number that you want to access. To access all of the pages, you'll need to write a loop.

To get the repositories a user has *starred* (marked as interesting), we can use the following API endpoint:

`https://api.github.com/users/VikParuchuri/starred`

We can add two pagination query parameters to it - `page`, and `per_page`.`page` is the page we want to access, and `per_page` is the number of records we want to see on each page. Typically, API providers enforce a cap on how high `per_page` can be, because setting it to an extremely high value defeats the purpose of pagination.

Check out the [Github API documentation on pagination](https://developer.github.com/v3/#pagination).

### Instructions

- Get the second page of repositories that Vik Paruchuri starred from the `https://api.github.com/users/VikParuchuri/starred`endpoint.Assign the JSON of the response to `page2_repos`.

```
params = {"per_page": 50, "page": 1}
response = requests.get("https://api.github.com/users/VikParuchuri/starred", headers=headers, params=params)
page1_repos = response.json()
params = {"per_page": 50, "page": 2}
response = requests.get("https://api.github.com/users/VikParuchuri/starred", headers=headers, params=params)
page2_repos = response.json()
```

## 6: User-Level Endpoints

So far, we've looked at endpoints where we need to explicitly provide the username of the person whose information we're looking up. For example, we used `https://api.github.com/users/VikParuchuri/starred` to pull up the repositories that `VikParuchuri` starred.

Since we've authenticated with our token, the system knows who we are, and can show us some relevant information without our having to specify our username.

Making a *GET* request to `https://api.github.com/user` will give us information about the user the authentication token is for.

There are other endpoints that behave like this. They automatically provide information or allow us to take actions as the authenticated user.

### Instructions

- Make a *GET* request to the `"https://api.github.com/user"` endpoint.Assign the JSON of the result to the `user` variable.

```
# Enter your code here.
response = requests.get("https://api.github.com/user", headers=headers)
user = response.json()
```

## 7: POST Requests

So far, we've been making *GET* requests. We use *GET* requests to retrieve information from a server (hence the name GET). There are a few other types of API requests.

For example, we use *POST* requests to send information (instead of retrieve it), and to create objects on the API's server. With the GitHub API, we can use *POST* requests to create new repositories.

Different API endpoints choose what types of requests they will accept. Not all endpoints will accept a *POST* request, and not all will accept a *GET* request. You'll have to consult the [API's documentation](https://developer.github.com/v3/) to figure out which endpoints accept which types of requests.

We can make *POST* requests using `requests.post`. *POST* requests almost always include data, because we need to send the data the server will use to create the new object.

We pass in the data in a way that's very similar to what we do with query parameters and *GET* requests:

```

```

```
payload = {"name": "test"}
```

```
requests.post("https://api.github.com/user/repos", json=payload)
```

The code above will create a new repository named `test` under the account of the currently authenticated user. It will convert the payload dictionary to JSON, and pass it along with the *POST* request.

Check out GitHub's [API documentation for repositories](https://developer.github.com/v3/repos/) to see a full list of what data we can pass in with this *POST* request. Here are just a couple data points:

- `name` -- Required, the name of the repository
- `description` -- Optional, the description of the repository

A successful *POST* request will usually return a `201` [status code](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes) indicating that it was able to create the object on the server. Sometimes, the API will return the JSON representation of the new object as the content of the response.

### Instructions

- Create a repository named `learning-about-apis`.Assign the status code of the response to the `status`variable.

```
# Create the data we'll pass into the API endpoint.  While this endpoint only requires the "name" key, there are other optional keys.
payload = {"name": "test"}

# We need to pass in our authentication headers!
response = requests.post("https://api.github.com/user/repos", json=payload, headers=headers)
print(response.status_code)
payload = {"name": "learning-about-apis"}
response = requests.post("https://api.github.com/user/repos", json=payload, headers=headers)
status = response.status_code
```

## 8: PUT/PATCH Requests

Sometimes we want to update an existing object, rather than create a new one. This is where *PATCH* and *PUT* requests come into play.

We use *PATCH* requests when we want to change a few attributes of an object, but don't want to resend the entire object to the server. Maybe we just want to change the name of our repository, for example.

We use *PUT* requests to send the complete object we're revising as a replacement for the server's existing version.

In practice, API developers don't always respect this convention. Sometimes API endpoints that accept *PUT* requests will treat them like *PATCH* requests, and not require us to send the whole object back.

We send a payload of data with *PATCH* requests, the same way we do with *POST* requests:

```

```

```
payload = {"description": "The best repository ever!", "name": "test"}
```

```
response = requests.patch("https://api.github.com/repos/VikParuchuri/test", json=payload)
```

The code above will change the description of the `test` repository to `The best repository ever!` (we didn't specify a description when we created it).

A successful *PATCH* request will usually return a `200` status code.

### Instructions

- Make a *PATCH* request to the `https://api.github.com/repos/VikParuchuri/learning-about-apis` endpoint that changes the description to `Learning about requests!`.Assign the status code of the response to `status`.

```
payload = {"description": "The best repository ever!", "name": "test"}
response = requests.patch("https://api.github.com/repos/VikParuchuri/test", json=payload, headers=headers)
print(response.status_code)
payload = {"description": "Learning about requests!", "name": "learning-about-apis"}
response = requests.patch("https://api.github.com/repos/VikParuchuri/learning-about-apis", json=payload, headers=headers)
status = response.status_code
```

## 9: DELETE Requests

The final major request type is the *DELETE* request. The *DELETE* request removes objects from the server. We can use the *DELETE* request to remove repositories.

```

```

```
response = requests.delete("https://api.github.com/repos/VikParuchuri/test")
```

The above code will delete the `test`repository from GitHub.

A successful *DELETE* request will usually return a `204` status code indicating that it successfully deleted the object.

Use *DELETE* requests carefully - it's very easy to remove something important by accident.

### Instructions

- Make a *DELETE* request to the `https://api.github.com/repos/VikParuchuri/learning-about-apis` endpoint.Assign the `status_code` of the response to the variable `status`.

```
response = requests.delete("https://api.github.com/repos/VikParuchuri/test", headers=headers)
print(response.status_code)
response = requests.delete("https://api.github.com/repos/VikParuchuri/learning-about-apis", headers=headers)
status = response.status_code
```

## 10: Further Exploration

That's it for the major points of working with APIs, but feel free to continue exploring with [your own API token](https://github.com/settings/tokens). Then, consult the [API documentation](https://developer.github.com/v3/) to find good endpoints to query.

# Challenge: Working with the reddit API

## 1: Reddit

Over the past few missions, we learned how to make API requests, authenticate with an API server, and parse responses. In this challenge, you'll pull these concepts together to explore trending posts and comments on [reddit](https://www.reddit.com/).

Reddit is a community-driven link-sharing site. Users submit links to articles, photos, and other content. Other users upvote the submissions they like, and downvote the ones they dislike. Users can comment on submissions, and even upvote or downvote other people's comments.

Reddit consists of many smaller communities called subreddits where more focused communities can discuss niche posts. For example, [/r/python](https://www.reddit.com/r/python) is a Python-focused community, and [/r/sanfrancisco](https://www.reddit.com/r/sanfrancisco) is for discussing issues relating to the city of [San Francisco, CA](https://en.wikipedia.org/wiki/San_Francisco). The posts you submit to a subreddit will appear on the group's front page if enough users upvote them. Very popular subreddit posts may appear on reddit's home page.

Posts only stay on the main reddit and subreddit pages for a limited time. You can search for older posts, but it can be hard to find what you're looking for.

In this challenge, you'll practice:

- Retrieving a list of trending posts on a particular subreddit
- Exploring the comments on a single article
- Posting our own comment on an article

## 2: Authenticating With The API

The reddit API requires authentication. You authenticated with a token in a previous mission, but in this challenge, you'll use [OAuth](https://en.wikipedia.org/wiki/OAuth). OAuth can be fairly complex, but we've done some of the hard work already. You'll be using an authentication token, **13426216-4U1ckno9J5AiK72VRbpEeBaMSKk**, to authenticate in much the same way we did earlier, except that the header will look like this:

```

```

```
{"Authorization": "bearer 13426216-4U1ckno9J5AiK72VRbpEeBaMSKk"}
```

Note that we'll need to use the string `bearer` instead of the string `token` we used in the previous mission. That's because we're using OAuth this time. While we won't discuss OAuth right now, you can read about it on [Wikipedia](https://en.wikipedia.org/wiki/OAuth) and through [this blog post](https://blog.varonis.com/introduction-to-oauth/).

We'll also need to add a `User-Agent`header, which will identify us as `Dataquest` to the API:

```

```

```
{"Authorization": "bearer 13426216-4U1ckno9J5AiK72VRbpEeBaMSKk", "User-Agent": "Dataquest/1.0"}
```

We've imported `requests` for you already, so please avoid doing it again in this mission. Importing `requests` will overwrite some of the custom API logic we've developed for answer checking.

### Instructions

- Retrieve the [/r/python](https://www.reddit.com/r/python) subreddit's top posts for the past day.Make a GET request to `https://oauth.reddit.com/r/python/top` using the [get](http://docs.python-requests.org/en/master/user/quickstart/#passing-parameters-in-urls) method of the [requests](http://docs.python-requests.org/en/master/) library. See the [documentation for the `/r/python/top` endpoint](https://www.reddit.com/dev/api#GET_{sort}) if you need help.Pass in the header information we showed you earlier in this section.To retrieve only the top posts for the past day, pass in a query parameter `t` (for "time") and set its value to the string `day`.
- Use the [json](http://docs.python-requests.org/en/master/user/quickstart/#json-response-content) method on the response to get the JSON response data.
- Assign the JSON response data to the variable `python_top`.

```

headers = {"Authorization": "bearer 13426216-4U1ckno9J5AiK72VRbpEeBaMSKk", "User-Agent": "Dataquest/1.0"}
params = {"t": "day"}
response = requests.get("https://oauth.reddit.com/r/python/top", headers=headers, params=params)

python_top = response.json()
```

## 3: Getting The Most Upvoted Post

The variable `python_top` is a dictionary containing information about all of the individual posts that users submitted during the past day. However, the actual list of posts is buried inside a dictionary key, and you'll need to explore the dictionary to retrieve it. You can read more about `python_top`'s format [here](https://www.reddit.com/dev/api#listings).

There's a dictionary for each individual post that looks like this:

```

```

```
{'data': {'approved_by': None,
```

```
     'archived': False,
```

```
     'author': 'ingvij',
```

```
     ...
```

```
     'ups': 43,
```

```
     'url': 'http://hkupty.github.io/2016/Functional-Programming-Concepts-Idioms-and-Philosophy/',
```

```
     'user_reports': [],
```

```
     'visited': False},
```

```
     'kind': 't3'}
```

We've truncated the representation, but you can see the `ups` field, which contains the number of people who upvoted the post. The `id` field holds reddit's unique ID for the post.

### Instructions

- Explore the `python_top` dictionary.
- Extract the list containing all of the posts, and assign it to `python_top_articles`.
- Find the post with the most upvotes.
- Assign the ID for the post with the most upvotes to `most_upvoted`.

```

python_top_articles = python_top["data"]["children"]
most_upvoted = ""
most_upvotes = 0
for article in python_top_articles:
    ar = article["data"]
    if ar["ups"] >= most_upvotes:
        most_upvoted = ar["id"]
        most_upvotes = ar["ups"]
```

## 4: Getting Post Comments

Now that you have the ID for the most upvoted post, you can retrieve the comments on it using the [/r/{subreddit}/comments/{article}](https://www.reddit.com/dev/api#GET_comments_{article}) endpoint. You'll need to replace the items that have brackets around them with the appropriate values: *{subreddit} - The name of the subreddit the post appears in (omit the leading /r, because it already exists). Use python for the pythonsubreddit, for example. *`{article}` - The ID for the post whose comments we want to retrieve. It should look like this: `4b7w9u`.

You'll need to include the API's base URL, `https://oauth.reddit.com/`, before this endpoint to generate the full URL for the request.

### Instructions

- Get all of the comments on the `/r/python` subreddit's top post from the past day.Generate the full URL to query, using the subreddit name and post ID.Make a GET request to the URL.Get the response data using the response's [json](http://docs.python-requests.org/en/master/user/quickstart/#json-response-content) method.Assign the response data to the variable `comments`.

```

headers = {"Authorization": "bearer 13426216-4U1ckno9J5AiK72VRbpEeBaMSKk", "User-Agent": "Dataquest/1.0"}
response = requests.get("https://oauth.reddit.com/r/python/comments/4b7w9u", headers=headers)

comments = response.json()
```

## 5: Getting The Most Upvoted Comment

Querying the comments endpoint at [/r/{subreddit}/comments/{article}](https://www.reddit.com/dev/api#GET_comments_{article}) returns a list. The first item in the list contains information about the post, and the second item contains information about the comments.

Reddit users can nest comments. That is, they can comment on comments. Here's an [example](https://www.reddit.com/r/programming/comments/4b7uht/markov_chains_explained_visually/).

This means that comments have one more key than posts do. The additional key, `replies`, contains the nested comments. You can read more about that structure in [reddit's API documentation](https://www.reddit.com/dev/api#listings). Here's an example of a single comment that has nested comments:

```

```

```
{'data': {'approved_by': None,
```

```
      'archived': False,
```

```
      'author': 'larsga',
```

```
      ...
```

```
      'replies': {'data': {'after': None,
```

```
        'before': None,
```

```
        'children': [{'data': {'approved_by': None,
```

```
           'archived': False,
```

```
           'author': 'Deto',
```

```
           ...
```

```
           },
```

```
          ...
```

```
          ]
```

```
          }
```

```
          ...
```

```
          'url': 'https://www.reddit.com/r/Python/comments/4b6bew/using_pilpillow_with_mozjpeg/',
```

```
         'user_reports': [],
```

```
         'visited': False
```

```
         }
```

It's easier to focus on top-level comments only, and ignore the nested `replies`.

### Instructions

- Find the most upvoted top-level comment in `comments`.Extract the comments list from the `comments`variable, and assign it to `comments_list`.Assign the ID for the comment with the most upvotes to `most_upvoted_comment`.

```

comments_list = comments[1]["data"]["children"]
most_upvoted_comment = ""
most_upvotes_comment = 0
for comment in comments_list:
    co = comment["data"]
    if co["ups"] >= most_upvotes_comment:
        most_upvoted_comment = co["id"]
        most_upvotes_comment = co["ups"]
```

## 6: Upvoting A Comment

You can upvote a comment with the [/api/vote](https://www.reddit.com/dev/api#POST_api_vote) endpoint. You'll need to pass in the following parameters:

- `dir` - Vote direction: `1`, `0`, or `-1`. `1` is an upvote, and `-1` is a downvote.
- `id` - The ID for the post or comment to upvote.

### Instructions

- Make a POST request to the [/api/vote](https://www.reddit.com/dev/api#POST_api_vote) endpoint to upvote the most upvoted comment from the last screen.
- Assign the status code for the response to the variable `status`.

```

payload = {"dir": 1, "id": "d16y4ry"}
headers = {"Authorization": "bearer 13426216-4U1ckno9J5AiK72VRbpEeBaMSKk", "User-Agent": "Dataquest/1.0"}
response = requests.post("https://oauth.reddit.com/api/vote", json=payload, headers=headers)
status = response.status_code
```

## 7: Next Steps

In this challenge, you authenticated with an API, then retrieved and parsed responses. In the next mission, we'll dive into Web scraping.

# Web Scraping

## 1: Introduction

A lot of data aren't accessible through data sets or APIs. They may exist on the Internet as Web pages, though. One way to access the data without waiting for the provider to create an API is to use a technique called **Web scraping**.

Web scraping allows us to load a Web page into Python and extract the information we want. We can then work with the data using standard analysis tools like `pandas` and `numpy`.

Before we can do Web scraping, we need to understand the structure of the Web page we're working with, then find a way to extract parts of that structure in a sensible way.

We'll use the `requests` library heavily as we learn about Web scraping. This library enables us to download a Web page. We'll also use the `beautifulsoup` library to extract the relevant parts of the Web page.

## 2: Web Page Structure

Web pages use **HyperText Markup Language** (HTML). HTML isn't a programming language like Python. It's a **markup language** with its own syntax and rules. When a Web browser like Chrome or Firefox downloads a Web page, it reads the HTML to determine how to render it and display it to you.

Here's the HTML for a very simple Web page:

![Imgur](https://dq-content.s3.amazonaws.com/6ne0anS.png)

You can see what this page looks like [on our GitHub site](http://dataquestio.github.io/web-scraping-pages/simple.html).

HTML consists of **tags**. We open a tag like this:

![Imgur](https://dq-content.s3.amazonaws.com/tw5h1SV.png)

We close a tag like this:

![Imgur](https://dq-content.s3.amazonaws.com/HP2OQvC.png)

Anything in between the opening and closing of a tag is the content of that tag. We can nest tags to create complex formatting rules. Here's an example:

![Imgur](https://dq-content.s3.amazonaws.com/XGIR8GS.png)

The `b` tag bolds the content inside it, and the `p` tag creates a new paragraph. The HTML above will display as a bold paragraph because the `b` tag is inside the `p` tag. In other words, the `b` tag is nested within the `p` tag.

HTML documents contain a few major sections. The `head` section contains information that's useful to the Web browser that's rendering the page; the user doesn't see it. The `body` section contains the bulk of the content the user interacts with on the page.

Different tags have different purposes. For example, the `title` tag tells the Web browser what page title to display at the top of your tab. The `p` tag indicates that the content inside it is a single paragraph.

We won't cover tags comprehensively here, but please [read the Mozilla Developer Network's (MDN) article on HTML basics](https://developer.mozilla.org/en-US/Learn/Getting_started_with_the_web/HTML_basics) if you need more of a grounding on this topic. Check out [MDN's guide to the HTML element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element) for a list of all possible HTML tags. In order to do Web scraping effectively, you'll need a solid understanding of the various tags and how they work.

In this exercise, you'll make a *GET* request to `http://dataquestio.github.io/web-scraping-pages/simple.html`.

### Instructions

- Make a *GET* request to `http://dataquestio.github.io/web-scraping-pages/simple.html`, and assign the result to the variable `response`.
- Use `response.content` to get the content of the response, and assign it to `content`.Note how the content is the same as the HTML above.

```
# Write your code here.
response = requests.get("http://dataquestio.github.io/web-scraping-pages/simple.html")
content = response.content
```

## 3: Retrieving Elements From A Page

Downloading the page is the easy part. Let's say that we want to get the text in the first paragraph. Now we need to parse the page and extract the information we want.

We'll use the [BeautifulSoup](http://www.crummy.com/software/BeautifulSoup/) library to parse the Web page with Python. This library allows us to extract tags from an HTML document.

We can think of HTML documents as "trees," and the nested tags as "branches" (similar to a family tree). BeautifulSoup works the same way.

If we look at this page, for example, the root of the "tree" is the `html` tag:

![Imgur](https://dq-content.s3.amazonaws.com/C7qmC17.png)

The `html` tag contains two "branches," `head` and `body`. `head` contains one "branch," `title`. `body` contains one branch, `p`. Drilling down through these multiple branches is one way to parse a Web page.

To extract the text inside the `p` tag, we would first need to get the `body` element, then the `p` element, and then finally the text inside the `p` element.

### Instructions

- Get the text inside the `title` tag, and assign the result to `title_text`.

```
from bs4 import BeautifulSoup

# Initialize the parser, and pass in the content we grabbed earlier.
parser = BeautifulSoup(content, 'html.parser')

# Get the body tag from the document.
# Since we passed in the top level of the document to the parser, we need to pick a branch off of the root.
# With BeautifulSoup, we can access branches by using tag types as attributes.
body = parser.body

# Get the p tag from the body.
p = body.p

# Print the text inside the p tag.
# Text is a property that gets the inside text of a tag.
print(p.text)
head = parser.head
title = head.title
title_text = title.text
```

## 4: Using Find All

While it's nice to use the tag type as a property, it's not always a very robust way to parse a document. It's usually better to be more explicit by using the `find_all`method. This method will find all occurrences of a tag in the current element, and return a list.

If we only want the first occurrence of an item, we'll need to index the list to get it. Aside from this difference, it behaves the same way as passing in the tag type as an attribute.

### Instructions

- Apply the `find_all` method to get the text inside the `title` tag, and assign the result to `title_text`.

```
parser = BeautifulSoup(content, 'html.parser')

# Get a list of all occurrences of the body tag in the element.
body = parser.find_all("body")

# Get the paragraph tag.
p = body[0].find_all("p")

# Get the text.
print(p[0].text)
head = parser.find_all("head")
title = head[0].find_all("title")
title_text = title[0].text
```

## 5: Element IDs

HTML allows elements to have **IDs**. Because they are unique, we can use an ID to refer to a specific element.

Here's an example page:

![Imgur](https://dq-content.s3.amazonaws.com/WBG4aCQ.png)

You can see the page [here](http://dataquestio.github.io/web-scraping-pages/simple_ids.html).

HTML uses the `div` tag to create a divider that splits the page into logical units. We can think of a divider as a "box" that contains content. For example, different dividers hold a Web page's footer, sidebar, and horizontal menu.

There are two paragraphs on the page; the first is nested inside a `div`. Luckily, the paragraphs have IDs. This means we can access them easily, even through they're nested.

Let's use the `find_all` method to access those paragraphs, and pass in the additional `id` attribute.

### Instructions

- Get the text of the second paragraph (what's inside the second `p` tag), and assign the result to `second_paragraph_text`.

```
# Get the page content and set up a new parser.
response = requests.get("http://dataquestio.github.io/web-scraping-pages/simple_ids.html")
content = response.content
parser = BeautifulSoup(content, 'html.parser')

# Pass in the ID attribute to only get the element with that specific ID.
first_paragraph = parser.find_all("p", id="first")[0]
print(first_paragraph.text)
second_paragraph = parser.find_all("p", id="second")[0]
second_paragraph_text = second_paragraph.text
```

## 6: Element Classes

In HTML, elements can also have **classes**. Classes aren't globally unique. In other words, many different elements belong to the same class, usually because they share a common purpose or characteristic.

For example, you may want to create three dividers to display three of your photographs. You can create a common look and feel for these dividers, such as a border and caption style.

This is where classes come into play. You could create a class called "gallery," define a style for it once using CSS (which we'll talk about soon), and then apply that class to all of the dividers you'll use to display photos. One element can even have multiple classes.

![Imgur](https://dq-content.s3.amazonaws.com/T2TguLL.png)

Take a look at [this page](http://dataquestio.github.io/web-scraping-pages/simple_classes.html) to see how we've used classes to style paragraphs.

We can use `find_all` to select elements by class. We'll just need to pass in the `class_` parameter.

### Instructions

- Get the text in the second inner paragraph, and assign the result to `second_inner_paragraph_text`.
- Get the text of the first outer paragraph, and assign the result to `first_outer_paragraph_text`.

```
# Get the website that contains classes.
response = requests.get("http://dataquestio.github.io/web-scraping-pages/simple_classes.html")
content = response.content
parser = BeautifulSoup(content, 'html.parser')

# Get the first inner paragraph.
# Find all the paragraph tags with the class inner-text.
# Then, take the first element in that list.
first_inner_paragraph = parser.find_all("p", class_="inner-text")[0]
print(first_inner_paragraph.text)
second_inner_paragraph = parser.find_all("p", class_="inner-text")[1]
second_inner_paragraph_text = second_inner_paragraph.text

first_outer_paragraph = parser.find_all("p", class_="outer-text")[0]
first_outer_paragraph_text = first_outer_paragraph.text
```

## 7: CSS Selectors

**Cascading Style Sheets**, or **CSS**, is a language for adding styles to HTML pages. You may have noticed that our simple HTML pages from the past few screens didn't have any styling; all of the paragraphs had black text and the same font size. Most Web pages use CSS to display a lot more than basic black text.

CSS uses **selectors** to add styles to the elements and classes of elements you specify. You can use selectors to add background colors, text colors, borders, padding, and many other style choices to the elements on HTML pages.

An in-depth lesson on CSS is outside the scope of this mission. If you'd like to learn more about it on your own, [MDN's guide](https://developer.mozilla.org/en-US/docs/Web/Guide/CSS/Getting_started) is a great place to start.

What we do need to know is how *CSS selectors* work.

This CSS will make all of the text inside all paragraphs red:

```

```

```
p{
```

```
    color: red
```

```
 }
```

You can see what this style looks like [on our GitHub site](http://dataquestio.github.io/web-scraping-pages/simple_red.html).

This CSS will change the text color to red for any paragraphs that have the class `inner-text`. We select classes with the period or dot symbol (`.`):

```

```

```
p.inner-text{
```

```
    color: red
```

```
 }
```

You can see what the result looks like [here](http://dataquestio.github.io/web-scraping-pages/simple_inner_red.html).

This CSS will change the text color to red for any paragraphs that have the ID `first`. We select IDs with the pound or hash symbol (`#`):

```

```

```
p#first{
```

```
    color: red
```

```
 }
```

Take a look at the results [on our GitHub site](http://dataquestio.github.io/web-scraping-pages/simple_ids_red.html).

You can also style IDs and classes without using any specific tags. For example, this CSS will make the element with the ID `first` red (not just paragraphs):

```

```

```
#first{
```

```
    color: red
```

```
 }
```

This CSS will make any element with the class `inner-text` red:

```

```

```
.inner-text{
```

```
    color: red
```

```
 }
```

In the examples above, we used *CSS selectors* to select one or more elements, then apply styles to only those elements. *CSS selectors* are very powerful and flexible.

Perhaps not surprisingly, we also use *CSS selectors* to select elements when we do Web scraping.

## 8: Using CSS Selectors

We can use BeautifulSoup's `.select`method to work with *CSS selectors*. Here's the HTML we'll be working with on this screen:

![Imgur](https://dq-content.s3.amazonaws.com/uOaCMeY.png)

You may have noticed that the same element can have both an ID and a class. We can also assign multiple classes to a single element; we just separate the classes with a space.

Take a look at the [Web page](http://dataquestio.github.io/web-scraping-pages/ids_and_classes.html) that corresponds to the HTML above.

### Instructions

- Select all of the elements that have the class `outer-text`.
  - Assign the text of the first paragraph that has the class `outer-text` to `first_outer_text`.
- Select all of the elements that have the ID `second`.
  - Assign the text of the first paragraph that has the ID `second` to the variable `second_text`.

```
# Get the website that contains classes and IDs.
response = requests.get("http://dataquestio.github.io/web-scraping-pages/ids_and_classes.html")
content = response.content
parser = BeautifulSoup(content, 'html.parser')

# Select all of the elements that have the first-item class.
first_items = parser.select(".first-item")

# Print the text of the first paragraph (the first element with the first-item class).
print(first_items[0].text)
first_outer_text = parser.select(".outer-text")[0].text

second_text = parser.select("#second")[0].text
```

## 9: Nesting CSS Selectors

We can nest CSS selectors similar to the way HTML nests tags. For example, we could use selectors to find all of the paragraphs inside the `body` tag. Nesting is a very powerful technique that enables us to use CSS to do complex Web scraping tasks.

This selector will target any paragraph inside a div tag:

```

```

```
div p
```

This selector will target any item inside a `div` tag that has the class `first-item`:

```

```

```
div .first-item
```

This one is even more specific. It selects any item that's inside a `div` tag inside a `body` tag, but only if it also has the ID `first`:

```

```

```
body div #first
```

This selector zeroes in on any items with the ID `first` that are inside any items with the class `first-item`:

```

.first-item #first
```

As you can see, we can nest CSS selectors in infinite ways. This allows us to extract data from websites with complex layouts. You can test selectors by using the `.select` method as you write them. Because it's easy to write a selector that doesn't work the way you expect, we highly recommend doing this.

## 10: Using Nested CSS Selectors

Now that we know about nested CSS selectors, let's try them out. We can use them with the same `.select` method we used for our CSS selectors.

We'll be practicing on this HTML:

![Imgur](https://dq-content.s3.amazonaws.com/H34hK8I.png)

It's an excerpt from a box score of the [2014 Super Bowl](https://en.wikipedia.org/wiki/Super_Bowl_XLVIII), a [National Football League](https://en.wikipedia.org/wiki/National_Football_League) (NFL) game in which the New England Patriots played the Seattle Seahawks. The box score contains information on how many yards each team gained, how many turnovers each team had, and so on. Check out the [Web page](http://dataquestio.github.io/web-scraping-pages/2014_super_bowl.html) this HTML renders.

The page renders as a table with column and row names. The first column is for the Seattle Seahawks, and the second column is for the New England Patriots. Each row represents a different statistic.

### Instructions

- Find the Total Plays for the New England Patriots, and assign the result to `patriots_total_plays_count`.
- Find the Total Yards for the Seahawks, and assign the result to `seahawks_total_yards_count`.

```
# Get the Superbowl box score data.
response = requests.get("http://dataquestio.github.io/web-scraping-pages/2014_super_bowl.html")
content = response.content
parser = BeautifulSoup(content, 'html.parser')

# Find the number of turnovers the Seahawks committed.
turnovers = parser.select("#turnovers")[0]
seahawks_turnovers = turnovers.select("td")[1]
seahawks_turnovers_count = seahawks_turnovers.text
print(seahawks_turnovers_count)
patriots_total_plays_count = parser.select("#total-plays")[0].select("td")[2].text

seahawks_total_yards_count = parser.select("#total-yards")[0].select("td")[1].text
```

## 11: Beyond The Basics

We've covered the basics of HTML and how to select elements, which are key foundational blocks.

You may be wondering why Web scraping is useful, given that in most of our examples, we could easily have found the answer by looking at the page. The real power of Web scraping lies in getting information from a large amount of pages very quickly.

Let's say we wanted to find the total number of yards each NFL team gained in every single NFL game over an entire season. We could do this manually, but it would take days of boring drudgery. We could write a script to automate this in a couple of hours instead, and have a lot more fun doing it.

