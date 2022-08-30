---
title: 'React JS Building an interface'
date: 'August 28, 2022'
excerpt: 'React JS Building an interface'
cover_image: '/images/posts/Git-Collaboration.png'
---

# React installation. 
```
npx create-react-app <app name>
```

after create react project you can go inside folder and run ``` yarn start ``` to start react application. 


Let take a look to see which module will install along with the create-react-app.
- Webpack: is a static module bundler for modern JavaScript applications. It's main job is to manage how our application is assembled and the loading of different module into an application. 
- Babel: Javacript compiler, it will let you use code with latest version and create back ward compatible with older version. So you can till using on the older browser. 
- Es Lint | extension: is a tool for identifying and reporting on patterns found in ECMAScript/JavaScript code. You will need to install the Es lint extension to use this module. 
- Jest: Module for testing the react frame work.
- Web Virtals: is a perfomance monitoring tool for your site. 

## Project Config.

The project have two main folder, the first one is public folder has files that Webpack will manage. In that folder you will see the index.html, inside this file have ``` <div id="root"> </div>``` this position where react js will inject the content of this app to.

The second one is src folder, you will see the index.js file, this in the entry point for javascript application, This fille will import alot of library in to it. 


# Customizing your installs.
## Clean project. 

Remove the unnessesary file in our project. 


