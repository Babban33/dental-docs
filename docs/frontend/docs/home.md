---
id: HomePage
title: HomePage.jsx
sidebar_position: 1
---

# `HomePage.jsx` Component

This page contains the code for Landing Page of webapp.

----

### Imports

```javascript
import React, { useEffect, useState } from "react";
import NavButton from "../../components/btn";
import content from "../data.json";
import { isMobile } from 'react-device-detect';
import { useNavigate } from 'react-router-dom';
```

- **setLanguage**: Function to update the language state.
- **navigate**: Function from useNavigate to handle page navigation.

### Component Definition

```javascript
function HomePage() {
  const [language, setLanguage] = useState(localStorage.getItem('lang') || 'en');
  const navigate = useNavigate();
```
- **function HomePage()**: Defines the main functional component.
- **language**: A state variable that holds the currently selected language, initialized with the language stored in localStorage or defaults to **'en'** (English).
- **setLanguage**: A function to update the language.
- **navigate**: Allows the component to navigate to other pages when needed.

```javascript
useEffect(() => {
  localStorage.setItem('lang', language);
}, [language]);
```
This hook runs whenever the `language` state changes, updating the `localStorage` to save the user’s preferred language.

### `handleLanguageChange` Function
```javascript
const handleLanguageChange = (event) => {
  const newLanguage = event.target.value;
  setLanguage(newLanguage);
  localStorage.setItem('lang', newLanguage);
};
```
Updates the language state and localStorage with the new language selected by the user from a dropdown.

### `handleButtonClick` Function

```javascript
const handleButtonClick = async () => {
  try {
    const stream = await navigator.mediaDevices.getUserMedia({ video: true });
    if (stream) {
      stream.getTracks().forEach(track => track.stop());
      navigate('/info');
    }
  } catch (error) {
    console.error('Camera access denied:', error);
    alert("Camera access is required to proceed.");
  }
};
```
Asynchronously requests access to the user’s camera. If access is granted, the video stream is stopped immediately, and the user is navigated to the `/info` route. If access is denied, an alert prompts the user that camera access is required.


### Language Selector

```html
<select
    className={`border border-gray-300 rounded-md p-2 bg-white text-gray-700 ${isMobile ? 'w-full' : ''}`}
    value={language}
    onChange={handleLanguageChange}
>
    <option value="en">English</option>
    <option value="hi">Hindi</option>
    <option value="mar">Marathi</option>
</select>
```
This element is the dropdown selector for preferred language. After selection it writes the prefered language to `localStorage`.