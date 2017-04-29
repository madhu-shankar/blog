---
published: true
---
### Must know Junit Rules

In this blog post I will be writing about some Junit Rules that are very useful while writing test cases. This is not a introduction to Junit Rules. I will just be writing about some of the rules that Junit provides out of the box and how they could be extended to your use case.

1.[Expected Exception](http://junit.org/junit4/javadoc/4.12/org/junit/rules/ExpectedException.html)
As the name suggests, this is for verifying the exception thrown when you invoke a method. This provides good helper methods by which you can verify the cause of the exception, the message in the exception and much more. This greatky reduces the amount of boiler plate that we write for verifying the exceptions. Below is a simple code snippet showing the usage of this rule,

{%  highlight java %}
public class ExpectedExceptionExampleTest {
     @Rule
     public ExpectedException thrownException = ExpectedException.none();

     @Test
     public void throwsNothing() {
         // no exception expected, none thrown: passes.
     }

     @Test
     public void throwsExceptionWithMessageSubString() {
         thrownException.expect(IllegalArgumentException.class);
         expectMessage("Invalid Argument"); 
         throw new IllegalArgumentException("Invalid Argument while processing");
     }
 }
{% endhighlight %}

2.[External Resource](http://junit.org/junit4/javadoc/4.12/org/junit/rules/ExternalResource.html)
3.[Temporary Folder](http://junit.org/junit4/javadoc/4.12/org/junit/rules/TemporaryFolder.html)
4.[Timout](http://junit.org/junit4/javadoc/4.12/org/junit/rules/Timeout.html)
5.[Verifier](http://junit.org/junit4/javadoc/4.12/org/junit/rules/Verifier.html)


[Ref](http://junit.org/junit4/javadoc/4.12/org/junit/rules/MethodRule.html)
