---
title: Phase 2 Project blog
date: 2023-07-01 12:00:00 -500 
---

# Phase 2 Project Blog

## Introduction

Life had been pretty rough for me. Recently started a new position and moved to night shift and work by myself. I sometime regret moving into this role but it is a change of pace and a new challenge for my career. I did not have much time to polish my project, I wanted to add a dictionary function to this project as well but time got out of hand.

There are a lot of public API and I found one to generate random words that I never seen before. Bewilered by my lack of vocabulary I decided to make my project around this and make a game out of it to test my memory.

This is the link to my [project](https://yealt-flatiron-project2.netlify.app/). Deployed with Render for backend and Netlify for Front end.

The first thing after I recreated my new repository is to initialize React

```js
npm init react-app phase-2-project
```

This command needed a directory, so I had to "```cd .. ```" back to my phase-2 class folder to use. 

The next step was to set up my json server using the same command I used last project

```js
npm install -g -json-server
```

After having both of these component set up, I noticed that react server would be running on ```localhost:3000``` and will interfere with my json server file. I decided to set my json server to port 4000 and used a ```npm run``` command to start the server instead of writing out the whole thing

```js
"server": "json-server --watch db.json --port=4000"
```

The last thing of my setup process was to set up react-router-dom

```js
npm install react-router-dom
```


## Process

The first thing I did was to test my navigation using react-router-dom. ```<Link>``` allowed us to move between different url link and ```<Route>``` allowed us to implement what that url link will include. Clicking on Home link will return us to localhost:3000/ and clicking on About will take us to localhost:3000/About
```js
import { BrowserRouter as Router,Routes, Route,Link } from 'react-router-dom';

import HomePage from './Home';
import AboutPage from './About';

function App() {
  
  return (
    <Router>
      <div className="App">
        <nav>            
              <Link to="/"> Home </Link>
              <Link to="/About"> About </Link>         
        </nav>
      <Routes>
        <Route path="/" element={<HomePage></HomePage>}></Route>
        <Route path="/About" element={<AboutPage></AboutPage>}></Route>
      </Routes>


        <header className="App-header">
        </header>
      </div>

    </Router>
  );
}

export default App;
```

Next I decided to get fetch GET to work, grabbing the data from json file and set it to an ```useState``` variable.

A ```useState``` variable has 2 value, one hold the value that will be set by the other. In the code below, existingWords will be holding the data pulled from db.json file, and setExistingWords set those value into the variable.

```${apiURL}``` is used here instead of localhost:4000 because it was deployed into Netlify, discussed later in this blog.

Keep in mind that this project will only take words! If number are inputted, it will returned a runtime error when fetch is called because ```toLowerCase()``` does not support numbers.

```js

    const [existingWords, setExistingWords] = useState([]);

    const fetchExistingWords = () => {
        fetch(`${apiURL}/words`)
            .then((r) => r.json())
            .then((data) => {
                setExistingWords(data.map((wordObj) => wordObj.word.toLowerCase()));
            })
        };
```

I decided that spamming the submit button might be an issue because it will add the same words into the database. I created a logic to detect if words submitting and the existingWords matches. If it's true, certain useState variable will be set true and this will be contribute to the message display after the submit button is pressed.
```js
 // Check if the word already exists in the database
        if (existingWords.includes(words.toLowerCase())) {
            setDuplicateError(true); //Set duplicate error
            setSuccess(false); //Clear success
            setEmpty(false); //Clear empty
            setWords(""); //Clear input
            return;
        }
        if(words === ""){
            setEmpty(true); //Set empty
            setSuccess(false); //Clear success
            setDuplicateError(false); // Clear duplicate error
            return;
        }
```
Below code returned react components. Messages are displayed based on whether the words are successfully submitted.

If duplicate is detected while fetching, the ```duplicateError``` boolean become true and the red message will appear.
```js
    return (
        <form className="words" onSubmit={handleSubmit}>
            <label>
                Word:
                <input type="text" value={words} onChange={handleNewWord} />
            </label>
            <button type="submit">Add Word</button>

            //Logics for messages
            {duplicateError && <p style={{ color: "red"}}>Word already exists!</p>}
            {empty && <p style={{ color: "red"}}>Enter a word before submit!</p>}
            {success && <p style={{color: "green"}}>Word added!</p>}
        </form>
    );
```


After established a baseline on gathering data from json-server. I set up another page to show list of all the words in the database.
```deleteWord``` is a function that used fetch delete method and remove the words based on their id. This work by setting a new object with all the previous data EXCEPT the word we want to delete.

```js

    const deleteWord = (id) => {

        fetch(`${apiURL}/words/${id}`,{
            method: "DELETE",
        })
        .then((r)=>r.json())
        .then(()=>{
            setWords((data)=>data.filter((w)=> w.id !== id))
        })
    }

    const listWords = words.map((word) => (
        <li key={word.id}>
            {word.word}
            <button onClick={()=>deleteWord(word.id)}>Delete</button>
        </li>
    ))

    return (
        <div>
        <ul>{listWords}</ul>
        </div>
    )
```

Now we can submit word and delete them from the database. I decided to add some other function to make the site more interesting. A random word generator API was used to pulled new words onto the website and allowed user to add it to the database.

A word guessing game where the user can guess whether the word is from the database or from the api, returning correct or incorrect message.

If time permit I will come back and add a dictionary functionality and potentially a points system for different users based on the name they put in before playing

## Deploy the website
Using Netlify, I was able to host the front-end website

Using Render, I was able to create a web server for my db.json that allowed my Netlify to pull and add data to.

In the site configuration, I needed to added an environment variables that allowed us to use different site based on the name of the variable. For this project, ```REACT_APP_BACKEND_API_URL``` is the variable I used to set the json-server link.

Last modification to the back-end code is to changed all of my localhost:4000/ to ```${apiURL}```
```js
const apiURL = process.env.REACT_APP_BACKEND_API_URL

//Example of how the environment varialbe is used.
const handleAddWord = () => {
        fetch(`${apiURL}/words`,{
            method: "POST",
            headers: {
                "Content-Type":"application/json",
            },
            body: JSON.stringify({word}) //Add word into the database
        })
}
```



![https://i.imgur.com/s3KsmJH.png](https://i.imgur.com/s3KsmJH.png)


## Conclusion

React certainly make creating a website much easier and more interactive than normal html. I enjoy doing this project much more than the first project. I still not sure what I'm actually need to do for these blogs but it's a good reflection for myself on what I did throughout the project and re-teaching myself of what I ahd done.