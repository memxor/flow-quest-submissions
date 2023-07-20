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
pub contract CyptoLangsContract {

    pub var dictionaryOfCyptoLangss: @{String: CyptoLangs}

    pub resource CyptoLangs {
        pub let language: String
        init(_language: String) {
            self.language = _language
        }
    }
}
```

### Explain, in your own words, why references can be useful in Cadence.

References are useful in Cadence because they allow us to access data without moving it around. They help maintain resource security and prevent unnecessary duplication, making them essential when working with valuable assets like resources on the Flow Blockchain.

## Chapter 3 Lession 4
