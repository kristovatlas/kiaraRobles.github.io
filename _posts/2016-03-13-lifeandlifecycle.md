---
layout: post
comments: true
title: Life and Life Cycle
tags: [cryptocurrency]
---
# Life and Life Cycle

##### Today is my birthday month. And I am giving myself the gift of releasing an app to the app store.

The Cat O'Clock is iOS alarm clock meows on time and wakes you up with a cute cat GIF. I like GIFs.For the most part, the app is fairly simple. The tableview reflects the alarm data and a second view is presented to display the GIF when an alarm goes off. The interesting part in setting the alarm as a long running task in the background. 

#### Each alarm is made up of three fictional parts:
1. NSTimer
2. AVAudioPlayer
3. NSNotificationCenter

An alarm manager class intertwines these components. AVAudioPlayer plays a constant sound at with no volume in the background. Timer sets a count down to the alarm. When the comes, the timer triggers an NSNotification which stops the sound, and plays a new sound with volume. Adding alarm functionalities like a sleep functionalities or repeat intervals would take place by changing the logic of the local notification properties. The part that may seems odd about this process is the role of AVAudioPlayer. Why play a silent sound in the background?

#### Apple Allows for three ways to carry out a long running background method:
1. Audio - The application plays audible content to the user while in the background.
2. Location - The application keeps users informed of their location, even while running in the background.
3. VIOP - The application provides the ability for the user to make phone calls using an Internet connection.

All of these have to be stated in the Info.plist. For this app I claimed both audio and viop background functionality by adding this to plist:
		 <key>UIBackgroundModes</key>
		 <array>
					<string>audio</string>
					<string>voip</string>
		 </array>

**AVAudioPlayer plays a silent sound to keep the app running in the background, so that the alarm timer stays active.**

#### There are 6 methods that the app delegate calls that trigger/signal the next step in the app life cycle.
The three that are significant to this app are in bold below:

		 application:didFinishLaunchingWithOptions:
		 applicationDidBecomeActive:
		 applicationWillResignActive:
		 applicationDidEnterBackround:
		 applicationWillEnterForegound:
		 applicationWillTerminate:

Because applicationDidEnterBackround: cancels all the active local notifications and timers in the app there are functionally three times that the alarms get set. First, when the app is launched with application:didFinishLaunchingWithOptions. Secondly, when the alarm data is modified. And lastly, all local notifications and timers are canceled when applicationDidEnterBackround: is called. So in this method I call the long running background method to set the final set of alarms. As of right now, I have not found a way to keep the alarms running in the background without removing all notification and setting new ones. The default of applicationDidEnterBackround: is to cancel all current tasks so any method called in here executes after all other live tasks are canceled. 

For more details on on the fictionality of the application check out the code at GitHub repository [here](https://github.com/kiaraRobles/Cat-O-Clock/tree/master/Cat%20O'Clock)
https://github.com/kiaraRobles/Cat-O-Clock). 
