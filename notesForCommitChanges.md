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