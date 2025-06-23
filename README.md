# Authentication & Authorization | Part 2 | Cheat Sheet

## Concepts in Focus
- JWT Token
- Storage Mechanisms
- Cookies
- Why Cookies?
- Cookies vs Local Storage
- Third Party Package js-cookie
- Redirect Component
- withRouter
- E-Commerce Application

## 1. JWT Token
JSON Web Token is a standard used to create Access Tokens. These access tokens are also called JWT Tokens.

The client uses these access tokens on every subsequent request to communicate with the Server.

**Note**
While making HTTP Request, we have to send an access token in the HTTP Headers with the key `Authorization`.

**Example:**
`Authorization: Bearer jwt_token`

### 1.1 Storing JWT Token in State
When we store the JWT Token in the state,
- On page refresh, the JWT token won't be available
- It is difficult to pass state information to every component

## 2. Storage Mechanisms
- **Client-Side Data Storage**
  Storing Data on the Client
- **Server-Side Data Storage**
  Storing Data on the Server using some kind of Database

Different types of Client-Side data storage mechanisms are:
- Local Storage
- Cookies
- Session Storage
- IndexedDB, etc.

## 3. Cookies
A cookie is a piece of data that is stored on the user's computer by the web browser.

A cookie is made up of:
- **Name & Value**
- **Expires** − The date the cookie will expire. If this is blank, the cookie will expire when the visitor quits the browser.
- **Domain** − The domain name of your site.
- **Path** − The path to the directory or web page that set the cookie. This may be blank if you want to retrieve the cookie from any directory or page.
- **Secure** − If this field contains the word "secure", then the cookie may only be retrieved with a secure server. If this field is blank, no such restriction exists, etc.

### 3.1 Why Cookies?
With cookies, we can set the expiry duration.

**Examples:**
- Banking Applications - Cookies get expired in minutes
- Facebook - Cookies get expired in months or years

### 3.2 Cookies vs Local Storage

| Cookies                       | Local Storage                     |
|-------------------------------|-----------------------------------|
| We can set an expiration for Cookies | Local storage data never expires |
| Cookies can store up to 4KB of data | Local Storage can store up to 5 to 10 MB of data |


## 3.3 js-cookie Methods

**js-cookie methods are:**

- `Cookies.set()` - It is used to set the cookie  
- `Cookies.get()` - It is used to get the cookie  
- `Cookies.remove()` - It is used to remove the cookie  

---

### 3.3.1 Cookies.set()

**Syntax:**

```js
Cookies.set('CookieName', 'CookieValue', {expires: DAYS});
```

**Example:**

```js
Cookies.set('ACCESS_TOKEN', 'Us1L90PXl...', {expires: 1});
```

---

### 3.3.2 Cookies.get()

It returns `undefined` if the cookie expires or does not exist.

**Syntax:**

```js
Cookies.get('CookieName');
```

**Example:**

```js
Cookies.get('ACCESS_TOKEN');
```

---

### 3.3.3 Cookies.remove()

**Syntax:**

```js
Cookies.remove('CookieName');
```

**Example:**

```js
Cookies.remove('ACCESS_TOKEN');
```

---

## 4. Redirect Component

The `react-router-dom` provides the `Redirect` component.  
It can be used whenever we want to redirect to another path.

**Syntax:**

```jsx
<Redirect to="PATH" />
```

**Example:**

```jsx
<Redirect to="/login" />
```

---

### 4.1 Redirect Component vs history Methods

- Use the `Redirect` Component when you have to stop displaying UI and navigate to a route.  
  **Ex:** Inside Class Component - `render()`
- In all other cases, use `history.push()` or `history.replace()`  
  **Ex:** `onSubmit`, `onClick` event callback functions

**Note**  
The Redirect component uses the `history.push` and `replace` methods behind the scenes.

---

## 5. withRouter

The `history` prop will be available for only components which are directly given to `Route`.

To provide `history` prop to other components, we can wrap it with the `withRouter` function while exporting it.

**Example:**

```js
import { withRouter } from 'react-router-dom';

...

export default withRouter(ComponentName);
```

---

## 6. E-Commerce Application

- Make an Authentication Request to Login API  
- Handle Login API Response  

**On Login Success:**
- Store the JWT Token

**On Login Failure:**
- Show error message or prompt

check the repo for code
