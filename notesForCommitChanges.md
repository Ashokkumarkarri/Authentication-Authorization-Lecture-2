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