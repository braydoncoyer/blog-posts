# Effective Use of beforeEach and afterEach in Angular Unit Tests

As an Angular developer, you may be familiar with the importance of writing unit tests to ensure the reliability and correctness of your code. However, did you know that the lifecycle methods `beforeEach` and `afterEach` can be extremely useful when writing unit tests in Angular? In this article, we will discuss the purpose and benefits of using these methods in your unit tests.

## **What are the beforeEach and afterEach methods?**

The `beforeEach` and `afterEach` methods are part of the `jasmine` testing framework, which is commonly used in Angular for writing unit tests. These methods allow you to specify code that should be run before or after each unit test in a test suite.

## **Why are beforeEach and afterEach important in Angular unit tests?**

There are several reasons why using the `beforeEach` and `afterEach` methods can be beneficial when writing unit tests in Angular.

First, using these methods can help you ensure that each unit test is running in a consistent and predictable environment. By setting up test environment variables or resetting the application state before each test, you can avoid issues caused by tests relying on the state of previous tests or unexpected changes in the environment.

Second, using the `beforeEach` and `afterEach` methods can help you keep your unit tests organized and easy to understand. By abstracting out setup and teardown logic into these methods, you can keep your unit tests focused on testing specific functionality, rather than cluttered with extraneous setup and teardown code.

Finally, because the redundant setup and teardown code was moved into those lifecycle hooks, test performance improves for your testing suites.

## **How do I use the beforeEach and afterEach methods in my Angular unit tests?**

To use the `beforeEach` and `afterEach` methods in your Angular unit tests, you will need to import the `TestBed` utility from the `@angular/core/testing` module and call the `TestBed.configureTestingModule()` method to set up your test environment. You can then use the `beforeEach` and `afterEach` methods within the `TestBed.configureTestingModule()` method to specify code that should be run before or after each test.

Here's an example of how you might use the `beforeEach` and `afterEach` methods in an Angular unit test:

```typescript
import { TestBed } from '@angular/core/testing';

describe('MyComponent', () => {
  let component: MyComponent;
  let fixture: ComponentFixture<MyComponent>;

  beforeEach(() => {
    TestBed.configureTestingModule({
      declarations: [ MyComponent ]
    });

    fixture = TestBed.createComponent(MyComponent);
    component = fixture.componentInstance;
  });

  afterEach(() => {
    component = null;
    fixture = null;
  });

  it('should do something', () => {
    // test code goes here
  });
});
```

In the example above, the `beforeEach` method is used to configure the test environment and create an instance of the `MyComponent` component. The `afterEach` method is then used to reset the `component` and `fixture` variables to `null` after each test. This ensures that each unit test is running in a clean and consistent environment.

While it is the convention to use a `beforeEach` method to create a component instance, it’s important to note that you can use a `beforeEach` lifecycle method with each `describe` block you use in your test file.

## **Common Use Cases for beforeEach and afterEach**

There are many different use cases for the `beforeEach` and `afterEach` methods when writing unit tests in Angular.

Some common examples include, but are not limited to:

*   Mocking out external services
    
*   Resetting application state
    

### **Mocking Out External Services**

One common use case for the `beforeEach` and `afterEach` methods is mocking out external services that are used by the component or service being tested. This can be useful for isolating the unit under test (also called the code under test) and preventing it from making actual network requests or interacting with real database systems.

To mock out an external service, you can use the `TestBed.overrideProvider()` method within the `beforeEach` block to replace the real service with a mock or spy. Here's an example of how you might do this:

```typescript
import { TestBed } from '@angular/core/testing';
import { HttpClient } from '@angular/common/http';

describe('MyComponent', () => {
  ...
  let httpClientSpy: jasmine.SpyObj<HttpClient>;

  beforeEach(() => {
    const spy = jasmine.createSpyObj('HttpClient', ['get']);

    TestBed.configureTestingModule({
      declarations: [ MyComponent ],
      providers: [
        { provide: HttpClient, useValue: spy }
      ]
    });

    fixture = TestBed.createComponent(MyComponent);
    component = fixture.componentInstance;
    httpClientSpy = TestBed.inject(HttpClient) as jasmine.SpyObj<HttpClient>;
  });

  afterEach(() => {
    component = null;
    fixture = null;
  });

  it('should make a GET request to the server', () => {
    httpClientSpy.get.and.returnValue(of({ data: 'test data' }));

    component.makeRequest();

    expect(httpClientSpy.get).toHaveBeenCalled();
  });
});
```

