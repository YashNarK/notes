- [Redux patterns](#redux-patterns)
  - [**1. Classic (Folder-by-Type) Pattern**](#1-classic-folder-by-type-pattern)
  - [**2. Feature-Based (Ducks) Pattern**](#2-feature-based-ducks-pattern)
  - [**3. Redux Toolkit (RTK) Pattern** *(Modern Standard)*](#3-redux-toolkit-rtk-pattern-modern-standard)
  - [**4. Entity Adapter Pattern** *(For Normalization \& Performance)*](#4-entity-adapter-pattern-for-normalization--performance)
  - [**5. Sagas \& Middleware Pattern** *(For Side Effects)*](#5-sagas--middleware-pattern-for-side-effects)


# Redux patterns

Redux has evolved over time, leading to several patterns for organizing state management in applications. Here are the most common Redux patterns:  

## **1. Classic (Folder-by-Type) Pattern**  
- Organizes Redux files by type (e.g., `actions/`, `reducers/`, `constants/`).  
- Common in early Redux projects but leads to scattered logic and maintainability issues.  
- Example structure:  
  ```
  /redux
    ├── actions/
    │   ├── userActions.js
    │   ├── productActions.js
    ├── reducers/
    │   ├── userReducer.js
    │   ├── productReducer.js
    ├── constants/
    │   ├── userConstants.js
    │   ├── productConstants.js
  ```

## **2. Feature-Based (Ducks) Pattern**  
- Groups all Redux logic related to a feature into a single file.  
- Follows the **DUCKS** standard (a modular approach to Redux).  
- Reduces fragmentation and improves maintainability.  
- Example structure:  
  ```
  /redux
    ├── features/
    │   ├── userSlice.js
    │   ├── productSlice.js
  ```
  
## **3. Redux Toolkit (RTK) Pattern** *(Modern Standard)*  
- Built on **Redux Toolkit (RTK)**, which simplifies state management.  
- Uses `createSlice` to manage reducers, actions, and initial state in a single place.  
- Supports async actions using `createAsyncThunk`.  
- Example:  
  ```js
  import { createSlice, createAsyncThunk } from '@reduxjs/toolkit';

  const fetchUser = createAsyncThunk('user/fetchUser', async (userId) => {
    const response = await fetch(`/api/users/${userId}`);
    return response.json();
  });

  const userSlice = createSlice({
    name: 'user',
    initialState: { data: null, status: 'idle' },
    reducers: {},
    extraReducers: (builder) => {
      builder
        .addCase(fetchUser.pending, (state) => { state.status = 'loading'; })
        .addCase(fetchUser.fulfilled, (state, action) => { 
          state.status = 'succeeded';
          state.data = action.payload;
        });
    }
  });

  export default userSlice.reducer;
  ```

## **4. Entity Adapter Pattern** *(For Normalization & Performance)*  
- Uses Redux Toolkit's `createEntityAdapter` to handle collections efficiently.  
- Helps in managing **normalized state** and selectors.  
- Example:  
  ```js
  import { createEntityAdapter, createSlice } from '@reduxjs/toolkit';

  const usersAdapter = createEntityAdapter();

  const userSlice = createSlice({
    name: 'users',
    initialState: usersAdapter.getInitialState(),
    reducers: {
      addUser: usersAdapter.addOne,
      updateUser: usersAdapter.updateOne,
      removeUser: usersAdapter.removeOne,
    },
  });

  export default userSlice.reducer;
  ```

## **5. Sagas & Middleware Pattern** *(For Side Effects)*  
- Uses **Redux Saga** for handling side effects like API calls.  
- Example:  
  ```js
  function* fetchUserSaga(action) {
    try {
      const user = yield call(api.fetchUser, action.payload);
      yield put({ type: 'FETCH_USER_SUCCESS', payload: user });
    } catch (error) {
      yield put({ type: 'FETCH_USER_FAILURE', error });
    }
  }
  ```