Title: Sending iMessages and SMS through Messages.app with AppleScript
Date: 2015-02-20
Tags: OSX applescript workflow

I was searching around for ways to automate sending iMessages, so I could write a plugin for [Hammerspoon](http://www.hammerspoon.org/). I found various scripts lurking around the place for sending iMessages, but I also found one that can send SMS if you have SMS Relay enabled (which means you need OS X 10.10 and an iPhone running iOS 8.1).
I figured I'd collect them as a single post, to help future searchers, so without further ado, here are two stripped down AppleScript snippets that let you control Messages.app to send either an iMessage, or an SMS.
Firstly, sending an iMessage:

```applescript
tell application "Messages"</span>
  send "This is an iMessage" to buddy "foo@bar.com" of (service 1 whose service type is iMessage)</span>
end tell</span>
```

The buddy address can be either an email or a phone number that's registered with Apple for use with iMessage.
Secondly, sending an SMS:

```applescript
tell application "Messages"</span>
  send "This is an SMS" to buddy "+1234567890" of service "SMS"</span>
end tell</span>
```

Here, the buddy address should be a phone number.
Simple!
(and for the Hammerspoon users, you'll find hs.messages available in the next release, 0.9.23)
