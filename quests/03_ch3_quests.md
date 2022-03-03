# 03 - Chapter 3 Quests

<img src="https://media.istockphoto.com/photos/young-chimpanzee-simia-troglodytes-picture-id92202969?k=20&m=92202969&s=612x612&w=0&h=lNCQP52FyfgiQgeJzY-jw332WiDGxrLvTaeqHxIjHjw=" alt="drawing" width="150"/>

## Chapter 3 Day 1

### CH3D1 Quest 1: List 3 reasons why structs are different from resources
1. Structs can be duplicated
2. Structs can be overwritten
3. Structs can be created whenever needed

### CH3D1 Quest 2: Describe a situation where a resource might be better to use than a struct.
A resource is more secure than a struct and is therefore a better choice to represent NFTs

### CH3D1 Quest 3: What is the keyword to make a new resource?

`create`

### CH3D1 Quest 4: Can a resource be created in a script or transaction (assuming there isn't a public function to create one)?

A resource can't be created in a script nor transaction and only in the smart contract (considering the assumption above).

### CH3D1 Quest 5: What is the type of the resource below?

```swift
pub resource Jacob {

}
```
The type of the resource is `@Jacob`

### CH3D1 Quest 6: Let's play the "I Spy" game from when we were kids. I Spy 4 things wrong with this code. Please fix them.

```swift
pub contract Test {

    // Hint: There's nothing wrong here ;)
    pub resource Jacob {
        pub let rocks: Bool
        init() {
            self.rocks = true
        }
    }

    pub fun createJacob(): Jacob { // there is 1 here
        let myJacob = Jacob() // there are 2 here
        return myJacob // there is 1 here
    }
}
```

#### Fixes
```swift
pub contract Test {

    // Hint: There's nothing wrong here ;)
    pub resource Jacob {
        pub let rocks: Bool
        init() {
            self.rocks = true
        }
    }

    pub fun createJacob(): @Jacob { // 1 fix
        let myJacob <- create Jacob() // 2 fixes
        return <- myJacob // 1 fix
    }
}
```

## Chapter 3 Day 2

### CH3D2 Quest 1: Write your own smart contract that contains two state variables: an array of resources, and a dictionary of resources. Add functions to remove and add to each of them. They must be different from the examples.

#### Resource
```swift
pub resource NFT {
        pub let id: UInt64

        pub let beastTemplate: BeastTemplate

        init(beastTemplate: BeastTemplate) {
        self.beastTemplate = beastTemplate

        // Increment the global Beast IDs
        BasicBeast.totalSupply = BasicBeast.totalSupply + 1

        // Set unique Beast ID to the newly incremented totalSupply
        self.id = BasicBeast.totalSupply
        }
    }
```

#### Array of NFTs
```swift
    pub var arrayOfNFTs: @[NFT]

    pub fun addNftToArray(nft: @NFT) {
        self.arrayOfNFTs.append(<- nft)
    }

    pub fun removeNftToArray(index: Int): @NFT {
        return <- self.arrayOfNFTs.remove(at: index)
    }

    init() {
        self.arrayOfNFTs <- []
    } 
```

#### Dictionary of NFTs
```swift
    pub var dictionaryOfNFTs: @{UInt64: NFT}

    pub fun addNftToDictionary(nft: @NFT) {
        let key = nft.id
        
        self.dictionaryOfNFTs[key] <-! nft
    }

    pub fun removeNftToDictionary(key: UInt64): @NFT {
        let nft <- self.dictionaryOfNFTs.remove(key: key) ?? panic("Could not find the NFT!")
        return <- nft
    }

    init() {
        self.dictionaryOfNFTs <- {}
    }
```

