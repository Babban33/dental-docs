---
id: Navbar
title: NavBar.jsx
sidebar_position: 2
---

# `NavBar.jsx` Component Documentation

The `NavBar` component is a simple navigation bar that displays logos for different organizations associated with the project. This component is responsive, adapting the logo size based on the device (mobile or desktop).

---

### Imports

```javascript
import React from 'react';
import Birac from './birac.png';
import Betic from './betic.jpg';
import SDK from './sdk.png';
import College from './college.png';
import { isMobile } from 'react-device-detect';
```
**Logo Imports `(Birac, Betic, SD Kalmegh (SDK), GHRCE(College))`**: These imports load images from the local project folder to display as logos in the navigation bar.

### JSX Structure

```javascript
return (
  <div className="flex justify-center items-center -mb-16">
    <div className="flex items-center space-x-4 p-4">
      <img src={College} alt="GHRCE Logo" className={logoSize} />
      <img src={Birac} alt="BIRAC Logo" className={logoSize} />
      <img src={SDK} alt="SDK Logo" className={logoSize} />
      <img src={Betic} alt="Betic Logo" className={logoSize} />
    </div>
  </div>
);
```
- **Outer `<div>`**: This container centers the navbar contents.
- **Inner `<div className="flex items-center space-x-4 p-4">`**: Adds horizontal spacing of 1rem between each logo. Adds padding for visual balance around the logos.
- **Logo `<img>` Tags**: Each logo is rendered as an `<img>` element with a src attribute pointing to the corresponding image import and an alt text description.