# 02 - Chapter 2 Quests

<img src="https://i.pinimg.com/originals/f7/d1/6b/f7d16bfe87638d58a38f27b7aeb00f4c.jpg" alt="drawing" width="150"/>

## Chapter 2 Day 1 Quest 1: Deploy a contract to account 0x03 called "JacobTucker"
- Deploy a contract to account 0x03 called "JacobTucker". 
- Inside that contract, declare a constant variable named is, and make it have type String. 
- Initialize it to "the best" when your contract gets deployed.

<img src="https://raw.githubusercontent.com/bz-hashtag-0780/Flow-Zero-to-Jacob/main/quests/screenshots/CH2D1Q1.png" alt="drawing" width="600"/>

## Chapter 2 Day 1 Quest 2: Check that your variable is actually equals "the best".
- Check that your variable is actually equals "the best" by executing a script to read that variable. 
- Include a screenshot of the output.

<img src="https://raw.githubusercontent.com/bz-hashtag-0780/Flow-Zero-to-Jacob/main/quests/screenshots/CH2D1Q2.png" alt="drawing" width="600"/>

## Chapter 2 Day 2 Quest 1: Explain why we wouldn't call changeGreeting in a script

changeGreeting is a function to modify data which requires running a transaction whereas scripts are used solely for reading data

## Chapter 2 Day 2 Quest 2: What does the AuthAccount mean in the prepare phase of the transaction?

When a transaction gets access to an AuthAccount the transaction will have full access to its data such as storage and public keys.

## Chapter 2 Day 2 Quest 3: What is the difference between the prepare phase and the execute phase in the transaction?

Prepare phase: To access AuthAccount data
Execute phase: To call functions

## Chapter 2 Day 2 Quest 4

- Add two new things inside your contract:
  - A variable named myNumber that has type Int (set it to 0 when the contract is deployed)
  - A function named updateMyNumber that takes in a new number named newNumber as a parameter that has type Int and updates myNumber to be newNumber

<img src="https://raw.githubusercontent.com/bz-hashtag-0780/Flow-Zero-to-Jacob/main/quests/screenshots/CH2D2Q4.1.png" alt="drawing" width="600"/>

- Add a script that reads myNumber from the contract
- Add a transaction that takes in a parameter named myNewNumber and passes it into the updateMyNumber function. Verify that your number changed by running the script again.

<img src="https://raw.githubusercontent.com/bz-hashtag-0780/Flow-Zero-to-Jacob/main/quests/screenshots/CH2D2Q4.2.png" alt="drawing" width="600"/>
Script to read myNumber

<img src="https://raw.githubusercontent.com/bz-hashtag-0780/Flow-Zero-to-Jacob/main/quests/screenshots/CH2D2Q4.3.png" alt="drawing" width="600"/>
Transaction for call updateMyNumber function

<img src="https://raw.githubusercontent.com/bz-hashtag-0780/Flow-Zero-to-Jacob/main/quests/screenshots/CH2D2Q4.4.png" alt="drawing" width="600"/>
Check if function works.


## Chapter 2 Day 3 Quest 1: In a script, initialize an array (that has length == 3) of your favourite people, represented as `String`s, and `log` it.

<img src="https://raw.githubusercontent.com/bz-hashtag-0780/Flow-Zero-to-Jacob/main/quests/screenshots/CH2D3Q1.png" alt="drawing" width="600" />

## Chapter 2 Day 3 Quest 2: In a script, initialize a dictionary that maps the `String`s Facebook, Instagram, Twitter, YouTube, Reddit, and LinkedIn to a `UInt64` that represents the order in which you use them from most to least. For example, YouTube --> 1, Reddit --> 2, etc. If you've never used one before, map it to 0!

<img src="https://raw.githubusercontent.com/bz-hashtag-0780/Flow-Zero-to-Jacob/main/quests/screenshots/CH2D3Q2.png" alt="drawing" width="600" />

## Chapter 2 Day 3 Quest 3: Explain what the force unwrap operator `!` does, with an example different from the one I showed you (you can just change the type).

It is used to unwrap an optional to its actual type. So `String?` becomes `String`

<img src="https://raw.githubusercontent.com/bz-hashtag-0780/Flow-Zero-to-Jacob/main/quests/screenshots/CH2D3Q3.png" alt="drawing" width="600" />

## Chapter 2 Day 3 Quest 4: Using this picture below, explain...
    - What the error message means
    - Why we're getting this error
    - How to fix it
<img src="https://raw.githubusercontent.com/bz-hashtag-0780/Flow-Zero-to-Jacob/main/chapter2.0/images/wrongcode.png" alt="drawing" width="600" />

The error message says that it expected a String but instead it got a String optional (String or nil)
When dictionaries are accessed, the values returned are optionals. Meaning it might be nil or in the above case a String. The force-unwrap operator `!` must be used to return the actual String type and not just the optional.
It should look like the following

<img src="https://raw.githubusercontent.com/bz-hashtag-0780/Flow-Zero-to-Jacob/main/quests/screenshots/CH2D3Q4.png" alt="drawing" width="600" />