#### Full contract
```swift
access(all) contract BasicBeast {

    pub var nextBeastTemplateID: UInt32

    pub var totalSupply: UInt64 

    access(self) var beastTemplates: {UInt32: BeastTemplate} 

    pub struct BeastTemplate {
        pub let beastTemplateID: UInt32
        pub let dexNumber: UInt32
        pub let name: String

        init(dexNumber: UInt32, name: String ) {
          pre {
                  name != "": "New BeastTemplate name cannot be blank"
              }

          self.beastTemplateID = BasicBeast.nextBeastTemplateID 
          self.dexNumber = dexNumber
          self.name = name

          // Increment the ID so that it isn't used again
            BasicBeast.nextBeastTemplateID = BasicBeast.nextBeastTemplateID + 1
        }
    }

    pub fun createBeastTemplate(dexNumber: UInt32, name: String): UInt32 {

            // Create the new BeastTemplate
            var newBeastTemplate = BeastTemplate(dexNumber: dexNumber, name: name,)

            let newID = newBeastTemplate.beastTemplateID

            // Store it in the contract storage
            BasicBeast.beastTemplates[newID] = newBeastTemplate

            return newID
        }

    pub fun getBeastTemplate(beastTemplateID: UInt32): BasicBeast.BeastTemplate? {
        return self.beastTemplates[beastTemplateID]
    }

    // NFT non-conforming to the NFT standard
    pub resource NFT {
        pub let id: UInt64

        pub let beastTemplate: BeastTemplate

        init(beastTemplate: BeastTemplate) {
        self.beastTemplate = beastTemplate

        // Increment the global Beast IDs
        BasicBeast.totalSupply = BasicBeast.totalSupply + 1

        // Set unique Beast ID to the newly incremented totalSupply
        self.id = BasicBeast.totalSupply
        }
    }

    // Dictionary of NFTs

    pub var dictionaryOfNFTs: @{UInt64: NFT}

    pub fun addNftToDictionary(nft: @NFT) {
        let key = nft.id
        
        self.dictionaryOfNFTs[key] <-! nft
    }

    pub fun removeNftToDictionary(key: UInt64): @NFT {
        let nft <- self.dictionaryOfNFTs.remove(key: key) ?? panic("Could not find the NFT!")
        return <- nft
    }

    // Array of NFTs

    pub var arrayOfNFTs: @[NFT]
    
    pub fun addNftToArray(nft: @NFT) {
        self.arrayOfNFTs.append(<- nft)
    }

    pub fun removeNftToArray(index: Int): @NFT {
        return <- self.arrayOfNFTs.remove(at: index)
    }
    

    init() {
        self.nextBeastTemplateID = 1
        self.beastTemplates = {}
        self.totalSupply = 0

        self.dictionaryOfNFTs <- {}
        self.arrayOfNFTs <- []
    }

}

```

## Chapter 3 Day 3

### CH3D3 Quest 1: Define your own contract that stores a dictionary of resources. Add a function to get a reference to one of the resources in the dictionary.

```swift
pub contract BasicBeast {

    pub var totalSupply: UInt64

    pub var dictionaryOfBeasts: @{UInt64: Beast}

    pub resource Beast {
        pub let id: UInt64
        pub let name: String

        init(name: String) {
            self.name = name

            // Increment the global Beast IDs
            BasicBeast.totalSupply = BasicBeast.totalSupply + 1

            // Set unique Beast ID to the newly incremented totalSupply
            self.id = BasicBeast.totalSupply
        }
    }

    pub fun getReference(key: UInt64): &Beast {
        return &self.dictionaryOfBeasts[key] as &Beast
    }

    init() {
        self.totalSupply = 0

        self.dictionaryOfBeasts <- {
            1: <- create Beast(name: "Moon"), 
            2: <- create Beast(name: "Saber")
        }
    }
    
}
```

### CH3D3 Quest 2: Create a script that reads information from that resource using the reference from the function you defined in part 1.

#### Script
```swift
import BasicBeast from 0x04

pub fun main(): String {
  let ref = BasicBeast.getReference(key: 2)
  return ref.name
}
```
#### Result
```swift 
22:25:35 read resource Result
{"type":"String","value":"Moon"}

22:25:45 read resource Result
{"type":"String","value":"Saber"}
```

### CH3D3 Quest 3: Explain, in your own words, why references can be useful in Cadence.

We want to use references because moving a resource just to read or update its data is a pain in the ass.

## Chapter 3 Day 4

### CH3D4 Quest 1: Explain, in your own words, the 2 things resource interfaces can be used for (we went over both in today's content)
1. Interfaces are used as requirements. When an interface is implemented by a resource, the resource must conform to the interface's fields and functions.
2. Interfaces are used to expose stuff. Using interfaces makes it possible to expose only some functions to a certain user.

### CH3D4 Quest 2: Define your own contract. Make your own resource interface and a resource that implements the interface. Create 2 functions. In the 1st function, show an example of not restricting the type of the resource and accessing its content. In the 2nd function, show an example of restricting the type of the resource and NOT being able to access its content.

```swift
pub contract BasicBeast {

    pub var totalSupply: UInt64

    pub resource interface IBeast {
        pub let id: UInt64
        pub let name: String
        pub var nickname: String
    }

    pub resource Beast: IBeast {
        pub let id: UInt64
        pub let name: String
        pub var nickname: String

        init(name: String) {
            self.name = name

            self.nickname = ""

            // Increment the global Beast IDs
            BasicBeast.totalSupply = BasicBeast.totalSupply + 1

            // Set unique Beast ID to the newly incremented totalSupply
            self.id = BasicBeast.totalSupply
        }

        pub fun changeNickname(nickname: String) {
            self.nickname = nickname
        }
    }

    pub fun updateNicknameWithoutInterface() {
        let beast: @Beast <- create Beast(name: "Moon")
        beast.changeNickname(nickname: "New")
        log(beast.nickname) // "New"

        destroy beast
    }

    // Restricted
    pub fun updateNicknameInterface() {
      let beast: @Beast{IBeast} <- create Beast(name: "Moon")
        beast.changeNickname(nickname: "New")
        log(beast.nickname) // "New"

        destroy beast
    }

    init() {
        self.totalSupply = 0
    }
    
}
```

