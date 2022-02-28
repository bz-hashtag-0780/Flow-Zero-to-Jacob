# 02 - Chapter 2 Quests

<img src="https://i.pinimg.com/originals/f7/d1/6b/f7d16bfe87638d58a38f27b7aeb00f4c.jpg" alt="drawing" width="150"/>

## Chapter 2 Day 1 Quest 1: Deploy a contract to account 0x03 called "JacobTucker"
- Deploy a contract to account 0x03 called "JacobTucker". 
- Inside that contract, declare a constant variable named is, and make it have type String. 
- Initialize it to "the best" when your contract gets deployed.


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
- Add a script that reads myNumber from the contract
- Add a transaction that takes in a parameter named myNewNumber and passes it into the updateMyNumber function. Verify that your number changed by running the script again.
