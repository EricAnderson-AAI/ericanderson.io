A **variadic function** is a function where the **total number of parameters are unknown** and can be adjusted at the time the method is called.

The C programming language, along with many others, have an interesting little feature called an **ellipsis argument**. When an ellipsis is used in a methods signature, that method can then accept a varying number of arguments.

Example C Variadic Function:

    int main(void) {
    	variadic_func(4, 1, 2, 3, 4); // Returns 10 as the sum
    	variadic_func(2, 1, 2); // Returns 3 as the sum
    	return 0;
	}
 
	void variadic_func(int num_args, ...) {
      int ellipses_args, i;
      int sum = 0;
      va_list list_pointer;
      va_start(list_pointer, num_args);
 
      for(i = 0; i < num_args; i++ ) {
          ellipses_args = va_arg(list_pointer, int);
          sum = ellipses_args;
      }
 
      va_end(list_pointer);
      printf("%i\n", sum); // Outputs the sum of the given args
	}
    
The **variadic_func()** has a signature with 2 parameters but thanks to the ellipsis it can accept a variable number of arguments. Now let's see how we can take this same concept and implement it in JavaScript.

### JavaScript And ...

Since JavaScript is a functional language, **every function can be variadic** and in various JavaScript libraries you'll commonly see **Array.prototype.slice.call(arguments)** used to get a variable number of trailing arguments back as an array.

Unfortunately, JavaScript does not currently support ellipses; it is however in the works with ECMAScript 6. But for now, we are stuck implementing a function that will collect trailing arguments into an array for us:

JavaScript Variadic Implementation:

	(function () {
    	var slice = Array.prototype.slice,
        	variadic = function (fn) {
            	var fnLength = fn.length;
             
                if (fnLength < 1 ) {
                    return fn;
                }
                else if (fnLength === 1) {
                    return function () {
                        return fn.call(this, slice.call(arguments, 0));
                    }
                }
                else {
                  return function () {
                      var numberOfArgs = arguments.length,
                          namedArgs = slice.call(arguments, 0, fnLength - 1),
                          numberOfMissingNamedArgs = Math.max(fnLength - numberOfArgs - 1, 0),
                          argPadding = new Array(numberOfMissingNamedArgs),
                          variadicArgs = slice.call(arguments, fn.length - 1);
                       
                      return fn.apply(this, namedArgs.concat(argPadding).concat([variadicArgs]));
                  }
              }
          },
          ellipsisFun = function (firstArg, ellipsis) {
              return [firstArg, ellipsis];
          };
    
          console.log(ellipsisFun('one', 'two', 'three')); // Returns ["one", "two"]
          console.log(variadic(ellipsisFun)('one', 'two', 'three')); // Returns ["one", ["two", "three"]]
	}());

The JavaScript implementation of a variadic function takes a single function as it's argument and returns a new function that then passes in every additional argument as an array to the last parameter.

### Disclaimer

In JavaScript, a functions parameters can be accessed from the **arguments object**.

Using Arguments Object:

	function argumentsObject () {
    	for (var i = 0; i < arguments.length; i++) {
        	console.log(arguments[i]);
    	}
	}
 
	argumentsObject('I', 'should', 'sleep', 'more'); // Outputs all 4 strings to the console
    
One might think that going through all this trouble to create a variadic function isn't worth it. Just know that **using the arguments object is considerably slower than directly accessing argument bindings**.

But if performance is a top priority then you probably shouldn't be using a function that takes a variable number of arguments in that section anyway.

### Practical Usage

So when would we use a variadic function? 
One example, that's used often on the web, is an event triggering mechanism which accepts a variable number of arguments.

Original Trigger:

	function (item) {
      var args;
      if (!this._events) return this;
      args = slice.call(arguments, 1);
      return this;
	}
    
Could be rewritten to be:

	variadic(function (item, args) {
      var args;
      if (!this._events) return this;
      return this;
    }
    
### Wrap up

Variadic functions are quite useful and powerful in their own way; and if used properly, in some edge cases they can improve performance. One final note; earlier in my disclaimer I stated that using the arguments object is considerably slower than not.

[Here is a jsperf benchmark](http://jsperf.com/how-slow-is-arguments/2) created by [Chris Khoo](https://gist.github.com/khoomeister) that shows how slow arguments usage is. And for good measure here is a benchmark for [arguments vs. array arguments](http://jsperf.com/arguments-vs-array-argument/2). As always, if you have any questions or see any blatant mistakes in this post don't hesitate in letting me know!