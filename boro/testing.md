# Testing with React Native



# Storybook

Consider the pros/cons
* This also reminds me of considering the pros/cons of using
  
## Pros
1. You can see the components in a website or for react-native-web. I think the best part of this is just showing it to other team members or company. Mainly to get feedback on design or flow.
   1. Perhaps they think *"Hey I think the UI could be made better like this or the UX is kinda janky with this"*
   2. Ownership of the app: When the entire company gets to see the development visually, it gives them a sense of ownership. I might not be directly involved in the app development but my contribution to the company allows this app to be created.
2. Improves onboarding for developers or UI/UX designers
3. See all available reusable components without redo past work.
4. Scales well with larger teams that requires collaboration
5. Visually see the components or stories without having to login, click this or that, enter info etc.
   1. Essentially, just do UI development
6. Designers/QA can do UX testing
7. Run `Jest` testcases in storybook as well
8. Large backing from many well known companies (Airbb, Algolia, Atlassian, Lyft and Salesforce) Not likely to be deprecated or support die off
   1. [Storybook React Native](https://github.com/storybookjs/react-native) seems to be less popular. Although isn't the most important part related to the main `Storybook` projectd

## Cons
1. Initial setup time. Adding Storybook into the project. Adding configurations and all that jazz.
2. Learning curve on how to use it, set up or create stories. All of which add time to feature development.
4. Refactoring features/design changes require updates to the stories
   1. Adding demos/docs each time a component was added, changed or removed
   2. (Do we need to add docs for the components? Shouldn't the visual appearance of the components tell us everything?)

[Aricle on different front-end-documentation](https://css-tricks.com/front-end-documentation-style-guides-and-the-rise-of-mdx/)

> “Storybook has made it really easy for designers who code to collaborate with engineers. By working in storybook they don’t need to get a whole environment running (docker container, etc). For Wave, we have many important components that are only visible in the middle of a process that is short lived and time consuming to reproduce (i.e. a loading screen that only shows while a user is having their payment account set up). Before Storybook, we didn’t have a good way to work on these components and were forced to temporary hacks in order to make them visible. Now, with Storybook we have an isolated place to easily work on them, which has the bonus feature of being easily accessible for designers and PMs. It also makes it really easy for us to show off these states in sprint demos.”
> 
> – Dan Green, Wave Financial

I 100% agree. Having to set up temporary loading states or trying to hack up something is quite annoying. Even having to set the `<Scene>` to the component I'm testing is annoying. Especially if I have some data coming in from a <Mutation> or <Query>.

Storybook allows you test your components in isolation. You can set w.e props in your component and see how it works. View temporary loading states or do some visual regression tests.

Your designers and PM can see what the feature is like and comment/discuss about it. Overall, it seems fundamentally a collaboration tool and testing tool.

The main difficulty is that I lack experience working with the tool. I don't know how much time it takes to setup a story. I don't know how difficult it is to set it up for the project (I was encountering corejs issues and infinite loading). And I don't know how much time it'll require to maintain it when we refactor components or such.

However from my experience, testing different use cases manually took time. Trying to setup intermediary transition states required time and effort. Sometimes trying to test the component by passing a prop was difficult due to components in a `<Screen>` being so coupled together.
* AllCashOrdersScreen
  * PastPayment Flatlist
  * FuturePayment Flatlist
* Insights feed
* No Hit Decline Screen (Issue was testing `ReviewInfo` screen)

## Business Case

Present a business case for management to go with this setup
### Costs
* Requires time (dev time isn't cheap)
* Extra configurations or extra work (Longer processes)
  * Although I think dev time will be roughly the same due to having to do visual regression testing
  * QA/Designers will have more input on the component designs
* Integrate `Jest` unit testing and such

### Benefits
* Better UX feedback from designers/QA
* Less time or manual setup for QA to test components
  * Manual testing of the app is still required but less work on isolated components or stories
* Less bugs/UI issues when having more testing/test cases
  * Both from `Jest` unit tests and visual tests
  * Tests can also be used as documentation
* (Debatable) Faster development times
  * The issue is that I don't have experience working with Storybook so it's hard to make a case on it.
  * However, I'm assured that based on my past experiences the coupling of all these isolated components in development is unnecessary and slows down development. It should be when we finish UI development and testing isolated components that we combine them all together into a story to do integrated testing.

All in all. Does this either
1. Make the company more money - Features and UX that attracts consumers
2. Save the company money - Less important for a startup that is looking to grow/expand

I still find that the greatest benefit confers to larger sized teams like airbnb, atlassian, lyft, auth0, salesforce and jetbrains. 

## Other Stuff

What is webpack?
* Bundler. It takes all your files in your applications and bindles it together into one JS file. Webpack works will babel to transform your JSX into JS code.

What is babel?
* Babel is a tool that compiles your JS to older versions of JS so that it can be compatible across browsers.

## Testing

Where is the part in the code where it uses `preset-env` to transform your sources so that they can run in Node.js?

Test we write follows the basic pattern:
Arrange -> Act -> Assert

Jest will have setup a JSDOM instance for you so you can do the arrange, act part immediately.

Don't isolate your component from React. How your component renders and updates in React is vital to how your application works.

JSDOM doesn't have a cleanup hook that removes your rendered nodes on your DOM. You're adding the `View` or `Text` tag manually. However, `react-testing-library` will do that for you.
* Wraps interactions into `act`
* Automatically unmount components after a test is completed

`react-testing-library` appends its own instance to the DOM so you dont have to create nodes yourself.

* All these queries are scoped to the component so you don't have to use screen or pass a container as a arg.
* `RTL` scopes your queries to the rendered component's root container
  * Ensures that assertions apply to elements within the component under test

#### TIL
[Text Container](https://reactnative.dev/docs/text#containers)

When you have a `<Text>` as the outer parent of inner `elements`, the elements will wrap. They will not be using the Flexbox layout but using text layout.

ex
```javascript
<Text>
  <Text>Izone </Text>
  <Text>is the best group!</Text>
</Text>
/*
|Izone |is the best group!|
*/
// Compared to
<View>
  <Text>Izone </Text>
  <Text>is the best group!</Text>
</View>
/*
|Izone |
|is the best group!|
*/
```
### Guiding principles of RTL/RNTL

Test should resemble how users interact with your code (component, page) as much as possible. Recommend this order of priority

1. Queries Accessible to Everyone: Reflect exp of visual users/assistive tech
   1. getBy Text/DisplayValue/PlaceholderText/LabelText/HintText/AccessibilityState/AccessibilityValue
2. Queries Users can infer: Query every element exposed in accessibility tree as a role ex) button, images
3. Test IDS: Users can't see or hear these. Only recommended when can't match text or doesn't make sense.

The whole point is to write maintainable test for your RN components
1. Avoid including implementaion details of your components
2. Instead focus on tests that giv eyou confidence for which they are intended
3. Testbase to be maintainable in the long run so refactors (change to implementation not functionality) won't break your test and slow you and your team down

# Jest Matchers

`toBe` and `toEqual` are different. `toBe` uses `Object.is` to compare equality. Basically it compares if they're the same type, number and string but for `Object`, it compares if they are same reference. `toEqual` will recursively compare the values.

## Where and how to start testing

[Where to start testing](https://blog.usejournal.com/where-and-how-to-start-testing-your-react-native-app-%EF%B8%8F-and-how-to-keep-on-testin-ec3464fb9b41)


> If you’re currently developing the next great app with react native or already have an app in the store, you’ve probably thought about testing your app. So here’s the thing, most developer teams love the idea of testing their modules, screens and components but it rarely happens.
> We just can’t find the right time, in some cases we don’t have the knowledge, we’re not sure how to “Sell it” to the client or convince the stakeholders. We’re wondering what the consequences will be, will writing more tests slow down the team’s velocity?

Damn that hits hard. WOAH that's so true tbh.

>  You’ll find yourself dealing with more bugs, unhappy customers and an exhausted support team.

### Prioritization

Prioritize on important modules, screens, utils and core logics first. Also includes the places wehre you're fetching data or making external calls. Well I'm fetching data everywhere with GraphQL lol. 

### What is a custom hook?

There's a testing library for it [react-hooks-testing-library](https://github.com/testing-library/react-hooks-testing-library)

Extracting component logic into reusable functions. [custom hooks](https://reactjs.org/docs/hooks-custom.html)

So you can extract `useQuery` or all these hooks or stateful logic because in the end `hooks` and `components` are functions.

Before in class components we had to share stateful logic through `render props` and `higher-order-components`. Now `hooks` makes it easier by extracting it to a third function.

Wow this is exactly what I need

## Test-Doubles

`Test-double` is an object or procedure you use to replace a real object/procedure that is hard to test. This sounds like in `Jest` we use `mocks` to fake `implementation` or return values.

The issue is that unit testing shouldn't include having side-effects modifying your implementation of the test.

Side-effect is an operation, function, expression that modies some state value outside it's local env. In other words, it has an observable effect besides returning a value to the invoker of the operation.

ex) Side effects modifying a non-local bariable, static local variable, mutable argument passed by reference, performing I/O and calling other side-effect fns.

[Explanation of Sinon and Test Doubles](https://www.testim.io/blog/sinon-js-tutorial/#:~:text=Sinon%20JS%20is%20a%20popular,deterministic%2C%20as%20they%20should%20be.)
^ I agree that this is a very good resource on understanding Test Doubles and such. However, I don't have the time right now to learn `Sinon.js` and such.

#to-read #sinonjs #test-doubles #side-effects

## Behavioral Testing with useContext

The best way to test Context is to make our tests unaware of it's existence. Because Context is not specifically related or important to our testing.

To be honest in our case, we don't want to test the provider. It's not necessary for us because the ThemeContextProvider and the value for theme is static. We are not changing the theme of app at all.

I understand know. All we have to import is not `<ThemeContext>` but rather import `ThemeContextProvider from '<path>/ThemeContextProvider`. We wrap our component we're testing with `<ThemeContextProvider>`.

```javascript
const { getByText } = render(
    <ThemeContextProvider>
      <MockedProvider mocks={mocks} addTypeName={false}>
        <CashLimitWidget />
      </MockedProvider>
    </ThemeContextProvider>
  );
```

## Issues with Act is not Wrapped

[Error that act is not wrapped](https://davidwcai.medium.com/react-testing-library-and-the-not-wrapped-in-act-errors-491a5629193b)

[Using mockproviders](https://typeofnan.dev/mocking-apollo-graphql-queries-in-react-testing/)

`findBy` combines `getBy` queries and `waitFor` queries. They accept waitFor options as the last argument. This means that it expects an element to appear but the change in the DOM does not happen immediately.

`describe` - encompasses all tests `it` for one type of functionality

AAA
1. Given - some precondition
2. When - Some action executed by the function you're testing
3. Then - the expect outcome

## Encountered Issues

1. [x] `react-test-renderer` version conflicts with `react-native-testing-library`.
2. [ ] jest test runs too slow (It shouldn't take 8s to run 2 tests)

### React Testing using GraphQL

### LottieAnimationView errors

This is also related to the yellowbox errors
[Lottie react native 2.6.1 issues](https://github.com/lottie-react-native/lottie-react-native/issues/502)

## GraphQL Mock Provider

[Testing Apollo Mutations](https://jkettmann.com/testing-apollo-how-to-test-if-a-mutation-was-called-with-mockedprovider)