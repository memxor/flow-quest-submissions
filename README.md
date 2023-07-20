# Beginner Cadence

## Chapter 1 Lession 1

### What is Blockchain?

Blockchain is a decentralized and secure digital ledger shared across a network of computers. It stores information in blocks, linked together using cryptography, ensuring that once data is recorded, it cannot be altered. This technology eliminates the need for intermediaries and enables transparent, tamper-resistant, and efficient record-keeping for various applications beyond cryptocurrencies.

### What is a Smart Contract?

A smart contract is a self-executing contract with predefined conditions written in code. Once these conditions are met, the contract automatically executes the specified actions without the need for human intervention. Operating on a blockchain, smart contracts enable trustless and transparent transactions, making them a powerful tool for automating and securing various agreements and processes.

### Whats the difference between Transactions and Scripts?

A transaction is a function call that changes data on the blockchain. It involves exchanging value or information between parties and may incur fees depending on the blockchain network. On the other hand, a script is used to view data on the blockchain without altering it. Scripts are usually free and do not involve any changes to the underlying blockchain data.

## Chapter 1 Lession 2

### What are the 5 Cadence Programming Language Pillars?

- Safety and Security
- Clarity
- Approachability
- Developer Experience
- Resource Oriented Programming

### In your opinion, even without knowing anything about the Blockchain or coding, why could the 5 Pillars be useful?

- Safety and Security - The focus on safety and security ensures that any applications built using Cadence are less prone to vulnerabilities and potential hacks. It promotes a robust and reliable foundation for decentralized applications.
- Clarity - Clarity in code is crucial for easy comprehension and verification. Even for non-developers, clear and readable code allows them to understand the intent and logic behind that code, ensuring transparency and confidence in the application's functionality.
- Approachability - The language's familiarity to other programming languages makes it accessible for people with prior programming experience. Even without specific knowledge of Cadence, those with general coding knowledge can find it easier to grasp the concepts and get started with smart contract development.
- Developer Experience - A positive developer experience means that working with Cadence should be smooth, efficient, and frustration-free. This ensures that developers can debug and troubleshoot code effectively, leading to faster development and reduced errors.

## Chapter 2 Lession 1

### Jacob Tucker is the best! Contact

