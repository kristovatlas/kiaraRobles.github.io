---
layout: post
title: From Code to AppStore in 10 Days
---
# From Code to AppStore in 10 Days

What does building an app really look like? We know that for iOS the tools are always the same: Xcode and people. As far a software life cycle goes, a large app in maintenance is going to look dramatically different from launching a fresh app from a new concept. When creating an entirely new app, my first thought is to set a timeline. For this one, my goal was to have it done as fast as possible.

A bitcoin paper wallet is a means of storing access to your digital currency offline. For some reason, the app store doesnt have an app for this. Let's see if I can make one! If the goal is speed, why do work someone else has already done? The first thing I did, was to check out open source projects on Github that might have done something similar.

### 1. From Open Source to Crafted Beauty

Seach Github.

At first glance it seems like this attempt to search for paper wallets on Github is sparse. But if we're willing to sift thru the "<> code" section (on the left), we might find some catalysts to our app building ambitions. I find that searching based on language and sorting based on "Resently indexed is the most fruitful.

![](http://i.imgur.com/wGRNxXM.png)

| [fluidjax/paper_wallet_generator]() | [markchangjz/iOS-Example](https://github.com/markchangjz/iOS-Example/tree/9f180dbb725626e75b3cbd3dafa39d46a21184c4/AirPrint) |
|--------|-------|
|     ![](http://i.imgur.com/dKyK7xF.png)   |    ![](http://i.imgur.com/VFvlnGa.gif)   |

The two core features to building a bitcoin paper wallet app are to generate QR codes and send them to a printer. I've found two open source projects that implement these goals separately, that I can use as my starting points for this project.

##### Read open source code like you're a tourist new city.

Walk around in someones elses code and try to understand its culture. If you undertake the entire project all at once you going to get overwhelmed. Every city is unique. Each has its own preferential sports team, its architectural style. Let yourself be comfortable with getting lost, run the code. Take pictures (run the debugger, throw breakpoints, pull methods into new buttons until you understand how the classes and moving parts are interacting.

##### Implement core feature: (big steps) 
- Set up general class structure
- Get the necessary components on screen

What is the main functionality of your app? Focus on that! Make a list of the core functionality and stick to it. The temptation to add features, buttons, and sliding bars is real. But it must be resisted, with the dedication of a saint. Every new feature takes time, even just thinking about new features take time. 

###### If you're thinking about new unnecessary features ask yourself: 
###### Is this something a competitor is doing? 
1. IF yes, can I do it better?
2. IF no, is it even valuable?
    
The evolution of building an app happens in both gradual changes and big steps. Get the huge steps out of the way first. And **commit to git for every step of the way**.

### 2.1 Make it beautiful: (gradual process)

- Add an icon
- Make a intro screen
- Beautify the buttons
- Try out different screen transitions
- Specify the logic for the navigation and status bars

In comparison to the core functionality of the app, the visuals can take some time. There are a lot of little pieces of code that needs to be added to get the desired effects. But the end result is something that hopefully looks finished and professional. 

######  Which app are you more likely to buy?
| Before | After |
|--------|-------|
|     ![](http://i.imgur.com/bB8L3PX.gif)   |    ![](http://i.imgur.com/QFfugDN.gif)   |

### 2.2 Testing, and debugging
- Rotate the app
- Test test the app in every simulated size
- Try to break it with every conceivable user input.
- For larger applications, write tests

In reality, testing and debugging is happening every step of the process. As you're building a new feature, you're constantly testing its parts to see how it preforms. Once you've built the application to the point where any changes you could think of would qualify as adding a major feature.. Stop.

### 3. Push it to the App Store

There are three main moving parts to this process: [Apple Developer](developer.apple.com), Xcode, and [Itunes Connect](itunesconnect.apple.com)

1. **Sign up for Apple Developer.** Once you've built an app you have to give credit where credit is due, and you have to give Apple their cut. A developer account is ~$100 and well worth the privilege of having your creation in the app store. Here you're going to have to create a signing certificate to sign your apps from Xcode.

2. **Reserve your App Name**. In iTunes Connect click: 
    - My apps
    - "+" > New app
    - Select: A name, primary language, bundle ID, and SKU. The bundle ID is a unique identifier the system uses for your app. It looks a little wonky, but the input needs to be in reverse DNS notation and it's recommended that you use your company name and application name.
    ![](http://imgur.com/Q8XY7yP.png)
    - The SKU is (stockkeeping unit): *The SKU is any alphanumeric sequence of letters and numbers you’d like to use to be uniquely identified in Apple’s system. You may create any string of UTF-8 letters and numbers, as long as it is unique to your developer account. This SKU is internal only and is not seen by users at any time. After you have submitted your metadata, this SKU is not editable.* I reused the bundle ID, but you can choose anything you want.

3. **Build an App Archive in Xcode.** Its recommended that you comment out all your NSLogs for performance reasons. Click: 
    - On the blue file (in the left Navigator pane)
    - Build settings
    - In code signing switch from *iOS Developer* to *iOS Distribution*
    ![](http://imgur.com/zw31lHo.png)
    - Scroll down to packaging, and switch the *Product Bundle Identifier* to the bundle ID you assigned in iTunes connection.
    - In the list of iOS simulators switch to iOS device. If it's not properly selected the option to archive your app will be greyed out.
    ![](http://imgur.com/W7IzzTj.png)
    - Product > Archive
    - Validate
    ![](http://imgur.com/lRijuwn.png)

4. **Submit in iTunes Connect.** The build you've done in Xcode should be recognized here. In iTunes connection, you can submit your app to Apple, or send it to the beta TestFlight for testing. Click: 
    - On the left hand panel select *Prepare for Submission*

    There are a few things you'll have to add here, but the two that are the most tedious are:

    - Apple requires that you submit screen shots for every device version  
    ![](http://imgur.com/lbbO3os.png)
    
    - And the build (app version) that will only be available to select if the Buildle ID, and the signing all went well.
    
Once the build it summited, you're done! And you're free to start working on the next version or your next app.

###  Sign-up [here](https://kiararobles.wufoo.com/forms/z1spzi0g0jz1ktx/) to beta test the app
