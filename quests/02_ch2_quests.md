# 02 - Chapter 2 Quests

<img src="https://i.pinimg.com/originals/f7/d1/6b/f7d16bfe87638d58a38f27b7aeb00f4c.jpg" alt="drawing" width="150"/>

## Chapter 2 Day 1

### CH2D1 Quest 1: Deploy a contract to account 0x03 called "JacobTucker"
- Deploy a contract to account 0x03 called "JacobTucker". 
- Inside that contract, declare a constant variable named is, and make it have type String. 
- Initialize it to "the best" when your contract gets deployed.

<img src="https://raw.githubusercontent.com/bz-hashtag-0780/Flow-Zero-to-Jacob/main/quests/screenshots/CH2D1Q1.png" alt="drawing" width="600"/>

### CH2D1 Quest 2: Check that your variable is actually equals "the best".
- Check that your variable is actually equals "the best" by executing a script to read that variable. 
- Include a screenshot of the output.

<img src="https://raw.githubusercontent.com/bz-hashtag-0780/Flow-Zero-to-Jacob/main/quests/screenshots/CH2D1Q2.png" alt="drawing" width="600"/>

### CH2D2 Quest 1: Explain why we wouldn't call changeGreeting in a script

changeGreeting is a function to modify data which requires running a transaction whereas scripts are used solely for reading data

### CH2D2 Quest 2: What does the AuthAccount mean in the prepare phase of the transaction?

When a transaction gets access to an AuthAccount the transaction will have full access to its data such as storage and public keys.

### CH2D2 Quest 3: What is the difference between the prepare phase and the execute phase in the transaction?

Prepare phase: To access AuthAccount data
Execute phase: To call functions

### CH2D2 Quest 4

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


### CH2D3 Quest 1: In a script, initialize an array (that has length == 3) of your favourite people, represented as `String`s, and `log` it.

<img src="https://raw.githubusercontent.com/bz-hashtag-0780/Flow-Zero-to-Jacob/main/quests/screenshots/CH2D3Q1.png" alt="drawing" width="600" />

### CH2D3 Quest 2: In a script, initialize a dictionary that maps the `String`s Facebook, Instagram, Twitter, YouTube, Reddit, and LinkedIn to a `UInt64` that represents the order in which you use them from most to least. For example, YouTube --> 1, Reddit --> 2, etc. If you've never used one before, map it to 0!

<img src="https://raw.githubusercontent.com/bz-hashtag-0780/Flow-Zero-to-Jacob/main/quests/screenshots/CH2D3Q2.png" alt="drawing" width="600" />

### CH2D3 Quest 3: Explain what the force unwrap operator `!` does, with an example different from the one I showed you (you can just change the type).

It is used to unwrap an optional to its actual type. So `String?` becomes `String`

<img src="https://raw.githubusercontent.com/bz-hashtag-0780/Flow-Zero-to-Jacob/main/quests/screenshots/CH2D3Q3.png" alt="drawing" width="600" />

### CH2D3 Quest 4: Using this picture below, explain...
    - What the error message means
    - Why we're getting this error
    - How to fix it
<img src="https://raw.githubusercontent.com/bz-hashtag-0780/Flow-Zero-to-Jacob/main/chapter2.0/images/wrongcode.png" alt="drawing" width="600" />

The error message says that it expected a String but instead it got a String optional (String or nil)
When dictionaries are accessed, the values returned are optionals. Meaning it might be nil or in the above case a String. The force-unwrap operator `!` must be used to return the actual String type and not just the optional.
It should look like the following

<img src="https://raw.githubusercontent.com/bz-hashtag-0780/Flow-Zero-to-Jacob/main/quests/screenshots/CH2D3Q4.png" alt="drawing" width="600" />

### CH2D4 Quest 1: Deploy a new contract that has a Struct of your choosing inside of it (must be different than Profile).

<img src="https://raw.githubusercontent.com/bz-hashtag-0780/Flow-Zero-to-Jacob/main/quests/screenshots/CH2D4Q1.png" alt="drawing" width="600" />

### CH2D4 Quest 2: Create a dictionary or array that contains the Struct you defined.

<img src="https://raw.githubusercontent.com/bz-hashtag-0780/Flow-Zero-to-Jacob/main/quests/screenshots/CH2D4Q2.png" alt="drawing" width="600" />

### CH2D4 Quest 3: Create a function to add to that array/dictionary.

<img src="https://raw.githubusercontent.com/bz-hashtag-0780/Flow-Zero-to-Jacob/main/quests/screenshots/CH2D4Q3.png" alt="drawing" width="600" />

### CH2D4 Quest 4: Add a transaction to call that function in step 3.

```swift
import BasicBeast from 0x02

transaction(dexNumber: UInt32, name: String) {
    
    let currentBeastTemplateID: UInt32

    prepare(acct: AuthAccount) {
        self.currentBeastTemplateID = BasicBeast.nextBeastTemplateID;
      }

    execute {
        BasicBeast.createBeastTemplate(dexNumber: dexNumber, name: name)
    }

    post {
        BasicBeast.getBeastTemplate(beastTemplateID: self.currentBeastTemplateID) != nil:
            "BeastTemplate doesn't exist"
    }
}
```

### CH2D4 Quest 5: Add a script to read the Struct you defined.

```swift
import BasicBeast from 0x02

pub fun main(beastTemplateID: UInt32): BasicBeast.BeastTemplate {

    let beastTemplate = BasicBeast.getBeastTemplate(beastTemplateID: beastTemplateID)
        ?? panic("BeastTemplate doesn't exist")

    return beastTemplate
}
```

### Running the script

```swift
17:58:07 get beast template Result:
{"type":"Struct","value":
  {"id":"A.0000000000000002.BasicBeast.BeastTemplate","fields":
    [
        {"name":"beastTemplateID","value":
           {"type":"UInt32","value":"1"}
        },
        {"name":"dexNumber","value":
          {"type":"UInt32","value":"1"}
        },
        {"name":"name","value":
          {"type":"String","value":"Moon"}
         }
      ]
    }
 }

17:58:11 get beast template Result:
{"type":"Struct","value":
  {"id":"A.0000000000000002.BasicBeast.BeastTemplate","fields":
    [
       {"name":"beastTemplateID","value":
          {"type":"UInt32","value":"2"}
        },
       {"name":"dexNumber","value":
          {"type":"UInt32","value":"2"}
        },
       {"name":"name","value":
          {"type":"String","value":"Saber"}
        }
     ]
   }
}
```
