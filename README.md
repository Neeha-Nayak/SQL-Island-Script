**Narrator:
This is you!
After the survived plane crash, you will be stuck on SQL Island for the time being. By making progress in the game, you will find a way to escape from this island.**

**This field is where you write your commands. The complete game will be controlled by means of the database language SQL.
You don't know anything about SQL commands yet? No problem. You will find out all about them here!
Down here you find all tables which you can access: village, inhabitant and item. Each of these tables include several columns (in brackets). You will require this information throughout the entire game!**

**Tables are :**
        1.	VILLAGE (villageid, name, chief)
        2.	INHABITANT (personid, name, villageid, gender, job, gold, state)
        3.	ITEM (item, owner)
   
**In case you want to restart the game or replay the game instructions, just click on this menu button.
Me:	In case you want to restart the game or replay the game instructions, just click on this menu button.
Oh dear, what happened? It seems that I am the only survivor of the air crash. Wow, there are some villages on this island.**

SELECT *
FROM village

**It seems there are a few people living in these villages. How can I see a list of all inhabitants?**

SELECT *
FROM INHABITANT

**Woah, so many people!
Man! I'm hungry. I will go and find a butcher to ask for some free sausages.**

SELECT * 
FROM inhabitant 
WHERE job = 'butcher'

**There you are! Enjoy your meal! But take care of yourself. As long as you are unarmed, stay away from villains. Not everyone on this island is friendly.
Thank you, Edward! Okay, let's see who is friendly on this island...**

SELECT * 
FROM INHABITANT 
WHERE state = 'friendly'

**There is no way around getting a sword for myself. I will now try to find a friendly weaponsmith to forge me one. (Hint: You can combine predicates in the WHERE clause with AND)**

SELECT * 
FROM INHABITANT 
WHERE state = 'friendly' AND job = 'weaponsmith'

**Oh, that does not look good. Maybe other friendly smiths can help you out, e.g. a blacksmith. Try out: job LIKE '%smith' to find all inhabitants whose job ends with 'smith' (% is a wildcard for any number of characters).**

SELECT * 
FROM INHABITANT 
WHERE state = 'friendly' AND job LIKE '%smith'

**That looks better! I will go and visit those smiths.
Paul the Major of Monkeycity enter
Paul: Hi stranger! Where are you going? I'm Paul, I'm the major of Monkeycity. I will go ahead and register you as a citizen.**
INSERT INTO 
inhabitant (name, villageid, gender, job, gold, state) 
VALUES 
('Stranger', 1, '?', '?', 0, '?')

**No need to call me stranger! What's my personid? (Hint: Use a SELECT query without an asterisk. In former queries, the * stands for: all columns. Instead of the star, you can also address one or more columns (seperated by a comma) and you will only get the columns you need.)**
SELECT personid 
FROM INHABITANT 
WHERE name = 'Stranger'

**I  went to a friendly weaponsmith, Ernest
Me: Hi Ernest! How much is a sword?
Ernest: I can offer to make you a sword for 150 gold. That's the cheapest you will find! How much gold do you have?**

SELECT gold 
FROM INHABITANT 
WHERE personid = 20

**Me: Damn! No mon, no fun. There has to be another option to earn gold other than going to work. Maybe I could collect ownerless items and sell them! Can I make a list of all items that don't belong to anyone? (Hint: You can recognize ownerless items by: WHERE owner IS NULL)**

SELECT * 
FROM ITEM
WHERE owner IS NULL

**So much cool stuff!
Yay, a coffee cup. Let's collect it!**

UPDATE item 
SET owner = 20 
WHERE item = 'coffee cup'

**Do you know a trick how to collect all the ownerless items?**

UPDATE ITEM
SET owner = 20
WHERE owner IS NULL

**Now list all of the items I have!**

SELECT * 
FROM ITEM 
WHERE owner = '20'

