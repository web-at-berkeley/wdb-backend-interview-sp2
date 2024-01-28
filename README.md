# WDB Backend Technical Project Sp '24 - Taylor Swift on Tour 🗣️

## Preface

Thank you for applying to Web Development at Berkeley and for taking the time to work on this technical project. Projects are due **January 29th, 2024 at 11:59 PM**.

This project is designed for **you** to gauge whether you want to apply to the **bootcamp** or **industry** branch. Completing all checkpoints for the branch you are applying to is highly preferred, but don't worry if you aren't able to finish everything! As an estimate, you shouldn't need to spend any longer than ~3-4 hours on this project -- we don't want this to be a huge burden on you.

## Clarifications

This section will be updated with clarifications as they come up!
## Submission Instructions

Welcome to WDB's backend project for development branch applicants — Spring 2024 👋

Make sure you read this ENTIRE DOCUMENT, especially these instructions, carefully before you start. If you have any questions please reach out to [webatberkeley@gmail.com](webatberkeley@gmail.com).

To submit your project, please place your submission into a GitHub repo that is set to private. You
will be submitting your code on [Gradescope](https://www.gradescope.com/). If you do not have a
Gradescope account, please create one and if you are unable to create one, please email us
immediately. The Gradescope course code is **4GEP2N**. You will see two different assignments:
`Frontend Project` and `Backend Project`. _Please only submit to Backend Technical Project._ You can ignore Frontend Technical Project.

The technical project will be due by **January 29th, 2024 at 11:59 PM**. We will be unable to respond to clarification emails sent in after then, so if you have any questions about the project, please let us know before then.

Also, this page may potentially keep changing if we get some frequently asked questions, so keep this repository bookmarked and check back on it every now and then! If there are major changes however, we'll make sure to email you about those.

## Introduction

Taylor Swift, an influential artist in popular culture and the music industry, has one of the most largest and dedicated fan bases. However, with the recent incident with Ticketmaster in 2023 selling her Eras Tour tickets, many fans were frustrated about Ticketmaster’s inefficient ticketing system. Taylor Swift wants to change that.  

The Taylor Swift management team is looking to bring their ticket-selling and concert-management system to the web and is seeking to build a backend service with the following features:

1. Register a fan after purchasing a ticket along the price of their ticket 
2. Get a list of all names who purchased tickets
3. Perform a "fan shout" and score it
4. Get the highest-score shout across all fans **(INDUSTRY ONLY)**
5. Buy a power-up item **(INDUSTRY ONLY)**

In particular, they want you to build an API capable of handling all of the functionality mentioned above.

_**NOTE: THERE ARE SEPARATE TASKS FOR BOOTCAMP VS. INDUSTRY**_. If you are applying to the bootcamp branch but you think you can complete the industry tasks, feel free to give them a shot! This will help us identify which branch might be the best fit for you as well.

# API Specification

The managment has listed a comprehensive set of rules/business logic for the event (_make sure you read through this carefully_).

## Register Fan/Ticket Price

Register a fan and associate them with the price of the ticket they paid (ticket prices can vary). Every fan also has a "vocalRange" field (which represents how many meters their voice will go). Lastly, the fan has a "location" which represents their distance (in meters) from the stage.

You can assume that all fans and tickets have a unique price. You can assume all names are UNIQUE and CASE INSENSITIVE, only including letters a-z.

Example request:

```
POST /fans
```

Request body:

```json
{
  "fanName": "Alice",
  "ticketPrice": 700,
  "vocalRange": 600,
  "location": 500
}
```

The response can look like anything. You should raise an error if a field is missing, with a descriptive error message.

## Get All Fans

Return a list of all fans and their vocal range.

**Industry Checkpoint**: Add additional functionality to this API route allowing you to include a query parameter `sortedByName=true` that determines if you should return by sorted order of fanName. (ex: GET /fans?sortedByName=true)

Example request:

```
GET /fans
```

Example response:

```json
{
  "pairs": [
    {
      "fanName": "Alice",
      "ticketPrice": "800"
    },
    {
      "fanName": "Cady",
      "ticketPrice": "1000"
    }
  ]
}
```

## Perform Fan Shout and Score it

Of course, no concert is complete without a fan yelling at Taylor Swift when she comes on stage! This route should perform the call for a specific fan.

We determine the score of a fan shout based on the fans' location and the fans' vocalRange. If the vocalRange is exactly equal to the location, the score is equal to 0. If the vocal range is greater, the score is the absolute difference between the location and the vocalRange. In both these cases, return the score.

If the vocalRange is less, we should raise an error with a descriptive message. If the fan name passed in hasn't been registered, that should raise an error too. The `bestShout` endpoint should only operate on fans who have performed a shout. If you implemented it a different way, you can keep it that way an note that in your assumptions doc though — it won't negatively affect you!

Example request:

```
GET /fanShout/Alice
```

Example response:

```json
{
  "score": 100
}
```

## Get Highest Score Shout (INDUSTRY ONLY)

Taylor wants to know who her loudest fan is based on our scoring system! This route should return the score of the best shout across all fans, as well as the fan. You can either assume no ties or break ties randomly, just make sure it is clear which option you go for.

Example request:

```
GET /bestShout
```

Example response:

```json
{
  "fanName": "Alice",
  "score": 100
}
```

## Buy a Power-up Item **(INDUSTRY ONLY)**

Some fans have said they want to be able to use the best and latest tech to improve their scores! Implement an API route that lets fans purchase an item and add it to their inventory. The response should be a list of all the items in the user's inventory.

- Each item has a "boost", which increases the vocalRange by that amount.
- Items cannot be removed from the inventory.
- All items in the inventory get used when a fanCall is made.

Note that completing this section will also involve modifying your existing fanCall route!

Example request (note the route has the fanName in it!):

```
POST /buyItem/Alice
```

Request body:

```json
{
  "item": "megaphone",
  "boost": 150
}
```

Example response:

```json
{
  "inventory": ["megaphone", "mysterious drugs"]
}
```

## Some closing thoughts

Some of these routes are naturally harder than others. If you aren't able to finish all of the routes for the project, **don't worry**! It's supposed to be challenging, and we don't expect everyone to finish it. We have created separate checkpoints for the bootcamp vs. industry, so you don't necessarily need to complete the full project in order to move on in this round :)

Note: we highly recommend using MongoDB as your database for this project, although if you don't have experience with it, any NoSQL database is also a great alternative. If none of those work however, you are still welcome to use other databases.

It is also encouraged to use JavaScript/TypeScript with Node.js for your backend, but it's completely fine if you would rather use a different stack.

Finally, we recommend using [Postman](https://www.postman.com/) or a similar tool to test your API.

## Design Doc

In addition to building out this API, you will need to write up a short design doc (designdoc.md). We don't intend for this to take very much time, but we want to hear some of the choices you made and why. To be specific, here are some points you might want to talk about:

- Why did you choose to organize your data schemas/models in a particular way?
- Feel free to talk a bit about the "harder" routes that you worked on and how you approached them — harder is completely subjective, so feel free to get creative here!
- How did you decide on certain response codes or errors?

This should be at most a page, so feel free to be brief!

## Assumptions

There are many details that are left intentionally vague. Though you are very much welcome to
email us to ask for clarifications, we will most likely tell you to use your best judgement.
Because of this, feel free to create a `assumptions.md`, where you can type out and
voice any assumptions you made throughout this project - we really want to hear about your rationale behind every design decision.
