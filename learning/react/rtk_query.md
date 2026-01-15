1.  React Component: A component (e.g., your LoginPage) needs to find out how a user can log in (SSO, Credentials, etc.).
2.  `useGetLoginMethods()`: The component calls your custom hook.
3.  `useLoginMethodsMutation()`: Your custom hook calls the auto-generated RTK Query hook. This hook doesn't fetch the data immediately. Instead, it
    returns a mutation trigger function.
4.  `mutation({ username })`: Your hook then calls this trigger function, passing the username.
5.  RTK Query Engine: RTK Query takes over. It executes the POST request
    to /api/3/auth/login as defined in loginAPI.ts. It handles the entire lifecycle:
    dispatching pending actions, making the API call, and then dispatching fulfilled or rejected actions.
6.  Redux Store: The state of the request (isLoading, isSuccess, data, error) is updated in the Redux store automatically.
7.  `.unwrap()`: The .unwrap() call on the result of the mutation returns a Promise that resolves with the actual data or rejects with the error. Your
    custom hook awaits this and returns the data to the component
8.  Data Transformation: Before the data is returned, the transformResponse function (fromAPIMethods) is called to map the raw API response into the
    application's domain model.

So, the architecture is:

React Component -> Custom Hook -> RTK Query Hook -> Redux Store -> API
