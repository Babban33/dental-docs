---
id: InfoPage
title: Information.jsx
sidebar_position: 2
---

# Information Page

The `InfoPage` component is designed as an information-gathering form for users to enter personal details such as name, age, gender, and village. It includes a virtual keyboard for non-mobile devices, allowing input in the `name`, `age`, and `village` fields.

### Imports
```javascript
import React, { useState, useEffect } from "react";
import { useNavigate } from "react-router-dom";
import Keyboard from "react-simple-keyboard";
import "react-simple-keyboard/build/css/index.css";
import content from "../data.json";
import { isMobile } from "react-device-detect";
```
- **React and Hooks**: Utilizes useState and useEffect for managing state and lifecycle events.
- **Navigation**: Uses useNavigate from react-router-dom to handle page redirection.
- **Keyboard**: Imports Keyboard from react-simple-keyboard for a custom virtual keyboard interface.

### State Variables
```javascript
const [name, setName] = useState("");
const [age, setAge] = useState("");
const [gender, setGender] = useState("");
const [village, setVillage] = useState("");
const [isNameVisible, setIsNameVisible] = useState(false);
const [isNumVisible, setIsNumVisible] = useState(false);
const [isVillageVisible, setIsVillageVisible] = useState(false);
const [layoutName, setLayoutName] = useState("default");
const [language, setLanguage] = useState('en');
const navigate = useNavigate();
```
- `name`, `age`, `gender`, `village`: Store user input for respective fields.
- `isNameVisible`, `isNumVisible`, `isVillageVisible`: Control visibility of the virtual keyboard for each field.
- `layoutName`: Controls the layout mode of the virtual keyboard (default or shift).
- `language`: Holds the current language selection, retrieved from localStorage.

### `handleSubmit` Function
```javascript
const handleSubmit = (e) => {
    e.preventDefault();
    if (localStorage.getItem("formData")) {
        localStorage.removeItem("formData");
    }
    if(localStorage.getItem("osmf")){
        localStorage.removeItem("osmf");
    }
    else if(localStorage.getItem("gingivitis")){
        localStorage.removeItem("gingivitis");
    }
    else if(localStorage.getItem("pheno")){
        localStorage.removeItem("pheno");
    }
    else if(localStorage.removeItem("calculus")){
        localStorage.removeItem("calculus");
    }
    else if(localStorage.getItem("caries")){
        localStorage.removeItem("caries");
    }
    
    const formData = {
        name: name,
        age: age,
        gender: gender,
        village: village
    };
    localStorage.setItem("formData", JSON.stringify(formData));
    if (isMobile) {
        navigate("/selection");
    } else {
        navigate("/opening");
    }
    console.log(formData);
};
```
- Clears previous form data and any existing data in localStorage related to diagnostic information (like osmf, gingivitis, etc.).
- Creates an object formData containing name, age, gender, and village values.
- Saves formData to localStorage.
- Navigates to either /selection (on mobile devices) or /opening (on non-mobile devices).

### `toggleNameKeyboard`, `toggleAgeKeyboard`, `toggleVillage`
```javascript
const toggleNameKeyboard = () => {
    if (!isMobile) setIsNameVisible(true);
    setIsNumVisible(false);
    setIsVillageVisible(false);
};
```
- Toggle the visibility of the virtual keyboard for name, age, or village fields.
- These functions also ensure only one keyboard is visible at a time.

