# Commit Log: React Login Authentication

---

## ✅ Commit 1: Function for Error Message Handling

### 🔹 Context:
In the previous video, we learned how to:
- Send an authentication request (login API).
- Handle the response on successful login.

### 📌 In this session:
We focus on what to do **when the login request fails**.
We’ll also explore:
- How to work with **JWT (JSON Web Token)** returned from a successful login call.
- Where and how to **store JWT**.

---

### 🔍 Handling Login Failure:

When the login request fails, the error details can be accessed from:
```js
data.error_msg
```
So we wrote a function to handle login failure:
```js
onSubmitFailure = errorMsg => {
  console.log(errorMsg)
}
```
We call this function when the login API fails:

```js
else {
  this.onSubmitFailure(data.error_msg) // ".error_msg" gives the error message from API response
}
```

##### In the next commit, we will render this error message on the login page UI for better user feedback.

---


## ✅ Commit 2: Rendering the Error to the UI

### 📌 Goal:
To display the error message on the login page when the login request fails.

---

### 🧠 Step 1: Declare state variables

We add the following to `state`:

```js
this.state = {
  password: '',
  showSubmitError: false, // Initially false to hide the error message
  errorMsg: '',           // Will hold the actual error text
}
```

#### 🧠 Step 2: Update state on failure
We modify the onSubmitFailure function to update the state:

```js

onSubmitFailure = errorMsg => {
  console.log(errorMsg)
  this.setState({
    showSubmitError: true,
    errorMsg: errorMsg,
  })
}

```

#### 🧠 Step 3: Extract values in render
Inside the render() method:


```js
const { showSubmitError, errorMsg } = this.state

```

####  🧠 Step 4: Conditionally render the error message
We show the error only if showSubmitError is true:

```js
{showSubmitError && <p className="error-message">*{errorMsg}</p>}

```

`🔜 Next Commit:`
We will explore:
  Why we need to store the JWT (JSON Web Token)
  Where and how to store it securely

---



## ✅ Commit 3: Storing JWT in Cookies

### 📌 Goal:
To store the JWT token received from the login API so we can use it in future requests.

---

### 🧠 Why do we need to store JWT?

- On a **successful login**, the API returns a `jwt_token`.
- We will send this token in the **Authorization header** (as `Bearer token`) for every future request.
- This is how the server will know the request is coming from a valid (authenticated) user.

---

### ❓ Where should we store the JWT?

Let’s explore some options:

#### ❌ Not in React state:
1. **State gets reset** when the page refreshes — JWT will be lost.
2. **State is local to a component** — other components won't have access to the token easily.

So storing JWT in React component state is not ideal.

---

### ✅ Requirements:
1. JWT should **persist even after page reload**.
2. JWT should be **accessible across the whole application**.

---

### 🗃️ Storage Options:

There are two types of storage:

