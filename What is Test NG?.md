# What is Test NG?

<aside>
💡 TestNG (Test Next Generation) is a testing framework designed for the Java programming language to simplify the testing process and make it more powerful.

</aside>

It is inspired by JUnit and NUnit but introduces some new functionalities that make it more flexible and easier to use. 

TestNG is widely used for test automation in Java applications, particularly in the field of unit testing, integration testing, and end-to-end testing.

Key features of the TestNG framework include:

1. **Annotations:** TestNG uses annotations to define the configuration and test methods. Annotations provide metadata about the code, helping in easy configuration and execution of tests. Common annotations include `@Test`, `@BeforeTest`, `@AfterTest`, `@BeforeMethod`, `@AfterMethod`, and more.

- **@Test:** Indicates that a method is a test method. TestNG runs this method and reports the results.
    
    ```java
    javaCopy code
    @Test
    public void testExample() {
        // Test code goes here
    }
    
    ```
    
- **@BeforeTest and @AfterTest:** Specify actions to be taken before a test suite starts or after it finishes.
    
    ```java
    javaCopy code
    @BeforeTest
    public void setUp() {
        // Pre-test preparations
    }
    
    @AfterTest
    public void tearDown() {
        // Post-test cleanup
    
    ```
    
- **@BeforeMethod and @AfterMethod:** Specify actions to be taken before each test method starts or after it finishes.
    
    ```java
    javaCopy code
    @BeforeMethod
    public void beforeTestMethod() {
        // Preparations before each test method
    }
    
    @AfterMethod
    public void afterTestMethod() {
        // Cleanup after each test method
    
    ```
    

These annotations allow you to control the order of execution of test methods, and perform setup and cleanup operations. They provide a powerful way to organize and customize your tests.