**Find a friendly inhabitant who is either a dealer or a merchant. Maybe they want to buy some of my items. (Hint: When you use both AND and OR, don't forget to put brackets correctly!)**

SELECT *
FROM INHABITANT
WHERE 
    state = 'friendly'
    AND
    (job = 'dealer' or job = 'merchant');

**I went to Helen Grasshead
Helen: I'd like to get the ring and the teapot. The rest is nothing but scrap. Please give me the two items. My personid is 15.**

UPDATE ITEM 
SET owner = 15 
WHERE item = 'ring' or item = 'teapot'

**Helen : Thank you
Here, some gold!**

UPDATE inhabitant 
SET gold = gold + 120 
WHERE personid = 20

**Me: Unfortunately, that's not enough gold to buy a sword. Seems like I do have to work after all. Maybe it's not a bad idea to change my name from Stranger to my real name before I will apply for a job.**

UPDATE INHABITANT
SET name = 'Krish'
WHERE personid = 20

**Since baking is one of my hobbies, why not find a baker who I can work for? (Hint: List all bakers and use 'ORDER BY gold' to sort the results. 'ORDER BY gold DESC' is even better because then the richest baker is on top.)**

SELECT *
FROM INHABITANT
WHERE job = 'baker' 
ORDER BY gold DESC

**Helen: Aha, Paul! I know him!
I went to meet Paul
Paul: Hi, you again! So, Krish is your name. I saw you want to work as a baker? Okay! You will be paid 1 gold for 100 bread rolls.
Me: (8 hours later...) Here, I made ten thousand bread rolls! I quit! This should be enough money to buy a sword. Let's see what happens with my gold balance.**

UPDATE inhabitant 
SET gold = gold + 100 - 150 
WHERE personid = 20

**Paul: Here's your new sword, Krosh! Now you can go everywhere.
Me: My name is Krish! Thanks anyway!
Is there a pilot on this island by any chance? He could fly me home**.

SELECT *
FROM INHABITANT
WHERE job = 'pilot'

**Oh no, his state is 'kidnapped'.
Paul: Horrible, the pilot is held captive by Dirty Dieter! I will show you a trick how to find out the name of the village where Dirty Dieter lives.
The expression presented here is called a join. It combines the information of the inhabitant table with information of the village table by matching villageid values.**

SELECT village.name
FROM 
    village, 
    inhabitant 
WHERE
    village.villageid = inhabitant.villageid 
    AND
    inhabitant.name = 'Dirty Dieter'

**Thanks for the hint! I can use the join to find out the chief's name of the village Onionville. (Hint: In the column 'chief' in the village table, the personid of the chief is stored)**

SELECT INHABITANT.name
FROM VILLAGE, INHABITANT
WHERE 
    VILLAGE.villageid = INHABITANT.villageid
    AND
    VILLAGE.name = 'Onionville'

**I've got it! I will visit Fred and ask him about Dirty Dieter and the pilot.
Um, how many inhabitants does Onionville have?**

SELECT COUNT(*) 
FROM 
    inhabitant, 
    village 
WHERE 
    village.villageid = inhabitant.villageid 
    AND 
    village.name = 'Onionville'

**Fred: Hello Krish, the pilot is held captive by Dirty Dieter in his sister's house. Shall I tell you how many women there are in Onionville? Nah, you can figure it out by yourself! (Hint: Women show up as gender = 'f')**
SELECT COUNT(*)
FROM VILLAGE, INHABITANT
WHERE 
    VILLAGE.villageid = INHABITANT.villageid
    AND
    VILLAGE.name = 'Onionville'
    AND 
    INHABITANT.gender = 'f'

**Oh, only one woman. What's her name?**

SELECT INHABITANT.name
FROM VILLAGE, INHABITANT
WHERE 
    VILLAGE.villageid = INHABITANT.villageid
    AND
    VILLAGE.name = 'Onionville'
    AND 
    INHABITANT.gender = 'f'

**Let’s go!
Krish, if you hand me over the entire property of our nearby village Cucumbertown, I will release the pilot. I will show you now what this property consists of.**

SELECT SUM(inhabitant.gold) 
FROM
    inhabitant, 
    village 
WHERE 
    village.villageid = inhabitant.villageid
    AND 
    village.name = 'Cucumbertown'

**Oh no, baking bread alone can't solve my problems. If I continue working and selling items though, I could earn more gold than the worth of gold inventories of all bakers, dealers and merchants together. How much gold is that?**
SELECT SUM(Inhabitant.gold)
FROM INHABITANT, VILLAGE
WHERE 
    Village.name = "Cucumbertown" 
    AND 
    (Inhabitant.job = "merchant" or Inhabitant.job = "baker" or Inhabitant.job = "dealer");

**That's not enough.
Let's have a look at how much average gold people own, depending on their job.**

SELECT 
    job, 
    SUM(inhabitant.gold), 
    AVG(inhabitant.gold) 
FROM inhabitant 
GROUP BY job 
ORDER BY AVG(inhabitant.gold)

**Very interesting: For some reason, butchers own the most gold. How much gold do different inhabitants have on average, depending on their state (friendly, ...)?**

SELECT 
    state, 
    AVG(gold)
FROM INHABITANT
GROUP BY state
ORDER BY gold DESC

**Ok, so the only way is to mug the villains.
Or I might as well go ahead and just kill Dirty Dieter with my sword!**

DELETE FROM inhabitant
WHERE name = 'Dirty Dieter'

**Dirty Diane:  Heeeey! Now I'm very angry! What will you do next, Krish?**

DELETE FROM INHABITANT 
WHERE name = 'Dirty Diane'

**Yeah! Now I release the pilot!**

UPDATE INHABITANT
SET state = "friendly"
WHERE job = "pilot"

**Thank's for releasing me, Krish! I will fly you home!
I take my sword, some gold and lots of useless items with me as a souvenir. What a big adventure!
The game is over. Get your certificate of completion now! If you want to change the name on the certificate, use an UPDATE command on the inhabitants table.**
