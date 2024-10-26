# README: Implementing Login Functionality

## Overview

This README outlines the steps taken to implement the login functionality in the project. It covers the libraries installed, files updated, and code modifications made to facilitate user authentication using Firebase.

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [Libraries Installed](#libraries-installed)
3. [File Structure](#file-structure)
4. [Code Updates](#code-updates)
5. [How to Test](#how-to-test)


## Libraries Installed

The following libraries were installed to enable login functionality:

```bash
# Install Firebase for authentication
npm install firebase




## Code Updates

### 1. Firebase Initialization (`src/firebase.ts`)

Make sure to initialize Firebase in this file:

```javascript
import { initializeApp } from 'firebase/app';
import { getAuth } from 'firebase/auth';

const firebaseConfig = {
  apiKey: import.meta.env.VITE_FIREBASE_API_KEY,
  authDomain: import.meta.env.VITE_FIREBASE_AUTH_DOMAIN,
  projectId: import.meta.env.VITE_FIREBASE_PROJECT_ID,
  storageBucket: import.meta.env.VITE_FIREBASE_STORAGE_BUCKET,
  messagingSenderId: import.meta.env.VITE_FIREBASE_MESSAGING_SENDER_ID,
  appId: import.meta.env.VITE_FIREBASE_APP_ID,
};

const app = initializeApp(firebaseConfig);
const auth = getAuth(app);

export { auth };
```

### 2. AuthProvider Component (`src/auth/context/jwt/auth-provider.tsx`)

The `AuthProvider` component was updated to include login functionality:

```javascript
import { signInWithEmailAndPassword } from 'firebase/auth';
import { useMemo, useEffect, useReducer, useCallback } from 'react';
import axios, { endpoints } from 'src/utils/axios'; 
import { auth } from 'src/firebase'; 
import { AuthContext } from './auth-context'; 
import { setSession, isValidToken } from './utils'; 
import { AuthUserType, ActionMapType, AuthStateType } from '../../types'; 

 const login = useCallback(
    async (email: string, password: string) => {
      try {
        const res = await signInWithEmailAndPassword(auth, email, password);
        const { user } = res;
        const accessToken = await user.getIdToken();

        sessionStorage.setItem(STORAGE_KEY, JSON.stringify(accessToken));

        dispatch({
          type: Types.LOGIN,
          payload: { user: { ...user, accessToken } },
        });
      } catch (error) {
        console.error(error);
      }
    },
    [dispatch]
  );
```



