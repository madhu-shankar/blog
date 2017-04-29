---
published: true
---
### Must know Junit Rules

In this blog post I will be writing about some Junit Rules that are very useful while writing test cases. This is not a introduction to Junit Rules. I will just be writing about some of the rules that Junit provides out of the box and how they could be extended to your use case.

1.[Expected Exception](http://junit.org/junit4/javadoc/4.12/org/junit/rules/ExpectedException.html):
As the name suggests, this is for verifying the exception thrown when you invoke a method. This provides good helper methods by which you can verify the cause of the exception, the message in the exception and much more. This greatky reduces the amount of boiler plate that we write for verifying the exceptions. Below is a simple code snippet showing the usage of this rule,

{% highlight java %}
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

2.[External Resource](http://junit.org/junit4/javadoc/4.12/org/junit/rules/ExternalResource.html):
This rule can be used when you are accessing some external resource like a database in your tests. This is an abstract rule, which provide two methods before and after which would contain but the setup and teardown logic respectively. This the rule I found most useful, this rule can be for various resources like contacting external database, setting up and teardown in-memory database etc. You can define a rule for the database you are using in your project and reuse it in all the test classes, pretty cool!. Below is a simple code snippet showing the usage.

{% highlight java %}
public static class ExternalResourceExampleTest {
  DataBase database= new DataBase();

  @Rule
  public ExternalResource resource= new ExternalResource() {
      @Override
      protected void before() throws Throwable {
          database.openConnection();
          database.load(testData)
         };

      @Override
      protected void after() {
          database.delete(testData);
          database.closeConnection();
         };
     };

  @Test
  public void testFoo() {
      database.query(expression);
     }
 }
{% endhighlight %}

3.[Temporary Folder](http://junit.org/junit4/javadoc/4.12/org/junit/rules/TemporaryFolder.html):
You can use this rule if you need a temporary folder in tests to create and delete files. Below is a code snippet showing the usage.
{% highlight java %}
public static class TemporaryFolderTest {
  @Rule
  public TemporaryFolder tempFolder= new TemporaryFolder();

  @Test
  public void testUsingTempFolder() throws IOException {
      File createdFile= tempFolder.newFile("NewFile");
      File createdFolder= tempFolder.newFolder("NewSubFolder");
      // ...
     }
 }
{% endhighlight %}

4.[Timout](http://junit.org/junit4/javadoc/4.12/org/junit/rules/Timeout.html):
As the name says, you can add timeouts to your tests. The tests will fail if they do not complete in the set timeout. This rule is useful when you are invoking some remote services and having a timeout on such tests make sense.
{% highlight java %}
public static class GlobalTimeoutTest {

  @Rule
  public Timeout globalTimeout= new Timeout(20, TimeUnit.Seconds);

  @Test
  public void invokeHighLatencyAPI() throws InterruptedException {
      restClient.get();
  }

  @Test
  public void infiniteLoop() {
      while (true) {}
  }
 }
{% endhighlight %}

5.[Verifier](http://junit.org/junit4/javadoc/4.12/org/junit/rules/Verifier.html):
This rule could be used to add global verifications in your test classes.
{% highlight java %}
public static class VerifierTest {
   private SomeClass underTest = new SomeClass();
   
   @Rule
   public Verifier verifier = new Verifier() {
       @Override public void verify() {
          assertFalse(underTest.hasFailures());
       }
   }

   @Test
   public void testThatMightCaseFailures() {
       // ...
   }
}
{% endhighlight %}


[Reference](http://junit.org/junit4/javadoc/4.12/org/junit/rules/MethodRule.html)
