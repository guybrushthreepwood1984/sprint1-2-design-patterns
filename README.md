sprint1-2-design-patterns

Prototype (Clone)

I chose this example to get a better understanding of class, instances and inheritance. So far, although it is a thrilling subject, my knowledge of Classes is limited. Clean and maintainable code is the goal. 

To understand Prototypes in TS, I had to look up the prototype chain in JS: "Every object in JavaScript has a built-in property, which is called its prototype. The prototype is itself an object, so the prototype will have its own prototype, making what's called a prototype chain. The chain ends when we reach a prototype that has null for its own prototype."


Object.getPrototypeOf.()

Object.create


Pros of Prototype: 
- Alternative to subclassing
-  The Prototype pattern is available in TypeScript out of the box with a JavaScript’s native Object.assign() method.
-  your code shouldn’t depend on the concrete classes of objects that you need to copy.
- You get an alternative to inheritance when dealing with configuration presets for complex objects.
- recognizable via 'clone' or 'copy' methods
- more efficient for big objects or if many instances of an object are needed
- dynamically change the behavior of objects at runtime by modifying their prototype.

Cons:
- circular reference

So far, this seems to 

    Optionally, create a centralized prototype registry to store a catalog of frequently used prototypes.

For cloning an existing object, the Prototype pattern does not depend on the Class, but rather the instantiated object itself. An object, which provides the cloning method, is the Prototype.  



Code examples

```ts

class ProductPrototype {
    title: string;
    price: number;

    constructor(title: string, price: number) {
        this.title = title;
        this.price = price;
    }

    addDiscount (discount: number) {
        const discountedPrice = this.price * (100-discount)/100
        this.price = discountedPrice 
    }

    describe() {
        console.log(`Title: ${this.title}, Price: ${this.price} euro`);
    }



// "this" would call the constructor and create a completety new object. With "Object.create" we keep all values of already created instance.
// The Object.create() static method creates a new object, using an existing object as the prototype of the newly created object.
    clone(): this {
        const clone = Object.create(this);
        return clone;
    }
}

const productPrice10 = new ProductPrototype('', 10);

const book = productPrice10.clone();
book.title = 'A hitchhikers guide to the galaxy';

const movie = productPrice10.clone();
movie.title = 'El día de la bestia';
movie.addDiscount(10) 

book.describe();  
// Title: A hitchhikers guide to the galaxy, Price: 10
movie.describe();  
// Title: El día de la bestia, Price: 9
console.log(Object.getPrototypeOf(book))
// // ProductPrototype: {
//   "title": "",
//   "price": 10
// } 
console.log(Object.getPrototypeOf(ProductPrototype))
// function () { [native code] } 
// implemented in JS engine, source code not visible 
```

Note: With subclasses, it is important to insert the clone() method in each one of them.

Sources:

https://refactoring.guru/design-patterns/prototype

https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/Object_prototypes

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/create


https://www.youtube.com/watch?v=9QgwRVWX920