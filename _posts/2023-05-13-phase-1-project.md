---
title: hello world
date: 2023-05-13 12:00:00 -500 
---

# Phase 1 Project Blog

## Introduction

Retaining information can be hard, but if the same material is repeated, anyone can remember the materials. Flashcard can be one of the most important tools when studying, but only seeing definition and vocabulary might deter people from continue to study. So why not mixed something that you like with something that you want to learn? This webpage is designed to help you studying while looking at your favorite picture, associating a concept with an image can help enforcing the idea into your head.

After looking through the public API that we are allowed to use, I found a website that revolved about cat and their network code (https://http.cat/) By entering the nextwork error code after the http link, we will get a picture of a cat with their meaning.

This is the link to my [project](https://github.com/yeah1tnt/phase-1-project-test).

[https://http.cat/100](https://http.cat/100)
![error code 100](https://http.cat/100)

There are a few shortcut that I learned while I figuring out what I wanted to do with my project, for example if you type ! and press tab, the following DOCTYPE will automatically filled out

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    
</body>
</html>
```

## Process

To keep all the data collected on the website, we will be using JSON server to save everything we put into the website. To set up json server, the following command was used. 

```js
npm install -g json-server
```
For the website to work, we also need to start json-server for the website to pull/push data between the two environments.
```js
json-server --watch db.json
```
```json
{
  "catPage": [
    {
      "id": 1,
      "error": "100",
      "content": "continue",
      "url": ""
    },
    {
      "error": "101",
      "content": "Switching Protocols",
      "url": "",
      "id": 2
    }
  ]
}
```
This is my first test of the database. By having the error code as its main identifier and content as their description, we will be able to pull up what we wanted. The url key was added later when I decided to expanding the website's functionality to include custom picture url that any user can add.

The first challenge I encountered was to figure out how to pull data from db.json objects into the array. The following code was my first iteration and it did not work as expected.

```js
fetch("http://localhost:3000/catPage",{
        method: "GET"
    })
        .then(function (response){
            response.json();
        })
        .then(function(data){

            data.forEach(function(cat){
                catErr.push(cat.error);
                catCon.push(cat.content);
                catUrl.push(cat.url);
            });


        })
        .catch(function (error){
            console.log("Something went wrong");
            console.log(error);
        })
```

The first catch was calling for a promise in the db.json, but because it was missing the ```return``` keyword, the promise was not being return so nothing can be pulled from the database. 

```js
        .then(function (response){
            return response.json();
        })
```

Adding more features to the website I decided create a way for a user to add whatever they wanted to as long as both error id and description are both filled out. Before this was added, pressing the submit button would added new blank entries into the database, causing the randomized function to be pull up unwanted data. I added a condition so that if an error is detected when trying to display the image, the index will be removed from the array so it will not be pulled again.

I also need to check if the value is already in the database. A simple duplicate check function was created. The function below compare the input of the user with the array existing array.
```js
function checkDuplicate(arr,input){
   for(let i = 0; i < arr.length; i++){
        if (arr[i] === input){
            return true;
        }
   }
   return false;
}
```

There is another challenge of updating the array after the POST fetch is made. I tried to call the GET fetch again but this would push duplicate values into the array. By adding the value input into the current array, I can avoid calling GET fetch altogether. Below is the final POST condition

```js
if((error.value !=="" && !checkDuplicate(catErr,error.value)) && (description.value !=="" && !checkDuplicate(catCon,description.value)) ){
        fetch("http:localhost:3000/catPage",{
            method: "POST",
            headers: {
                "Content-Type": "application/json"
            },
            body: JSON.stringify({
                "error": error.value,
                "content": description.value,
                "url": url.value
            })

        })
        // Add new POST value into the array
        catErr.push(error.value);
        catCon.push(description.value);
        catUrl.push(url.value);
    }else{
        document.getElementById("status").innerHTML = "Error and description have to be inputted OR entry is already in the database";
    }
```

## Conclusion

Overall this project was pretty fun to make and a good learning experience to apply everything that I had learned throughout the course. Thank you for reading.