# swedcond18-answers

1. What is Long polling?
   Long polling is a way to keep a connection persistent with a server and that doesn’t use protocols such as websockets or Servet sent events.
   In a regular Polling, you would have a request made to the server asking for new information on a time delay, downside to that would be constantly requesting data where there is none to be provided.

Long polling uses the same method difference would be that the server would deliver information without delays.
Here is a flow below.

1. A request is being made to the server
2. The server keeps the connection alive until there is new data to be delivered
3. Once the server sees the message, the server responds to the requesting
4. The browser makes a new request right away.
   This is good because if the connection is lost, the request will be made again.

1a.
async function subscribe() {
let response = await fetch("/subscribe");

if (response.status == 502) {
await subscribe();
} else if (response.status != 200) {

    showMessage(response.statusText);

    await new Promise(resolve => setTimeout(resolve, 1000));
    await subscribe();

} else {

    let message = await response.text();
    showMessage(message);

    await subscribe();

}
}

subscribe();
In the code example above, you see that the subscribe function makes a fetch and t hen waits for the response. It handles it and the calls itself again and would go on like this.

Although the server architecture must be able to work with these kind of code considering the process would consume a lot of memory. Perhaps have some code to prevent to many request at once.

1b. Alternative would be using Websockets (socket.io) but is normaly used when communcation is event based in both directions. Websockets requires persistent connection which might not be suitable for mobile networks where it might be overloaded in crowded areas.

1c. Pros would be that Long polling is widely supported by most devices and can handle it.
While the Cons are:
It may use a lot of resources from the server considering having a connection open at all times and requests being made “constantly”.
If a client has two browser tabs open, it will consume a lot of server resources and having the client side save data to localstorage or indexedDB, there might be issues of data being overwritten.

If the server implementation is not well coded, there might be issues with clients data interfering with each other and one may not even receive expected data.

2. Difference between Cookie and Local Storage
   It might come down to security issues, Cookies are not accessible via JS for the cookies to be set, which makes the Cookies more secure than LocalStorage. While both are vulnerable to XSS, Localstorage is also vulnerable to CSRF attacks and does not provide secure attributes to block attacks. Cookies can be more secure if implemented securly.

3. Different types of HTTP methods
4. GET ( used to read/retrieve data)
5. POST (used to create new resource / send new data to API for example )
6. PATCH (used to modify/update existing data)
7. DELETE (used to delete resource)
8. PUT (used to update or replace resource with new request data)
9. OPTIONS (used to describe the communication options for the resource)

10. Difference between an object and array
    Objects are mutable data structure that can be anything. It stores the data in key value pair
    ex:
    const obj = {
    name: “Rawand”,
    age: ‘28’,
    }

console.log(obj.name, obj.age)

while an array is an ordered collection of data and can be accessed through Index key
ex : names = [‘Rawand’, ‘SwedCon’]

console.log(names[0], names[1])

5.What is reactjs?
React JS is a component based framework for Javascript which helps coding easier and write more readable code. Instead of writing same code over and over again, you write a component that can be reused when needed.
a. It is used for declaring and using state variables, performing side effects like making api calls or setting subs. Managing lifecycles of a compenent. (usestate,useEffect..). Using hooks makes code easier to understand and can be used to reuse logic between components.
b. Calling hooks in order is a must, calling hooks at top level of react functions, Call hooks within react functions.

Calling hooks at top level of react functions is necessary because if it is called in a loop or a nested function. It will lead to unexpected behavior.
Also clearing life cycles is a great thing to do, so that react components can unmount when not needed.
c. Jsx is a syntax extension for JavaScript that allows you to write HTML like code in javascript code. React library is fond of it.
d. enviroment variables are used to store secure information that is needed for the application to work, such as API keys, API base urls etc.. You create a .env file and declare your secret variables.
REACT_APP_API_KEY=secretKeyHere
REACT_APP_BASE_URL=https://apiUrlHere.com
and call it later inside your application
const apiKey = process.env.REACT_APP_API_KEY
const baseUrl = process.env.REACT_APP_BASE_URL
e. Props or Properties are a way to pass data from a parent component to a child component.
f. In order to update a state in react, we use the setState method. const [count, setCount] = useState(0);

we can create a button and once pressed we update the name.
<button onClick={() => setCount(count + 1)}>
Click me
</button>

g. Yes, but you need to wrap them around a container element often used is a div element
h. Component-Driven Development is an approach that focuses on building reusable modular components.
Reusability: By building components that are self-contained and modular, you can easily reuse them throughout your app, or for different projects. It saves time and effort.
Maintainability: Components that are designed to be independent and self-contained are generally easier to maintain and update. By breaking down your app into smaller, more manageable pieces, it makes changes and updates easier.
Testability: Components that are isolated and modular are easier to test than a larger codebase. This makes it easier to check if your app is reliable and performs well.

I. Code for components
I. Login Button

const LoginButton = ({ onClick }) => (
<button type="button" onClick={onClick}>
Login
</button>
);

and to use that component:

const [isLoggedIn, setIsLoggedIn] = useState(false);

const handleLoginClick = () => {
// Perform login action
setIsLoggedIn(true);
};

<div>
      {isLoggedIn ? (
        <p>You are logged in</p>
      ) : (
        <LoginButton onClick={handleLoginClick} />
      )}
    </div>

ii. A Group Component with the following values “React” ,“LIA” , “Carelyo“ each value can be selected

const options = ['React', 'LIA', 'Carelyo'];
const [selectedOption, setSelectedOption] = useState(null);

