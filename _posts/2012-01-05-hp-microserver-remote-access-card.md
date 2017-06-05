Title: HP Microserver Remote Access Card
Date: 2012-01-05
Tags: Techie

I've been using an HP ProLiant Microserver (N36L) as my fileserver at home, for about a year and it's been a really reliable little workhorse.
Today I gave it a bit of a spruce up with 8GB of RAM and the Remote Access Card option.
Since it came with virtually no documentation, and since I can't find any reference online to anyone else having had the same issue I had, I'm writing this post so Google can help future travellers.
When you are installing the card, check in the BIOS's PCI Express options that you have set it to automatically choose the right graphics card to use. I had hard coded it to use the onboard VGA controller.
The reason for this is that the RAC card is actually a graphics card, so the BIOS needs to be able to activate it as the primary card.
If you don't change this setting, what you will see is the RAC appear to work normally, but its vKVM remote video feature will only ever show you a green screen window, with the words "OUT OF RANGE" in yellow letters.
Annoyingly, I thought this was my 1920x1080 monitor confusing things, so it took me longer to fix this than it should have, but there we go.