In the example above, the `beforeEach` block uses the `TestBed.overrideProvider()` method to replace the `HttpClient` service with a spy object. The spy object is then injected into the component and used to verify that the component is making the expected network request in the `it` block.

&lt;aside&gt; 📣 Interested in learning how to write unit tests for an HTTP Service? I have another article [here that covers the topic in-depth](https://braydoncoyer.dev/blog/how-to-unit-test-an-http-service-in-angular)!

&lt;/aside&gt;

### **Resetting the Application State**

Another common use case for the `beforeEach` and `afterEach` methods is resetting the state of the application before and after each test. This can be useful for ensuring that each unit test is running in a clean environment and is not affected by the state of previous tests.

To reset the application state, you can use the `beforeEach` method to dispatch actions that reset the state of the application, or to set up test data in the store or database.

Here's an example of how you might do this using NgRx for state management:

```typescript
import { TestBed } from '@angular/core/testing';
import { provideMockStore } from '@ngrx/store/testing';

describe('MyComponent', () => {
	let component: MyComponent;
	let fixture: ComponentFixture<MyComponent>;
	let store: MockStore;

	beforeEach(() => {
		TestBed.configureTestingModule({
		declarations: [ MyComponent ],
		providers: [
			provideMockStore({ initialState: { user: {} } })
		]
	});

	fixture = TestBed.createComponent(MyComponent);
	component = fixture.componentInstance;
	store = TestBed.inject(MockStore);
});

afterEach(() => {
	component = null;
	fixture = null;
});

it('should display the user name if the user is logged in', () => {
	store.setState({ user: { name: 'John Doe' } });
	fixture.detectChanges();
	
	const userNameElement = fixture.debugElement.query(By.css('.user-name')).nativeElement;
	expect(userNameElement.textContent).toEqual('John Doe');
});

it('should not display the user name if the user is not logged in', () => {
	store.setState({ user: {} });
	fixture.detectChanges();
	const userNameElement = fixture.debugElement.query(By.css('.user-name')).nativeElement;
	expect(userNameElement.textContent).not.toEqual('John Doe');
});

it('should not display the user name if the user is not logged in', () => {
	store.setState({ user: {} });
	fixture.detectChanges();
	
	const userNameElement = fixture.debugElement.query(By.css('.user-name'));
	expect(userNameElement).toBeNull();
});
});
```

In the example above, the `beforeEach` block sets up the test environment and injects a mock store into the component. The `afterEach` block then resets the `component` and `fixture` variables to `null`.

In the `it` blocks (the individual unit tests), the store's state is reset before each test case using the `store.setState()` method, which ensures that each test is running in a clean and consistent environment.

### **Setting Up Test Data**

Another common use case for the `beforeEach` and `afterEach` methods is setting up test data that is used by the unit under test. This can be useful for abstracting out data setup logic and keeping your unit tests focused on testing specific functionality.

To set up test data, you can use the `beforeEach` method to create and configure test objects or data structures that are used in the unit tests. Here's an example of how you might do this:

```typescript
import { TestBed } from '@angular/core/testing';

describe('MyComponent', () => {
  ...
  let testData: TestData[];

  beforeEach(() => {
    TestBed.configureTestingModule({
      declarations: [ MyComponent ]
    });

    fixture = TestBed.createComponent(MyComponent);
    component = fixture.componentInstance;

    testData = [
      { id: 1, name: 'Test Item 1' },
      { id: 2, name: 'Test Item 2' },
      { id: 3, name: 'Test Item 3' }
    ];
  });

  afterEach(() => {
    component = null;
    fixture = null;
  });

  it('should display the correct number of items', () => {
    component.items = testData;
    fixture.detectChanges();

    const itemElements = fixture.debugElement.queryAll(By.css('.item'));
    expect(itemElements.length).toEqual(testData.length);
  });
});

```

In the example above, the `beforeEach` block sets up the test environment and creates an array of test data called `testData`.

The `testData` array is then used in the `it` block to verify that the component is displaying the correct number of items.

## **Conclusion**

In conclusion, the `beforeEach` and `afterEach` methods are important tools to use when writing unit tests in Angular. These methods can help you ensure that each unit test is running in a consistent and predictable environment, keep your unit tests organized and easy to understand and improve the performance of your unit tests.

By utilizing the `beforeEach` and `afterEach` methods in your Angular unit tests, you can improve the reliability and maintainability of your code.

## **Additional Resources**

*   [**Angular TestBed documentation**](https://angular.io/api/core/testing/TestBed)
    
*   [**Jasmine**](https://jasmine.github.io/api/3.6/global.html#beforeEach) `beforeEach` and `afterEach` documentation