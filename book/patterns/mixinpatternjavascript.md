# The Mixin Pattern

In traditional programming languages such as C++ and Lisp, Mixins are classes which offer functionality that can be easily inherited by a sub-class or group of sub-classes for the purpose of function re-use.

## Sub-classing

For developers unfamiliar with sub-classing, we will go through a brief beginners primer on them before diving into Mixins and Decorators further.

Sub-classing is a term that refers to inheriting properties for a new object from a base or superclass object. In traditional object-oriented programming, a class B is able to extend another class A. Here we consider A a superclass and B a subclass of A. As such, all instances of B inherit the methods from A. B is however still able to define its own methods, including those that override methods originally defined by A.

Should B need to invoke a method in A that has been overridden, we refer to this as method chaining. Should B need to invoke the constructor A (the superclass), we call this constructor chaining.

In order to demonstrate sub-classing, we first need a base object that can have new instances of itself created. let's model this around the concept of a person.

````js
var Person = function( firstName, lastName ){

  this.firstName = firstName;
  this.lastName = lastName;
  this.gender = "male";

};
````

Next, we'll want to specify a new class (object) that's a subclass of the existing Person object. Let us imagine we want to add distinct properties to distinguish a Person from a Superhero whilst inheriting the properties of the Person "superclass". As superheroes share many common traits with normal people (e.g. name, gender), this should hopefully illustrate how sub-classing works adequately.

````js
// a new instance of Person can then easily be created as follows:
var clark = new Person( "Clark", "Kent" );

// Define a subclass constructor for for "Superhero":
var Superhero = function( firstName, lastName, powers ){

    // Invoke the superclass constructor on the new object
    // then use .call() to invoke the constructor as a method of
    // the object to be initialized.

    Person.call( this, firstName, lastName );

    // Finally, store their powers, a new array of traits not found in a normal "Person"
    this.powers = powers;
};

Superhero.prototype = Object.create( Person.prototype );
var superman = new Superhero( "Clark", "Kent", ["flight","heat-vision"] );
console.log( superman );

// Outputs Person attributes as well as powers
````

The Superhero constructor creates an object which descends from Person. Objects of this type have attributes of the objects that are above it in the chain and if we had set default values in the Person object, Superhero is capable of overriding any inherited values with values specific to it's object.

## Mixins

In JavaScript, we can look at inheriting from Mixins as a means of collecting functionality through extension. Each new object we define has a prototype from which it can inherit further properties. Prototypes can inherit from other object prototypes but, even more importantly, can define properties for any number of object instances. We can leverage this fact to promote function re-use.

Mixins allow objects to borrow (or inherit) functionality from them with a minimal amount of complexity. As the pattern works well with JavaScripts object prototypes, it gives us a fairly flexible way to share functionality from not just one Mixin, but effectively many through multiple inheritance.

They can be viewed as objects with attributes and methods that can be easily shared across a number of other object prototypes. Imagine that we define a Mixin containing utility functions in a standard object literal as follows:

````js
var myMixins = {

  moveUp: function(){
    console.log( "move up" );
  },

  moveDown: function(){
    console.log( "move down" );
  },

  stop: function(){
    console.log( "stop! in the name of love!" );
  }

};
````

We can then easily extend the prototype of existing constructor functions to include this behavior using a helper such as the Underscore.js _.extend() method:

````js
// A skeleton carAnimator constructor
function CarAnimator(){
  this.moveLeft = function(){
    console.log( "move left" );
  };
}

// A skeleton personAnimator constructor
function PersonAnimator(){
  this.moveRandomly = function(){ /*..*/ };
}

// Extend both constructors with our Mixin
_.extend( CarAnimator.prototype, myMixins );
_.extend( PersonAnimator.prototype, myMixins );

// Create a new instance of carAnimator
var myAnimator = new CarAnimator();
myAnimator.moveLeft();
myAnimator.moveDown();
myAnimator.stop();

// Outputs:
// move left
// move down
// stop! in the name of love!
````

As we can see, this allows us to easily "mix" in common behaviour into object constructors fairly trivially.

In the next example, we have two constructors: a Car and a Mixin. What we're going to do is augment (another way of saying extend) the Car so that it can inherit specific methods defined in the Mixin, namely driveForward() and driveBackward(). This time we won't be using Underscore.js.

Instead, this example will demonstrate how to augment a constructor to include functionality without the need to duplicate this process for every constructor function we may have.

````js
// Define a simple Car constructor
var Car = function ( settings ) {

    this.model = settings.model || "no model provided";
    this.color = settings.color || "no colour provided";

};

// Mixin
var Mixin = function () {};

Mixin.prototype = {

    driveForward: function () {
        console.log( "drive forward" );
    },

    driveBackward: function () {
        console.log( "drive backward" );
    },

    driveSideways: function () {
        console.log( "drive sideways" );
    }

};


// Extend an existing object with a method from another
function augment( receivingClass, givingClass ) {

    // only provide certain methods
    if ( arguments[2] ) {
        for ( var i = 2, len = arguments.length; i < len; i++ ) {
            receivingClass.prototype[arguments[i]] = givingClass.prototype[arguments[i]];
        }
    }
    // provide all methods
    else {
        for ( var methodName in givingClass.prototype ) {

            // check to make sure the receiving class doesn't
            // have a method of the same name as the one currently
            // being processed
            if ( !Object.hasOwnProperty.call(receivingClass.prototype, methodName) ) {
                receivingClass.prototype[methodName] = givingClass.prototype[methodName];
            }

            // Alternatively (check prototype chain as well):
            // if ( !receivingClass.prototype[methodName] ) {
            // receivingClass.prototype[methodName] = givingClass.prototype[methodName];
            // }
        }
    }
}


// Augment the Car constructor to include "driveForward" and "driveBackward"
augment( Car, Mixin, "driveForward", "driveBackward" );

// Create a new Car
var myCar = new Car({
    model: "Ford Escort",
    color: "blue"
});

// Test to make sure we now have access to the methods
myCar.driveForward();
myCar.driveBackward();

// Outputs:
// drive forward
// drive backward

// We can also augment Car to include all functions from our mixin
// by not explicitly listing a selection of them
augment( Car, Mixin );

var mySportsCar = new Car({
    model: "Porsche",
    color: "red"
});

mySportsCar.driveSideways();

// Outputs:
// drive sideways
````

### Advantages & Disadvantages

Mixins assist in decreasing functional repetition and increasing function re-use in a system. Where an application is likely to require shared behaviour across object instances, we can easily avoid any duplication by maintaining this shared functionality in a Mixin and thus focusing on implementing only the functionality in our system which is truly distinct.

That said, the downsides to Mixins are a little more debatable. Some developers feel that injecting functionality into an object prototype is a bad idea as it leads to both prototype pollution and a level of uncertainty regarding the origin of our functions. In large systems this may well be the case.

I would argue that strong documentation can assist in minimizing the amount of confusion regarding the source of mixed in functions, but as with every pattern, if care is taken during implementation we should be okay.
