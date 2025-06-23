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

🔜 Next Commit:
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