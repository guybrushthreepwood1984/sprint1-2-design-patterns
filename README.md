# Desgin pattern: Prototype (Clone)

I chose this example to get a better understanding of class, instances and inheritance. So far, although it is an interesting subject, my knowledge of Classes is limited.  As I understand it, prototypes provide an alternative for inheritance in subclasses. If implented correctly, it helps to write clean and maintainable code. 

For cloning an existing object, the Prototype pattern does not depend on the Class, but rather the instantiated object itself. An object, which provides the cloning method, is the Prototype.  

### Pros of Prototype: 
- Available in TypeScript out of the box with a JavaScript’s native Object.create() method.
- Clones do not depend on the concrete classes of objects but rather the copied object itself.
- More efficient for copying big objects or if many instances of an object are needed
- Dynamically change the behavior of objects at runtime by modifying their prototype.

### Cons:
- Circular reference (= objects reference each other, causing an infinite loop)

To understand Prototypes in TS, I had to look up the prototype chain in JS: "Every object in JavaScript has a built-in property, which is called its prototype. The prototype is itself an object, so the prototype will have its own prototype, making what's called a prototype chain. The chain ends when we reach a prototype that has null for its own prototype."
To look this up, you can use the **Object.getPrototypeOf.()** method.

In existing code, the pattern is recognizable via 'clone' or 'copy' methods.


### Code example

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



// returning "this" alone would call the constructor and create a completety new object. With "Object.create()" we keep all values of the already created instance.
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
// Title: A hitchhikers guide to the galaxy, Price: 10 euro
movie.describe();  
// Title: El día de la bestia, Price: 9 euro
console.log(Object.getPrototypeOf(book))
// // ProductPrototype: {
//   "title": "",
//   "price": 10
// } 
console.log(Object.getPrototypeOf(ProductPrototype))
// function () { [native code] } 
// implemented in JS engine, source code not visible 
```

Note: If using subclasses, it is important to insert the clone() method in each one of them.

Sources:

https://refactoring.guru/design-patterns/prototype

https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/Object_prototypes

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/create

Prototype Pattern | Implementation in TypeScript | Software Design patterns series
https://www.youtube.com/watch?v=9QgwRVWX920