# Type Script

> Day 1

## Functions

> When we create any function in TS, while passing any variable or parameter in it we need to define there DataTypes else TS will consider it as "any" which is not a good practice

>> Worng way
```TypeScript
function addTwo(num){	
    return num+2;
}
```

>> Write way
```TypeScript
function addTwo(num: number){		//Specifying data type of parameter
    return num+2;
}

addTwo(3);
```

> But we can pass only two parameters if we want to without causing any error. For that we need to declare anyone(1) of these variable as "false" so we can ignore that particuler variable while passing parameters.
```TypeScript
function login2(name:String , email:String, isPaid : boolean = false){
    console.log(name);
    console.log(isPaid);
    console.log(email);
}

login2("Pritam","my@gmail.com");
```

> In TypeScript we have return types same as we have in java, function below can return number value only.
```TypeScript
function login2(num: number, num2 : number) : number{
    return num+num2;
}
```

> In TypeScript we can return an user too
```TypeScript
function createUser():{name:String,email:String}{

    return{name:"myname",email:"myemail"}

}
```
>While looping any array or a list we can define the output what it will return
>Now this is how we travers a simple array  
```TypeScript
const heros = ["IronMan","ShaktiMan","HanuMan"];

heros.map(hero=>{
    return `hero is ${hero}`
})
```
> Here we are defining the return type as string
```TypeScript
const heros = ["IronMan","ShaktiMan","HanuMan"];

heros.map((hero): string =>{

    return `hero is ${hero}`
})
```
> So in TypeScript we have spacial type of function that never returns any values for that function we use "never" keyword
> Function given below will return nothing not even void but it will work as a checkpoint and will throw message based on given input

```TypeScript
function handleError(message: string) : never{
    throw new Error(message)
}
```
> When it comes in passing any object as parameter in a function we can pass easily
> In example given below we have passed an object contains 2 values which is obvious

```TypeScript
function craeteUser({name: string, email:String}){

}
craeteUser({name: "myname",email:"myemail"});
```
> But in this example we are trying to pass an object contains 3 values and that is not allowed cause our function was build to carry an object with two values 

```TypeScript
function craeteUser({name: string, email:String}){

}
craeteUser({name: "myname",email:"myemail",isActive:true});
```

> We can pass an object which has 3 values even our function was build for object with 2 variable, By creating a new object and pass it into createUser() function
> This should not be allowed but it is

```TypeScript
function craeteUser({name: string, email:String}){

}
let newUser = {name: "myname",email:"myemail",isActive:true};

craeteUser(newUser);
```

## Type Aliases
> Type Aliases allow defining types with a custom name
> Its like You can call a boolean bool but bool will act as boolean

```TypeScript
type User = {		// User is an object 
    name:string;
    email:String;
    contact:number;
};

function creaUser(user :User){	// we can pass user as an object here
    user.contact;
}
creaUser({name:"myname",contact:808503453, email:"myemail@gmail.com"})
```

> Here we need to take care, 
1. There is a new work called "readonly" and it used to make any variable unchangable.
2. There is question mark which make any variable optional to intialize. If we initialize it will be used in code else will be ignored

```TypeScript
type User = {
    readonly id:String
    name:String;
    email:String;
    contact:number;
    credintCard?:String		// The Question Mark(It makes a variable optional to initialize while craete a new Object).
};

function creaUser(user :User) :User{
    return user;
}

let newUser =creaUser({id:"sdjaosjdda", name:"myname",contact:808503453, email:"myemail@gmail.com"})

newUser.id= "newfdsfdfvrefreg43" 		// We cannot change id cause it was defined to 'readonly'
newUser.contact = 4534634654;
newUser.email = "ksnwnf9e@gmail.com";
newUser.name = "Changed name";
```
> We can use type in a different way too .. I dont know why we will use it like this good to know we use it like this

```TypeScript
type cardNumber = {
    cardNumber:String;
}

type cardCvv = {
    cardCvv:Number;
}

type cardDetails = cardNumber & cardCvv;
```



> Now we are having an Interesting thing called Union Type 
> Its a type that can as two or more datatypes.

```TypeScript
// EX. 1

let id : string | number;

id = 23;
id = "23";


// EX. 2

type contact = {
    contactNumber:number;
    email:string;
}

type basicDetails = {
    name:String;
    sirName:String;
}

let userInformation : contact | basicDetails = {email:"myemail",name:"Pritam",contactNumber:234324234, sirName:"MySirname"};
```

> We can have use Union type as a parameter in any function

```TypeScript
function checkId(id : number | string){
    id.toLowerCase();			// here is a check we cannot use property .toLowerCase with id cause id can be an number
}
```

> if we want to use String spescific function than we need to use a filter for it as shown in Ex.
```TypeScript
function checkId(id : number | string){
    if(typeof id === "string"){
        id.toLowerCase();
    }
    if(typeof id === "number"){
        id + 2;
    }
}
```

> Union helps an array in containing 2 or more types of data inside one array 