<div>
      {options.map(option => (
        <label key={option}>
          <input
            type="radio"
            value={option}
            checked={selectedOption === option}
            onChange={() => setSelectedOption(option)}
          />
          {option}
        </label>
      ))}
    </div>
iii. A Card Component with the following information (Name, Email, Image, Button)

const Card = ({ name, email, image, onButtonClick }) => (

  <div>

    <h3>{name}</h3>
    <p>{email}</p>
    <img src={image} alt={name} style={{ width: 200, height: 200 }} />
    <button type="button" onClick={onButtonClick}>
      Click me!
    </button>

  </div>
);

and we call the Card components

const handleButtonClick = () => {
function logic for button click
};

<Card
name= “Rawand”
email=”test@gmail.com”
image=”https://image.com/test.jpg”
onButtonClick={handleButtonClick}

/>

iv. An api call fetching information
here is an example with useState and useEffect to fetch and set data to our state.

const [data, setData] = useState(null);

useEffect(() => {
const fetchData = async () => {
const response = await fetch('https://fetchUrlHere.com/endpoint');
const data = await response.json();
setData(data);
};
fetchData();
}, []);

and then render out the data

<div>
      {data ? (
        <h1>{data.info}</h1>
      ) : (
        <h1>data still loading…</h1>
      )}
</div>


6. challange screenshots are in Challange-Screenshot folder

7. What is Typescript?
   Typescript is a programming language of Javascript, it adds optional static typing to the language which helps maintaining the code better and easier to understand.
   a. one of the main benefits of Typescript is that it catches type errors at compile time rather than at runtime, Which helps identifying errors and fixing them a lot easier. As Typescript includes features such as classes,interfaces and modules it does help write more organized code.
   b.
   Pros:

- It adds optional static typing to JavaScript, which can help improve the reliability and maintainability of your code
- It can catch type errors at compile-time, rather than at runtime, which can help you find and fix bugs faster
- It includes features such as classes, interfaces, and modules, which can help you write more modular and organized code
- It can be integrated with many popular JavaScript libraries and frameworks, such as React, Angular, and Vue.js
- It is widely used in the industry, so it is easy to find resources and support for it

Cons:

- It adds an additional layer of complexity to your codebase, which can take some time to learn and set up.
- It requires an additional build step, which can add overhead to your development workflow
- It may not be necessary for smaller projects, where the benefits of static typing may not outweigh the added complexity

c. Small function written in Javascript and Typescript
Javascript:

function add(x, y) {
return x + y;
}
Typescript:
function add(x: number, y: number): number {
return x + y;
}
Difference is that in Javascript the types are not defined which can be whatever while in Typescript we specify that the types are number. This way we know what to feed the functions written and is easier to read and understand.

8. What is a user story?
   A user story is a description of a feature or a requirement from a perspective of an end user/client.

a. As a busy college student, I want to be able to track my class schedule and assignments so that I can stay organized and on top of my work.
Detailed info of that user story:

- I can input my class schedule into the app
- I can input my assignments and due dates into the app
- The app sends me reminders for upcoming assignments and exams
- I can view my schedule and assignments in a calendar view or a list view
- I can mark assignments as complete or incomplete.

b.

c. User stories are a way to know what the functional requirements of an application is and set is as a goal to work towards. With user stories we can build a full story of an application and create near perfect copy of the clients imagination.

9. What is git?
   Git is a free and open source system that is designed to handle different projects. It is easy to learn and you work with local repositories until you are ready to push/upload them to a remote repository and share with people/team.
   a. Git hooks are scripts that Git executes before or after events such as committing, pushing, and receiving. They are useful for automating tasks and integrating Git with other tools.
   b. A conventional commit is a commit message that follows a specific format, which helps to standardize and automate the way that commit messages are written. The purpose of this standard is to make it easier to automatically generate useful information from commit messages, such as a change log or a release note.
   c. write a commit message for the following code change (created a new component and imported it into the the navbar component, the component shows up only when a user is logged and shows time left before the current session ends )
   as we are adding a new feauture we can do
   feat(navbar): add session countdown timer for logged-in users

10. What is Accessibility on the web and why is it important?
    Web accessibility refers to the practice of making websites and web applications usable by people of all abilities and disabilities. This includes people with visual, auditory, motor, and cognitive impairments, as well as people who may not have access to the latest hardware or software.

Web accessibility is important because it ensures that everyone can use and access the web, regardless of their abilities or disabilities.
a. what are the benefit of building an accessible website

- Improved usability: By following web accessibility guidelines, you can make your website more usable for everyone, not just people with disabilities. This can make your website more user-friendly and improve the user experience for all visitors.
- Government websites are required to be fully accessible considering their target audience is all of the people in the country.
- Increased reach: By making your website accessible to people with disabilities, you can reach a wider audience and potentially increase your user base.
- Improved SEO: Making your website accessible can also improve your search engine optimization (SEO) by making it easier for search engines to crawl and index your website's content.
- Cost savings: Building an accessible website from the start can save you time and money in the long run, as it is much more expensive to retrofit an inaccessible website than it is to build an accessible one from the beginning.

b. name some pitfalls when creating website that affect accessibility

- Using images with text embedded inside of them
- Using only color to determine important messages, people with visual impairments will not distinguish between certain colors.
- Not having alt text to images, Screen readers won’t know what is being shown on the screen and also for SEO sake it is always a good practice.
- Using non-semantic HTML elements, only using divs and spans is not a good practice especially for screen readers because it won’t understand the site structure. Semantic elements such as header, footer, article is better practice.
