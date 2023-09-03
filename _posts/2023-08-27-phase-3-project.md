---
title: Phase 3 Project blog
date: 2023-08-27 12:00:00 -500 
---

# Phase 3 Project Blog

## Introduction

Travelling for a new role, opening a new building for Amazon, and getting trained on completely new equipment weighted heavily on my mind over the last month. Our new site launched mid-september and everything seem so unpolished and unfinished that I already felt the stress of when the building launched.

This project really helped me escaped the work and the stress I felt at work, as well as the weird sickness that just appear out of nowhere and destroyed me physically. Then a hurricane came by and say hello, gave me 2 days off work rest and recovered, if only this hurricane came a week earlier and give me more time on this project.

My project built upon my [last project](https://yealt-flatiron-project2.netlify.app), where I made a flashcard generator. Instead of just adding words to the database, I used a public API to pulled definition based on the word inputted and added them to the database. Here is the link to my [phase 3 project](https://github.com/yeah1tnt/phase-3-project)

## Process

Following the template set up by the school, I created my python envinronment with the necessary libraries

```python
pipenv install sqlalchemy alembic
```

sqlalchemy is used in this project as database management and alembic is used as a database migration. Later on, faker was used to generate random word and seed the first iteration of my database and ipdb was used to troubleshoot input and variables.

```
|- Pipfile
|- Pipfile.lock
|- README.md
|- lib
    |- cli.py
    |- debug.py
    |- helpers.py
    |- db
        |-models.py
        |-seed.py
```

This is the project tree where pipfile will track packages installed, cli.py will be the main file where our interface is coded, helpers.py will be the sub file with all our function called within cli and models.py file set up our template of the database.

My first step was to create a flow for my CLI. This later get more and more complicated as more ideas come to mind and too much of a change would changes the whole structure, in the end I decided to stick with the final iternation of the tree flow

```
|- Ask for username
    |               (|- Register username (if user/name isn't in db))
    |- Access user data
        |= Show all users and their score
        |- Reset current user's score
    |- Access dictionary ()
    |   |- List all word/definition
    |   |- Add word manually
    |       |- Choose adjective
    |   |- Add word randomly
    |       |- Choose adjective
    |   |- Delete word
    |       |- Enter word to delete
    |   |- Search word
    |- Play dictionary game
        |- Show score
        |- Play game
```

The final product did not look completely the same as this. Some changes were made to just fixed bug and overal ease of use for the user/player. Add word randomly now generate a random word and pick its own part of speech instead of user input. Search word wasn't in there because there is already an option to show all word.

To combat duplicate entry, I decided that only one word can be in the database. Word with different part of speech can have different meaning and it would confuse the user when they play the game

To continue keeping the loop going, I used user_input to kept it going.

```python
    def main(self):
        user_input = input("Enter your username: ")
        user_input = user_input.lower()
        if user_input in self.user_name:
            print(f"\nWelcome, {user_input}!")
        else:
            addUser_0(session, user_input)

        while user_input:
            print("\n       MAIN MENU")
            print("\nEnter a to access user settings")
            print("Enter b to access the dictionary")
            print("Enter c to start dictionary game")
            print("Enter e to exit")
            user_choice = input("\nEnter your choice: ")
            if user_choice.lower() == "a":
                myCLI.userAccess_01(self, user_input)
            elif user_choice.lower() == "b":
                myCLI.wordAccess_02(self, user_input)
            elif user_choice.lower() == "c":
                myCLI.gameAccess_03(self, user_input)
            elif user_choice.lower() == "e":
                exit()
            else:
                print("\nInvalid input. Please try again")
```

Above is the main menu code. ```user_input``` in this case is the user_name. While user_name is true, the loop will continue going forever until ```user_choice``` is equal to e. I wanted to change ```user_input``` to just ```user_name``` but because I already wrote most half of the code with this variable name, I decided to kept it.

```user_input``` here is use in almost every other loop, my main justification for this is for the program to keep track of who is accessing the program. This allow ```user_input``` (username) to pulled relevant score, reset score, and modify score when they finally decided to play the game.

Let go through the main logic of picking the word and fake choices

```python
    dict = [dict.word for dict in session.query(Dictionary).all()]
    answer = choice(dict)
    answer_def = session.query(Dictionary).filter(Dictionary.word == answer).first().definition
    answer_choice = [answer_def]
    while len(answer_choice) < 4:
        extra = choice(dict)
        extra_def = session.query(Dictionary).filter(Dictionary.word == extra).first().definition
        if extra_def not in answer_choice:
            answer_choice.append(extra_def)
    random.shuffle(answer_choice)
```

dict created an array with all of the word in Dictionary table, this array will then go into ```answer```, where choice() from random will pick one word in the array. ```answer_def``` is the definition of the word by filtering out the word. ```answer_choice``` is the array that will print out with all option the user can pick, the while function generate fake answer choice and will only append if the definition are not already in the array. ```random.shuffle``` is a random function that randonmized the order in an array, this allow the game to have ranndom element with the choice and kept track of their location.


## Conclusion

Every menu pages allow the user to go back to the main menu or exit the program. I did not want the user to feel stuck in one option, and also have the option to quit at any point. One of the feature I forgot to implement is the level system. Every user started at level 1 when their name is registered, each level up reward can be given out or more feature can be unlock.

This project was fun to make, python is definitely one of the easier language to learn and code in. 