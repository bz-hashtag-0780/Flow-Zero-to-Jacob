# 03 - Chapter 3 Quests

<img src="https://media.istockphoto.com/photos/young-chimpanzee-simia-troglodytes-picture-id92202969?k=20&m=92202969&s=612x612&w=0&h=lNCQP52FyfgiQgeJzY-jw332WiDGxrLvTaeqHxIjHjw=" alt="drawing" width="150"/>

## Chapter 3 Day 1 Quest 1: List 3 reasons why structs are different from resources
1. Structs can be duplicated
2. Structs can be overwritten
3. Structs can be created whenever needed

## Chapter 3 Day 1 Quest 2: Describe a situation where a resource might be better to use than a struct.
A resource is more secure than a struct and is therefore a better choice to represent NFTs

## Chapter 3 Day 1 Quest 3: What is the keyword to make a new resource?

`create`

## Chapter 3 Day 1 Quest 4: Can a resource be created in a script or transaction (assuming there isn't a public function to create one)?

A resource can't be created in a script nor transaction and only in the smart contract (considering the assumption above).

## Chapter 3 Day 1 Quest 5: What is the type of the resource below?

```swift
pub resource Jacob {

}
```
The type of the resource is `@Jacob`

## Chapter 3 Day 1 Quest 6: Let's play the "I Spy" game from when we were kids. I Spy 4 things wrong with this code. Please fix them.

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

### Fixes
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
