# Rails Debugging Activity
In this activity you will practice Rails debugging techniques using a real-world Ruby on Rails project.

## Learning Goals
By working through this activity you will gain exposure to the following software development skills:
* Setting up a production-ready Rails application for local development
* Navigating a large Rails codebase
* Reading user reports to identify potentially broken functionality
* Investigating potentially broken functionality in your local development environment
* Creating a set of steps to reproduce a user-reported bug
* Using `pry` and `better_errors` to pin-point the source of a bug
* Determining which mitigation or correction options are most viable for a given bug

## RailsBridge and Bridge Troll
[RailsBridge](http://railsbridge.org/) is an organization that runs free coding workshops all over the world, specifically targeted at groups of people that are underrepresented in tech. Members of RailsBridge and the open source community have built [Bridge Troll](https://www.bridgetroll.org/) to help with the planning and organization of these workshops.

Before you setup the Bridge Troll application for local development, please browse through this repository to get a feel for the project. There will probably be many files and Rails features and techniques that you are not familiar with yet, and that's totally fine. Understanding everything that the Bridge Troll project does is **not** the goal for this activity.

## Project Setup
First, fork and clone our copy of the Bridge Troll repository (this repo) -- you **must** use our copy, as the real repository does not contain the bugs you will be fixing in this activity!

After you have cloned the repo you should follow the setup instructions from [the project's README file](./README-bridge_troll.md). Specifically, follow the **Quickstart**, **Running tests**, and **Seed Data** sections. The Bridge Troll developers have created a setup script that you can run to automatically get everything set up, however there are additional steps you'll need to complete before running the script.

As suggested by the Bridge Troll README, you should play around with the application and get a feel for the different parts of the app. This will be important later for understanding the user reports for each bug. Especially check out the "Seed Data" section for instructions on how to log into the app with seeded user accounts.

## Debugging in Rails
Please refer to [this textbook resource on debugging](https://github.com/Ada-Developers-Academy/textbook-curriculum/blob/master/00-programming-fundamentals/debugging-user-reports.md) for more details on the process for moving from a user-provided report of incorrect behavior to a complete set of steps to use for reproducing the exact same behavior (known as "repro steps"), which is very helpful for debugging.

After fixing all of the bugs in this activity you should be able to run the Bridge Troll project's tests (see the README for details on how to do that) with no failures.

## Example
#### 1. User Report
The beginning of this whole process is to receive a bug report from a user. Sometimes the bug reports are detailed and have clear descriptions of what is going on. Most times however, there's a fair bit of interpretation and guess work that must be applied to fully understand the report.

Read through the following user report a couple of times before proceeding to the next step:
> When I go to an event's detail page, click the link that says "Click here for more information about class levels in this course!", and visit the page about Class Levels, it has a header with a typo that says "Class Levles for Ruby on Rails" instead of "Class Levels for Ruby on Rails"

#### 2. Repro Steps
Once given a user report like this we need to first translate it into a set of repro steps.

This report doesn't talk about a situation where the user needs to be logged in or not logged in, so right now we don't have any information that to reproduce it we need to log in. Because this report is talking about the specific Class Levels page that was reached through an event's detail page, we need at least one existing event that has that link. On that page we then need to navigate and find the page in question and observe the typo.

1. Open the website in Chrome.
1. Under the "Upcoming events" section locate an event [probably the "Seeded Test Event"].
1. Under that event's details click the "Click here for more information about class levels in this course!" link (or find a different event if that link is not present).
1. Observe that the header "Class Levles ..." has a typo.

#### 3. Bug Finding
Now that we have a set of repro steps that we can follow to trigger the bug, we can start using those steps to pin-point what code is involved.

Because this bug involves the UI primarily (something is being displayed to the user in an undesirable way), we should start with our view code. It's possible that the underlying issue is actually elsewhere in the code base, but the view code is a good place to start.

We need to identify which view template is responsible for the Operating System Breakdown table. One way of figuring this out would be to look at the URL (e.g. "http://localhost:3000/events/1/levels") and match that up with a particular route, then use that to find the associated controller action and eventually view template.

However, a simpler option would be to search through the whole project for the text "Class Levles for Ruby on Rails" or "Class Levles", because that's the literal text for the header on this page. You can search through your whole project in Atom by using Cmd + Shift + F.

Note: Did "Class Levles for Ruby on Rails" produce any search results?

Either option should eventually have you end up at the file `app/views/events/levels.html.erb`.

From here we can find that line 4 is the part where we output the header.

#### 4. Bug Fixing
We've located the code that is most likely involved with our bug:
```erb
<h2>Class Levles for <%= (@event.course || Course.find_by_name("RAILS")).title %></h2>
```

Now time to implement the fix!

```erb
<h2>Class Levels for <%= (@event.course || Course.find_by_name("RAILS")).title %></h2>
```

Revisit an earlier question: Did "Class Levles for Ruby on Rails" produce any search results? Why not?

With that change in place, we can reload the page and check if our fix worked. Since this is a UI issue we don't have any automated tests that we can run to verify the fix, but with all of the other bugs in the activity you should see that at least one of the failing tests is working again if the bug has been fixed properly.

## Activity
Now that we've gotten the project set up, and worked through fixing an example bug based on a user report, we can start to work on the meat of the activity. Below you will find three different user reports of incorrect behavior that someone experienced when using the app. These reports are short and do not include much in the way of specific details.

### Strategy / Tips
The first part of your job will be to investigate the Bridge Troll application and its functionality to narrow down what part of the app the user was interacting with, and eventually create a set of repro steps for each bug. For example, if the bug mentions saving an event as a draft... you'll want to figure out exactly what an "event" is in the Bridge Troll app, as well as what it means to save one as a draft. Run the app with `rails s` and start exploring!

Once you have a handle on the context of a particular user report, and have converted it into a set of reliable repro steps, you can start debugging! Use all of the various debugging tools we've seen before (e.g. `puts` statements, pry, and the better_errors page) to narrow down exactly what lines of code are involved. Then begin considering how those lines might be changed in order to correct the bug.

It can also help to run the test suite, since there are tests which are failing now as a result of bug that I've introduced. If you're unsure of what changes you might need to make to the code to fix a bug, the failing tests might provide additional clues as to how the developers were intending for the code to work.

### User Reports

#### 1. Operating System
> When I look at the Organizer Console for an event, the Operating System Breakdown table has difficult-to-read percentages listed for each OS.

#### 2. Saving event as draft
> When I save an event as a draft, I get a 500 error even though I have filled out all of the required fields.

#### 3. Removing event RSVP
> When I remove an attendee's RSVP for an event, I get an error and the RSVP is not removed.
