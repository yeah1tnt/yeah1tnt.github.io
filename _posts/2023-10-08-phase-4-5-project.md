---
title: Phase 4/5 Project blog
date: 2023-10-07 12:00:00 -500 
---

# Phase 4/5 Project Blog

## Introduction

My building launched on September 6 and I have been working non-stop. All the free time I had was put into playing videogames to destress myself from all the work and new responsibilities that was threw upon me. The games themselves are the main motivation for the ideas of this project

This project allow user to build their own character with the five common classes that most rpg/mmo have. I also used to play Ragnarok Online as a kid, and really enjoyed their stats allocation system. Although, I might not balance the game very well, some stat might not work as well for other classes



## Process

I kept track of my progress with the logs I put in the readme. This is the link to my [project](https://github.com/yeah1tnt/phase-5-project)

Following the template, I created my python environment with the following command. This is to set up the backend

```
pipenv install flask flask-sqlalchemy flask-migrate sqlalchemy-serializer flask-restful flask-cors faker
```

To set up the frontend, I created the react environment with the following command

```
npx create-react-app -client --use-npm
```

First step I worked on was to set up the models.py for Signup and Login. After it is made, flask is initialized and flask_brcrypt is used to encrypt the password. The command below are used to set up flask and create app.db

```
flask db init
flask db revision --autogenerate -m"init"
flask db upgrade
```

Frontend is a lot easier for me to visualize, I usually make the UI first and see what can be done, but this project give me a more broader idea of being a full-stack developer. Backend was as important as frontend and nothing will work without it. While I was coding I also thought about creating all the logic in backend, but it seem front end was easier for that. React is more versatile than python in term of showing human interface in my opinion.

After finishing up with sign up and sign in. I worked on getting everything separated. Character must be linked to user id and nothing can be see without user logging in.

It was a challenge trying to set up the point allocation system and how to balance what point will be use for what. The button to increase and decrease available point take a long time for me to debug why it's not working as intended. I gave up on having the stat consume more point when it's higher than a certain threshold like many other game, but it did not work as expected.

Another challenge was having the same endpoint for multiple purpose. For delete and put method of fetch you need the character id to pick the right character to change, but when getting all character, no id will be needed. The code below show set up the condition for both. When character_id isn't specified, the method pull all character. This was also use later for situation, where I needed to pull all data into a variable to keep track but filter out certain situation for certain dungeon.

```python
    def get(self, character_id = None):
        if character_id == None:
            if not session['user_id']:
                return {'message': 'Not authorized'}, 401
            user_id = session['user_id']
            character = Character.query.filter_by(user_id=user_id).all()
            if not character:
                return {'message': 'No characters found'}, 404

            character_dict = [character.to_dict() for character in character]
            print(character_dict)
            return character_dict, 200
        
        character = Character.query.filter_by(id=character_id).first()
        character_dict = character.to_dict()
        print(character_dict)
        if not character:
            return {'message': 'Character not found'}, 404
        
        return character_dict, 200
```

After character is made, dungeon and monster followed. The models for them were created first, and this is where I started to work on my seed.py to generate datas that allow me to work on other aspect instead of focusing on making one monster at a time.

When I made my models, the name of the class were Dungeon and Monster. This was interferring with the backend app.py, where I also named them Dungeon and Monster. The error I encounter was ```AttributeError: Object "Dungeon" has no Query attribute``` 
It was using my backend class as my models and made me spend hours troubleshooting in confusion. There are some other simple syntax error and bug that I encounter that really made me question if I'm really want to do this everyday.

Battle logic was next, due to limited time, I made the combat very simple. Only one button to attack and one button to go to the next battle/dungeon.

Lastly I wanted to create encounter and situation where the character can make choices that will impact the gameplay, story and engagement. To create a custom scenerio for every dungeon and every battle, I would have to spend more than a year to get everything ready. I opted out to just get one scenerios work on every dungeon as a concept.


## Conclusion

This project was not as fun to make as the other, but it does give me an idea of what it would be like being a developer. I was not sastified with the final product but deadline exists and doing the best you can is always good.