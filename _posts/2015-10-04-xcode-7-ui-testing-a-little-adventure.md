---
layout: post
title: "Xcode 7 UI Testing: A Little Adventure"
date: "2015-10-04 01:06:38 -0700"
categories:
  - Software Development
  - iPhone/iPad/iOS
tags: []
comments: []
excerpt_separator: <!--more-->
redirect_from: "/wp/2015/10/04/xcode-7-ui-testing-a-little-adventure/"
---

One of the big features announced for developers at this year's WWDC event was [automated UI testing](https://developer.apple.com/videos/play/wwdc2015-406/). This is something I think all developers should be doing in some capacity, whether with Apple's tool or something else.

In iOS land, we've been able to do this for a while with the [Automation instrument](https://developer.apple.com/library/watchos/documentation/DeveloperTools/Conceptual/InstrumentsUserGuide/UIAutomation.html). But the tests were in JavaScript (not Objective-C or Swift), and had to be executed separately from normal unit tests.

I was excited to see that Xcode 7 would include a test recorder that could generate test code in the same language as my app. I just got done kicking the tires (better late than never), and the results were... mixed. I want to record my results here while they're fresh in my mind in case anybody else runs into the same issues I did (including my future self).

<!--more-->

### The App

<aside>
  <figure>
     <img src="/assets/images/2015/10/ExercisesForProgrammersCalc1-300x260.png" alt="Screen shot of the Calculator app" />

     <figcaption>I won't be winning any Apple Design Awards with this one.</figcaption>

  </figure>
</aside>

I made a simple 4-function calculator app for one of the exercises in Brian Hogan's [Exercises for Programmers: 57 Challenges to Develop Your Coding Skills](http://amzn.to/1Ro8Il8). Basically it takes two numbers as inputs, and spits out their sum, difference, product, and quotient.

### The Test Cases

It's a pretty straightforward app, so it seemed like a great place to get my feet wet with some UI tests:

- Input `10` and `5` (just like in the book) and verify the outputs. Very straightforward.
- Input `5` and `10`; verify outputs. There are a couple interesting cases here: `5 - 10` is a negative number, and `5 / 10` is a decimal.
- Input `foo` and `bar`; verify the app refuses to operate on such garbage. In a real app, I might display an informative error message, but for this simple app I just want the outputs to go blank.

### Recording the First Test

I hit the record button, filled in the inputs with `10` and `5`, and clicked on each of the output numbers (just to see how the generated code would refer to them). Here's the resulting code:

```swift
let app = XCUIApplication()
app.otherElements
   .containingType(.StaticText, identifier:"Calculator")
   .childrenMatchingType(.TextField)
   .elementBoundByIndex(0)
   .typeText("10")
app.otherElements
   .containingType(.StaticText, identifier:"Calculator")
   .childrenMatchingType(.TextField)
   .elementBoundByIndex(1)
   .typeText("5")
app.staticTexts["15"].tap()
app.staticTexts["5"].tap()
app.staticTexts["50"].tap()
app.staticTexts["2"].tap()
```

Yikes. A couple things jump out here:

- The inputs are referenced by index, meaning this test code will break if another field is ever added.
- The outputs are referenced by the values they contain. That's not horrible in this instance, but certain inputs might lead to the same value in multiple outputs. For example, `1 * 1` and `1 / 1` both result in a `1` output. We really should reference the fields by some sort of unique identifier, then verify the results they contain.

<aside>
  <img src="/assets/images/2015/10/AccessibilityOptions.png" alt="Accessibility - Which option to choose?" />
</aside>

It turns out the solution to both problems is to use accessibility. But there's a big gotcha here:

Which accessibility option is the right one? I went with Label, and had some initial success, but it ended up causing problems later. It turns out Identifier is the correct option. And if you do choose Label, you'll end up having to [repair it later](https://forums.developer.apple.com/thread/10428#48820) (like I did).

Now that all those identifiers are filled in, let's try recording that test again. Here's the resulting code:

```swift
let app = XCUIApplication()
app.textFields["input1"].typeText("10")
app.textFields["input2"].typeText("5")
app.staticTexts["sumOutput"].tap()
app.staticTexts["differenceOutput"].tap()
app.staticTexts["productOutput"].tap()
app.staticTexts["quotientOutput"].tap()
```

Much better! Now let's replace those `.tap()` calls with some verification:

```swift
let app = XCUIApplication()
app.textFields["input1"].typeText("10")
app.textFields["input2"].typeText("5")
XCTAssertEqual("15", app.staticTexts["sumOutput"].label)
XCTAssertEqual("5", app.staticTexts["differenceOutput"].label)
XCTAssertEqual("50", app.staticTexts["productOutput"].label)
XCTAssertEqual("2", app.staticTexts["quotientOutput"].label)
```

And let's run this thing!

<figure><img src="/assets/images/2015/10/KeyboardFocusError.png" alt="UI Testing Failure - Neither element nor any descendant has keyboard focus." />
<figcaption>UI Testing Failure - Neither element nor any descendant has keyboard focus.</figcaption>
</figure>

Ugh. That field had focus when I recorded the test because I tapped on it first. But the test code doesn't explicitly carry out that tap. So let's add it manually in the code:

```swift
let app = XCUIApplication()
app.textFields["input1"].tap()
app.textFields["input1"].typeText("10")
app.textFields["input2"].tap()
app.textFields["input2"].typeText("5")
XCTAssertEqual("15", app.staticTexts["sumOutput"].label)
XCTAssertEqual("5", app.staticTexts["differenceOutput"].label)
XCTAssertEqual("50", app.staticTexts["productOutput"].label)
XCTAssertEqual("2", app.staticTexts["quotientOutput"].label)
```

And finally, we have a passing test! The other test cases can be created trivially by modifying this case.

### One More Thing...

Somewhere in this little adventure, I did a "Reset Content and Settings" in the simulator, and the test broke. That first attempt to access `input1` would fail. Eventually I ran the app normally (not in test mode) and filled in the fields manually. After that, I was able to run the test successfully again.

Now I'm unable to reproduce that problem, so hopefully it was just a fluke and others won't run into it. But if you do, try stepping through your test manually. It might help.
