## 5 Basic Tips for Angular Unit Testing

Unit testing provides assurance that your application works as intended by running an automated piece of code that invokes a unit of work (a separate piece of code). The test passes or fails based on an assumption about the behavior of that unit of work (we call this the code under test).

While unit testing across frontend frameworks hold the same core principals, it isn’t surprising that unit testing in Angular holds some key differences. Unit testing is a skill that requires time and patience to develop. If you’re learning how to write unit tests in Angular, here are 5 basic tips to accelerate your learning: understand Angular dependencies, test in isolation, write granular tests, test logic rather than the DOM and write your tests before the implementation code.

## Understand Angular Dependencies and Modules
The first tip is to take time to understand how Angular handles dependencies. This tip is a prerequisite to the next tip; you’ll need to identify dependencies in order to properly test in isolation.

Angular’s Module architecture is a bit unique, and probably one of the hardest parts for beginners to pick up. Angular Modules are built on top of ES Modules - a solution to sharing code between files. A module, at its core, is simply a way to import and export code for other files to consume. There are differences in the way ES Modules and Angular Modules work, but the core idea remains the same.

Angular Modules list dependencies that other code (components, services, etc) can use. For example, in order to use and consume a reusable button component in your application, it needs to be registered in a corresponding Angular Module. If it’s not, the compiler will throw an error.
Why is this important? That brings us to the second tip.

## Test in Isolation
Testing in isolation means that the unit that is being tested should be separate from other parts of the application. What does this mean when we talk about unit testing in Angular? Whatever you're testing (whether that be a component, service, pipe, etc) should have all other dependencies separated/mocked. If you think it through, this makes sense. 

We don’t want to test the entire application, we only want to test a specific slice of it. That’s the whole point of unit testing!

If you don't test in isolation, you'll end up with hours of headache as you sift through ambiguous console errors trying to figure out why (and where!) your tests are failing.

As mentioned earlier, in order to test in isolation, you need to mock out dependencies. This is why it’s very important to understand how Angular handles dependencies. A dependency can be a component you’re consuming, a service that is injected, or a handful of other things.

Thankfully, mocking is very straightforward. If you want to learn how to mock an Angular component, [read my other article](https://braydoncoyer.dev/blog/mocking-components-in-angular). If you want to mock an Angular service, I wrote [another little article here](https://braydoncoyer.dev/blog/mocking-services-in-angular) to show you how.

## Write Granular Unit Tests
Thirdly, I recommend that you write small, independent unit test cases. It can be tempting to write a catch-all test scenario where you call an implementation function and make multiple expectations in a single unit test. Failing tests that have multiple assertions make it difficult to understand what went wrong.

Rather than falling into the catch-all single test case scenario, identify how a single unit can be split into multiple test cases (if the situation calls for it). For example, if a component function subscribes to a service and updates local component state with the result, you can easily create two or three test cases instead of a single bloated test.

For more information about what makes an Angular unit test valuable, [click here](https://braydoncoyer.dev/blog/what-makes-a-unit-test-valuable).

## Test logic, not the DOM
This tip can be a bit controversial. It’s possible to write unit tests that search the DOM for elements, perform an action (like a click), and assert that certain behavior was executed.
While I do think that some situations calls for this type of structure, it should not be the norm. If you find yourself writing a bunch of DOM queries in your tests, you may want to delegate those tasks to an End-to-End (E2E) test.

Consider the classic calculator example which contains many buttons that perform various mathematical operations. Each time a button is clicked, data is manipulated and a new number or sum is displayed on the screen. This is a perfect scenario for a unit test! Data is changing with each button click; the calculator produces a certain output when given a certain input.

On the other hand, it’s not uncommon for a button to navigate the user to a different page, or to make something else appear or disappear. Rather than solely changing data, these scenarios represent application functionality and is a great opportunity to write an E2E test.

## Test First, Code Second (Test Driven Development)
Finally, and perhaps most important, discipline yourself to write your unit test cases first before you write the component or service logic. Does that sound odd? It’s okay if it does - it’s a bit backwards in a sense.

Writing test cases first is called Test Driven Development (TDD). Rather than the implementation code influencing how the unit test is written, TDD allows the test scenario to drive the implementation of the code. Because of this, code written in a TDD pattern generally is cleaner and less bloated.

Test Driven Development has a few rules and conventions that go along with it. If you’re want to learn more about TDD, [BrowserStack has a detailed explanation](https://www.browserstack.com/guide/what-is-test-driven-development).

Remember, unit testing in this fashion takes time to learn; it’s a skill that you must develop. I encourage you to start small and experience the benefits that TDD provides.

## Conclusion
In this article, we looked at five general tips for unit testing in Angular. If you’re starting to learn how to test in Angular and feel overwhelmed, remember that unit testing is a skill that takes time to develop.

My hope is that by understanding Angular dependencies, testing in isolation, writing granular test cases, testing the underlying logic rather than the DOM, and giving Test Driven Development a try, you’ll have a better toolbox to successfully accelerate your learning and have the skills needed to write tests that provide assurance that your code behaves as expected.