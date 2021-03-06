_How does Guice compare to Spring et al_
### Spring Comparison

The Spring Framework, created by Rod Johnson, blazed the Java dependency injection trail. If not the first dependency injection framework, you can certainly credit Spring with pioneering much of what we know about dependency injection and bringing it into the mainstream. Guice might not exist (at least not this early) if not for Spring's example.

Guice and Spring don't compete directly. While Spring is a comprehensive stack, Guice focuses on dependency injection. Even so, the first question most people ask is, "how does Guice compare to Spring?" Rather than repeat the same spiel N times, we figured it best to answer the inevitable question once.

Let us start by saying, the Guice team has a great deal of respect for the Spring developers, community, and their overall attitude. We're more than willing to work together in any capacity. We even gave a few core Spring developers a sneak peek at Guice six months before the 1.0 release, and they've had access to the source code repository ever since.

Without further ado, how does Guice compare to Spring?

Guice is anything but a rehash of Spring. Guice was born purely out of use cases from one of Google's biggest (in every way) applications: [AdWords](http://adwords.google.com). We sat down and asked ourselves, how do we really want to build this application going forward? Guice enables our answer. 

Guice wholly embraces annotations and generics, thereby enabling you to wire together and test objects with measurably less effort. Guice proves you can use annotations instead of error-prone, refactoring-adverse string identifiers. 

Some new users fear that the number of annotations will get out of hand. Fear not. You won't have nearly as many different annotations in a Guice application as you have string identifiers in a Spring application.
  1. You only need to use annotations when a binding type alone isn't sufficient.
  2. You can reuse the same annotation type across multiple bindings (`@Transactional` for example). 
  3. Annotations can have attributes with values. You can bind each set of values separately to the same type if you like.

Spring supports two polar configuration styles: explicit configuration and auto-wiring. Explicit configuration is verbose but maintainable. Auto-wiring is concise but slow and not well suited to non-trivial applications. If you have 100s of developers and 100s of thousands of lines of code, auto-wiring isn't an option. Guice uses annotations to support a best-of-both-worlds approach which is concise but explicit (and maintainable).

Some critics have tried to find an analog to Spring's XML (or new Java-based configuration) in Guice. Most times, there isn't any, but sometimes you can't use annotations, and you need to manually wire up an object (a 3rd party component for example). When this comes up, you write a custom provider. A custom provider adds one level of indirection to your configuration and is roughly 1:1 with Spring's XML in complexity. The remaining majority of the time, you'll write significantly less code. 

Eric Burke recently [ported a Spring application to Guice](http://stuffthathappens.com/blog/2007/03/09/guicy-good/):
```
  At the end of the day, I compared my (Guice) modules — written in Java — to my 
  Spring XML files. The modules are significantly smaller and easier to read.

  Then I realized about 3/4 of the code in my modules was unnecessary, 
  so I stripped it out. They were reduced to just a few lines of code each.
```
Oftentimes, it doesn't make sense to port perfectly working code which you won't be changing any time soon to Guice, so Guice works with Spring when possible. You can [bind your existing Spring beans](http://google.github.io/guice/api-docs/latest/javadoc/com/google/inject/spring/SpringIntegration.html). Guice supports AOP Alliance method interceptors which means you can use Spring's popular transaction interceptor.

From our experience, most AOP frameworks are too powerful for most uses and don't carry their own conceptual weight. Guice builds method interception in at the core instead of as an afterthought. This hides the mechanics of AOP from the user and lowers the learning curve. Using one object instead of a separate proxy reduces memory waste and more importantly reduces the chances of invoking a method without interceptors. Under the covers, Guice uses a separate handler for each method minimizing performance overhead. If you intercept just one method in a class, the other methods will not incur overhead.