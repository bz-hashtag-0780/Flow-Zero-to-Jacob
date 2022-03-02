# 05 - Chapter 5 Quests

<img src="https://t4.ftcdn.net/jpg/02/55/89/59/360_F_255895994_daLC1HyNe3LsJyoVyDo8vBLvC8V4F5yi.jpg" alt="drawing" width="200"/>

## Chapter 5 Day 1

### CH5D1 Quest 1: Describe what an event is, and why it might be useful to a client.

Events are emitted to the blockchain when executing functions. It's used so others know what has happened. This way we can be updated about a certain smart contract without having to manually check once a day, every hour or every minute.

### CH5D1 Quest 2: Deploy a contract with an event in it, and emit the event somewhere else in the contract indicating that it happened.

```swift
pub contract BasicBeast {

    pub event BeastMinted(id: UInt64)

    pub var totalSupply: UInt64

    pub resource interface IBeast {
        pub let id: UInt64
        // Restrict from accessing name
        // pub let name: String
        pub var nickname: String
        pub fun changeNickname(nickname: String)
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

            emit BeastMinted(id: self.id)
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

    pub fun createBeast(name: String): @Beast {
    return <- create Beast(name: name)
  }

    init() {
        self.totalSupply = 0
    }
    
}
```

### CH5D1 Quest 3: Using the contract in step 2), add some pre conditions and post conditions to your contract to get used to writing them out.

```swift
pub contract BasicBeast {

    pub event BeastMinted(id: UInt64)

    pub var totalSupply: UInt64

    pub resource interface IBeast {
        pub let id: UInt64
        // Restrict from accessing name
        // pub let name: String
        pub var nickname: String
        pub fun changeNickname(nickname: String)
    }

    pub resource Beast: IBeast {
        pub let id: UInt64
        pub let name: String
        pub var nickname: String

        init(name: String) {
          pre {
                  name != "": "New BeastTemplate name cannot be blank"
              }
            self.name = name

            self.nickname = ""

            // Increment the global Beast IDs
            BasicBeast.totalSupply = BasicBeast.totalSupply + 1

            // Set unique Beast ID to the newly incremented totalSupply
            self.id = BasicBeast.totalSupply

            emit BeastMinted(id: self.id)
        }

        pub fun changeNickname(nickname: String) {
            post {
                self.nickname != "": "The nickname is not blank."
            }
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

    pub fun createBeast(name: String): @Beast {
    return <- create Beast(name: name)
  }

    init() {
        self.totalSupply = 0
    }
    
}
```

### CH5D1 Quest 4: For each of the functions below (numberOne, numberTwo, numberThree), follow the instructions.

```swift
pub contract Test {

  // TODO
  // Tell me whether or not this function will log the name.
  // name: 'Jacob'
  // It will log the name 'Jacob' as long as the input is 'Jacob'
  pub fun numberOne(name: String) {
    pre {
      name.length == 5: "This name is not cool enough."
    }
    log(name)
  }

  // TODO
  // Tell me whether or not this function will return a value.
  // name: 'Jacob'
  // It will not return the name 'Jacob' but will return any input name that is valid and add ' Tucker' in the end of it
  pub fun numberTwo(name: String): String {
    pre {
      name.length >= 0: "You must input a valid name."
    }
    post {
      result == "Jacob Tucker"
    }
    return name.concat(" Tucker")
  }

  pub resource TestResource {
    pub var number: Int

    // TODO
    // Tell me whether or not this function will log the updated number.
    // Also, tell me the value of `self.number` after it's run.
    // self.number will be 1 but the post-condition would make the function fail.
    pub fun numberThree(): Int {
      post {
        before(self.number) == result + 1
      }
      self.number = self.number + 1
      return self.number
    }

    init() {
      self.number = 0
    }

  }

}
```

## Chapter 5 Day 2

### CH5D2 Quest 1: Explain why standards can be benefitial to the Flow ecosystem.

Having standards makes it possible for many different applications to work with many types of digital assets as they all follow the same structure. It allows for scalability and helps make the ecosystem grow much faster.

### CH5D2 Quest 2: What is YOUR favourite food?