```TypeScript
const data: number[] = [1,2,4,5,6,4,8];				// Noraml array
const data2: string[] = ["sfsdf","sdfsdf","sdfsd"];
const dataFix: number[] | string[] = [1,2,4,5,5,7,9,0];		// This array can contain either number or String values inside it
const dataMix: (number |string)[] = [1,2,4,5,5,"dsad","ssfd"];	// This array can contain mix type of data (we can add boolean value too if we want to)
```

> Day 2

## Tuples

> This is how we create a sipmle array
```TypeScript
let user :(string | number)[] = [1234,"name"];
```

> This is how we are create tuple
> There is a thing to notice while creating an array we separate them our dataType using 'OR' symbol but in 'touples' we separate them with comma's and the dataType should be arranges like this format

```TypeScript
let newUser:[string, boolean, number];
newUser = ["name",true,3542];

// newUser = ["name",3423,true]; -- it will show error with this
```
## Interface

> Interface is more like 'Type' but an Interface can contain methods which need to implemet while creating an object of it.
```TypeScript
interface myInterFace{

    readonly id : string;   // id cannot be canged cause of readonly perpose
    name : string;
    age : number;
    email : string;
    contactNumber : number;
    cookies ?: string;      // cookies are optional we can igonre it while creating an object

}

const user: myInterFace = {
    // Initializing an object just like type
    id : "myId",
    name : "myname",
    age : 21,
    email : "myemail@gmail.com",
    contactNumber : 9789678954

}

interface myInterFace2{

    readonly id : string;
    name : string;
    age : number;
    email : string;
    contactNumber : number;
    cookies ?: string;
    optStart() : string;        // Function without parameters
    optStop(opt : string) : string;     // Fucntion with parameters

}

const user2: myInterFace2 = {
    
    id : "myId",
    name : "myname",
    age : 21,
    email : "myemail@gmail.com",
    contactNumber : 9789678954,
    // Implementing function
    optStart: () => {
        return "opration started";
    },
    optStop:(opt : "stop") => {
        return "Opertation stopped"
    }

}
```

> **Benifits of an Interface**
Example : 

1. We can re-open it any time 
2. We can inherit it 

```TypeScript
interface myInterFace{

    readonly id : string;
    name : string;
    age : number;
    email : string;
    contactNumber : number;

}

interface myInterFace {
    married : boolean;      // Re-opening and adding one more variable
}

interface Admin extends myInterFace {       // Inheriting another interface 
    
    haveJob : boolean;
    salaried : boolean;
}

const myUser : Admin = {    // creating an object using properties from both

    id : "edasdsid",
    married : true,
    name : "myName",
    age : 21,
    email : "sdfsdfsd@email.com",
    contactNumber : 8593458343,
    haveJob : true,
    salaried : true,

}
```

# Access Modifiers
> Access Modifiers are used for access properties of a class

```TypeScript
class User{

    private name : string;
    // By default every variable has 'public' access modifire
    age : number;
    email : string;
}

let userName = new User();

userName.name;  // It will give us error cause name is not accessable outside the 'User' class
```
> **Constructor**

```TypeScript
class Admin{
    constructor(    // This is how we create constructor in TypeScript

        public adminName : string,
        private id : string,
        public age : number

         ){
    }
}
```

**Getter And Setter**

> Getter and setter are used to get value of an object or set value of an object

```TypeScript
class Admin{
    constructor(    // This is how we create constructor in TypeScript

        public adminName : string,
        private id : string,
        public age : number

         ){

    }

    get getAdminName() : string{
        return this.adminName;
    }

    set setAdminAge(newAge : number){   // a setter cannot have a return type not even void
        this.age = newAge;
    }
    
    // getters and setter can be private so they will be accessable inside class only
    private get getAdminId() : string{    // a getter cannot have parameters
        return this.id;
    }
}
```
> **Protected**
> 'Protected' is a keyword that allows you to access one class properties inside another class, not directly but by 'inheritence' using 'extends' keyword. 

```TypeScript
class Admin{
    constructor(    // This is how we create constructor in TypeScript

        public adminName : string,
        private id : string,
        protected age : number

         ){
    }

    get getAdminName() : string{
        return this.adminName;
    }

    set setAdminAge(newAge : number){   // a setter never have a return type
        this.age = newAge;
    }

    // getters and setter can pe private so they will be accessable inside class only
    private get getAdminId() : string{    // a getter cannot have parameters
        return this.id;
    }
}

class Customer extends Admin{

    customerId : string;
    changeAdminAge(){   // we can access age inside another cause it is protected not private 
        this.age = 34;
    }

}
```

## Abstract Class 
> In TypeScript, an abstract class is a class that cannot be instantiated directly but can only be used as a base class for other classes.

```TypeScript

abstract class Shape {
    abstract getArea() : number;

    printArea(){
        console.log(this.getArea);
        
    }
}

class circle extends Shape{
    constructor(private radius : number){
        super();
    }
    getArea(): number {
        return this.radius;
    }

}
```

