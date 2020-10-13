---
layout: post
title: Creating an authentication system using Firebae and React
date: 2020-09-22T22:04:02.036Z
learned:
  - React hooks
  - Authentication
  - Interacting with a complicated state
---

[Firebase] is great because it lets you plug in an authentication system super
easily. Basically, all you have to do is write something like:

[firebase]: https://firebase.google.com/

```javascript
firebase.auth().signIn()
```

and bam! Your user is signed in. [It's a bit more complicated than
that][firebase login] but we don't have to worry about that now. What we do have
to worry about is that **we can't store any more information about the user!!**
Firebase only lets you sign a user in or out, and lets you store some
information based on how the user signed in (e.g. their email, their github
account, etc.). We can't store any extra data on them (perhaps their tasks, in a
to-do website)

[firebase login]: https://firebase.google.com/docs/auth

So what can we do about this. Well, we can use [Firestore], which is a database
provided as part of Firebase to store extra data on the user. There are a few
problems that arise when trying to "link" between Firebase auth and Firestore,
and I'll walk you through them.

[firestore]: https://firebase.google.com/docs/firestore

For this example, we'll be making a website that lets you upload a link to your
website. All you can do is sign in, and paste you website link. It's saved in
Firestore. That's all. Let's see how we can save a website link to the user.

Linking between Auth and Firestore
----------------------------------

We need some kind of "foreign key" (this is a no-sql database, so those don't
really exist) to link between the Authentication system and Firestore. Ok, easy,
we can just use the user id (`userId`). This is unique in Firebase
Authentication so we can safely use it to link between an account and a document
in Firestore.

![Diagram showing Firebase and Firestore
relationship](https://i.imgur.com/OLuJRlm.png)

Let's go on to some more interesting things....

Using this data in an app
-------------------------

Right, so the main problem is figuring out how to, in that order:

  1. Sign in the user
  2. Bring the data

Without being super cumbersome.

Something like

```javascript
firebase.auth().signIn().then(user => {
  firebase.collection('users').doc(user.userid).get()
    .then(data => {
      // Now we have the user and the user's data
    })
})
```

is very annoying to do in each page that needs the data. Also, we want to be
DRY!

### The solution

...sigh...

After more than 50 hours of working on this, I can promise you that this is the
best way. I could explain every option and why it doesn't work (buy me a coffee
and I will), but, for this article, here is the solution I came up with.

Firstly, we need to manage the state of the following attributes:

  * Loading - used for presenting a "loading" icon
  * Getting the login details from Firebase
  * Getting the additional data about the user
  * Has any errors

(almost) all of these features are independent from eachother. So, it makes
sense to track each one's state individually. However, instead of making four
seperate `useState` calls, we can use [react-tracked].

[react-tracked]: https://github.com/dai-shi/react-tracked

react-tracked uses some special magic to only re-render when one _part_ of an
object changes. So, we can save all of these attributes in a single object. This
object is then passed down using React Context to lower down components.

Thus, each component can use the login state as needed, only re-rendering when
required.

Conclusion
----------

I think that working on this aspect of a project shows that the things that seem
simple aren't always that easy to implement properly.

If you're wondering how to implement this practically, I'm working on building a
package which does all this automatically. But, for now, the best I can say is
good luck.

