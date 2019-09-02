# Where to begin
The best place to start is to familiarize yourself with the platform architecture and how it all fits together. Start with the [walkthrough](https://github.com/smartcitiesdata/smartcitiesdata/wiki/Architecture) in this wiki and spend as much time as you feel is necessary to get comfortable with what's going on.

When in doubt, [ask questions](https://gitter.im/smartcitiesdata/community).

# Follow the issues
If you see an opportunity to contribute a new feature or discover an unreported bug, dive in. If you already know what component you want to add or extend, follow your muse. However, if you're looking for inspiration, check out the project's Kanban board on [Zenhub](app.zenhub.com/workspaces/smartcolumbusos-5d08f55f08ccbe75911ce796). Once you've signed in with your Github ID you'll see an aggregated backlog of all the issues that have been opened against any of the project's public repositories. The core team opens issues for all new features and bugs identified with any component of the system, so it's all there for you to peruse.

## Stake your claim
With the possible exception of the latest entries in the "New Items" column, all issues are prioritized from top to bottom within a column. Not only can you tell from the state of the board what the team's currently working on, but what our priorities are. Anything that isn't actively in play is up for grabs. If you see an issue to which you'd like to contribute, stake your claim by reaching out to the team and commenting on the issue itself.

## New additions and feature suggestions
Speaking of which, if you _do_ discover bugs or have a great idea for a new feature you'd like to contribute to the OS that hasn't already been identified, open an issue on the appropriate repository explaining it and express your interest in submitting the PR to implement the necessary change(s). If you're not sure what repo should house the issue, ask the team or just stick it here and we'll help figure out its permanent home.

It's up to you to argue for your idea and make the case it's valuable to be added to the platform for the benefit of everyone.

# Testing
Before any pull requests can be accepted into the project all tests for the repo being modified must be in a passing state. To avoid going down a rabbit hole with an incompatible environment, setup, or unexpected bug, it's a good idea to run all the tests when you first clone the repo or fetch the latest changes to ensure it's starting in a green state.

As you modify the code, ensure that you're adding tests accordingly or updating existing tests based on your changes. There's not hard and fast rule for code coverage, but the reviewers will ask for additional test coverage if they feel the new code isn't covered adequately. 

There's also no hard and fast rule for whether or not a function or component should be validated with unit tests or integration tests. Use your best judgement and try to cover all of the success and failure scenarios you can imagine. Keep the [testing pyramid](https://martinfowler.com/articles/practical-test-pyramid.html) in mind and apply your efforts accordingly. Mocks and stubs should be applied strategically and sparingly; for validating small functions they're ideal but with containerization technology making it simple and fast to stand up almost any service dependency it's better to just write an integration test to capture a more "production-like" scenario whenever possible.

# Review
When you're ready to submit your change for review, commit and open a pull request against the origin repository. At least one member of the core team must review and approve the request although don't be surprise if more give feedback.

If changes are required to bring the pull request into the master branch, the reviewer(s) will leave appropriate comments on the pull request explaining and issues and making suggestions for how to mitigate them. These will typically be explicitly technical in nature unless a particular stylistic choice is counter to the norms of the community, or is too confusing or ambiguous. Please be polite and respectful during the discussion process but don't be afraid to make your case to the team if you feel strongly about anything.