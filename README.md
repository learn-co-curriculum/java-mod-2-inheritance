# Inheritance

## Learning Goals

- Explain inheritance
- Use inheritance in Java

## What is Inheritance?

Another object-oriented programming pillar is **inheritance**, which is the
ability for one class to inherit from another class. We can think of inheritance
as a hierarchical classification where a class can inherit another class'
features (attributes and methods). In this lesson, we will explore how to use
inheritance in Java.

## Inheritance in Java

Let's look at a very simple example - the `Animal` class is meant to model what
an animal is and what an animal can do. In this simplified example, we will say
that an Animal has a name and can eat:

```java
package com.flatiron.animal;

public class Animal {
   private String name;

   Animal() {
      this.name = "unknown";
   }

   public String getName() { return name; }
   public void setName(String name) { this.name = name; }

   public void eat() {
      System.out.println("Yum! I like to eat!");
   }
}


```

If we wanted to create a class to model a Cat and a Parrot, we could start from
scratch, or take advantage of the fact that cats and parrots are animals too.
This means that they both have the same properties as the`Animal` class and can
do all the things an `Animal` object can too.

Not only that, we can then either add properties and behaviors to the `Cat` and
`Parrot` classes to indicate how they may _be_ different or _behave_
differently from the `Animal` class.

So how can we have the `Cat` and `Parrot` class inherit from the `Animal`
class? In Java, an inheritance relationship is like a parent-child relationship
in which some of the parent's characteristics may extend to their child's
characteristics. The keyword here is **extends** - just like a parent-child
relationship in the real world, a child class `extends` the parent class. Let's
look at our 2 new classes to see how this inheritance wil work with our example:

```java
package com.flatiron.animal;

public class Cat extends Animal {
   private boolean isDeclawed;

   public Cat() {
      super();
      this.isDeclawed = false;
   }

   public boolean getIsDeclawed() { return isDeclawed; }
   public void setIsDeclawed(boolean isDeclawed) { this.isDeclawed = isDeclawed; }

   public void useLitter() {
      System.out.println("I just used the litter - I'm a clean cat!");
   }
}
```

```java
package com.flatiron.animal;

public class Parrot extends Animal {
   private int numWings;
   private boolean inFlight;

   public Parrot() {
      super();
      this.numWings = 2;
      this.inFlight = false;
   }

   public int getNumWings() { return numWings; }
   public void setNumWings(int numWings) { this.numWings = numWings; }

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
the examples:

1. A class can extend another class, using the keyword `extends` in the class
   definition.
2. The class that extends is called the **derived class**, the **subclass**, or
   the **child class**. In our example, both `Cat` and `Parrot` are child
   classes.
3. The class that is extended is called the **base class**, the **superclass**,
   or the **parent class**. In our example, `Animal` is the parent class.
4. A child class can call its parent class' constructor by using the method
   `super()`. This will initialize the `name` variable to "unknown" when used
   in the `Cat` and `Parrot` class.

Another aside on the keyword **super** is that it can be used to invoke not
only the constructor of a parent class, but also the variables and methods
belonging to the parent class.

Let's put all of these classes together to use in a `main()` method!

```java
package com.flatiron.animal;

public class Main {
   public static void main(String[] args) {
      Cat tom = new Cat();
      tom.setName("Tom");
      System.out.println("Hello! My name is " + tom.getName());
      tom.eat();
      tom.useLitter();

      Parrot polly = new Parrot();
      System.out.println("I'm a parrot and my name is " + polly.getName());
      polly.eat();
      polly.startFlying();
   }
}

```

The above will produce the following output:

```plaintext
Hello! My name is Tom
Yum! I like to eat!
I just used the litter - I'm a clean cat!
I'm a parrot and my name is unknown
Yum! I like to eat!
I'm flying - weeee!!!
```

Notice in the above example when we put all our inherited classes together, both
the `Cat` object and the `Parrot` object can eat using the parent class' `eat()`
method! The `Cat` and the `Parrot` objects can also use their own methods, like
`useLitter()` and `startFlying()`.

We can also see that using the `super()` method to invoke the `Animal`
constructor worked well too! When the `super()` method is called in the `Parrot`
constructor, it initializes the `name` variable to "unknown" - like it does in
the `Animal` class' constructor! So when we instantiate a new `Parrot` object
and access its name, we print out that the name is "unknown".

## Method Overriding

Maybe we want to personalize how our `Cat` eats. But the `Animal` class already
defines an `eat()` method... so how can we customize it to fit the `Cat` a
little better?

The answer is called **method overriding**. If a method is defined in both
the parent class and the child class, the child class' method will override the
parent class' method using the `@Override` annotation.

```java
// Here we are overriding the Animal::eat() method
@Override
public void eat() {
    super.eat();
    System.out.println("I like to eat cat food!");
}
```

Notice the `super` keyword is back! In this particular example, we will still
print "Yum! I like to eat!" along with printing "I like to eat cat food!" as
well.

We could also omit the `super` keyword if we don't want to reuse the method
we are overriding.

```java
@Override
public void eat() {
    System.out.println("I like to eat cat food!");
}
```

Now when we call the `eat()` method on an instance of a `Cat`, it will only
print "I like to eat cat food!"

There are a couple of rules when overriding methods:

- Only inherited methods can be overridden.
  - This means no `private` methods can be overridden.
- The parent class and child class _must_ have the exact same method name,
  return type, and parameter list in order for the child class to properly
  override.
- Methods declared with the non-access modifiers `static` and `final` cannot be
  overridden.
- The overriding method does not necessarily need to have the same access
  modifier as the parent class' method, but the overriding method must not have
  a more restrictive access modifier.

## The Protected Access Modifier

When we defined the `Animal`, `Cat`, and `Parrot` class, did you notice how they
were all in the same package, `com.flatiron.animal`?

But what if we move the `Parrot` class out of the `com.flatiron.animal` package
into a new package called `com.flatiron.bird`? Does this have an effect on
anything?

Let's look back at our `Animal` class for a minute. Notice that the constructor
for the `Animal` class does not have an access modifier. This means it is
using the default access modifier. As a reminder, the classes, variables, and
methods defined without an access modifier are available to all other classes
in the same package. But when we move the `Parrot` class out of the package, it
will no longer have access to the constructor, and therefore, cannot use
`super()` to initialize the `name` variable.

If we try to compile our program, we'd run into an error like this:

```plaintext
error: Animal() is not public in Animal; cannot be accessed from outside package
```

There is one more access modifier we have not gone over yet that could come in
handy here - it is the **protected access modifier**. Classes, variables,
methods, and constructors defined with this keyword are accessible by classes
within the same package or subclasses. Since `Parrot` is considered a subclass
of `Animal`, we could change the access modifier from the default to
`protected`!

```java
package com.flatiron.animal;

public class Animal {
    private String name;

    // Change constructor's access modifier to protected
    // Allows subclasses to still make use of this constructor
    protected Animal() {
        this.name = "unknown";
    }

   public String getName() { return name; }
   public void setName(String name) { this.name = name; }

   public void eat() {
      System.out.println("Yum! I like to eat!");
   }
}
```

The `protected` keyword is less restrictive than the default access modifier
but still more restrictive than a `public` access modifier. Therefore, the
ordering of access modifiers from least restrictive to most restrictive is as
follows:

- `public`
- `protected`
- `default`
- `private`
