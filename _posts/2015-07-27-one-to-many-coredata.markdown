---
layout:     post
title:      "One-to-many relationship using coredata"
subtitle:   "One of the biggest difficulties that I faced when learning how to use Coredata was dealing with one-to-many..."
date:       2015-07-27 01:00:00
author:     "jaisonv"
comments:   true
---

One of the biggest difficulties that I faced when learning how to use Coredata was dealing with one-to-many relationship. In every website or Stack Overflow's question they only explain how to build the one-to-many relationship, but I couldn't find about how to add a child object to its parent.

To start i created the the model in the .xcdatamodeld file.

Basically, I created 2 entities (Parent and Child) and a relationship in each one pointing the "Destination" to each other and the "Inverse" to the other's relationship.

The Parent entity with it's attributes and relationship.

![Coredata entities](/img/2015-07-27/img_1.png)

As a parent can have many children, this is how the relationship is named. In the relationship's inspector the type must be set as "To Many".

![Coredata entities](/img/2015-07-27/img_2.png)

The Child's structure is similar the Parent.

![Coredata entities](/img/2015-07-27/img_3.png)

Its relationship's inspector will have the type set as "To One".

![Coredata entities](/img/2015-07-27/img_4.png)

After modeling the entities I ask Xcode to generate de model classes.

![Coredata entities](/img/2015-07-27/img_5.png)

We have the Parent class:

{% highlight erlang %}
import Foundation
import CoreData

class Parent: NSManagedObject {
    @NSManaged var name: String
    @NSManaged var age: NSNumber
    @NSManaged var children: NSSet
}
{% endhighlight %}

And the Child class:

{% highlight erlang %}
import Foundation
import CoreData

class Child: NSManagedObject {
    @NSManaged var name: String
    @NSManaged var age: NSNumber
    @NSManaged var parent: Parent
}
{% endhighlight %}

The class Parent has a var "children" of type NSSet and the class Child has a var "parent" of type "Parent".

This is what it should look like.

{% highlight erlang %}
// create parent
let parent = NSEntityDescription.insertNewObjectForEntityForName("Parent", inManagedObjectContext: appDelegate.managedObjectContext) as! Parent
parent.name = "Bob"
parent.age = 40

// create child
let child = NSEntityDescription.insertNewObjectForEntityForName("Child", inManagedObjectContext: appDelegate.managedObjectContext) as! Child
child.name = "Junior"
child.age = 18

// add child to parent
child.parent = parent

// save data

var error: NSError?
appDelegate.managedObjectContext?.save(&error)
{% endhighlight %}

That's it... Just set the Child's "parent" variable to the Parent object and Coredata does all the work.

To access each Parent's child it's only needed to iterate over the Parent's children variable.

{% highlight erlang %}
for child in parent.children {
  println(child.name)
}
{% endhighlight %}
