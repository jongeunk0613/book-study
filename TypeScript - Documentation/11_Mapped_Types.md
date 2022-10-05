# Mapped Types

A mapped type is a generic type which uses a union of **PropertyKeys** (frequently created via a `keyof`)  
to iterate through keys to create a type.  

```javascript
type OptionsFlags<Type> = {
  [Property in keyof Type]: boolean;
};
```

They are build on the syntax for **index signatures**,  
which are used to declare the types of properties which have not been declared ahead of time.  

```javascript
type OnlyBoolsAndHorses = {
  [key: string]: boolean | Horse;
};
 
const conforms: OnlyBoolsAndHorses = {
  del: true,
  rodney: false,
};
```

ex: `OptionsFlags` will take all the properties from the type `Type` and change their values to be a `boolean`.  

```javascript
type OptionsFlags<Type> = {
  [Property in keyof Type]: boolean;
};

type FeatureFlags = {
  darkMode: () => void;
  newUserProfile: () => void;
};
 
type FeatureOptions = OptionsFlags<FeatureFlags>;      
// type FeatureOptions = {
//     darkMode: boolean;
//     newUserProfile: boolean;
// }
```

<br/>

## Mapping Modifiers















