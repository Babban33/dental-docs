---
id: options
title: Options.jsx
sidebar_position: 3
---

# `Options.jsx` Page

```javascript
<button onClick={() => navigate('/osmf')} className='m-2 bg-indigo-500 hover:bg-indigo-600 text-white font-bold py-3 px-6 rounded-full'>{content["Select"][language].dis1}</button>
<button onClick={() => navigate('/gingivitis')} className='m-2 bg-indigo-500 hover:bg-indigo-600 text-white font-bold py-3 px-6 rounded-full'>{content["Select"][language].dis2}</button>
<button onClick={() => navigate('/phenotype')} className='m-2 bg-indigo-500 hover:bg-indigo-600 text-white font-bold py-3 px-6 rounded-full'>{content["Select"][language].dis3}</button>
<button onClick={() => navigate('/calculus')} className='m-2 bg-indigo-500 hover:bg-indigo-600 text-white font-bold py-3 px-6 rounded-full'>{content["Select"][language].dis4}</button>
<button onClick={() => navigate('/caries')} className='m-2 bg-indigo-500 hover:bg-indigo-600 text-white font-bold py-3 px-6 rounded-full'>{content["Select"][language].dis5}</button>
```
The SelectionPage.jsx component is located in `/src/Pages/Info/` and represents a selection page that allows users to choose from different options related to various disease detection modules.