<img src="https://c.tenor.com/rd6D2wMN7psAAAAd/burger-food.gif" alt="drawing" width="600"/>

### CH5D2 Quest 3:. Please fix this code (Hint: There are two things wrong):

The contract interface:
```swift
pub contract interface ITest {
  pub var number: Int
  
  pub fun updateNumber(newNumber: Int) {
    pre {
      newNumber >= 0: "We don't like negative numbers for some reason. We're mean."
    }
    post {
      self.number == newNumber: "Didn't update the number to be the new number."
    }
  }

  pub resource interface IStuff {
    pub var favouriteActivity: String
  }

  pub resource Stuff: IStuff { //Must implement the interface
    pub var favouriteActivity: String
  }
}
```

The implementing contract:
```swift
import ITest from 0xWherever

pub contract Test: ITest { //Make sure the contract actually implements the ITest
  pub var number: Int
  
  pub fun updateNumber(newNumber: Int) {
    self.number = 5
  }

  pub resource interface IStuff {
    pub var favouriteActivity: String
  }

  pub resource Stuff: IStuff {
    pub var favouriteActivity: String

    init() {
      self.favouriteActivity = "Playing League of Legends."
    }
  }

  init() {
    self.number = 0
  }
}
```

## Chapter 5 Day 3

### CH5D3 Quest 1: What does "force casting" with `as!` do? Why is it useful in our Collection?

It helps us make sure only a specific type of resource can be stored into our collection.

### CH5D3 Quest 2: What does `auth` do? When do we use it?

`auth` reference is what we must use in order to get a downcasted reference.

### CH5D3 Quest 3: This last quest will be your most difficult yet. Take this contract:

```swift
import NonFungibleToken from 0x02
pub contract CryptoPoops: NonFungibleToken {
  pub var totalSupply: UInt64

  pub event ContractInitialized()
  pub event Withdraw(id: UInt64, from: Address?)
  pub event Deposit(id: UInt64, to: Address?)

  pub resource NFT: NonFungibleToken.INFT {
    pub let id: UInt64

    pub let name: String
    pub let favouriteFood: String
    pub let luckyNumber: Int

    init(_name: String, _favouriteFood: String, _luckyNumber: Int) {
      self.id = self.uuid

      self.name = _name
      self.favouriteFood = _favouriteFood
      self.luckyNumber = _luckyNumber
    }
  }

  pub resource Collection: NonFungibleToken.Provider, NonFungibleToken.Receiver, NonFungibleToken.CollectionPublic {
    pub var ownedNFTs: @{UInt64: NonFungibleToken.NFT}

    pub fun withdraw(withdrawID: UInt64): @NonFungibleToken.NFT {
      let nft <- self.ownedNFTs.remove(key: withdrawID) 
            ?? panic("This NFT does not exist in this Collection.")
      emit Withdraw(id: nft.id, from: self.owner?.address)
      return <- nft
    }

    pub fun deposit(token: @NonFungibleToken.NFT) {
      let nft <- token as! @NFT
      emit Deposit(id: nft.id, to: self.owner?.address)
      self.ownedNFTs[nft.id] <-! nft
    }

    pub fun getIDs(): [UInt64] {
      return self.ownedNFTs.keys
    }

    pub fun borrowNFT(id: UInt64): &NonFungibleToken.NFT {
      return &self.ownedNFTs[id] as &NonFungibleToken.NFT
    }

    init() {
      self.ownedNFTs <- {}
    }

    destroy() {
      destroy self.ownedNFTs
    }
  }

  pub fun createEmptyCollection(): @NonFungibleToken.Collection {
    return <- create Collection()
  }

  pub resource Minter {

    pub fun createNFT(name: String, favouriteFood: String, luckyNumber: Int): @NFT {
      return <- create NFT(_name: name, _favouriteFood: favouriteFood, _luckyNumber: luckyNumber)
    }

    pub fun createMinter(): @Minter {
      return <- create Minter()
    }

  }

  init() {
    self.totalSupply = 0
    emit ContractInitialized()
    self.account.save(<- create Minter(), to: /storage/Minter)
  }
}
```

