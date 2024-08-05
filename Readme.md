
# useApi Hook

The `useApi` hook is a custom React Native hook designed to handle API calls with built-in state management for loading, error handling, and data manipulation.

## Installation

npm i useApi
## Usage

```javascript
import useApi from 'path-to-your-hook';
import { fetchSomeData } from 'path-to-your-api-calls';

const MyComponent = () => {
  const { data, loading, error, fetchData, alterData } = useApi({
    apiCallingFunction: fetchSomeData,
    apiParameters: [/* your parameters here */],
    runOnTimeOfScreenMount: true, // optional
    initialLoadingState: true,    // optional
    initialData: [],              // optional
    reFetchDependencies: [/* dependencies for refetching */] // optional
  });

  return (
    <View>
      {loading && <Text>Loading...</Text>}
      {error && <Text>Error: {error}</Text>}
      <Text>Data: {JSON.stringify(data)}</Text>
      <Button title="Refetch Data" onPress={() => fetchData(true, [/* new parameters */])} />
    </View>
  );
};

export default MyComponent;
```

## Arguments

### `apiCallingFunction`

- **Type:** `(...params: any[]) => Promise<any>`
- **Description:** The function that makes the API call. It should return a promise.
- **Required:** Yes

### `apiParameters`

- **Type:** `any[]`
- **Description:** Parameters to be passed to the `apiCallingFunction`.
- **Required:** Yes

### `apiCustomReturnFunction`

- **Type:** `(data: any) => any`
- **Description:** Custom function to transform the API response data before updating the state.
- **Required:** No

### `runOnTimeOfScreenMount`

- **Type:** `boolean`
- **Description:** If true, the API call will be made when the component mounts.
- **Required:** No

### `initialLoadingState`

- **Type:** `boolean`
- **Description:** Initial loading state of the hook.
- **Required:** No

### `initialData`

- **Type:** `any`
- **Description:** Initial data state of the hook.
- **Required:** No

### `reFetchDependencies`

- **Type:** `any[]`
- **Description:** Dependencies array for re-fetching data when any of these dependencies change.
- **Required:** No

## Example

```javascript
import React, { useEffect } from 'react';
import { View, Text, Button } from 'react-native';
import useApi from 'path-to-your-hook';
import { fetchUserData } from 'path-to-your-api-calls';

const UserProfile = () => {
  const { data, loading, error, fetchData, alterData } = useApi({
    apiCallingFunction: fetchUserData,
    apiParameters: [userId],
    runOnTimeOfScreenMount: true,
    initialLoadingState: true,
    initialData: {},
    reFetchDependencies: [userId]
  });

  useEffect(() => {
    if (error) {
      console.error('API Error:', error);
    }
  }, [error]);

  return (
    <View>
      {loading && <Text>Loading user profile...</Text>}
      {error && <Text>Error loading profile: {error}</Text>}
      <Text>User Data: {JSON.stringify(data)}</Text>
      <Button title="Reload Profile" onPress={() => fetchData(true, [userId])} />
    </View>
  );
};

export default UserProfile;
```

## Functions Returned by the Hook

### `fetchData`

- **Description:** Function to manually trigger the API call. Optionally, you can pass new parameters and a custom return function.
- **Arguments:**
  - `showLoader` (boolean): If true, sets loading state before making the API call.
  - `params` (any[]): Optional new parameters for the API call.
  - `customReturnFunc` ((data: any) => any): Optional custom return function to process the response data.

### `alterData`

- **Description:** Function to manually alter the data state.
- **Arguments:**
  - `data` (any): The new data to set in the state.

## Contributing
Created by Chandan Nath
