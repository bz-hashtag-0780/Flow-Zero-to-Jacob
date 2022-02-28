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

### CH3D2 Quest 6: Write your own smart contract that contains two state variables: an array of resources, and a dictionary of resources. Add functions to remove and add to each of them. They must be different from the examples.

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
        
        let oldNFT <- self.dictionaryOfNFTs[key] <- nft
        destroy oldNFT
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
        
        let oldNFT <- self.dictionaryOfNFTs[key] <- nft
        destroy oldNFT
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
