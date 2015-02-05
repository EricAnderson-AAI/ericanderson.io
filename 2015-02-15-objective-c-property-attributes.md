With the Apple watch coming soon there has been an influx of new iOS developers. Because of this I keep seeing the same type of conversations and pull requests centered around this one topic **property attributes**. I have seen developers use various attributes *by default*, without really knowing what they do; leading me to believe that many Objective-C programmers think that the **@property** is some sort of black magic that automagically solves problems.

### The @property Defined
The @property is a property decalaration which declares one or two methods and describes them at runtime. @property declarations are shorthand for declaring accessor methods, i.e. **getters** and **setters**.

### @property Attributes

#### Nonatomic

`nonatomic` is used for multithreading purposes. Setting a property as nonatomic means that if any other thread wants access to it, it can, in respect to multithreading rules. Nonatomic is not the default behavior so you will have to explicitly add it to the property. Also note that unexpected behavior can occur when two different threads access the same variable at the same time.

#### Copy

`copy` is required when the object is **mutable**. Use this if you need the value of the object as it is at *this* moment, and you don't want that value to reflect any changes made by other owners of the object. You will need to release the object in non-garbage collected environments when you are finished with it because you are retaining the copy.

#### Assign

`Assign` is somewhat the opposite to copy. When calling the getter of an assign property, it returns a reference to the actual data. Typically you use this attribute when you have a property of a primitive type (float, int, BOOL...)

#### Retain

`retain` is required when the attribute is a pointer to an object. The setter generated by **@synthesize** will add to the retain count of the object. You will need to release the object when you are finished with it. By using retain it the object will occupy memory in autorelease pool. Methods like **alloc** include an implicit retain.

#### Strong

`strong` is a replacement for the retain attribute, as part of Objective-C Automated Reference Counting (ARC). In non-ARC code it's just a synonym for retain.

#### Weak

`weak` is similar to strong except that it won't increase the reference count. The parent object does not become an owner of the child object, it only holds a reference to it. If the parents object's reference count drops to 0, even if you're pointing to the child object, the child object will be deallocated from memory as well.

#### Assign

`assign` is the default for C primitive values and simply performs a variable assignment.

If you're still confused about @property attributes or see some typo's or other mistakes in this post, leave a comment below or [submit a pull request](https://github.com/EricAnderson-AAI/evlsnoopy.com).