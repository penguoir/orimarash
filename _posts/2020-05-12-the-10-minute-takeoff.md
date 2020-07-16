---
layout: post
title: Creating a competition using Firebase realtime PubSub
date: 2020-05-12T17:38:38.036Z
learned:
  - Database structure + real-time databases
  - Using Google Maps API
  - React
---
As part of a school fundraiser, our class had to find some way to raise money for a charity. One of the students in our class (Miles Hinders-Green) had the idea to run a contest to see who could get the furthest away from school in 10 minutes. [Felix Ronneberger](https://github.com/conition) and I liked this idea so we decided to turn this into a proper project.

There were a few problems the race needed to be solved for it to run properly. First, there had to be a way to track the players so the winner could be fairly and objectively decided. Secondly, there had to be a way to view the contestants. Viewing would create another income opportunity for the charity, as we could charge people to watch the locations of the players. We thought that these two problems could be solved with an app and a website. So we set out to create both in time for the race.

## The database

To track where each player is, there had to be a shared location for the app to send the location and for the website to read the location. We decided to use Firebase’s Firestore because it was cheap, could update in real-time, and would be fairly easy to integrate into an app and a website.

Since this would be a fairly small event, and the data stored isn’t sensitive, we weren’t too worried about security. We implemented some simple rules to stop random people from accessing the database, but I’m sure that with enough effort that could be cracked easily. In an industry application, we would have implemented more security rules, with authentication and verification.

Since Firestore is no-SQL, structuring the database took some time. In the end, we decided to save the data like so:

![Database schema of "coords", which holds 'id'; 'longitutde' and 'latitude'; distance; user id; and timestamp](/assets/10mtakeoff/database.png)

A collection called \`coords\`, inside of which are loads of documents, each with a timestamp, user, aerial distance (as the crow flies) from the starting location, and coordinates. The app would—every time the location changed—send the new information. The website—every time the information changed—would present the new information.

## The website

To create the website, I decided to use React. It’s fast and (more importantly) has a range of premade components I could use. This meant that I didn’t have to code everything from scratch, saving development time and hassle, as well as preventing obvious bugs.

One of the challenges I faced was to get the map to work properly, after doing some research I decided to use \`google-map-react\` to shorten my development time on the website. This React component gave me all the tools I needed to present the information from the database.

Since we are simply saving lots of timestamps of each player, there had to be some way of getting all the most recent information displayed. Here is the code I used to format that information from the database:

```javascript
function format(values) {
  var obj = values.reduce((total, x, i) => {
    if (total[x.id]) {
      if (total[x.id].timestamp < x.timestamp) {
        // If the id was already recorded and the timestamp is more recent, replace with newer value
        
        // Check that distance exists in new one, if it doesn't, add it from previous
        if (!x.distance) {
          x.distance = total[x.id].distance
        }
        
        total[x.id] = x
      }
    } else {
      console.log('id doesnae exist yet')
      // If the id doesn't exist, create it. 
      total[x.id] = x
    }

    console.log(total)
    return total
  }, {})
  
  return Object.values(obj)
}
```

Another problem I had is deciding which filter to put on the information obtained from the app. For the map, I needed the most updated information for the most number of players. For the leaderboard, I needed the highest distances from school. Using Firestore’s filters, I could get this information:

```javascript
firebase.firestore().collection('coords').orderBy('timestamp', 'desc').limit(10000)
firebase.firestore().collection('coords').orderBy('distance', 'desc').limit(10000)
```

I limited the number of documents collected to make sure I wouldn’t blow my budget.

I uploaded the website quite easily using [Now.sh](https://github.com/zeit/now), a platform designed to quickly get websites up and running. I already had an account so this step was rather simple.

[The repository containing all the code for the website is on Github.](https://github.com/penguoir/10mtakeoff) We wanted the ability to collaborate with others and possibly extend the website further so Github was the obvious way to go.

## Summary

![Taking off](/assets/10mtakeoff/taking-off.jpg)

_Taking off_

I think both Felix and I learned a lot through working on this project. Especially the importance of advertising, which we neglected until the very end of the project. This meant that we only had 3 participants, and managed to raise just under £20 through contributions of passers-by... Hooray?
