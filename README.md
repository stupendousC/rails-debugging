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
Please refer to [this textbook resource on debugging](https://github.com/Ada-Developers-Academy/textbook-curriculum/blob/master/00-programming-fundamentals/debugging-user-reports.md) for more details on the process for moving from a user-provided report of incorrect behavior to a complete set of repro steps to use for debugging.

After fixing all of the bugs in this activity you should be able to run the Bridge Troll project's tests (see the README for details on how to do that) with no failures.

## Example
#### 1. User Report
The beginning of this whole process is to receive a bug report from a user. Sometimes the bug reports are detailed and have clear descriptions of what is going on. Most times however, there's a fair bit of interpretation and guess work that must be applied to fully understand the report.

Read through the following user report a couple of times before proceeding to the next step:
> When I look at the Organizer Console for an event, the Operating System Breakdown table has difficult-to-read percentages listed for each OS.

#### 2. Repro Steps
Once given a user report like this we need to first translate it into a set of repro steps.

Because this report is talking about the "Organizer Console" we know that we must first log in as an organizer to access that page. Once logged in we need to find a particular event and visit the organizer page for it. On that page we then need to locate the "Operating System Breakdown" section.

1. Open the website in Chrome.
1. Click the "Sign In" button.
1. Enter "organizer@example.com" for the email and "password" for the password.
1. Under the "Upcoming events" section locate an event [probably the "Seeded Test Event"].
1. Under that event's details click the "Organizer Console" button (or find a different event if that button is not present).
1. Scroll to the bottom of the page.
1. Observe that within the "Operating System Breakdown" table each row has a count and a percentage of total student attendees with that operating system.
1. Observe that for each row where the percentage is not exact it is instead listed with a very large number of digits, e.g. "10.204081632653061%". [Note: You may not see this because the seeded event is random.]

#### 3. Bug Finding
Now that we have a set of repro steps that we can follow to trigger the bug, we can start using those steps to pin-point what code is involved.

Because this bug involves the UI primarily (something is being displayed to the user in an undesirable way), we should start with our view code. It's possible that the underlying issue is actually elsewhere in the code base, but the view code is a good place to start.

We need to identify which view template is responsible for the Operating System Breakdown table. One way of figuring this out would be to look at the URL (e.g. "http://localhost:3000/events/1/organizer_tools") and match that up with a particular route, then use that to find the associated controller action and eventually view template.

However, a simpler option would be to search through the whole project for the text "Operating System Breakdown", because that's the literal text for the header on this table. You can search through your whole project in Atom by using Cmd + Shift + F.

Either option should eventually have you end up at the file `app/views/events/organizer_tools/_operating_system_breakdown.erb`. This partial view template is responsible for just the OS table section of the organizer tools page.

From here we can find that line 20 is the part where we output the contents of the "Count" column in the table, including the percentage.

#### 4. Bug Fixing
We've located the code that is most likely involved with our bug:
```erb
<%= count %> (<%= ((count.to_f / total) * 100) %>%)
```

Now we need to figure out how to modify it to fix the issue. There are several ways of going about this, depending on exactly what results we want. In this example I'm going to specify that we want to only output 2 decimal points for the percentage. The simplest way to solve that is by using [Ruby's format string functionality](https://ruby-doc.org/core-2.2.0/String.html#25-method):
```erb
<%= count %> (<%= '%.2f' % ((count.to_f / total) * 100) %>%)
```

With that change in place, we can reload the page and check if our fix worked. Since this is a UI issue we don't have any automated tests that we can run to verify the fix, but with all of the other bugs in the activity you should see that at least one of the failing tests is working again if the bug has been fixed properly.

## Bugs
### Saving event as draft
> When I save an event as a draft, I get a 500 error.

### Removing event RSVP
> When I remove an attendee's RSVP for an event, I get an error and the RSVP is not removed.

### Re-joining event after cancelling RSVP
> When I cancel my RSVP for a full event, I can only re-join the waitlist even if there are now available spots.
