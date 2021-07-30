# Clean Code Notes
1. [Clean Code]()
2. [Meaningful Names]()
3. [Functions]()
4. [Comments]()
5. [Formatting]()
6. [Objects and Data Structures]()
7. [Error Handling](#error-handling)
8. [Boundaries](#boundaries)

## Clean Code <a name="clean-code"></a>

## Meaningful Names<a name="meaningful-names"></a>

## Functions <a name="functions"></a>

## Comments <a name="comments"></a>

## Formatting <a name="formatting"></a>

## Objects and Data Structures <a name="objects-and-data-structures"></a>

## Error Handling <a name="error-handling"></a>
1. **Use Exceptions Rather Than Return Codes**
    - Error flags or returned error codes must be checked by the caller, cluttering code and obscuring function logic. It's cleaner to throw an exception when encountering and error.
2. **Write Your _Try-Catch-Finally_ Statement First**
    - Starting with a _try-catch-finally_ helps you define what the user of that code should expect, no matter what goes wrong with the code that is executed in the _try_. We can then proceed to build the rest of the logic that we need, and can pretend that nothing goes wrong. Try to write tests that force exceptions, and then add behavior to your handler to satisfy your tests.
3. **Use Unchecked Exceptions**
    - The price of checked exceptions is an [Open/Closed Principle](https://springframework.guru/principles-of-object-oriented-design/open-closed-principle/) violation. If you throw a checked exception fro a method in your code and the _catch_ is three levels above, you must declare that exception in the signature of each method between you and the _catch_. This means any changes at a a low level can force cascading signature changes up to the high level. In general, checked exceptions aren't worth it.
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
12. Don't Pass Null
    - Unless working with an API which expects you to pass _null_, you should avoid passing _null_ whenever possible. In most languages there is no good way to deal with a _null_ that is passed by a caller accidentally, thus it makes sense to forbid passing _null_ by default [(See Lombok's @NonNull)](https://projectlombok.org/features/NonNull). Taking this approach allows allows you to assume that a _null_ in an argument list is an indication of a problem.

## Boundaries <a name="boundaries"></a>
1. **Using Third-Party Code** <a name="boundaries-1"></a>
    - Consider the following code:
        ```
        Map sensors = new HashMap();
        Sensor s = (Sensor)sensors.get(sensorId);
        ```
       In this scenario, the client carries the responsibility of getting an object from the map an casting it to the correct type. This code fails effectively tell it's whole story and can be cleaned up with the use of generics.
       ```
       Map<Sensor> sensors = new HashMap<>();
       Sensor s = sensors.get(sensorId);
       ```
       Additionally, `Map` is a broad interface containing a number of default methods such as `clear()`. If we begin passing this Map around, suddenly any method that gets a hold of this object has the ability to start deleting items from it, even if we don't necessarily need/want them to. In this example we might restrict editting by using immutable or unmodifiable maps via `Map.of()` or `ImmutableMap.of()`. But what if we want more fine-grained control of what methods are available, and what if the underlying code of this third-party interface changes? Now consider the following:
       ```
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
    - Sometimes APIs are poorly documented black boxes, or simply don't offer the interface we'd like. In situations like this, consider designing the interface you wish you had, then create an [ADAPTER](https://en.wikipedia.org/wiki/Adapter_pattern) to encapsulate the interaction with the API. This provides a single place to change when the API evolves. Additionally, we can create fake adapters to test our interface, and create boundary tests to verify the API is being used correctly.

6. **Clean Boundaries**
    - Code at boundaries need clear separation and tests that define expectations. Avoid letting too much of our code know about third-party particulars and instead depend on something you control. Management of third-party boundaries can be accomplished by having very few places that refer to them (see [Using Third-Party Code](#boundaries-1) and [Using Code That Does Not Yet Exist](#boundaries-4)).