![Jacob Tucker](https://i.imgur.com/XSp1Yst.png)

## Chapter 2 Lession 2

### Explain why we wouldn’t call changeGreeting in a script

It's because changeGreeting changes the state of the blockchain. And we can't change the state of the blockchain from a script.

### What does the AuthAccount mean in the prepare phase of the transaction?

The AuthAccount represents the user's authenticated account. When a user initiates a transaction, they need to sign the transaction to approve it. This signature is a form of authentication, confirming that the user is authorized to make changes to their account's data.

### What is the difference between the prepare phase and the execute phase in the transaction?

The prepare phase verifies and accesses data in the user's account (AuthAccount) during a transaction. It confirms the transaction's authenticity and authorization through signature verification. On the other hand, the execute phase executes the transaction's logic, interacting with smart contracts and changing the blockchain's state based on the transaction's code.

### My number Contract

![My Number](https://i.imgur.com/eHe1r7B.png)

## Chapter 2 Lession 3

### Array Example

```cadence
pub fun main()
{
    var favPeople = ["Jacob", "Jacob", "Jacob"];
    log(favPeople)
}
```

### Dictionary Example

```cadence
pub fun main()
{
    var usedApps: {String: Int} = {
        "Youtube": 1,
        "Instagram": 2,
        "Twitter": 3,
        "Reddit": 4,
        "LinkedIn": 5,
        "Facebook": 0
    };
    log(usedApps)
}
```

### Explain what the force unwrap operator ! does, with an example different from the one I showed you.

```cadence
var optionalNumber: Int? = 42

// Using force-unwrap to extract the value from the optional
let unwrappedNumber: Int = optionalNumber!

print(unwrappedNumber) // Output: 42
```

In this example, we have an optional variable optionalNumber, which has a value of 42. When we use the force-unwrap operator (!) on optionalNumber, it extracts the underlying integer value (42 in this case) and assigns it to the constant unwrappedNumber. As a result, unwrappedNumber now holds the value 42. However, we need to be careful when using the force-unwrap operator. If the optional variable optionalNumber were nil instead of having a value, using the force-unwrap operator on it would cause a the program to panic and abort the whole function.

### Using this picture below, explain… 1. What the error message mean 2. Why we’re getting this error 3. How to fix it

1. The error means that it was expecting a type String but got a String Optional
2. Because we are passing a String Optional instead of a String
3. fix: return thing[0x03]!

## Chapter 2 Lession 3

### Profile Contract

Contract
```cadence
pub contract StudentInfo
{
  pub var students: {Address: Student};

  pub struct Student
  {
    pub let name: String;
    pub var class: Int;
    pub var grade: String;
    pub var remarks: String;
    pub var address: Address;

    init(name: String, class: Int, grade: String, remarks: String, address: Address)
    {
      self.name = name;
      self.class = class;
      self.grade = grade;
      self.remarks = remarks;
      self.address = address;
    }
  }

  pub fun addStudent(name: String, class: Int, grade: String, remarks: String, address: Address)
  {
    let newStudent = Student(name: name, class: class, grade: grade, remarks: remarks, address: address);
    self.students[address] = newStudent;
  }

  init()
  {
    self.students = {};
  }
}
```

Transaction
```cadence
import StudentInfo from 0x01

transaction(name: String, class: Int, grade: String, remarks: String, address: Address)
{
    prepare(signer: AuthAccount) {}

    execute 
    {
        StudentInfo.addStudent(name: name, class: class, grade: grade, remarks: remarks, address: address)
    }
}
```

Script
```cadence
import StudentInfo from 0x01

pub fun main(account: Address): StudentInfo.Profile {
    return StudentInfo.students[account]!
}
```

## Chapter 3 Lession 1

### In words, list 3 reasons why structs are different from resources.

1. Copyability: Structs can be copied like regular data types, while resources cannot be copied. This means that when working with resources, you must be explicit about how you move and handle them, ensuring better security and control.

2. Loss Prevention: Unlike structs, resources are designed to be hard to lose. They cannot be lost or overwritten accidentally, reducing the risk of valuable data being misplaced or destroyed unintentionally.

3. Creation Control: Structs can be created anywhere, even outside the contract, without much restriction. On the other hand, resources can only be created within the contract using the "create" keyword, allowing developers to have better control over when and where resources are made.

### Describe a situation where a resource might be better to use than a struct.

In a blockchain game where players own valuable and unique NFTs, using resources becomes essential. Resources provide heightened security by preventing unauthorized duplication or tampering of NFTs, ensuring their uniqueness and integrity. With resources, developers gain explicit control over ownership, enabling precise management of asset transfers between players. Additionally, resources' immutability ensures a transparent and auditable history of each NFT's ownership, enhancing player trust and facilitating a robust in-game economy.

### What is the keyword to make a new resource?

The keyword to make a new resource in Cadence is "create".

### Can a resource be created in a script or transaction (assuming there isn’t a public function to create one)?

No, a resource cannot be created directly in a script or transaction if there isn't a public function to create one. Resources can only be created using the "create" keyword within the contract where they are defined. 

### What is the type of the resource below?

Jacob

### Let’s play the “I Spy” game from when we were kids. I Spy 4 things wrong with this code. Please fix them.

Fixed Code
```cadence
pub contract Test
{
    pub resource Jacob
    {
        pub let rocks: Bool
        init()
        {
            self.rocks = true
        }
    }

    pub fun createJacob(): @Jacob
    {
        let myJacob <- create Jacob()
        return <- myJacob
    }
}
```

## Chapter 3 Lession 2

Write your own smart contract that contains two state variables: an array of resources, and a dictionary of resources. Add functions to remove and add to each of them. They must be different from the examples above.
```cadence
pub contract StudentInfo
{
  pub var studentsDictionary: @{String: Student};
  pub var studentsArray: @[Student];

  pub resource Student
  {
    pub let name: String;

    init(name: String)
    {
      self.name = name;
    }
  }

  pub fun addStudentDictionary(name: String)
  {
    let oldStudent <- self.studentsDictionary[name] <- create Student(name: name);
    destroy oldStudent;
  }

  pub fun removeStudentDictionary(name: String)
  {
    destroy self.studentsDictionary.remove(key: name) ?? panic("Name Doesnt Exist");
  }

  pub fun addStudentArray(name: String)
  {
    self.studentsArray.append(<- create Student(name: name));
  }

  pub fun removeStudentArray(index: Int)
  {
    destroy self.studentsArray.remove(at: index);
  }

  init()
  {
    self.studentsDictionary <- {};
    self.studentsArray <- [];
  }
}
```

## Chapter 3 Lession 3

### Define your own contract that stores a dictionary of resources. Add a function to get a reference to one of the resources in the dictionary.

```cadence
pub contract CyptoLangsContract {

  pub var dictionaryOfCyptoLangss: @{String: CyptoLangs}

  pub resource CyptoLangs {
    pub let language: String
    init(_language: String) {
      self.language = _language
    }
  }

  init() {
    self.dictionaryOfCyptoLangss <- {
      "Solana": <- create CyptoLangs(_language: "Rust"),
      "Flow": <- create CyptoLangs(_language: "Cadence")
    }
  }

  pub fun getReference(key: String): &CyptoLangs? {
    return &self.dictionaryOfCyptoLangss[key] as &CyptoLangs?
  }

  pub fun getCyptoLangsLanguage(key: String): String {
    let CyptoLangsRef = self.getReference(key: key)
    if CyptoLangsRef != nil {
      return CyptoLangsRef!.language
    }
    return "Language not found."
  }
}
```

### Create a script that reads information from that resource using the reference from the function you defined in part 1.

```cadence
import CyptoLangsContract from 0x01

pub fun main(key: String): String {
    let ref = CyptoLangsContract.getReference(key: key)

    if ref != nil {
        return ref!.language
    } else {
        return "Language not found."
    }
}
```

### Explain, in your own words, why references can be useful in Cadence.

References are useful in Cadence because they allow us to access data without moving it around. They help maintain resource security and prevent unnecessary duplication, making them essential when working with valuable assets like resources on the Flow Blockchain.

## Chapter 3 Lession 4

### Explain, in your own words, the 2 things resource interfaces can be used for (we went over both in today’s content)

Resource interfaces in Cadence serve two main purposes: specifying requirements and access control. They act as guidelines for resources, defining the fields and functions they must implement. Additionally, when associated with a resource, interfaces control access, exposing only the specified elements while keeping the rest hidden. This ensures consistency, data privacy, and security within the codebase.

### Define your own contract. Make your own resource interface and a resource that implements the interface. Create 2 functions. In the 1st function, show an example of not restricting the type of the resource and accessing its content. In the 2nd function, show an example of restricting the type of the resource and NOT being able to access its content.

```cadence
pub contract MyContract 
{
  pub resource interface IMyResource {
    pub var name: String
  }

  pub resource MyResource: IMyResource {
    pub var name: String

    pub fun getInfo(): String {
      return "Hello, I am ${self.name}."
    }

    init(name: String) {
      self.name = name
    }
  }

  pub fun exampleNoRestriction(): String {
    let newResource: @MyResource <- create MyResource(name: "Alice")
    let info = newResource.getInfo()
    destroy newResource
    return info
  }

  pub fun exampleWithRestriction(): String {
    let newResource: @MyResource{IMyResource} <- create MyResource(name: "Bob")
    let info = newResource.getInfo() //ERROR HERE: Member of restricted type not available: getInto()
    destroy newResource
    return info
  }
}
```

### How would we fix this code?

Fixed Code
```cadence
pub contract Stuff {

    pub struct interface ITest {
      pub var greeting: String
      pub var favouriteFruit: String
    }

    pub struct Test: ITest {
      pub var greeting: String
      pub var favouriteFruit: String

      pub fun changeGreeting(newGreeting: String): String {
        self.greeting = newGreeting
        return self.greeting
      }

      init() {
        self.greeting = "Hello!"
        self.favouriteFruit = "Mango!"
      }
    }

    pub fun fixThis() {
      let test: Test = Test()
      let newGreeting = test.changeGreeting(newGreeting: "Bonjour!")
      log(newGreeting)
    }
}
```

## Chapter 3 Lession 5

### Access Control Contract, Where the variables can be read from or written in

Contract
```cadence
access(all) contract c {
  pub var testStruct: SomeStruct

  pub struct SomeStruct {

    pub(set) var a: String

    pub var b: String

    access(contract) var c: String

    access(self) var d: String

    pub fun publicFunc() {}

    access(contract) fun contractFunc() {}

    access(self) fun privateFunc() {}


    pub fun structFunc() {
      self.a = "Jacob" //Can be read from and written to
      self.b = "Jacob" //Can be read from and written to
      self.c = "Jacob" //Can be read from and written to
      self.d = "Jacob" //Can be read from and written to
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
        c.testStruct.a = "Jacob" //Can be read from and written to
        c.testStruct.b = "Jacob" //Can be read from
        c.testStruct.c = "Jacob" //Can be read from
        c.testStruct.d = "Jacob" //Can't read or write
        self.e = 0 //Can be read from and written to
      }

      init() {
          self.e = 17
      }
  }

  pub fun createSomeResource(): @SomeResource {
      return <- create SomeResource()
  }

  pub fun questsAreFun() {
      self.testStruct.a = "Jacob" //Can be read from and written to
      self.testStruct.b = "Jacob" //Can be read from
      self.testStruct.c = "Jacob" //Can be read from
      self.testStruct.d = "Jacob" //Can't read or write
  }

  init() {
      self.testStruct = SomeStruct()
  }
}
```

Script
```cadence
import SomeContract from 0x01

pub fun main() {
      SomeContract.a = "Jacob" //Can be read from and written to
      SomeContract.b = "Jacob" //Can be read from
      SomeContract.c = "Jacob" //Can be read from
      SomeContract.d = "Jacob" //Can be read from
}
```

## Chapter 4 Lession 1

### Explain what lives inside of an account.

Inside a Flow blockchain account, there are three main components: contract code, account storage, and account metadata. The contract code is deployed within the account, and multiple contracts can coexist in the same account. Account storage acts as a container for the account's data, residing at paths such as /storage/, /public/, and /private/. 

### What is the difference between the /storage/, /public/, and /private/ paths?

The /storage/ path is accessible only to the account owner, while /public/ can be read by anyone but modified only by the owner. The /private/ path is accessible only to the owner and authorized users. Account metadata holds essential information about the account, such as its address and account keys.

### What does .save() do? What does .load() do? What does .borrow() do?

`.save()` is used to store data into account storage. It takes the data to be saved and the storage path as parameters.

`.load()` is used to retrieve data from account storage. It takes the storage path as a parameter and returns the data.

`.borrow()` is used to get a reference to data in account storage without copying it. It also takes the storage path as a parameter.

### Explain why we couldn’t save something to our account storage inside of a script.

Scripts in Cadence are read-only operations, meaning they cannot modify the state of the blockchain, including account storage. Saving data to account storage is a write operation that changes the state, and only transactions are allowed to perform such modifications.

### Explain why I couldn’t save something to your account.

Because each account in the Flow blockchain has its own isolated storage, which is accessible only to the owner of that account.

### Define a contract that returns a resource that has at least 1 field in it. Then, write 2 transactions: 1. A transaction that first saves the resource to account storage, then loads it out of account storage, logs a field inside the resource, and destroys it. 2. A transaction that first saves the resource to account storage, then borrows a reference to it, and logs a field inside the resource.

Contract
```cadence
pub contract MessageContract 
{
  pub resource Message 
  {
    pub var message: String

    init(_message: String) 
    {
      self.message = _message
    }
  }

  pub fun createMessage(_message: String): @Message 
  {
    return <- create Message(_message: _message)
  }
}
```

Transaction 1
```cadence
import MessageContract from 0x01

transaction{
  prepare(acc: AuthAccount)
  {
    acc.save<@MessageContract.Message>(<- MessageContract.createMessage(_message: "Hello"), to: /storage/message)
    let message <- acc.load<@MessageContract.Message>(from: /storage/message) ?? panic("Nothing found in the path")
    log(message.message)
    destroy message
  }
}
```

Transaction 2
```cadence
import MessageContract from 0x01

transaction{
  prepare(acc: AuthAccount)
  {
    acc.save<@MessageContract.Message>(<- MessageContract.createMessage(_message: "Hello"), to: /storage/message)
    let message = acc.borrow<&MessageContract.Message>(from: /storage/message) ?? panic("Nothing found in the path")
    log(message.message)
  }
}
```

## Chapter 4 Lession 2

### What does .link() do?

The .link() function in Cadence is used to associate a resource with a specific capability and make it accessible from the /public/ or /private/ paths. It allows us to create a "capability link" that points to the resource's actual storage location in the /storage/ path of an account.

### In your own words (no code), explain how we can use resource interfaces to only expose certain things to the /public/ path.

Resource interfaces in Cadence help us control what information from a resource is exposed to the /public/ path. We define a resource interface specifying the fields and functions we want to make public. Then, we modify our resource to implement that interface. Using .link(), we create a capability link between the /public/ path and our resource, restricting access to only the interface-defined elements. Other users can borrow a reference to the resource from /public/, but they'll see only the specified fields and functions, ensuring data visibility control.

### Deploy a contract that contains a resource that implements a resource interface. Then, do the following: 1. In a transaction, save the resource to storage and link it to the public with the restrictive interface. 2. Run a script that tries to access a non-exposed field in the resource interface, and see the error pop up. 3. Run the script and access something you CAN read from. Return it from the script.

Contract
```cadence
pub contract Profile
{
    access(contract) var totalUsersCount: UInt64;

    pub var publicProfileStoragePath: PublicPath;
    pub var storageProfileStoragePath: StoragePath;

    pub resource interface IUserProfilePublic
    {
        pub let id: UInt64;
        pub let address: String;
        pub var name: String;
        
        pub fun getUserProfileInfo(): UserProfileInfo;
    }

    pub resource UserProfile : IUserProfilePublic
    {
        pub let id: UInt64;
        pub let address: String;
        pub var name: String;

        pub fun getUserProfileInfo(): UserProfileInfo
        {
            return UserProfileInfo(self.id, self.name, self.address);
        }

        pub fun updateUserName(_ name: String)
        {
            self.name = name;
        }

        init(_ id: UInt64, _ name: String, _ address: String)
        {
            self.id = id;
            self.name = name;
            self.address = address;
        }
    }

    pub fun createUserProfile(_ name: String, _ address: String): @UserProfile
    {
        let newUserProfile <- create UserProfile(self.totalUsersCount, name, address);
        self.totalUsersCount = self.totalUsersCount + 1;
        return <- newUserProfile;
    }

    pub struct UserProfileInfo
    {
        pub let id: UInt64;
        pub let name: String;
        pub let address: String;

        init(_ id: UInt64, _ name: String, _ address: String)
        {
            self.id = id;
            self.name = name;
            self.address = address;
        }
    }

    init()
    {
        self.totalUsersCount = 0;

        self.publicProfileStoragePath = /public/userProfile;
        self.storageProfileStoragePath = /storage/userProfile;
    }
}
```

Transaction
```cadence
import Profile from 0x01;

transaction(name: String)
{
  prepare(acct: AuthAccount) 
  {
    let newUserProfile <- Profile.createUserProfile(name, acct.address.toString());
    acct.save(<- newUserProfile, to: Profile.storageProfileStoragePath);
    acct.link<&Profile.UserProfile{Profile.IUserProfilePublic}>(Profile.publicProfileStoragePath, target: Profile.storageProfileStoragePath);
  }
}
```

Script with Non-accessable Fields
```cadence
import Profile from 0x01;

pub fun main(add: Address)
{
  let publicCap = getAccount(add).getCapability(Profile.publicProfileStoragePath).borrow<&Profile.UserProfile{Profile.IUserProfilePublic}>() ?? panic("Can't find public path!");
  publicCap.updateUserName("Memxor"); //member of restricted type is not accessable: updateUserName
}
```

Scripts with accessable fields
```cadence
import Profile from 0x01;

pub fun main(add: Address): Profile.UserProfileInfo
{
  let publicCap = getAccount(add).getCapability(Profile.publicProfileStoragePath).borrow<&Profile.UserProfile{Profile.IUserProfilePublic}>() ?? panic("Can't find public path!");
  return publicCap.getUserProfileInfo();
}
```

## Chapter 4 Lession 3

### Why did we add a Collection to this contract? List the two main reasons.

The two main reasons for adding a Collection to the contract are:

1. **Efficient Storage:** Storing multiple NFTs in a Collection is more efficient than creating separate storage paths for each NFT.

2. **Allowing Deposits:** The Collection allows others to deposit their NFTs into our account while restricting access to the `withdraw` function, preventing unauthorized withdrawals.

### What do you have to do if you have resources “nested” inside of another resource? (“Nested resources”)

When you have resources "nested" inside another resource, you must include a `destroy` function for the outer resource. This function manually destroys the nested resources, ensuring proper cleanup and memory management when removing the outer resource. It prevents resource leaks and ensures efficient memory usage.

### Brainstorm some extra things we may want to add to this contract. Think about what might be problematic with this contract and how we could fix it. Idea #1: Do we really want everyone to be able to mint an NFT? 🤔. Idea #2: If we want to read information about our NFTs inside our Collection, right now we have to take it out of the Collection to do so. Is this good?

Idea #1: To address the concern of everyone being able to mint an NFT, we could implement a Minter Resource, which will hold the minting functionality. This resource will only be hold by the admin making sure only the admin can make new mints.

Idea #2: To improve the process of reading information about NFTs inside the Collection, we could create a function within the Collection that allows us to query specific NFTs without withdrawing them from the Collection. This function could take the NFT ID as an argument and return relevant information about the NFT, such as its metadata or ownership details. 

## Chapter 4 Lession 4

### Adding comments to contracts

```cadence
pub contract CryptoPoops {
  pub var totalSupply: UInt64

  // NFT resource represents a unique token with additional information.
  pub resource NFT {
    pub let id: UInt64 // A unique identifier for the NFT.
    pub let name: String // Name of the NFT.
    pub let favouriteFood: String // The NFT's favorite food.
    pub let luckyNumber: Int // A lucky number associated with the NFT.

    // Constructor to initialize the NFT with name, favorite food, and lucky number.
    init(_name: String, _favouriteFood: String, _luckyNumber: Int) {
      self.id = self.uuid
      self.name = _name
      self.favouriteFood = _favouriteFood
      self.luckyNumber = _luckyNumber
    }
  }

  // CollectionPublic resource interface defines functions that can be accessed by the public.
  pub resource interface CollectionPublic {
    pub fun deposit(token: @NFT) // Allows the public to deposit an NFT into the collection.
    pub fun getIDs(): [UInt64] // Allows the public to get a list of NFT IDs in the collection.
    pub fun borrowNFT(id: UInt64): &NFT // Allows the public to borrow a reference to an NFT in the collection.
  }

  // Collection resource represents a collection of NFTs owned by an account.
  pub resource Collection: CollectionPublic {
    pub var ownedNFTs: @{UInt64: NFT} // Dictionary to store NFTs mapped to their IDs.

    // Function to deposit an NFT into the collection.
    pub fun deposit(token: @NFT) {
      self.ownedNFTs[token.id] <-! token
    }

    // Function to withdraw an NFT from the collection using its ID.
    pub fun withdraw(withdrawID: UInt64): @NFT {
      let nft <- self.ownedNFTs.remove(key: withdrawID)
              ?? panic("This NFT does not exist in this Collection.")
      return <- nft
    }

    // Function to get a list of all NFT IDs stored in the collection.
    pub fun getIDs(): [UInt64] {
      return self.ownedNFTs.keys
    }

    // Function to borrow a reference to an NFT in the collection by its ID.
    pub fun borrowNFT(id: UInt64): &NFT {
      return (&self.ownedNFTs[id] as &NFT?)!
    }

    // Constructor to initialize an empty collection.
    init() {
      self.ownedNFTs <- {}
    }

    // Destroy function to clean up resources associated with the collection.
    destroy() {
      destroy self.ownedNFTs
    }
  }

  // Function to create an empty Collection and return a reference to it.
  pub fun createEmptyCollection(): @Collection {
    return <- create Collection()
  }

  // Minter resource represents a resource that can mint NFTs.
  pub resource Minter {

    // Function to create a new NFT and return a reference to it.
    pub fun createNFT(name: String, favouriteFood: String, luckyNumber: Int): @NFT {
      return <- create NFT(_name: name, _favouriteFood: favouriteFood, _luckyNumber: luckyNumber)
    }

    // Function to create a new Minter resource and return a reference to it.
    pub fun createMinter(): @Minter {
      return <- create Minter()
    }

  }

  // Init function initializes the contract, sets totalSupply to 0, and saves a Minter resource to account storage.
  init() {
    self.totalSupply = 0
    self.account.save(<- create Minter(), to: /storage/Minter)
  }
}
```

## Chapter 5 Lession 1

### Describe what an event is, and why it might be useful to a client.

An event in smart contracts allows contracts to notify external clients about specific occurrences or state changes on the blockchain.

### Deploy a contract with an event in it, and emit the event somewhere else in the contract indicating that it happened.

```cadence
pub contract MyContract {

  pub event SomethingHappened(message: String)

  pub fun triggerEvent(message: String) {
    emit SomethingHappened(message: message)
  }
}
```

### Using the contract in step 2), add some pre conditions and post conditions to your contract to get used to writing them out.

```cadence
pub contract MyContract {
  pub event SomethingHappened(message: String)

  pub fun triggerEvent(message: String) {
    pre {
      message.length > 0: "Message should not be empty."
    }
    post {
      let emittedMessage = before(SomethingHappened).message
      emittedMessage == message: "Event not emitted with the correct message."
    }

    emit SomethingHappened(message: message)
  }
}
```

### Contract Return Values

```cadence
pub contract Test {

  // TODO
  // Tell me whether or not this function will log the name.
  // name: 'Jacob'
  // Answer: Yes it will
  pub fun numberOne(name: String) {
    pre {
      name.length == 5: "This name is not cool enough."
    }
    log(name)
  }

  // TODO
  // Tell me whether or not this function will return a value.
  // name: 'Jacob'
  // Answer: No it wont
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
    // Answer: It will fail in post because `before(self.number)` is 0 but `result + 1` is 2, if it fails in post, the value of self.number shiould still be 0
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

## Chapter 5 Lession 2

### Explain why standards can be beneficial to the Flow ecosystem.

Standards in the Flow ecosystem promote interoperability, simplify development, and attract more projects and developers. They ensure consistency, security, and user-friendly interfaces, fostering a robust and sustainable blockchain ecosystem.

### What is YOUR favourite food?

I'm gonna be biased and say Hyderabadi Chicken Biryani

### Please fix this code

Fixed Interface Code 
```cadence
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

  pub resource Stuff: IStuff {
    pub var favouriteActivity: String
  }
}
```

Fixed Contract Code
```cadence
import ITest from 0x01

pub contract Test: ITest {
  pub var number: Int

  pub fun updateNumber(newNumber: Int) {
    self.number = 5
  }

  pub resource Stuff: ITest.IStuff 
  {
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

## Chapter 5 Lession 3

### What does “force casting” with as! do? Why is it useful in our Collection?

Force casting with `as!` allows explicit type changes and is useful when you are certain the conversion is safe. In the CryptoPoops contract, it is used to convert between `@NFT` and `@NonFungibleToken.NFT` to ensure compatibility with the NonFungibleToken standard. It must be used carefully, as incorrect conversions can lead to runtime errors. In the contract, it is employed when storing `@NFT` tokens as `@NonFungibleToken.NFT` in the dictionary.

### What does auth do? When do we use it?

`auth` marks a reference as "authorized," granting additional privileges for certain operations. It is used in scenarios where elevated access or permissions are required. In the CryptoPoops contract, `auth` is used to obtain authorized references when borrowing NFTs from the `ownedNFTs` dictionary. This allows the contract to access the full NFT metadata. 

### 

```cadence
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

    pub fun borrowAuthNFT(id: UInt64): &NFT {
      let ref = (&self.ownedNFTs[id] as auth &NonFungibleToken.NFT?)!
      return ref as! &NFT
    }
    
    pub fun getNFTMetadata(id: UInt64): {String: String, Int} {
      let nft = borrowAuthNFT(id)
      return {"name": nft.name, "favouriteFood": nft.favouriteFood, "luckyNumber": nft.luckyNumber}
    }

    pub fun getIDs(): [UInt64] {
      return self.ownedNFTs.keys
    }

    pub fun borrowNFT(id: UInt64): &NonFungibleToken.NFT {
      return (&self.ownedNFTs[id] as &NonFungibleToken.NFT?)!
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