## Generics

> Generics provide a way to create re-usable components(function) that can work with multiple types. We dont need to define the input and output type of a function.

```TypeScript
function identity<T>(arg: T): T {       // This function is defined as T and parameter is passed as and Output will be T type too.

    return arg;
}

const value = identity<number>(42);     // Here we are specifying the parameter Type alone with parameter
const result = identity("Hello, TypeScript!");  // Here we are not specifying type of parameter but component will determine it as a String so that it will become input and return type of that function.
```

> In Generics this is how we can use arrays we pass an array as parameter and generic will lock the value that we will take as an input 
> Here we are returning 'T' not an array of T so we need to return any one value from the array


```TypeScript
function printIndex<T>(product : T[]): T {
    return product[2];
}
```

> How we can arrow function with generics or how we traverse generic
> Its almost same as craeting an function we just need to add "<T>" there
> If someone it working in react then we need to add , in "<T>" like "<T,>" this so that react will know it knows it is not coming from JSX

```TypeScript
const getOneIndex = <T> (product : T[]) : T => {
    // Do some operation here

    return product[1];
}
```

> In generic Function we can pass more than one values

```TypeScript
function functionOne<T,U>(valOne: T, valTwo:U): object {
        return {
            valOne,
            valTwo
        }
}

// Here 'U' is extending number so input should have its all properties

function functionTwo<T,U extends number>(valOne: T, valTwo:U): object {     
    return {
        valOne,
        valTwo
    }
}

// calling 'functionTwo'

functionTwo(2,16);      // It's fine two pass number value at place of two

functionTwo(2,"you")    // This is not allowed to pass a string at place of 'U' cause you is extending properties from number and we are passing string in it
```
> Another Example
```TypeScript
interface myInterFace{
    name: string;
    email: string;
    password: string;
}
// We can extend any interface or type
function functionThree<T,U extends myInterFace>(valOne: T, valTwo:U): object {     
    return {
        valOne,
        valTwo
    }
}

functionThree("random String value",{name:"pritam",email:"myemail.com",password:"sknkfhiufe345"});
```

> Another use-case

```TypeScript

interface man{
    age: number;
    height: number;
    name: string;
}

interface women{
    age: number;
    height: number;
    name: string;
}

class people <T>{       // Class is generic
    public peopleList : (T)[] = [];       // list has no spessific type it can any type of data

    addPeople(person: T){
        this.peopleList.push(person);
    }
}

class peoples <man, women>{       // Class is generic
    public peopleList : (man | women)[] = [];       // list has 2 spessific type here

    addPeople(person: man){
        this.peopleList.push(person);
    }
}
```
## Narrowing
> Type narrowing is the removal(filteration) of types from a union(pair of types ex. (string | naumber | null)). To narrow a variable to a specific type, we use the type guard. We can use the 'typeof' operator with the variable name and compare it with the type that you expect for the variable. Basically we are covering every case that can cause error current code or in future

> We'll be using 'typeOf' here

```TypeScript
function narrowCheck(data : string | number | null){
    if(data){       // Check for 'null'
        if(typeof data === "string"){   // check for 'string'
            return data.toLowerCase;    
            
        }else if(typeof data === "number"){ // check for 'number'
            return data+3;
        }
        return null;

    }
}
```
> Example 2

```TypeScript

// We have 2 types here

type fish = {
    swim:() => void
}
type bird = {
    fly:()=> void
}

// Here we are chceking that the 'pet' we are accepting as parmeter is a fish or a bird

function isfish(pet : fish | bird): pet is fish{        // returning pet is fish means parmeter was a fish
    return(pet as fish).swim !== undefined;             // checking that the fish coming contains swim method or not
}
```

> Example 3
```TypeScript
interface Circle{
    kind: "circle";
    radius : number;
}

interface Square{
    kind: "square";
    side: number;
}

type Shape = Circle | Square;

// first way to check the shape is coming is a circle or a square
// This fuction has a drawBack that it cannot handle if a new shape will be added in above type 'Shape"
function getShape(shape : Shape){
    if(shape.kind === "circle"){
        return Math.PI * shape.radius ** 2;
    }
    return shape.side * 2;
}
```

> Example 4

```TypeScript
interface Circle{
    kind: "circle";
    radius : number;
}

interface Square{
    kind: "square";
    side: number;
}
// added rectangle 
interface Rectangle{
    kind: "rectangle";
    hight: number;
    width: number;
}

type Shape = Circle | Square | Rectangle;

// another way to check to check whether a shape is coming is a circle or else (Switch case)

function getProductShape(shape: Shape){
    switch(shape.kind){

        case "circle" : return Math.PI * shape.radius ** 2;
        case "square" : return shape.side * 2;
        case "rectangle": return shape.hight * shape.width;
        // We have handeled shape for all three types.. but what if in future we added shape 'triangle' inside type 'shape'
        default : 
            const newShape : never = shape;
            return newShape;
    }
}
```



