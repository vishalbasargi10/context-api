1. Creating the Context
File: UserContext.js

jsx
Copy code
import React from 'react';

const UserContext = React.createContext();

export default UserContext;
What it does: React.createContext() creates a Context object. This object has two components: Provider and Consumer. In this case, you are exporting UserContext which you will use to provide and consume context values throughout your app.
2. Providing the Context
File: UserContextProvider.js

jsx
Copy code
import React, { useState } from 'react';
import UserContext from './UserContext';

export default function UserContextProvider({ children }) {
    const [user, setUser] = useState(null);

    return (
        <UserContext.Provider value={{ user, setUser }}>
            {children}
        </UserContext.Provider>
    );
}
What it does:
This component uses the useState hook to manage the user state and setUser function.
It wraps its children with the UserContext.Provider, which makes the user and setUser values available to any component that consumes this context.
The value prop of the Provider contains the state and setter function ({ user, setUser }).
3. Consuming the Context
File: Login.js

jsx
Copy code
import React, { useContext, useState } from 'react';
import UserContext from '../context/UserContext';

function Login() {
    const [username, setUserName] = useState('');
    const [password, setPassword] = useState('');

    const { setUser } = useContext(UserContext);

    const handleSubmit = (e) => {
        e.preventDefault();
        setUser({ username, password });
    };

    return (
        <div>
            <h2>Login</h2>
            <input
                type="text"
                placeholder="UserName"
                value={username}
                onChange={(e) => setUserName(e.target.value)}
            />
            <input
                type="password"
                placeholder="Password"
                value={password}
                onChange={(e) => setPassword(e.target.value)}
            />
            <button onClick={handleSubmit}>Submit</button>
        </div>
    );
}

export default Login;
What it does:
This component uses useContext(UserContext) to access the setUser function from the context.
When the user submits the login form, handleSubmit is called, which updates the user state in the context with the new username and password.
File: Profile.js

jsx
Copy code
import React, { useContext } from 'react';
import UserContext from '../context/UserContext';

function Profile() {
    const { user } = useContext(UserContext);

    if (!user) return <div>Please Log In</div>;

    return (
        <div>
            WELCOME {user.username}
        </div>
    );
}

export default Profile;
What it does:
This component uses useContext(UserContext) to access the user object from the context.
If the user is not available (i.e., null), it displays a message prompting the user to log in.
If user is present, it displays a welcome message with the username.
4. Wrapping Everything in the Context Provider
File: App.js

jsx
Copy code
import './App.css';
import UserContextProvider from './context/UserContextProvider';
import Login from './components/Login';
import Profile from './components/Profile';

function App() {
    return (
        <UserContextProvider>
            <h1>Context-API</h1>
            <Login />
            <Profile />
        </UserContextProvider>
    );
}

export default App;
What it does:
The App component wraps Login and Profile components in the UserContextProvider.
This ensures that Login and Profile components, as well as any other components nested inside UserContextProvider, have access to the UserContext.
Summary
Create Context: UserContext is created using React.createContext().
Provide Context: UserContextProvider uses UserContext.Provider to make the user state and setUser function available throughout the component tree.
Consume Context:
Login updates the user state in the context.
Profile reads the user state from the context and displays information accordingly.
Wrap Components: App wraps Login and Profile in UserContextProvider, ensuring they have access to the context values.Functions to Provide Data
React.createContext()

Location: UserContext.js
Purpose: Creates a new context object. This object includes a Provider component and a Consumer component. You use the Provider to pass down the context values, and the Consumer (not used directly in your code) to access those values in child components.
UserContext.Provider

Location: UserContextProvider.js
Purpose: Provides the context value to its child components. The value prop contains the data ({ user, setUser }) that will be available to any component consuming this context.
Usage: Wraps the children components in the UserContextProvider component to make the user state and setUser function available.
Functions to Consume Data
useContext(UserContext)
Location: Login.js and Profile.js
Purpose: Retrieves the current context value for UserContext. This hook allows functional components to access the context data.
Usage:
In Login.js, it extracts the setUser function from the context to update the user state.
In Profile.js, it retrieves the user object from the context to display the username or a login prompt.
Function Calls to Update Context
setUser()
Location: Login.js
Purpose: Updates the user state in the context. This function is called when the form is submitted, setting the user object with the username and password values provided by the user.
Usage: setUser({ username, password }) is called in the handleSubmit function.
Lifecycle
UserContextProvider Component
Location: UserContextProvider.js
Purpose: Initializes the context provider with the user state and setUser function. It makes these available to all components within its subtree.
Usage: Wrapped around Login and Profile components in the App.js file.
In summary, React.createContext() is used to create the context, UserContext.Provider is used to provide the context values, and useContext(UserContext) is used to consume the context values in your components. The setUser function is called to update the context state, which then triggers re-renders in any components that consume this context.
