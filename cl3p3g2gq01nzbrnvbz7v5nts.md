## Learn How to Click a Button when Angular Unit Testing

Buttons play a large part in the user experience of your user interface. Angular makes working with buttons extremely easy, but perhaps you’ve hit a wall when your mindset switches to testing. Should you have unit test cases for button clicks in your Angular application? Is it really that important? And if so, how would you go about testing that scenario? 

**There are two prominent approaches when it comes to writing unit tests for button clicks in Angular: either you search the DOM for the button, perform an actual click and verify the expected behavior, or you simply call the component-code that will run when the button is clicked. Both options have their pros and cons. In this article, we’ll investigate each testing route thoroughly and look at various examples so that you understand everything you need to know about how to write unit tests for button clicks in Angular.** 

## Why and when should you unit test button clicks in Angular?

If you have some experience with automated testing, it wouldn’t be surprising if you’re wondering if a button click is something that even needs to be handled with a unit test in Angular. Perhaps in the past you’ve opted to forgo a unit test and defer that responsibility to an E2E (End-to-End) test. There isn’t anything wrong with that decision - E2E tests validate functionality by performing tests from a user’s experience by simulating real user scenarios in the application. 

A unit test, on the other hand, is a bit more granular. It’s an automated piece of code that invokes a unit of work (a separate piece of code) in the application and is usually approached from a black-box perspective. The test passes or fails based on an assumption or expectation about the behavior of that unit of work.