and add a function called `borrowAuthNFT` just like we did in the section called "The Problem" above. Then, find a way to make it publically accessible to other people so they can read our NFT's metadata. Then, run a script to display the NFTs metadata for a certain `id`.

You will have to write all the transactions to set up the accounts, mint the NFTs, and then the scripts to read the NFT's metadata.


#### Contract

import NonFungibleToken from 0x01

pub contract CryptoPoops: NonFungibleToken {
  pub var totalSupply: UInt64

  pub event ContractInitialized()
  pub event Withdraw(id: UInt64, from: Address?)
  pub event Deposit(id: UInt64, to: Address?)

  pub resource NFT: NonFungibleToken.INFT {
    pub let id: UInt64

    pub let name: String
    pub let favouriteFood: String
    pub let luckyNumber: Int

    init(_name: String, _favouriteFood: String, _luckyNumber: Int) {
      self.id = self.uuid

      self.name = _name
      self.favouriteFood = _favouriteFood
      self.luckyNumber = _luckyNumber
    }
  }

  pub resource interface CollectionPublic {
    pub fun deposit(token: @NonFungibleToken.NFT)
    pub fun getIDs(): [UInt64]
    pub fun borrowAuthNFT(id: UInt64): &NFT
  }

  pub resource Collection: NonFungibleToken.Provider, NonFungibleToken.Receiver, NonFungibleToken.CollectionPublic, CollectionPublic {
    pub var ownedNFTs: @{UInt64: NonFungibleToken.NFT}

    pub fun withdraw(withdrawID: UInt64): @NonFungibleToken.NFT {
      let nft <- self.ownedNFTs.remove(key: withdrawID) 
            ?? panic("This NFT does not exist in this Collection.")
      emit Withdraw(id: nft.id, from: self.owner?.address)
      return <- nft
    }

    pub fun deposit(token: @NonFungibleToken.NFT) {
      let nft <- token as! @NFT
      emit Deposit(id: nft.id, to: self.owner?.address)
      self.ownedNFTs[nft.id] <-! nft
    }

    pub fun getIDs(): [UInt64] {
      return self.ownedNFTs.keys
    }

    pub fun borrowNFT(id: UInt64): &NonFungibleToken.NFT {
      return &self.ownedNFTs[id] as &NonFungibleToken.NFT
    }

    pub fun borrowAuthNFT(id: UInt64): &NFT {
      let ref = &self.ownedNFTs[id] as auth &NonFungibleToken.NFT
      return ref as! &NFT
    }

    init() {
      self.ownedNFTs <- {}
    }

    destroy() {
      destroy self.ownedNFTs
    }
  }

  pub fun createEmptyCollection(): @NonFungibleToken.Collection {
    return <- create Collection()
  }

  pub resource Minter {

    pub fun createNFT(name: String, favouriteFood: String, luckyNumber: Int): @NFT {
      return <- create NFT(_name: name, _favouriteFood: favouriteFood, _luckyNumber: luckyNumber)
    }

    pub fun createMinter(): @Minter {
      return <- create Minter()
    }

  }

  init() {
    self.totalSupply = 0
    emit ContractInitialized()
    self.account.save(<- create Minter(), to: /storage/Minter)
  }
}

