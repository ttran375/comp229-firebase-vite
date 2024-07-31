# How to Connect Your React App to Firebase using Vite

## Create React App.

```sh
yarn create vite client --template react
```

## Install the Firebase Package

```sh
yarn add firebase
```

## Initialize Firebase in Your React App

`src/firebase_setup/firebase.js`

```js
// Import the functions you need from the SDKs you need
import { initializeApp } from "firebase/app";
import { getAnalytics } from "firebase/analytics";
import { getFirestore } from "@firebase/firestore"

// TODO: Add SDKs for Firebase products that you want to use
// https://firebase.google.com/docs/web/setup#available-libraries

// Your web app's Firebase configuration
// For Firebase JS SDK v7.20.0 and later, measurementId is optional
const firebaseConfig = {
    apiKey: "",
    authDomain: "",
    projectId: "",
    storageBucket: "",
    messagingSenderId: "",
    appId: "",
    measurementId: ""
};

// Initialize Firebase
const app = initializeApp(firebaseConfig);
const analytics = getAnalytics(app);

export const firestore = getFirestore(app)
```

`src/handles/handlesubmit.js`

```js
import { addDoc, collection } from "@firebase/firestore"
import { firestore } from "../firebase_setup/firebase"

const handleSubmit = (testdata) => {
    const ref = collection(firestore, "test_data") // Firebase creates this automatically

    let data = {
        testData: testdata
    }

    try {
        addDoc(ref, data)
    } catch (err) {
        console.log(err)
    }
}

export default handleSubmit
```

`src/App.jsx`

```js
import './App.css';
import handleSubmit from './handles/handlesubmit';
import { useRef } from 'react';

function App() {
  const dataRef = useRef()

  const submithandler = (e) => {
    e.preventDefault()
    handleSubmit(dataRef.current.value)
    dataRef.current.value = ""
  }

  return (
    <div className="App">
      <form onSubmit={submithandler}>
        <input type="text" ref={dataRef} />
        <button type="submit">Save</button>
      </form>
    </div>
  );
}

export default App;
```
