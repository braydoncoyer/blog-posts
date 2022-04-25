## What Makes a Unit Test Valuable?

The goal of unit testing is to ensure that your application behaves as expected. However, many unit tests miss the mark, neglecting to provide value to the business and failing to discover defects in the codebase. 

In this article, we’ll discover how to craft unit tests that are valuable and catch defects quickly. But first, let’s look at a definition of a unit test.

## What is a unit test?

A unit test is an automated piece of code that invokes a unit of work (a separate piece of code) in the application. The test passes or fails based on an assumption about the behavior of that unit of work (we call this the ***code under test***). A unit test is almost always written using a unit testing framework, allowing tests to be written efficiently and run quickly.

If you're just getting started with unit testing, one of the most important steps you can take is understanding the value that unit testing provides, and making sure that you have enough tests in place to keep your application stable. Whether you're writing a single component test or a suite of unit tests for an entire module, it's important for your code to be tested thoroughly and in isolation.

 


> 📢 If you want to learn more about testing in isolation, [I wrote another article](https://braydoncoyer.dev/blog/mocking-components-in-angular#testing-in-isolation) that may prove helpful.


Unit tests are most valuable when they are easy to read, test a single outcome, share the same structure, aren’t concerned about underlying logic, and all unit tests are repeatable. 

## Unit Tests Should be Easy to Read (Unit Testing Naming Conventions)

The naming convention of your unit tests is also important because it can help make the test’s purpose clear to new developers (this is especially useful for large teams where there are many developers contributing to a codebase). Furthermore, the more specific the test name, the easier it is to identify what went wrong when a test fails.

For example, imagine you have a function that uses geolocation services to return the longitude and latitude of the user’s current location. Instead of creating a test and calling it `getCurrentLocation()` or `getLocation()`, be a little more verbose and name the method `returnsLongAndLatOfUser()`. This name describes what the test is actually doing: checking to see if the function returns the expected location of the user.

Some testing frameworks have their own convention for naming your unit tests. Test functions in Jasmine, an open-source frontend testing framework, are declared as `it()`, and accept a parameter for the name of the test. This provides a suite of tests that can be read in plain English.

```jsx
it("returns the longitude and latitude of the current user", () => {
	// Test code here
})
```


> 💡 This test could, arguably, be split into two separate tests - one that checks that the function returns the *longitude* and one that checks that the function returns the *latitude*.


If this test were to fail, it would be very easy to understand what’s going wrong in your application. 

## Unit Tests Should Only Test One Outcome

This is a common mistake I see in many test suites. It can be tempting to write a catch-all test scenario where you call an implementation function and make multiple expectations in a single unit test. Tests that have multiple assertions and fail make it difficult to understand why the test is failing. And as you can expect, testing multiple outcomes in a single unit test usually have vague test names. 

Valuable Unit Tests should be granular and only make a single assertion. 

## Unit Tests Should Share the Same Structure

Ultimately, how you structure your unit tests is up to you and the team you are working with. Take the time to research patterns and conventions put into place by larger, more seasoned teams. Work with what makes sense for your team, and always keep an eye out for improvements that could be made.

## Unit Tests Shouldn’t be Concerned About the Underlying Logic

You must be very careful when writing unit tests to ensure that you do not couple the test to the implementation logic. If your test fails due to a change in implementation while the behavior remains the application remains the same, it can be an indicator that you may need to re-examine your test.

Valuable tests should only be concerned with the expected behavior of a class or method, not how that behavior is implemented.

## Unit Tests Should be Thorough

Thorough is a tricky word and can mean different things to different people. What's important is that you take the time to consider your code through different lenses and discover paths that may have been missed (if statements determine different logical routes; ensure your tests check each scenario!).

## A Unit Test Should be Repeatable

Finally, valuable unit tests are repeatable. If you run the same test ten times in a row, you should expect to get the same result each time.

If your unit tests fail this simple check, then something may be wrong with how your test is set up. 

## Conclusion

In this article, we looked at the characteristics of valuable unit tests. These points aid in the correctness of the code under test which saves time for the developers and saves the business a significant amount of money that would otherwise be spent on hours of debugging. Unit testing provides implicit code documentation and can be a great way for new developers to understand what’s happening in the codebase. Unit tests are extremely valuable - make sure you take the time to craft valuable tests.