#### Transaction: Setup Collection
```swift
import NonFungibleToken from 0x01

pub contract CryptoPoops: NonFungibleToken {
  pub var totalSupply: UInt64

  pub event ContractInitialized()
  pub event Withdraw(id: UInt64, from: Address?)
  pub event Deposit(id: UInt64, to: Address?)

  pub resource NFT: NonFungibleToken.INFT {
    pub let id: UInt64

    pub let name: String
    pub let favouriteFood: String
    pub let luckyNumber: Int

    init(_name: String, _favouriteFood: String, _luckyNumber: Int) {
      self.id = self.uuid

      self.name = _name
      self.favouriteFood = _favouriteFood
      self.luckyNumber = _luckyNumber
    }
  }

  pub resource interface CollectionPublic {
    pub fun deposit(token: @NonFungibleToken.NFT)
    pub fun getIDs(): [UInt64]
    pub fun borrowAuthNFT(id: UInt64): &NFT
  }

  pub resource Collection: NonFungibleToken.Provider, NonFungibleToken.Receiver, NonFungibleToken.CollectionPublic, CollectionPublic {
    pub var ownedNFTs: @{UInt64: NonFungibleToken.NFT}

    pub fun withdraw(withdrawID: UInt64): @NonFungibleToken.NFT {
      let nft <- self.ownedNFTs.remove(key: withdrawID) 
            ?? panic("This NFT does not exist in this Collection.")
      emit Withdraw(id: nft.id, from: self.owner?.address)
      return <- nft
    }

    pub fun deposit(token: @NonFungibleToken.NFT) {
      let nft <- token as! @NFT
      emit Deposit(id: nft.id, to: self.owner?.address)
      self.ownedNFTs[nft.id] <-! nft
    }

    pub fun getIDs(): [UInt64] {
      return self.ownedNFTs.keys
    }

    pub fun borrowNFT(id: UInt64): &NonFungibleToken.NFT {
      return &self.ownedNFTs[id] as &NonFungibleToken.NFT
    }

    pub fun borrowAuthNFT(id: UInt64): &NFT {
      let ref = &self.ownedNFTs[id] as auth &NonFungibleToken.NFT
      return ref as! &NFT
    }

    init() {
      self.ownedNFTs <- {}
    }

    destroy() {
      destroy self.ownedNFTs
    }
  }

  pub fun createEmptyCollection(): @NonFungibleToken.Collection {
    return <- create Collection()
  }

  pub resource Minter {

    pub fun createNFT(name: String, favouriteFood: String, luckyNumber: Int): @NFT {
      return <- create NFT(_name: name, _favouriteFood: favouriteFood, _luckyNumber: luckyNumber)
    }

    pub fun createMinter(): @Minter {
      return <- create Minter()
    }

  }

  init() {
    self.totalSupply = 0
    emit ContractInitialized()
    self.account.save(<- create Minter(), to: /storage/Minter)
  }
}
```

#### Transaction mint NFT 
```swift
import CryptoPoops from 0x02

transaction(name: String, favouriteFood: String, luckyNumber: Int) {

  let minterRef: &CryptoPoops.Minter

  let collectionRef: &CryptoPoops.Collection{CryptoPoops.CollectionPublic}

  prepare(signer: AuthAccount) {

    if signer.borrow<&CryptoPoops.Collection>(from: /storage/CryptoPoopsCollection) == nil {
    // Save the resource to account storage
    signer.save(<- CryptoPoops.createEmptyCollection(), to: /storage/MyCryptoPoopsCollection)

    // link the resource from the account storage
    signer.link<&CryptoPoops.Collection{CryptoPoops.CollectionPublic}>(/public/MyCryptoPoopsCollection, target: /storage/MyCryptoPoopsCollection)
                  ?? panic("A `@CryptoPoops.Collection` resource does not exist")
    }

    // Get minter reference
    self.minterRef = signer.borrow<&CryptoPoops.Minter>(from: /storage/Minter)
                  ?? panic("Cannot borrow minter reference")

    // Get collection reference
    self.collectionRef = getAccount(signer.address).getCapability<&CryptoPoops.Collection{CryptoPoops.CollectionPublic}>(/public/MyCryptoPoopsCollection)
                        .borrow() ?? panic("Cannot borrow collection reference ")

    
  }

  execute {
    self.collectionRef.deposit(token: <- self.minterRef.createNFT(name: name, favouriteFood: favouriteFood, luckyNumber: luckyNumber))
  }
}
```

#### Script to read poop
```swift
import CryptoPoops from 0x02

pub fun main(address: Address, id: UInt64) {

  let collectionRef = getAccount(address).getCapability<&CryptoPoops.Collection{CryptoPoops.CollectionPublic}>(/public/MyCryptoPoopsCollection)
                        .borrow() ?? panic("Cannot borrow collection reference ")

  let poopRef = collectionRef.borrowAuthNFT(id: id)
  
  log(poopRef.name)

}
```
