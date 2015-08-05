---
layout:     post
title:      "Using Extensions to add functionality to NSManagedObject classes"
subtitle:   "When I started programming in Objective-C I didn't know all the power this language had. It's common for me to learn by doing..."
date:       2015-08-05 01:00:00
author:     "jaisonv"
comments:   true
---

When I started programming in Objective-C I didn't know all the power this language had. It's common for me to learn by doing, and when I have doubts I google about it until I finish what I am doing. After giving my first steps I like reading a book to know everything the language can offer. When I read the book about Objective-C I found out the existence of [Protocols](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/WorkingwithProtocols/WorkingwithProtocols.html) and [Categories](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/CustomizingExistingClasses/CustomizingExistingClasses.html) and liked it a lot. I didn't know of anything like this in Java.

I will not talk about Objective-C here, there might be tons of contents about this language on the web. And since this year's WWDC I motivated myself to learn Swift.

In Swift we have the [Extensions](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Extensions.html), which is the equivalent to Categories in Objective-C.

From Swift's documentation:

> "Extensions can add new functionality to a type, but they cannot override existing functionality."

Working with Coredata, Xcode does all the work creating the NSManagedObject classes based on entities. But sometimes you need to add methods to these classes, and it's annoying to add these methods again every time you modify the entities and use Xcode to recreate the NSManagedObject classes. That's a good place where we can take advantage of Extensions. Let's see how it works.

Suppose we have an entity called "Person" having the properties "name" and "age". Xcode will the following NSManagedObject file named Person.swift:

{% highlight erlang %}
import Foundation
import CoreData

class Parent: NSManagedObject {
    @NSManaged var name: String
    @NSManaged var age: NSNumber
}
{% endhighlight %}

This way we create a Swift class to implement the Extension (I will name it PersonExtensions.swift):

{% highlight erlang %}
extension Person {

    func sayMyName {
        print(name)
    }
}
{% endhighlight %}

Now we can create objects and use its methods.

{% highlight erlang %}
let heisenberg = NSEntityDescription.insertNewObjectForEntityForName("Person", inManagedObjectContext: appDelegate.managedObjectContext) as! Person
heisenberg.name = "Heisenberg"
heisenberg.age = 51

heisenberg.sayMyName()
{% endhighlight %}

This only one of the many ways we can use Extensions. It is up to your creativity to think about how to use it in your project.
