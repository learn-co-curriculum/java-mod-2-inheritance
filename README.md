# Inheritance

## Learning Goals

- Explain inheritance
- Use inheritance in Java

## Introduction

A fundamental aspect of Object Oriented Programming is the ability for one class
to inherit from another class. Let's take our initial understanding of what a
class is as a grouping of "properties" and "actions", or said another way, let's
consider that a class can "be things" and "do things". With that in mind, the
extension of a class is all the things that its "base class" is and can do all
the things that its base class can do.

## Inheritance in Java

Let's look at a very simple example - the `Animal` class is meant to model what
an animal is and what an animal can do. In this simplified example, we will say
that an Animal has a number of legs (that number could be 0) and can breath:

```java
public class Animal {
    public int numLegs;

    public void takeBreath() {
        System.out.println("I took a breath");
    }
}
```

If I wanted to create a class to model a Cat and a Parrot, I could start from
scratch, or I could take advantage of the fact that cats and parrots are animals
and therefore have all the properties of my `Animal` class and can do all the
things that my `Animal` class can.

Not only that, I can then either add properties and behaviors to my `Cat` and
`Parrot` classes to indicate in which way they may _be_ different or _behave_
differently from my `Animal` class.

To extend a class, you define it like you would a normal class, but you follow
the class definition directly with the `extend` keyword and you specify the name
of the class you would like to inherit from. Let's look at our 2 new classes:

```java
public class Cat extends Animal {
    public boolean isDeclawed;

    public void useLitter() {
        System.out.println("I just used the litter - I'm a clean cat!");
    }
}
```

```java
public class Parrot extends Animal {
    public int numWings;
    private boolean inFlight = false;

    public void startFlying() {
        System.out.println("I'm flying - weeee!!!");
        inFlight = true;
    }

    public void stopFlying() {
        System.out.println("Coming down for a landing!");
        inFlight = false;
    }
}
```

We've introduced a few new terms, so let's examine them in the context of
examples:

1. A class can extend another class, using the keyword `extends` in the class
   definition
2. The class that extends is called the "subclass" or the "child class". In our
   exammple, both `Cat` and `Parrot` are subclasses
3. The class that is extended is called the "base class", the "superclass", or
   the "parent class". In our example, `Animal` is the base class.

The astute observers among you will have noticed that the `startFlying()` and
the `stopFlying()` methods also adjust the value of a boolean variable in
addition to displaying a message on the console. This is an example of "business
logic":

- As we designed the `Parrot` class, we decided that it would be useful to track
  whether the parrot is flying. For example, we may decide that the parrot
  should tell us it's _not flying_ if we ask it to stop flying while it's not
  flying. Our parrot is getting smart!
- We have introduced the concept of a `private` variable for the `inFlight`
  boolean. We will discuss this in more detail in another section, but `private`
  variables allow us (the creators of the class) to track aspects of the class
  without letting others (consumers of the class) be able to interfere with
  those variables. With the `inFlight` variable, we want to be the only ones who
  can change that status, through the "business logic" in our class, such as
  when we "startFlying", for example.

Let's add functionality (i.e. business logic) to our `Parrot` class that takes
advantage of the `inFlight` private variable. We're going to make the parrot let
us know when we've asked it to fly or land at the wrong time:

```java
public class Parrot extends Animal {
    public int numWings;
    private boolean inFlight = false;

    public void startFlying() {
        if (inFlight) {
            System.out.println("I'm already flying!");
        } else {
            System.out.println("I'm flying - weeee!!!");
            inFlight = true;
        }
    }

    public void stopFlying() {
        if (inFlight) {
            System.out.println("Coming down for a landing!");
            inFlight = false;
        } else {
            System.out.println("I can't stop flying when I'm not flying!");
        }
    }
}
```
