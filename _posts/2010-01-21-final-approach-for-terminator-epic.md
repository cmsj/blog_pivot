Title: Final approach for Terminator epic-refactor
Date: 2010-01-21
Tags: FOSS Techie Ubuntu Python Terminator

I'm done hacking on the Terminator epic-refactor branch for the evening and the following has been achieved today (in chronological order):

-   Fix a bug in handling URLs dropped on the window
-   Implement directional navigation
-   Implement geometry hinting
-   Fix a bug in group emitting that cause "Broadcast off" and "Broadcast to all" to become inverted
-   Implement WM\_URGENT bell handler

I'm *really* happy with how this is going. All that is left to have feature parity with trunk, I think, is some keyboard shortcut handlers.
I'd still love to get more testing results to make sure I haven't missed anything, but at this rate I'm expecting to be able to land the epic-refactor branch on trunk this weekend, after five and a half months.
Then I'm going to write a tool to convert old config files and we can think about putting out a 0.90 beta release. Exciting stuff!
