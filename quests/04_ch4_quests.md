# 04 - Chapter 4 Quests

<img src="https://i1.sndcdn.com/avatars-000815375770-jgmur7-t500x500.jpg" alt="drawing" width="150"/>

## Chapter 4 Day 1

### CH4D1 Quest 1: Explain what lives inside of an account. 
Inside an account you can deploy `contract code` and you can deploy multiple smart contracts to the same account. An account also has an `account storage` to store all of its data.

### CH4D1 Quest 2: What is the difference between the `/storage/`, `/public/`, and `/private/` paths?
`/storage/` can only be accessed by the account owner
`/public/` is available to everyone
`/private/` is available to the account owner and other accounts that the account owner has given access to

### CH4D1 Quest 3: What does `.save()` do? What does `.load()` do? What does `.borrow()` do?

`.save()`: To save data to an `account storage` path
`.load()`: To load data from the `account storage`
`.borrow()`: To get a reference to a resource in the `account storage`

### CH4D1 Quest 4: Explain why we couldn't save something to our account storage inside of a script.

Because you need to sign an transaction as the AuthAccount to get access to the `account storage` and save something to it.

### CH4D1 Quest 5: Explain why I couldn't save something to your account.

Because I need to sign a transaction in order to to do so or sign an transaction that gives you the capability to save something to my `account storage`

### CH4D1 Quest 6: Define a contract that returns a resource that has at least 1 field in it. Then, write 2 transactions:

    1) A transaction that first saves the resource to account storage, then loads it out of account storage, logs a field inside the resource, and destroys it.

    2) A transaction that first saves the resource to account storage, then borrows a reference to it, and logs a field inside the resource.
    
#### Some random contract
```swift
pub contract BasicBeast {

    pub var totalSupply: UInt64

    pub resource interface IBeast {
        pub let id: UInt64
        pub let name: String
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
#### Transaction 1 load()
```swift
import BasicBeast from 0x05

transaction(name: String) {
  prepare(signer: AuthAccount) {
    // Save the resource to account storage
    signer.save(<- BasicBeast.createBeast(name: name), to: /storage/MyBeastStorage)

    // Load the resource from the account storage
    let beast <- signer.load<@BasicBeast.Beast>(from: /storage/MyBeastStorage)
                  ?? panic("A `@BasicBeast.Beast` resource does not exist")

    // Log a field
    log(beast.name)

    // Destroy the bastard
    destroy beast
  }

  execute {

  }
}
```

#### Transaction 2 borrow()
```swift
import BasicBeast from 0x05

transaction(name: String) {
  prepare(signer: AuthAccount) {
    // Save the resource to account storage
    signer.save(<- BasicBeast.createBeast(name: name), to: /storage/MyBeastStorage)

    // Load the resource from the account storage
    let beast = signer.borrow<&BasicBeast.Beast>(from: /storage/MyBeastStorage)
                  ?? panic("A `@BasicBeast.Beast` resource does not exist")

    // Log a field
    log(beast.name)
  }

  execute {

  }
}
```
## Chapter 4 Day 2

### CH4D2 Quest 1: What does `.link()` do?

It links a capability for some data to either the /public/ or /private/ path so others can access the data.

### CH4D2 Quest 2: In your own words (no code), explain how we can use resource interfaces to only expose certain things to the `/public/` path.

Resource interfaces can expose certain things to the `/public/` path by only exposing certain functions in the interface.

### CH4D2 Quest 3: Deploy a contract that contains a resource that implements a resource interface. Then, do the following:

    1) In a transaction, save the resource to storage and link it to the public with the restrictive interface. 

    2) Run a script that tries to access a non-exposed field in the resource interface, and see the error pop up.

    3) Run the script and access something you CAN read from. Return it from the script.

#### Contract with a resource that implements an interface
```swift
pub contract BasicBeast {

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
#### Transaction save and link resource with a restrictive interface
```swift
import BasicBeast from 0x05

transaction(name: String) {

  prepare(signer: AuthAccount) {

    // Save the resource to account storage
    signer.save(<- BasicBeast.createBeast(name: name), to: /storage/MyBeastStorage)

    // link the resource from the account storage
    signer.link<&BasicBeast.Beast{BasicBeast.IBeast}>(/public/MyBeastStorage, target: /storage/MyBeastStorage)
                  ?? panic("A `@BasicBeast.Beast` resource does not exist")
  }

  execute {}
}
```
#### Script with error that can't access restricted field `name`
```swift
import BasicBeast from 0x05

pub fun main(address: Address): String {

  let publicCapability: Capability<&BasicBeast.Beast{BasicBeast.IBeast}> =
    getAccount(address).getCapability<&BasicBeast.Beast{BasicBeast.IBeast}>(/public/MyBeastStorage)

  let beast: &BasicBeast.Beast{BasicBeast.IBeast} = publicCapability.borrow() ?? panic("The capability doesn't exist or you did not specify the right type when you got the capability.")

  return beast.name // ERROR: member of restricted type is not accessible: name
}
```
#### Script that can access a non-restricted field `id`
```swift
import BasicBeast from 0x05

pub fun main(address: Address): UInt64 {

  let publicCapability: Capability<&BasicBeast.Beast{BasicBeast.IBeast}> =
    getAccount(address).getCapability<&BasicBeast.Beast{BasicBeast.IBeast}>(/public/MyBeastStorage)

  let beast: &BasicBeast.Beast{BasicBeast.IBeast} = publicCapability.borrow() ?? panic("The capability doesn't exist or you did not specify the right type when you got the capability.")

  return beast.id
}
```
## Chapter 4 Day 3

### CH4D3 Quest 1: What do you have to do if you have resources "nested" inside of another resource? ("Nested resources")

When you have nested resources you must define a `destroy()` function so the nested resource either gets destroyed or moved when the parent resource is destroyed.

### CH4D3 Quest 2: Brainstorm some extra things we may want to add to this contract. Think about what might be problematic with this contract and how we could fix it.

#### Idea #1: Do we really want everyone to be able to mint an NFT?.
If we don't want everyone to be able to mint. We could restrict the function using an interface or creating a resource where only accounts with the resource is able to call the mint function.

#### Idea #2: If we want to read information about our NFTs inside our Collection, right now we have to take it out of the Collection to do so. Is this good?
No, a better way would be to use references

## Chapter 4 Day 4

### CH4D4 Quest 1: Can you brainstorm any ways to distribute Minters to other people WITHOUT having to have 2 signers, or in other words, 2 AuthAccounts?

By creating a capabilitity `link()` of the 'Admin' and let the other account `borrow()` the capability to mint
