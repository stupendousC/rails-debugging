# Rails Debugging Activity
In this activity you will practice Rails debugging techniques using a real-world Ruby on Rails project, [Bridge Troll](https://www.bridgetroll.org/). Bridge Troll is the event planning web application used for all [RailsBridge](http://railsbridge.org/) (and related *Bridge) events.

For this activity you will work with a special copy of the Bridge Troll repository which has been modified to include a few bugs. After setting up the local development environment you will work through the debugging process for an example bug, including the implementation of a fix for that bug. Once you have corrected the example bug you should follow the same process to tackle the three other bugs. All of the bugs presented in this activity can be corrected with a specific, small change to the code.

## Learning Goals
By working through this activity you will gain exposure to the following software development skills:
* Setting up a production-ready Rails application for local development
* Navigating a large Rails codebase
* Reading user reports to identify potentially broken functionality
* Investigating potentially broken functionality in your local development environment
* Creating a set of steps to reproduce a user-reported bug
* Using `pry` and `better_errors` to pin-point the source of a bug
* Determining which mitigation or correction options are most viable for a given bug

## RailsBridge's Bridge Troll web application
RailsBridge is an organization that runs free coding workshops all over the world, specifically targeted at groups of people that are underrepresented in tech. Members of RailsBridge and the open source community have built Bridge Troll to help with the planning and organization of these workshops.

Before you setup the Bridge Troll application for local development, please browse through this repository to get a feel for the project. There will probably be many files and Rails features and techniques that you are not familiar with yet, and that's totally fine. Understanding everything that the Bridge Troll project does is **not** the goal for this activity.

### Setup
First, fork and clone our copy of the Bridge Troll repository (this repo) -- you **must** use our copy, as the real repository does not contain the bugs you will be fixing in this activity!

After you have cloned the repo you should follow the setup instructions from [the project's README file](./README-bridge_troll.md). Specifically, follow the **Quickstart**, **Running tests**, and **Seed Data** sections. The Bridge Troll developers have created a setup script that you can run to automatically get everything set up, however there are additional steps you'll need to complete before running the script.

As suggested by the Bridge Troll README, you should play around with the application and get a feel for the different parts of the app. This will be important later for understanding the user reports for each bug. Especially check out the "Seed Data" section for instructions on how to log into the app with seeded user accounts.

## Debugging in Rails
Please refer to [this textbook resource on debugging](https://github.com/Ada-Developers-Academy/textbook-curriculum/blob/master/00-programming-fundamentals/debugging-user-reports.md) for more details on the process for moving from a user-provided report of incorrect behavior to a complete set of repro steps to use for debugging.

After fixing all of the bugs in this activity you should be able to run the Bridge Troll project's tests (see the README for details on how to do that) with no failures.

### Example


### Bugs
#### Saving event as draft
#### Removing event RSVP
#### Re-joining event after cancelling RSVP
