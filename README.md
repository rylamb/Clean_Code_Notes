# Clean Code Notes
1. [Meaningful Names](#meaningful-names)
2. [Functions]()
3. [Comments]()
4. [Formatting]()
5. [Objects and Data Structures]()
6. [Error Handling](#error-handling)
7. [Boundaries](#boundaries)
8. [Unit Tests](#unittests)
9. [Classes](#classes)


## Meaningful Names<a name="meaningful-names"></a>
1. **Use Intention-Revealing Names**
    - Names should tell you why it exists, what it does, and how it's used.

2. **Avoid Disinformation**
    - Avoid ambiguous abbreviations. 
    - Don't put things like _list_ in the name unless it's actually a list. 
    - Avoid subtle name differences. Use consistent spelling.

3. **Make Meaningful Distinctions**
    - Avoid noninformative names (i.e. `a1, a2, ... aN`). 
    - Avoid redudant noise words (i.e. _variable_ in a variable name, or _String_ in _nameString_). 
    - Avoid arbitrary name changes for the sake of satisfying scope restrictions.

4. **User Pronounceable Names**
    - This makes no sense.
        ```java
        class DtaRcrd102 {
            private Date genymdhms; // generation date, year, month, day, hour, minute, seconds)
            private Date modymdhms;
            private final String pszqint = "102";
            /* ... */
        }
        ```
        This is better.
        ```java
        class Customer {
            private Date generationTimestamp;
            private Date modificationTimestamp;
            private final String recordId = "102;
            /* ... */
        }
        ```

5. **Use Searchable Names**
    - Single letter names and numeric constants are difficult to search and should only be used as local variables. The length of a name should correspond to the size of its scope.

6. **Avoid Encodings**
    - _**Type Encoding:**_ Unless you're at Microsoft, don't use Hungarian Notation or other forms of type encoding.
    - _**Member Prefixes:**_ Don't prefix member variables with _m\__. It's unseen clutter.
    - _**Interfaces and Implementations:**_ Leave interfaces unadorned. For instance, instead of naming your interface `IShapeFactory` and the concrete class `ShapeFactory` instead name the interface `ShapeFactory` and the concrete class `ShapeFactoryImpl`.

7. **Avoid Mental Mapping**
    - Refrain from using variables names that reader's must mentally translate into an actual concept.

8. **Class Names**
    - Classes and objects should have noun or noun phrase names like _Customer_, _WikiPage_, _Account_, and _AddressParser_. 
    - Avoid words like _Manager_, _Processor_, _Data_, or _Info_ in the name of a class.
    - A class name should not be a verb.

9. **Method Names**
    - Methods should have verb or verb phrase names like _postPayment_, _deletePage_, or _save_.
    - Accessors, mutators, and predicates should be named for their value and prefixed with _get_, _set_, and _is_ according to the javabean standard.
    - When constructors are overloaded, use static factory methods with names that describe the arguments. Example:
        ```java
        // DO THIS
        Complex fulcrumPoint = Complex.FromRealNumber(23.0);
        
        // DON'T DO THIS
        Complex fulcrumPoint = new Complex(23.0);
        ```

10. **Don't Be Cute**
    - Don't use slang, jokes, or colloquialisms.

11. **Pick One World per Concept**
    - Have a consistent lexicon. Avoid using _fetch_, _retrieve_, and _get_ to all mean the same thing. Likewise it's confusing to have things like a _controller_ and a _manager_ and a _driver_ in the same code base since they all similar in function.

12. **Don't Pun**
    - Avoid using the same word for two purposes. If many classes use the word _add_ to createa  new value by adding or concatenating two existing values, and you create a new class that puts a single parameter into a collection, don't use a method called _add_ for the sake of consistency. Instead opt for something like _insert_ or _append_ instead.
 
13. **Use Solution Domain Names**
    - People reading your code will be programmers, so feel free to use computer science terms, algorithm names, pattern names, math terms, etc. It isn't wise to draw every name from the problem domain because we don't want our coworkers running back and forth to product people asking what ever name means.

14. **Use Problem Domain Names**
    - When there is no "programmer-eese" for what you're doing, use the name from the problem domain.

15. **Add Meaningful Context**
    - Add context to names by enclosing them in well-named classes, functions, or namespaces. For example, the variables _firstName_, _lastName_, _street_, _houseNumber_, _city_, _state_, and _zipcode_ when taken together clearly indicate an address, but when taken individually (for example _state_) lose context. By encapsulating these variables in an _Address_ class and accessing then through `Address.getState()` you add clarity. Consider using prefixes (i.e. _addrState_) only as a last resort.

16. **Don't Add Gratuitous Context**
    - Shorter names are generaly better than longer ones, as long as they're clear. Add no more context to a name than necessary. For example, in an imaginary application called "Gas Station Deluxe" it's a bad idea to prefix every class with GSD. When you hit _G_ in your IDE you're awarded with a mile-long list of every class in the system. Likewise, say you invented a _MailingAddress_ class in _GSD_'s accounting module and named it _GSDAccountAddress_. Later, you need a mailing address for your customer contact application. Does it still make sense to use _GSDAccountAddress_? The name doesn't make sense anymore even if the functionality does.

## Functions <a name="functions"></a>
1. **Small!**

2. **Do One Thing**

3. **One Level of Abstraction per Function**

4. **Switch Statements**

5. **Use Descriptive Names**

6. **Function Arguments**

7. **Have No Side Effects**

8. **Command Query Separation**

9. **Prefer Exceptions to Returning Error Codes**

10. **Don't Repeat Yourself**

11. **Structured Programming**


## Comments <a name="comments"></a>


## Formatting <a name="formatting"></a>


## Objects and Data Structures <a name="objects-and-data-structures"></a>


## Error Handling <a name="error-handling"></a>
1. **Use Exceptions Rather Than Return Codes**
    - Error flags or returned error codes must be checked by the caller, cluttering code and obscuring function logic. It's cleaner to throw an exception when encountering and error.

2. **Write Your _Try-Catch-Finally_ Statement First**
    - Starting with a _try-catch-finally_ helps you define what the user of that code should expect, no matter what goes wrong with the code that is executed in the _try_. We can then proceed to build the rest of the logic that we need, and can pretend that nothing goes wrong. Try to write tests that force exceptions, and then add behavior to your handler to satisfy your tests.

3. **Use Unchecked Exceptions**
    - The price of checked exceptions is an [_Open/Closed Principle_](https://springframework.guru/principles-of-object-oriented-design/open-closed-principle/) violation. If you throw a checked exception from a method in your code and the _catch_ is three levels above, you must declare that exception in the signature of each method between you and the _catch_. This means any changes at a a low level can force cascading signature changes up to the high level. In general, checked exceptions aren't worth it.

4. **Provide Context With Exceptions**
    - Stack traces can't tell you the intent of an operation. Create informative messages to pass with exceptions. Mention the operation that failed and the type of failure.

5. **Define Exception Classes in Terms of a Caller's Needs**
    - Wrapping third-party APIs into classes allows for consolidation of exceptions into a single exception type, minimizes third-party API dependencies, simplifies mocks in tests, and untethers you from a vendor's API design choices.

6. **Define Normal Flow**
    - Use the [_SPECIAL CASE PATTERN_](https://visualstudiomagazine.com/blogs/tool-tracker/2015/12/special-case-pattern.aspx) to create classes or configure objects so that they handle special cases for you, minimizing the need for client code to deal with exceptional behavior. Instead, that behavior is encapsulated in special case objects.

7. **Don't Return Null**
    - If you're tempted to return null from a method, consider throwing an exception or returning a _SPECIAL CASE_ object instead. If you're calling a null-returning method from a third-party API, consider wrapping that method with a method that either throws an exception or returns a special case object. E.g.: 
      ```java
      // DO THIS:

      public List<Employee> getEmployees() {
          if ( .. there are no employees ..)
              return Collections.emptyList();
      }

      List<Employee> employees = getEmployees();
      for (Employee e : employees) {
          totalPay += e.getPay();
      }

      // DON'T DO THIS:

      List<Employee> employees = getEmployees();
      if (employees != null) {
          for (Employee e : employees) {
              totalPay += e.getPay();
          }
      }
      ```

12. **Don't Pass Null**
    - Unless working with an API which expects you to pass _null_, you should avoid passing _null_ whenever possible. In most languages there is no good way to deal with a _null_ that is passed by a caller accidentally, thus it makes sense to forbid passing _null_ by default _([see Lombok's @NonNull](https://projectlombok.org/features/NonNull))_. Taking this approach allows allows you to assume that a _null_ in an argument list is an indication of a problem.


## Boundaries <a name="boundaries"></a>
1. **Using Third-Party Code** <a name="boundaries-1"></a>
    - Consider the following code:
        ```java
        Map sensors = new HashMap();
        Sensor s = (Sensor)sensors.get(sensorId);
        ```
       In this scenario, the client carries the responsibility of getting an object from the map an casting it to the correct type. This code fails effectively tell it's whole story and can be cleaned up with the use of generics.
       ```java
       Map<Sensor> sensors = new HashMap<>();
       Sensor s = sensors.get(sensorId);
       ```
       Additionally, `Map` is a broad interface containing a number of default methods such as `clear()`. If we begin passing this Map around, suddenly any method that gets a hold of this object has the ability to start deleting items from it, even if we don't necessarily need/want them to. In this example we might restrict editting by using immutable or unmodifiable maps via `Map.of()` or `ImmutableMap.of()`. But what if we want more fine-grained control of what methods are available, and what if the underlying code of this third-party interface changes? Now consider the following:
       ```java
       public class Sensors {
           private Map sensors = new HashMap();
           
           public Sensor getById(String id) {
               return (Sensor) sensors.get(id);
           }
       }
       ```
      In the above example the interface at the boundary (`Map`) is now hidden. The interface can be tailored and constrained to meet the needs of the application. The use of generics is no longer and issue because type management is isolated inside the `Sensors` class. Additionally, if the implementation of `Map` changes then the number of places to fix is limited since we aren't passing it all over the code. This may seem unlikely, but this exactly scenario became a real problem in Java 5 with the introduction of generics.
      
      Not every use of a `Map` needs to be isolated in this form. Rather, it is advised to not pass `Maps` (or any other interface at a boundary) around your system. If you use a boundary interface like `Map`, keep it inside the class, or close family of classes, where it is used. Avoid returning it from, or accepting it as an argument to, public APIs.
      
2. **Exploring and Learning Boundaries** <a name="boundaries-2"></a>
    - Instead of immediately implementing third-party APIs, only to be bogged down in debugging sessions trying to determine if the bugs are in our code our theirs, consider writing _learning tests_. These tests can help us better understand how a third-party library functions in the scenarios that we intend to use them.

4. **Learning (Boundary) Tests Are Better Than Free** <a name="boundaries-3"></a>
    - Learning tests verify that the third-party packages we are using work the way we expect them to. Additionally, if third-party libraries are changed in a way that makes them incompatible with our tests, we find out right away. This helps us manage code breaking changes introduced by third-party libraries and ease the pain of migration to newer versions. We had to learn how to use the library anyway, so we might as well write some tests while we're at it.

5. **Using Code That Does Not Yet Exist** <a name="boundaries-4"></a>
    - Sometimes APIs are poorly documented black boxes, or simply don't offer the interface we'd like. In situations like this, consider designing the interface you wish you had, then create an [_ADAPTER_](https://en.wikipedia.org/wiki/Adapter_pattern) to encapsulate the interaction with the API. This provides a single place to change when the API evolves. Additionally, we can create fake adapters to test our interface, and create boundary tests to verify the API is being used correctly.

6. **Clean Boundaries**
    - Code at boundaries need clear separation and tests that define expectations. Avoid letting too much of our code know about third-party particulars and instead depend on something you control. Management of third-party boundaries can be accomplished by having very few places that refer to them _(see [Using Third-Party Code](#boundaries-1) and [Using Code That Does Not Yet Exist](#boundaries-4))_.


## Unit Tests <a name="unittests"></a>
1. **The Three Laws of TDD** <a name="unittests-1"></a>
    - _**First Law:**_ You may not write production code until you have written a failing unit test.
    - _**Second Law:**_ You may not write more of a unit test than is sufficient to fail, and not compiling is failing.
    - _**Third Law:**_ You may not write more production code than is sufficient to pass the currently failing test.
    
    These laws lock you into a cycle that is perhaps 30 seconds long. Tests and production code are written together, with dozens of tests written every day.

2. **Keeping Tests Clean** <a name="unittests-2"></a>
    - Test quality must rival that of production code as it enables flexibility, maintanability, resuability, stability, etc. Dirty tests are equivalent to having no tests.  Tests must change as production code evolves. The dirtier the tests, the harder they are to change, and the more likely you are to spent more time cramming new tests into the suite than it takes to write new production code. As test quality degrades, the risk of defects increases. 

3. **Clean Tests** <a name="unittests-3"></a>
    - Clean tests are defined by readability (consisting of clarity, simplicity, and density of expression). Say a lot with as little as possible. Leverage the [_BUILD-OPERATE-CHECK_](http://fitnesse.org/FitNesse.FullReferenceGuide.UserGuide.WritingAcceptanceTests.AcceptanceTestPatterns.BuildOperateCheck) pattern to divide tests into three equals parts. The first part builds the data, the second part operates on that test data, and the third part checks that the operation yielded the expected results. 
        
        - _**Domain-Specific Testing Language:**_ As you continue to refactor test code to maintain readibility, rather than using the APIs that programmers use to manipulate the system, aim to build up a set of functions and utilities that make use of those APIs. Those functions and utilities becomes a testing languages that programmers can use to help write tests and those that read them later on.
        
        - _**A Dual Standard:**_ Tests must be simple, succinct, and expressive, but they do NOT need to be as efficient as production code.

4. **One Assert/Concept per Test Rule** <a name="unittests-4"></a>
    - While a single assert is not always feasible, minimizing the number of asserts in a test helps to improve readability. Similarly, each test should have a single concept. Split up large tests into smaller tests using [_GIVEN-WHEN-THEN_](https://en.wikipedia.org/wiki/Given-When-Then) convention. Consider minimizing duplicate code by using the [_TEMPLATE METHOD_](https://en.wikipedia.org/wiki/Template_method_pattern) pattern and putting the _given/when_ parts in the base class, and the _then_ parts in different derivatives. Alternatively we could create a completely separate test class and put the _given_ and _when_ parts in the `@Before` function, and the _when_ parts in each `@Test` function.

5. **F.I.R.S.T** <a name="unittests-5"></a>
    - _**Fast:**_ Tests should run fast. Nobody wants to run slow tests. Still, they don't need to be as efficient as performance code _(see [Clean Tests](#unittests-3))_.
    
    - _**Independent:**_ Tests should not depend on each other. You should be able to run them independently and in any order.
    
    - _**Repeatable:**_ Tests should be repeatable in any environment.
    
    - _**Self-Validating:**_ Tests should have a boolean output. Either the pass or fail.
    
    - _**Timely:**_ Tests need to be written in a timely fashion. Unit tests should be written _just before_ production code that makes them pass. If you write tests after the production code, then you may find the production code hard to test. Worse, you may find yourself writing tests to fit the production code, rather than production code that satisfies the tests.


## Classes <a name="classes"></a>
1. **Class Organization**
    - Class organization occur such that the class begins with public static constants, followed by private static variables, then private instance variables. Below variables should be public functions with each private utilities called by a public function following after the public function itself. This follows the stepdown rule and helps the program read like a newspaper article.
    - _**Encapsulation:**_ Look to maintain privacy, then loosen encapsulation as a last resort. For testing purposes, make things in the protected or package scope as necessary.
3. **Classes Should Be Small**
    - _**The Single Responsibility Principle:**_ A class or module should have one, and only one, reason to change. In other words, one responsibility.
    - _**Cohesion:**_ Classes should have a small number of instance variables, with each method manipulating one or more of those variables.
5. **Organizing for Change**
    - Structure systems so that we mess with as little as possible when we update them with new or changed features. Ideally we incorporate new features by extending the system, not by making modifications to existing code. For example, break large classes up in to smaller classes that extend a parent class. This follows the [_Open/Closed Principle_](https://springframework.guru/principles-of-object-oriented-design/open-closed-principle/).
    - _**Isolating from Change:**_ Decouple classes by making them depend on abstractions rather than concrete details. This principle is know as the [_Dependency Inversion Principle (DIP)_](https://en.wikipedia.org/wiki/Dependency_inversion_principle). In the following example, the functionality for retrieving a stock price is abstracted away into the `StockExchange` interface and provided as a dependency to the `Portfolio` class.
        ```java
        public interface StockExchange {
            Money currentPrice(String symbol);
        }

        public Portfolio {
            private StockExchange exchange;

            public Portfolio(StockExchange exchange) {
                this.exchange = exchange;
            }
            // ...
        }
        ```
        This also means we can easily test this by stubbing the `StockExchange` functionality.
        ```java
        public class PortfolioTest {
            private FixedStockExchangeStub exchange;
            private Portfolio portfolio;

            @Before
            protected void setUp() throws Exception {
                exchange = new FixedStockExchangeStub();
                exchange.fix("MSFT", 100);
                portfolio = new Portfolio(exchange);
            }

            @Test
            public void GivenFiveMSFTTotalShouldBe500() throws Exception {
                portfolio.add(5, "MSFT")
                Assert.assertEquals(500, portfolio.value());
            }
        }
        ```