1. **Client-side storage** (stored on the user's machine)
2. **Server-side storage** (stored in a backend database — not relevant here)

#### Client-side options:
- Local Storage
- Cookies
- Session Storage
- IndexedDB
- And more...

We choose **cookies** for this case.

---

### 🍪 Why Cookies over Local Storage?

| Feature           | Cookies                | Local Storage           |
|------------------|------------------------|--------------------------|
| Expiry Time      | Can set expiry time    | Never expires by default |
| Storage Size     | ~4 KB                  | 5–10 MB                  |
| Automatically Sent | ✅ With every HTTP request | ❌ Not automatically sent |

Cookies are better here because:
- We can **set expiry duration**
- It's **smaller**, but ideal for storing tokens like JWT
- **Automatically sent with requests**, useful for auth flows

---
## 🔧 Installing js-cookie package

To handle cookies in React, we’ll use a 3rd-party package called **js-cookie**.

Install it using:

```js
npm install js-cookie --save
```


The `--save` flag adds it as a dependency in `package.json`, so it gets installed in other environments when running `npm install`.

🔑 **js-cookie methods:**

    `Cookies.set()` – Set a cookie
    `Cookies.get()` – Get a cookie
    `Cookies.remove()` – Remove a cookie

**Example syntax:**

```javascript
Cookies.set('cookieName', 'cookieValue', { expires: 30 }) // 30 days
```
### 🔽 Implementation in Code

#### 📥 Step 1: Import the package


```js
import Cookies from 'js-cookie'
```
#### 📤 Step 2: Store JWT on successful login

jwt_token we had already recieved from the login API, if login API is call sucess, we will get this JWT as reposne.
Now we are passing the jwt as arg to `onSubmitSuccess()` function

```js
if (response.ok === true) {
  this.onSubmitSuccess(data.jwt_token)
}
```
#### 💾 Step 3: Save JWT to Cookies
Here we will store the perviously generated JWT into user browser.

```js
onSubmitSuccess = jwtToken => {
  Cookies.set('jwt_token', jwtToken, { expires: 30 }) // Save token in cookies for 30 days
  const { history } = this.props
  history.replace('/') // Redirect to home page
}
```
### `note`:
 Using the 3 rd party package, we are only setting the JWT into user's browesr, this package does not generate the JWT, the JWT will be generated by the server and it will be sent when we do a sucessfull login API call.

---

🔜 Next Commit:
We will learn how to handle route redirection based on whether the JWT exists or not.

---

## ✅ Commit 4: Handling Route Redirections

Now we want to implement:

- If the user is **already logged in** and tries to access the **login route**, he should be **redirected to the home page**.
- If the user is **not logged in** and tries to access the **home page**, he should be **redirected to the login page**.

---

### 🔍 How to achieve this?

We need to check whether the user has the JWT token or not.  
If the token exists, redirect to home page.  
If not, redirect to login page.

---

### 📌 Syntax to get JWT from cookies:
```js
Cookies.get('cookieName')
```
Returns **undefined** if the cookie doesn't exist or has expired.

---

### 🔁 To redirect in React Router:

```js
import {Redirect} from 'react-router-dom'

<Redirect to="PATH" />
```

Whenever you want to **redirect to another path**,  
**Redirect Component** can be used inside the render method.

---

### 🧱 In our code:

```js
import {Redirect} from 'react-router-dom' // import

const jwtToken = Cookies.get('jwt_token') // getting the JWT from cookies

if (jwtToken !== undefined) {
  // if token exists, redirect to home
  return <Redirect to="/" />
}
```

---

### 🔄 Redirect Component vs History Methods

- ✅ **Use Redirect Component** when you want to stop showing the current UI and move to another route  
  👉 Example: Inside `render()` method of class components

- ✅ **Use history.push() or history.replace()** in places like `onClick`, `onSubmit`, etc.

📝 **Note:** Behind the scenes, the `Redirect` component uses the `history.push()` or `history.replace()` method.

---

### 🚫 Unauthenticated Scenario

If the user is **not logged in** and tries to access the **home route**,  
we have to redirect the user to the **login route**.

So, check if the JWT token is available.  
If it's not available, redirect to login.

---

### 🧱 Code in Home Component:

```js
import Cookies from 'js-cookie'
import {Redirect} from 'react-router-dom'

const jwtToken = Cookies.get('jwt_token')

if (jwtToken === undefined) {
  return <Redirect to="login" />
}
```

---

In the next commit, we will handle **logout functionality**.
---

## ✅ Commit 5: Adding Logout Functionality

Let's add **logout functionality** and handle **redirections**.

### 🎯 What should happen?
When we click the logout button:
- The JWT should be deleted from cookies
- The user should be redirected to the login page

---

### 🔘 Logout Button Logic

In the Header, we have a Logout button.  
We will add an `onClick` function called `onClickLogout()`.

In this function, we will write logic to delete the JWT.

```js
Cookies.remove('CookieName')
```

Example:
```js
import Cookies from 'js-cookie'
Cookies.remove('jwt_token')
```

---

### 🔁 Now we need to redirect
#### 🔁 Why we can't use `<Redirect />` in all places?

In React Router:

- The `<Redirect />` component is used **only inside the `render()` method** of a class component.
- It **stops rendering the current UI** and navigates to another route (like going to `/login` or `/home`).

#### ✅ Example use of `<Redirect />` inside `render()`:
```js
if (userNotLoggedIn) {
  return <Redirect to="/login" />
}
```

#### ❌ But outside `render()`, for example inside a function, we can't use `<Redirect />`.

---

#### 🧭 What do we use instead?

We use the **`history` object**.

React Router gives us a `history` object that has helpful methods like:

- `history.push('/some-path')` → Goes to a new route
- `history.replace('/some-path')` → Replaces the current route with a new one

#### ✅ You can use these inside any function like `onClick`, `onSubmit`, etc.

```js
const onClickLogout = () => {
  history.replace('/login')
}
```

---

### ⚠️ The catch: Who gets the `history` prop?

React Router **automatically provides** the `history` prop **only to components** that are directly connected to a route.

#### ✅ For example:
```js
<Route exact path="/home" component={Home} />
```
In the `Home` component, you can directly use:
```js
const { history } = this.props
```

#### ❌ But for components **not directly connected to a route** (like a `Header` component inside `Home`), the `history` prop is **not available** by default.

---

### 🛠️ How to fix this? Use `withRouter`

React Router gives us a helper called `withRouter`.

If you wrap your component with `withRouter`, it gives your component access to the `history`, `location`, and `match` props — even if it's not directly used in a route.

#### ✅ Example:

```js
import { withRouter } from 'react-router-dom'

const Header = props => {
  const onClickLogout = () => {
    const { history } = props
    history.replace('/login')
  }
  ...
}

export default withRouter(Header)
```

This way, even if `Header` is not used in a `<Route />`, it can still use the `history` prop to redirect.



---

### 🔧 In Header Code:

```js
import {Link, withRouter} from 'react-router-dom' // imported withRouter
import Cookies from 'js-cookie'

const onClickLogout = () => {
  // this function deletes the JWT and redirects to login page
  const {history} = props
  Cookies.remove('jwt_token')
  history.replace('/login')
}

export default withRouter(Header) 
// Header is used inside Home component, so to access history object, we wrapped Header with withRouter
```

---

