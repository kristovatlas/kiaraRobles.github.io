---
layout: post
comments: true
title: Contributing to The App Store for Developers
---
# Contributing to The App Store for Developers

> When asked to describe CocoaPods in one sentence [Orta Therox](https://twitter.com/orta) calls it, 
"The App Store for Developers". 

Only CocoaPods are even better, since they're free. But we all know the open source community is not just about a price tag of free code. It's about contributing to project you want to see exist, evolve, and grow. Channeling organizing open source participation through CocoaPods ultimately provides the users with a better experience by allowing developers to focus on the implementations they care about most. When done successfully a CocoaPod can become a crucial building block for thousands of apps. (*cough cough* AFNetworking!)

#### Let's make one!

There are several other blog posts on the topic of creating CocoaPods that I'd recommend. 
- [NSHipster CocoaPods](http://nshipster.com/cocoapods/)
- [Creating Your First CocoaPod](http://code.tutsplus.com/tutorials/creating-your-first-cocoapod--cms-24332)
- [Making a CocoaPod](https://guides.cocoapods.org/making/making-a-cocoapod.html)


#### Before you start, the general game plan is as follows:
1. Create the pod skeleton directory
2. Create a git repo
3. Update the metadata of the pod
4. Add the code for your pod.
5. Push it to the "CocoaPod Store"


#### 1. Create the pod skeleton directory
    // ♥ pod lib create KFRFuzzyDateTranslator
    Cloning `https://github.com/CocoaPods/pod-template.git` into `KFRFuzzyDateTranslator`.
    Configuring KFRFuzzyDateTranslator template.

Answer some questions:

    What language do you want to use?? [ ObjC / Swift ]
    Would you like to include a demo application with your library? [ Yes / No ]
    Which testing frameworks will you use? [ Specta / Kiwi / None ]
    Would you like to do view based testing? [ Yes / No ]
    What is your class prefix?

    Running pod install on your new library.
    Ace! you're ready to go!
    We will start you off by opening your project in Xcode
    open 'KFRFuzzyDateTranslator/Example/KFRFuzzyDateTranslator.xcworkspace'

#### 2. Create a github repo
    
Create a github repository then push the skeleton directory to its new home online
    
    // ♥ git add .
    // ♥ git commit -m “Initial Commit"
    // ♥ git remote add origin https://github.com/<GITHUB_USERNAME>/KFRFuzzyDateTranslator.git
    // ♥ git push -u origin master


#### 3. Update the metadata of the pod

My computer has no idea what a .podspec is, so you have to tell it what text editor to use in command line. Alterativy you can open your new project in xcode to edit the .podspec file there, or run the command below to open it in sublime text.

    // ♥ subl -n KFRFuzzyDateTranslator.podspec
    
My .podfile looks like this:

    #
    # Be sure to run `pod lib lint KFRFuzzyDateTranslator.podspec' to ensure this is
    # a valid spec before submitting.
    #
    # Any lines starting with a # are optional, but their use is encouraged
    # To learn more about Podspec see http://guides.cocoapods.org/syntax/podspec.html
    #
    
    Pod::Spec.new do |s|
      s.name             = "KFRFuzzyDateTranslator"
      s.version          = "0.1.0"
      s.summary          = "Converts NSStrings to NSDate objects."

    # This description is used to generate tags and improve search results.
    #   * Think: What does it do? Why did you write it? What is the focus?
    #   * Try to keep it short, snappy and to the point.
    #   * Write the description between the DESC delimiters below.
    #   * Finally, don't worry about the indent, CocoaPods strips it!  
      s.description      = <<-DESC 
      This pod was developed to convert a NSString into a NSDate object so you 
      dont have to use that date spinner anymore! Yay. The string "next Monday" 
      returns the NSDate object of the next Monday, relative to today. The class 
      interprets fuzzy human readable words to an exact date at midnight. 
      Currently, the days of the week are supported with the prefix "next", "this",
      or "last", as well as the words "yesterday", "tomorrow", and "today".
                       DESC

      s.homepage         = "https://github.com/kiaraRobles/KFRFuzzyDateTranslator"
      # s.screenshots     = "http://imgur.com/2bl8bRK.png", 
                            "http://imgur.com/4S8B91p.png", 
                            "http://imgur.com/TgSAweE.png"
      s.license          = 'MIT'
      s.author           = { "kiaraRobles" => "kiara.robles@gmail.com" }
      s.source           = { :git =>     
      "https://github.com/kiaraRobles/KFRFuzzyDateTranslator.git", :tag => s.version.to_s }
      # s.social_media_url = 'https://twitter.com/anarchoass'

      s.platform     = :ios, '7.0'
      s.requires_arc = true

      s.source_files = 'Pod/Classes/**/*'
      s.resource_bundles = {
        'KFRFuzzyDateTranslator' => ['Pod/Assets/*.png']
      }

      # s.public_header_files = 'Pod/Classes/**/*.h'
    end
    
Run this line:

    // ♥ pod lib lint KFRFuzzyDateTranslator.podspec
    pod lib lint KFRFuzzyDateTranslator.podspec
    -> KFRFuzzyDateTranslator (0.1.0)
    KFRFuzzyDateTranslator passed validation.
    
Yay! Tag the version

    // ♥ git tag 0.1.0
    // ♥ git push origin 0.1.0


#### 4. Add the code for your pod.

Open up your project directory, remove the file here:

![](http://imgur.com/irqPVeo.png)

And replace it with your classes here:

![](http://imgur.com/3sNTjGJ.png)

You may also want to add an example project and test files at this point.


#### 5. Push it to the "CocoaPod Store"

Validate your pod with this line:

    // ♥ pod spec lint KFRFuzzyDateTranslator.podspec
    -> KFRFuzzyDateTranslator (0.1.0)
    Analyzed 1 podspec.
    KFRFuzzyDateTranslator.podspec passed validation.trunk register 
    name@example.org 'Your Name' --description='macbook pro'

Register your session with this line:

    // ♥ pod trunk register name@example.org 'Your Name' --description='macbook pro'

Verify the session sent to your email address.. then:

    // ♥ pod trunk push KFRFuzzyDateTranslator.podspec
    Updating spec repo `master`
    Validating podspec
    -> KFRFuzzyDateTranslator (0.1.0)

    Updating spec repo `master`
    - Log messages:
       - November 11th, 17:48: Push for `KFRFuzzyDateTranslator 0.1.0' initiated.
       - November 11th, 17:48: Push for `KFRFuzzyDateTranslator 0.1.0' has been 
       pushed (1.20875384 s).
       

Done. Contribution to open source achieved!
