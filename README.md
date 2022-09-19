# Java-Is-Verbose

## Import Statements

This is an example of Java star imports:

```
import java.awt.*;
import java.sql.*;
import java.util.*;
```

It looks fine. Unfortunately, star imports are considered by many to be an antipattern in Java. Many IDEs/liters flag star imports as warnings. I used to assume the reason had something to do with performance. Apparently, the real issue is with naming conflicts. Adding more star imports can cause code to break because of ambiguity on which package to get things from. 

Ok, so you can't use star imports. But that means that the top of your Java files will all be littered with something like this:

```
import com.google.common.base.Preconditions.checkArgument;
import com.google.common.base.Preconditions.checkNotNull;
import com.google.common.base.Preconditions.checkState;
import com.google.common.graph.GraphConstants.ENDPOINTS_MISMATCH;
import com.google.common.collect.ImmutableSet;
import com.google.common.collect.Iterators;
import com.google.common.collect.Sets;
import com.google.common.collect.UnmodifiableIterator;
import com.google.common.math.IntMath;
import com.google.common.primitives.Ints;
import java.util.AbstractSet;
import java.util.Set;
import javax.annotation.CheckForNull;
```

I want to be clear that I'm not picking on the repositories I'm stealing the example code from. They may be well-done. I'm just trying to illustrate some of the pitfall of the language. 

Most IDEs automatically generate import statements, and the automatically hide them. As a general rule of thumb, when you IDE auto-generates code, and then auto-hides that code, that is a code smell for bad language design. That principle will come up again. 

Code should be written in such a way that it provides maximum information to the reader. Import statements are the first thing you see when open a file. If they are unreadable, they are failing at conveying useful information.

So is there a better way? There is. This is how you do imports in Typescript:

```
import { HttpClient, HttpErrorResponse } from "@angular/common/http";
import { Observable, throwError } from "rxjs";
import { catchError, map, tap } from 'rxjs/operators';
```

There are 7 imports total, but only 3 lines. Imports form the same source are compressed onto the same line, essentially eliminating the repeated code from the Java example. 

In Typescript, you can quickly skim over and say, "ok, this uses HTTP, and rxjs Observables, and rxjs operators." If the reader if familiar with those packages, they immediately get a vague overview of what the code will be doing. 

## Constructors



## Getters and Setters

A great deal of the verbosity of Java can be blamed on getters and setters.

```
class Demo {

    private int length; 
    private int width; 
    private int height; 

    public Demo(int length, int width, int height) {
        this.length = length;
        this.width = width;
        this.height = height;
    }

    public getLength() {
        return this.length;
    }

    public setLength(int length) {
        this.length = length;
    }

    public getWidth() {
        return this.length;
    }

    public setWidth(int width) {
        this.width = width;
    }

    public getHeight() {
        return this.height;
    }

    public setHeight(int height) {
        this.height = height;
    }
}
```

Java is a language I learned in university. I remember one day, the professor told us, "generally speaking, you should make all class variables private. There is almost never a good reason for a public field." One student chimed in, "if we're not allowed to use public variables, then we will just make getters and setters for everything." 

I think the response was something along the lines of, "You shouldn't expose everything. You should think hard before exposing something." The issue still remains, though: sometimes you do want a field exposed. You will find yourself wanting to implement a getter and setter, therefore. At that point, the variable is exposed, so why is it still private?

Getters and setters do provide some utility. You might want to do some action every time you update or access a variable (for example, log it, enforce constraints on changes, etc). Unfortunately, it is inadvisable to start with a public variable, and then later enforce the use of getters and setters should you need the functionality associated with it. That breaks backwards-compatibility. Therefore, developers opt to preemptively require the "private & getter & setter" trifecta.

Spring Boot, and probably other Java frameworks, actually require the use of getters and setters. Some developers use Lombok to auto-generate the required getters and setters at runtime. Others use their IDE to auto-generate them. These getters and setters are usually and the bottom of the class, lest anybody see them. Devs even sometimes use their IDE to automatically hide the getters and setters. This is the same code smell from before: when you are auto-generating something, and auto-hiding it, that is a sign onf poor language design. 

Is there a better way? I think htere is. I actually prefer how C# 3.0+ handles it.

```
public class Foo
{
    // getter and setter are attached directly to variable, on one line.
    public string bar { get; set; } 
}
```

Or in the case where you need the getter or setter to do something:

```
private string mFoo; // backing field
public string foo
{
    get { return mFoo; }
    set
    {
        mFoo = value;
        PerformSomeAction();
    }
}
```

I still don't like the fact that you need to use a backing field in order to make the getter or setter do something. However, if we need to introduce a backing variable, at least that change is internal to the class, and does not break backwards-compatibility. Therefore, this way is still superior to the Java style of making the user call getX and setX from the start.

```
myObject.setY(myObject.getX());
```

Hideous!

## No Default Arguments



## Strict Typing



## Curly Braces



## Boilerplate

