---
id: app-explanation
title: App.jsx
sidebar_position: 2
---

# `App.jsx` Component

This file defines the main component of the web application, setting up routing and layout structure.

---

### Imports

```javascript
import { BrowserRouter, Route, Routes } from 'react-router-dom';
import './App.css';
import NavBar from './components/navbar';
import HomePage from './Pages/Home/home';
import InfoPage from './Pages/Info/information';
import Gingi from './Pages/Gingi/gingivitis';
import Phenotype from './Pages/Pheno/phenotype';
import Osmf from './Pages/OSMF/osmf';
import Caries from './Pages/Caries/caries';
import MouthOpening from './Pages/Opening/opening';
import SelectionPage from './Pages/Options/options';
import Results from './Pages/Result/results';
import Calculus from './Pages/Calculus/calculus';

import { isBrowser } from 'react-device-detect';
```

- **`BrowserRouter`, `Route`, `Routes`**: Imported from `react-router-dom`, these are used to handle client-side routing in the application.
    - **`BrowserRouter`**: Wraps the entire application to enable routing.
    - **`Route`**: Defines individual routes (pages) in the application.
    - **`Routes`**: Groups multiple Route components.

- **App.css**: Imports the main stylesheet to apply global styles across the app.

- **Component Imports (`NavBar`, `HomePage`, `InfoPage`, etc.)**: These imports bring in different pages and components from the `/components` and `/Pages` directories, each representing a separate part of the app.

- **isBrowser**: Imported from `react-device-detect`, this variable is used to check if the application is being accessed on a desktop browser. It helps conditionally render content specific to browser environments.

---

### App Component

```javascript
function App() {
  return (
    <BrowserRouter>
      <div className="flex flex-col min-h-screen">
        <NavBar />
        <main className="flex flex-col justify-center items-center flex-grow py-20">
          <Routes>
            <Route path="/" element={<HomePage/>}/>
            <Route path="*" element={<h1>Page not found</h1>} />
            <Route path='/info' element={<InfoPage/>}/>
            {isBrowser && <Route path="/opening" element={<MouthOpening/>}/>}
            <Route path='/selection' element={<SelectionPage/>}/>
            <Route path="/osmf" element={<Osmf/>}/>
            <Route path="/gingivitis" element={<Gingi/>}/>
            <Route path="/phenotype" element={<Phenotype/>} />
            <Route path='/calculus' element={<Calculus/>}/>
            <Route path="/caries" element={<Caries/>}/>
            <Route path='/results' element={<Results/>}/>
          </Routes>
        </main>
      </div>
    </BrowserRouter>
  );
}
```
**Explination**

- `function App()`: Defines the main functional component of the application, App.

- `<BrowserRouter>...</BrowserRouter>`: The BrowserRouter component wraps the entire application, enabling routing capabilities.
- `<NavBar />`: The navigation bar component, imported at the top, is rendered here to provide consistent navigation across the app.
- `<main className="flex flex-col justify-center items-center flex-grow py-20">...</main>`: Defines the main content area of the application.

----

### Routes

The `<Routes>...</Routes>` component contains all individual route definitions. Each route specifies a `path` (URL) and a corresponding `element` (component) that will render when the URL matches.

- `<Route path="/" element={<HomePage/>}/>`: Renders the HomePage component at the root URL (/).

- `<Route path="*" element={<h1>Page not found</h1>} />`: A wildcard route (*) that matches any path not specified in the other routes. This displays a “Page not found” message when users navigate to an invalid URL.

- `<Route path='/info' element={<InfoPage/>}/>`: Renders the InfoPage component when the URL is /info.

- `{isBrowser && <Route path="/opening" element={<MouthOpening/>}/>}`: Conditionally renders the `MouthOpening` route only if the application is being accessed from a browser, as determined by `isBrowser`.

- **Additional Routes**:
    - **/selection**: Displays the `Disease Selection Page` component.
    - **/osmf**: Displays the Osmf component.
    - **/gingivitis**: Displays the Gingi component.
    - **/phenotype**: Displays the Phenotype component.
    - **/calculus**: Displays the Calculus component.
    - **/caries**: Displays the Caries component.
    - **/results**: Displays the Generated report.