To better understand what each annotation does, you can refer to the [TestNG Documentation](https://testng.org/doc/documentation-main.html).

---

2. **Test Configuration:** TestNG allows the configuration of tests through XML files, where you can define test suites, parallel execution, dependencies between test methods, and more. This makes it easy to manage and customize test execution.
    
    ### Sample TestNG XML File:
    

```xml
xmlCopy code
<!DOCTYPE suite SYSTEM "http://testng.org/testng-1.0.dtd">
<suite name="TestSuite">
    <test name="LoginTests">
        <classes>
            <class name="com.example.tests.LoginTest"/>
            <!-- Other test classes -->
        </classes>
    </test>

    <test name="HomePageTests">
        <classes>
            <class name="com.example.tests.HomePageTest"/>
            <!-- Other test classes -->
        </classes>
    </test>

    <!-- Parallel execution and other configuration options -->
    <parallel>methods</parallel>
    <thread-count>5</thread-count>
</suite>

```

In the above XML file:

- The **`<suite>`** element represents a test suite.
- **`<test>`** elements represent different test classes or groups.
- **`<classes>`** elements specify the test methods within a test class.
- **`<parallel>`** and **`<thread-count>`** are configuration options for parallel execution.

This XML file serves as an example for configuring test packages, suites, parallel execution, and other configuration details. This type of configuration allows you to organize and manage your tests more effectively.

---

3. **Parallel Execution:** TestNG supports parallel test execution, allowing tests to run concurrently, which can significantly reduce the overall test execution time. This is beneficial when dealing with a large number of test cases.
    
    ### Example:
    
    ```java
    javaCopy code
    import org.testng.annotations.Test;
    
    public class ParallelExecutionTest {
    
        @Test
        public void testMethod1() {
            System.out.println("Test Method 1 - Thread ID: " + Thread.currentThread().getId());
        }
    
        @Test
        public void testMethod2() {
            System.out.println("Test Method 2 - Thread ID: " + Thread.currentThread().getId());
        }
    
        @Test
        public void testMethod3() {
            System.out.println("Test Method 3 - Thread ID: " + Thread.currentThread().getId());
        }
    }
    
    ```
    
    In the above example, there are three separate test methods marked with the **`@Test`** annotation. These test methods will run concurrently in different threads. We can control this behavior through a TestNG XML file:
    
    ### TestNG XML File:
    
    ```xml
    xmlCopy code
    <!DOCTYPE suite SYSTEM "http://testng.org/testng-1.0.dtd">
    <suite name="ParallelSuite" parallel="methods" thread-count="3">
    
      <test name="ParallelTest">
        <classes>
          <class name="ParallelExecutionTest"/>
        </classes>
      </test>
    
    </suite>
    
    ```
    
    In this XML file, the **`parallel`** attribute is set to "methods," and the **`thread-count`** specifies how many threads will be used.
    
    ### Output:
    
    ```mathematica
    mathematicaCopy code
    Test Method 1 - Thread ID: 1
    Test Method 2 - Thread ID: 2
    Test Method 3 - Thread ID: 3
    
    ```
    
    This output demonstrates that each test method is running in a different thread. Parallel execution can be beneficial in large test suites or when acceleration of the testing process is required.
    
    ---
    
4. **Dependency Management:** TestNG allows the definition of dependencies between test methods, ensuring that certain methods are executed in a specific order. This is useful for scenarios where one test method depends on the success of another.

Dependencies are expressed using the **`dependsOnMethods`** or **`dependsOnGroups`** attributes. 

For example:

```java
javaCopy code
@Test
public void loginTest() {
    // Login operations
}

@Test(dependsOnMethods = "loginTest")
public void dashboardTest() {
    // Dashboard operations
}

```

In the above example, the **`dashboardTest`** method will be executed only after the **`loginTest`** method has completed successfully. This is useful for establishing a specific order for the execution of test methods and managing dependencies.

Below is a representation of this dependency:

```diff
diffCopy code
+-------------------+
| loginTest         |
+-------------------+
          |
          v
+-------------------+
| dashboardTest     |
+-------------------+

```

This visual representation illustrates that the **`dashboardTest`** method will be executed after the successful completion of the **`loginTest`** method.

Dependencies can help you better organize your test processes by ensuring that tests are executed in a specific and logical order.

---

5. **Data-Driven Testing:** TestNG supports data-driven testing, allowing the same test method to be executed with different sets of data. This is achieved through the use of data providers.
    
    To perform data-driven testing with TestNG, the **`@DataProvider`** annotation is commonly used. This allows running the same test method with different sets of data.
    
    For example, let's consider a scenario of validating username and password on a login page:
    
    5.1. **Test Method:**
    
    ```java
    javaCopy code
    import org.testng.Assert;
    import org.testng.annotations.Test;
    
    public class LoginTest {
    
        @Test(dataProvider = "loginData", dataProviderClass = TestData.class)
        public void loginTest(String username, String password) {
            // Performing login with the provided username and password
            boolean result = performLogin(username, password);
    
            // Checking if the login was successful
            Assert.assertTrue(result, "Login failed: " + username + " - " + password);
        }
    
        // Other methods and operations...
    }
    
    ```
    
    5.2. **Data Provider Class:**
    
    ```java
    javaCopy code
    import org.testng.annotations.DataProvider;
    
    public class TestData {
    
        @DataProvider(name = "loginData")
        public static Object[][] loginDataProvider() {
            return new Object[][] {
                {"user1", "pass1"},
                {"user2", "pass2"},
                {"user3", "pass3"}
                // Additional sets of data can be added
            };
        }
    }
    
    ```
    
    In this example, the **`loginTest`** test method will work with different combinations of usernames and passwords provided by the **`loginDataProvider`** method in the **`TestData`** class, which is annotated with **`@DataProvider`**. This allows testing the same scenario with various sets of data.
    
    ---
    
6. **Reporting:** TestNG generates detailed and user-friendly HTML reports, making it easy to analyze test results. These reports include information about passed and failed tests, execution time, and more.

---

7. **Integration with Other Tools:** TestNG can be integrated with various build tools such as Maven and Gradle. It is also commonly used in conjunction with Selenium for web application testing.

---

Overall, TestNG offers a robust and feature-rich framework for Java testing community to write and execute tests efficiently. It has become a popular choice in the Java testing community due to its flexibility, ease of use, and comprehensive features.
