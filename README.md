# Rails Debugging Activity
In this activity you will practice Rails debugging techniques using a real-world Ruby on Rails project, [Bridge Troll](https://www.bridgetroll.org/). Bridge Troll is the event planning web application used for all [RailsBridge](http://railsbridge.org/) (and related *Bridge) events.

For this activity you will first clone a special copy of the Bridge Troll repository and follow the setup instructions from its README to create an environment for local development of the application. This will be similar to how you have setup Rails projects for Ada, however it may be more involved because this application is "production-ready", rather than instructional.

After setting up the local development environment you will work through the debugging process for an example bug, including the implementation of a fix for that bug. Once you have corrected the example bug you should follow the same process to tackle the three other bugs that have been inserted into this codebase. All of the bugs presented in this activity can be corrected with a specific, small change to the code.

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

Before you setup the Bridge Troll application for local development, please browse through [the repository](https://github.com/railsbridge/bridge_troll) to get a feel for the project. There will probably be many features and Rails features and techniques that you are not familiar with yet, and that's totally fine. Understanding everything that the Bridge Troll project does is _not_ the goal for this activity.

### Setup
First, clone our copy of the Bridge Troll repository -- you **must** use our copy, as the real repository does not contain the bugs you will be fixing in this activity!

After you have cloned the repo you should follow the setup instructions from the project's README file. The Bridge Troll developers have created a setup script that you can run to automatically get everything set up, however there are a couple of steps you'll need to complete before running the script.

## Debugging in Rails
Debugging any application usually takes one of two forms: either you encounter the bug yourself while developing the application, or a user encounters the bug and files a report. You've already seen the first form of debugging quite a lot here at Ada because it's a natural, necessary part of building software.

The second form of debugging is something that you will likely begin to experience during your internship, and it has some unique aspects to it that inform the approach you'll need to take when debugging. Specifically, it is necessary to translate from the user's report into a set of "reproduction steps" (also called a "repro") which can be followed to trigger the bug.

You need to do this because users of our application generally do not have any understanding of how it actually works. They only see the user interface and so their explanation of what went wrong is usually framed in terms of how things are presented in the UI.

For example, they might make a purchase from your store and see that it does not show up in their purchases list. They might then report this to you as a bug with the purchase list. But upon investigation you might find that the issue was actually due to the payment gateway system returning an error code that your application did not expect, resulting in the purchase being dropped from the database.

Once you have a set of repro steps for a specific bug you can follow them in your local development environment and see the full error message and details (which are generally hidden from users in production deployments). At this point you can follow the same debugging process as when you encounter an error yourself.

### Example


### Bugs
#### Saving event as draft
#### Removing event RSVP
#### Re-joining event after cancelling RSVP