### CH3D4 Quest 3: How would we fix this code? 

```swift
pub contract Stuff {

    pub struct interface ITest {
      pub var greeting: String
      pub var favouriteFruit: String
    }

    // ERROR:
    // `structure Stuff.Test does not conform 
    // to structure interface Stuff.ITest`
    pub struct Test: ITest {
      pub var greeting: String

      pub fun changeGreeting(newGreeting: String): String {
        self.greeting = newGreeting
        return self.greeting // returns the new greeting
      }

      init() {
        self.greeting = "Hello!"
      }
    }

    pub fun fixThis() {
      let test: Test{ITest} = Test()
      let newGreeting = test.changeGreeting(newGreeting: "Bonjour!") // ERROR HERE: `member of restricted type is not accessible: changeGreeting`
      log(newGreeting)
    }
}
```

#### The fix

```swift
pub contract Stuff {

    pub struct interface ITest {
      pub var greeting: String
      pub var favouriteFruit: String
      pub fun changeGreeting(newGreeting: String): String // Add this to make the function accessible
    }

    // ERROR FIXED
    pub struct Test: ITest {
      pub var greeting: String
      pub var favouriteFruit: String // Add this to conform to the ITest interface

      pub fun changeGreeting(newGreeting: String): String {
        self.greeting = newGreeting
        return self.greeting // returns the new greeting
      }

      init() {
        self.greeting = "Hello!"
        self.favouriteFruit = "Bacon" // Initialize
      }
    }

    pub fun fixThis() {
      let test: Test{ITest} = Test()
      let newGreeting = test.changeGreeting(newGreeting: "Bonjour!") // ERROR FIXED
      log(newGreeting)
    }
}
```

## Chapter 3 Day 5

### CH3D5 Quest 1: For today's quest, you will be looking at a contract and a script. You will be looking at 4 variables (a, b, c, d) and 3 functions (publicFunc, contractFunc, privateFunc) defined in `SomeContract`. In each AREA (1, 2, 3, and 4), I want you to do the following: for each variable (a, b, c, and d), tell me in which areas they can be read (read scope) and which areas they can be modified (write scope). For each function (publicFunc, contractFunc, and privateFunc), simply tell me where they can be called.

```swift
access(all) contract SomeContract {
    pub var testStruct: SomeStruct

    pub struct SomeStruct {

        //
        // 4 Variables
        //

        pub(set) var a: String

        pub var b: String

        access(contract) var c: String

        access(self) var d: String

        //
        // 3 Functions
        //

        pub fun publicFunc() {}

        access(contract) fun contractFunc() {}

        access(self) fun privateFunc() {}


        pub fun structFunc() {
            /**************/
            /*** AREA 1 ***/
            /**************/
            
            // a: read and write
            // b: read and write
            // c: read and write
            // d: read and write
            
            // publicFunc() can be called here
            // contractFunc() can be called here
            // privateFunc() can be called here
        }

        init() {
            self.a = "a"
            self.b = "b"
            self.c = "c"
            self.d = "d"
        }
    }

    pub resource SomeResource {
        pub var e: Int

        pub fun resourceFunc() {
            /**************/
            /*** AREA 2 ***/
            /**************/
            
            // a: read and write
            // b: read
            // c: read
            // d: no access
            
            // publicFunc() can be called here
            // contractFunc() can be called here
        }

        init() {
            self.e = 17
        }
    }

    pub fun createSomeResource(): @SomeResource {
        return <- create SomeResource()
    }

    pub fun questsAreFun() {
        /**************/
        /*** AREA 3 ****/
        /**************/
        
        // a: read and write
        // b: read
        // c: read
        // d: no access
        
        // publicFunc() can be called here
        // contractFunc() can be called here
    }

    init() {
        self.testStruct = SomeStruct()
    }
}
```

This is a script that imports the contract above:
```swift
import SomeContract from 0x01

pub fun main() {
  /**************/
  /*** AREA 4 ***/
  /**************/
  
    // a: read and write
    // b: read
    // c: no access
    // d: no access
    
    // publicFunc() can be called here
}
```

<img src="https://media1.giphy.com/media/3og0IPNLPWSLR0gaxW/giphy.gif" alt="drawing" width="600"/>