> 🤔 If you’re interested in learning more about writing well-structured and valuable tests in Angular, [check out my other article](https://braydoncoyer.dev/blog/what-makes-a-unit-test-valuable).


A unit test is almost always written using a testing framework, allowing it to be written efficiently and run quickly. If you generate a new Angular project with the Angular CLI, your application comes with Jasmine and Karma (the testing framework and runner) out of the box.

> 📢 This article assumes that you’re using Jasmine and Karma for your unit testing. However, if you’re using another testing framework like Jest, the approach will be almost identical.


### Angular Button testing: Data Validation or Application Functionality

There isn’t a set rule on if buttons should be covered by a unit test. In fact, the decision of whether to write a unit test for a button click ultimately comes down to personal opinion. If you prefer to defer that functionality to an E2E test, that’s great! But in my opinion, there are certain situations where a button click unit test provides valuable reassurance in an Angular application.

Consider the classic calculator example which contains many buttons that perform various mathematical operations. Each time a button is clicked, data is manipulated and a new number or sum is displayed on the screen. This is a perfect scenario for a unit test! Data is changing with each button click; the calculator produces a ***certain output*** when given a ***certain input***. 

On the other hand, it’s not uncommon for a button to navigate the user to a different page, or to make something else appear or disappear. Rather than solely changing data, these scenarios represent application ***functionality*** and is a great opportunity to write an E2E test.

With this in mind, does your situation call for a unit test or would it be best to create an E2E test?

Recall that there are generally two approaches to writing a unit test for buttons: either you locate the button on the DOM and simulate a click, or you test against the code that will be run when the button is clicked by a user. Let’s look at the more complex example first.

## How to Test a Button Click in Angular

This approach may be useful in some situations, but the act of delegating a unit test to browse the DOM to find the button and perform a click is a point of contention. The unit test still makes expectations around the notion of what’s supposed to happen when the button is clicked, but many argue that performing the click is the responsibility of an E2E test.

Regardless, locating the button on the DOM is a trivial task, especially when you isolate the button into a reusable component. The following is an example of just that - a reusable isolated button component that, as mentioned prior, assumes you have TestBed configured properly with Jasmine and Karma.

```tsx
describe('Component: Button', () => {
  let fixture: ComponentFixture<ButtonComponent>;
  let component: ButtonComponent;

  beforeEach(async(() => {
    TestBed.configureTestingModule({
      imports: [ ],
      declarations: [ ButtonComponent ],
      providers: [  ]
    }).compileComponents().then(() => {
      fixture = TestBed.createComponent(ButtonComponent);
      component = fixture.componentInstance;
    });
  }));
});

it('should call onButtonClick when clicked', fakeAsync(() => {
  spyOn(component, 'onButtonClick');

  let button = fixture.debugElement.nativeElement.querySelector('button');
  button.click();

  tick();

  expect(component.onButtonClick).toHaveBeenCalled();
}));
```

The TypeScript file for this button component has a function called `onButtonClick` that is bound to the `button` element in the template. This test first spies on the local function, locates the button and then performs a click. After a simulated passage of time with `tick()`, we make an assertion that the `onButtonClick` function was called.


>📣 Events can be tested using `async` / `fakeAsync` function imported from `'@angular/core/testing’`.


Notice that the button was located on the DOM using `querySelector` and passing `button` as an argument. This works fine in an isolated component like this, but in different scenarios where multiple `button` elements may exist, you need to use something that provides more specificity. 

This example is pretty straightforward - we simply verify that the function is called when the button is clicked. But we can take this further. Let’s look at the `onButtonClick` function and see what else can be tested.

```tsx
@Output() buttonClicked: EventEmitter<any> = new EventEmitter<any>();

...

onButtonClick(): void {
  this.buttonClicked.emit();
}
```

Since this is a reusable button component, it makes sense to delegate the responsibility of functionality to whichever component consumes it. In order for the parent component to identify when the button has been clicked, it can listen to an event emitter inside the button component (`buttonClicked`). In response for the event being emitted, the parent component calls a local function to, for example, perform a mathematical operation in the calculator example above. 

From a testing perspective, it would provide value to have a unit test that ensures that the event is emitted when the `onButtonClick` function is called. Here’s what that may look like.

```tsx
it('should emit the event when #onButtonClicked is called', () => {
  const emitSpy = spyOn(component.buttonClickEvent, 'emit');

  component.onButtonClick();

  expect(emitSpy).toHaveBeenCalled();
});
```

Spies come to the rescue here again. The `emit` function lives on the `buttonClickEvent` object, and the test simply verifies that the spy was called when the code under test is executed.

### What about other situations?

There may be other situations in a reusable button component where unit tests could prove useful and provide reassurance that it’ll continue to work in the future with additional changes. We won’t be discussion or covering those scenarios in this article, though.

## How to Test Button Functionality in Angular

Now that the reusable button component has a supporting test cases, let’s turn our attention to situations where it may prove beneficial to test local code that is wired up to that button component.

Recall that the reusable button component emits an event when clicked. Other parts of our application can listen to that event, and call a local function to perform isolated operations.

Continuing our calculator idea from earlier, here’s one example where we consume the reusable button component and listen to the `buttonClickEvent`.

```html
<app-button (buttonClickEvent)="add(5)">
  5
</app-button>
```

We already have unit tests that locate the button on the DOM and initiate a click event, so there’s no need to test that here in the parent component. Instead, let’s look directly at the `add` function and see if there’s anything inside worth testing.

```tsx
add(toAdd: number) {
  this.total += toAdd;
}
```

This is a very basic and straightforward example specifically for this article. This function mutates data, and if you recall from earlier, this is a great opportunity to add supporting test cases.

But what do you test? 

For the `add` function, we would write a test that ensures the `total` class variable increments with the appropriate value passed to the function. This example is pretty simple, but the skill of determining what to test is something that comes with practice.


> 📣 If you’re wanting to develop the skill of identifying test cases, consider reading my article titled [The Gumball Machine - How To Quickly Identify Unit Test Cases](https://braydoncoyer.dev/blog/the-gumball-machine-how-to-quickly-identify-unit-test-cases)


Here’s what the test would look like. Again this assumes you have the test suite set up correctly with TestBed.

```tsx
it('should add 5 to the calculator total', () => {
  const expectedTotal: number = 10;
  component.total = 5;

  component.add(5);

  expect(component.total).toEqual(expectedTotal);
});
```

Notice that we call the `add` function directly in the parent component test. Remember, we already have assurance that the button works as intended when clicked, so in this case we simply call the code under test.

> ⚠️ It’s never good to trust that a test is working as intended without seeing it fail. I recommend making an intentional error in the `add` function (not the test) to see if the test fails when it needs to fail.

## Conclusion

In this article, we investigated the different ways to test button clicks in Angular. One option is to write a unit test that locates the button on the DOM, perform a click and assert that something happened in the test. On the other hand, it may be appropriate to simply call function that is called when the button is clicked and write expectations based on what occurs in the code under test. 

Ultimately, it comes down to your personal preference. Whichever route you choose to take, I hope this article has proven helpful and shown you how to write unit tests for button clicks in Angular.