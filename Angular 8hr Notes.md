# Angular 
> Angular framework is a component based framework, Components are main building block of an angular app. Without component we cannot build a proper anguler app.

## Components
> The component is a combination of data HTML template and logics. To create a component just 'rigthClick' on 'app' folder and click on 'New Folder' give it name and pass 'ts' as extention ex. "compname.component.ts".
This component represents an area of a view that show inside the browser.

> Change Port by this commant --ng serve -port 4212--

# Template 
> Template is a keyword wich is used to display all the content of this template value inside the browser

```TypeScript
@Component({
    selector: 'app-navbar',
    template: '<h1> Navbar Component </h1>',
    styles: ['h1{color: red}']
})
```

# Styles
> In code above you've notice that 'styles' keyword which add css inside template content. When write css normally we write it in multiple lines but we cannot write here in multiple lines. to write CSS in multiple line we need write our css inside 'backtics' not in 'single quotes' as shown in code below. Same we can do with templates.

```TypeScript
@Component({
    selector: 'app-navbar',
    template: `<h1> Navbar Component </h1>
                <p>Lorem ipsum dolor sit amet</p>`,
    styles: [`h1{
                color: orange;
                font-size: 23px;
    }`]
})
```
> It is not a good practice to write 'HTML' and 'CSS' like this. Cause it will be hard to read an debug. So we will craete a saparte file for both HTML or CSS and pass them inside TypeScript as shown below.

```TypeScript
import { style } from "@angular/animations";
import { Component } from "@angular/core";

@Component({
    selector: 'app-navbar',
    templateUrl: './navbar.component.html',
    styleUrls: ['./navbar.component.css']
})
```

# Create a component using 'Angular CLI'
>For that just run a command inside your terminal --ng g c compname--
>Here 'g' stands for generate and 'c' stands for component, 'compname' will be your component name. After running this you will a folder in your project with 4 files 1. HTML 2. CSS 3.TS 4.SPEC.TS 4th will be used for testing purpose.

**String InterPoletion or DataBinding in Angular**
> This is the way by which we can add or send data from TS file to HTML file and show it on web page. For example we will declare a title as string inside post.component.ts.
```TypeScript
export class PostComponent implements OnInit{

  title: string = "List of posts";

  constructor(){
  }
}
```
> Then fetch title from TS file we will use double curlyBraces -> {{}} and write 'title' between them and this is how we'll get title value 
```HTML
<p>{{title}}</p>    <!-- like this -->
```
>> Ass of now we know how to send data from TypeScript to HTML file. Now we'll read about how to get data from one component to another

## @Input
> Input is an operator which helps us to send data from parent component to child component. It requare four steps basically.
> 1. We will define and define one variable inside parent Component(inside Ts file) for now I'm going to add variable inside app component. Cause this class is parent of all class. For child class we will be using 'post' component.
```TypeScript
@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  title = 'my-app';
  parentMessage: string = 'message from parent Component';  // variable 'parentMessage' added and initialized
}
```
> 2. Here we need to tell that parent is ready to send some variable or data


> 3. Now we need to accept that variable value from app.component.ts(parent) to post.component.ts(child). So inside child component we are going to add few things.

```TypeScript
import { Component,OnInit,Input } from '@angular/core';     // added Input modeule here

@Component({
  selector: 'app-post',
  templateUrl: './post.component.html',
  styleUrls: ['./post.component.css']
})
export class PostComponent implements OnInit{

  title: string = "List of posts";

  @Input() fromParent:string = "";      // Opening door using @Input for variable coming from parent. From here we'll get this variable inside post.component.html to show it on webpage. As we displayed 'title'

  constructor(){ }

  ngOnInit(): void {
    throw new Error('Method not implemented.');
  }
}
```

> 4. Here we will get variable from its file to html file which will represent it on web page with the help of app.component.html
```HTML
<p>post works!</p>

<p>{{title}}</p>
<h3>{{fromParent}}</h3> <!-- getting data parent variable data -->
```

# @ViewChild Decorator
> This decorator is used to send data from child to parent. 
> Firstly import view child from angular/core and add @ViewChild decorator. Now import child Component and pass it inside @ViewChild decorator and give it a name.
> Here child component will as like postComponent. Now we have access of every variable inside postComponent, we need to extract it.
```TypeScript
import { Component, AfterViewInit, ViewChild, ChangeDetectorRef } from '@angular/core'; // Importing ViewChild decorator
// Added 'AfterViewInIt' to extract dataFrom child Component. Also added 'ChangeDetectorRef' to detect changes
import { PostComponent } from './post/post.component';// Importing postComponent(childComponent)

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent implements AfterViewInit{

  constructor( private cdref: ChangeDetectorRef ) {} 

  @ViewChild(PostComponent) childComp !: PostComponent; // passing child component inside @ViewChild Decorator and giving it a name childComp. I've Initialized it cause if i dont than typeScript will give me error
  message:string = '';// This will store the string value coming from postComponent 

  ngAfterViewInit() {   // 'ngAfterViewInIt' will help us to extract values from 'childComponent'
      if(this.childComp){
        this.message = this.childComp.postChild;  // extracting postChild from PostComponent and storing into message
        this.cdref.detectChanges(); // This line is to remove error cause when we initalize any variable from object, TypeScript will throw an error cause it dont know when and where we made chages so we need to tell it. By using detectChange method. 
      }
  }

  title = 'my-app';
  parentMessage: string = 'message from parent Component';
}
```
>Now we got message initialized with data. We just need to it on page using HTML file(app.component.html)
```HTML
<h1>Pritam Sir</h1>
<p>{{message}}</p>
```

# @Output
> This is one more decorator which will help us to send data from childComponent to parent component. We need 'Event Emmiter' also.
> First of all we will create an string variable inside childComponent(postComponent). Then we will import 'Output' and 'EventEmmiter'.
>Create an EventEmmiter. Now we need to create a button so we can use that 'messageEvent' using an function, And that function will be called after clicking the button. 
```TypeScript
import { Component, OnInit, Input, Output, EventEmitter } from '@angular/core';   // Added an Output decorator and EventEmmiter 

@Component({
  selector: 'app-post',
  templateUrl: './post.component.html',
  styleUrls: ['./post.component.css']
})
export class PostComponent implements OnInit{
  
  constructor() {};

  title: string = "List of posts";

  postList: string = "This list of post";

  postChild: string = 'Component from post as a child';

  messageFromPost:string = "This message coming after clicking a button";   // message create to show on web page by clicking the button via EventEmmiter.

  @Input() fromParent:string = ""; 

  @Output() messageEvent = new EventEmitter<string>();    // Event Created which will get call after clicking a button

  sendMessage() {   // This message will get call after clicking the button
    this.messageEvent.emit(this.messageFromPost);
  }

  ngOnInit(): void {
  }

}
```
```HTML
<p>post works!</p>

<app-postlist [fromPostParent]='postList'></app-postlist>

<button (click)="sendMessage()">Button</button>   <!--button created here. In javaScript we use 'onclick()' function but in TypeScript we have(click)="" to call a function -->

<p>{{title}}</p>
<h3>{{fromParent}}</h3>
```
> Till now we sent a message from child side but we need to recive it from parentSide too. So we will recive in HTML file of 'appComponent'(parent).
```HTML
<h1>Pritam Sir</h1>
<p>{{message}}</p>

<app-navbar></app-navbar>

<app-post [fromParent]='parentMessage' (messageEvent)="reciveMessage($event)"></app-post><!--recieveing message -->
```
>after recieving we need to call it inside TS file
```TypeScript
import { Component, AfterViewInit, ViewChild, ChangeDetectorRef } from '@angular/core';
import { PostComponent } from './post/post.component';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent implements AfterViewInit{

  constructor( private cdref: ChangeDetectorRef ) {} 

  @ViewChild(PostComponent) childComp !: PostComponent;
  message:string = '';

  ngAfterViewInit() {
      if(this.childComp){
        this.message = this.childComp.postChild;
        this.cdref.detectChanges();
      }
      
  }

  reciveMessage($event:string){     
    console.log($event);            

  }
    childMessageRecived:string ="";

  reciveMessage($event:string){
    console.log($event);                  // Reciving message.
    this.childMessageRecived = $event;    // We can use this message wherever we want, as of now we will show it only on our page by using HTML file
  }

  title = 'my-app';
  parentMessage: string = 'message from parent Component';
}
```
```HTML
<h1>Pritam Sir</h1>
<p>{{message}}</p>

<h2>{{childMessageRecived}}</h2>  <!-- here we representing our message coming from child component to parent. You add CSS on it if you want to show it uniqly -->

<app-navbar></app-navbar>

<app-post [fromParent]='parentMessage' (messageEvent)="reciveMessage($event)"></app-post>
```

**1. String Interpulation, 2. Property Binding, 3. Class Binding 4. Style Binding 5. Event Binding 6. Input Handling 7. One-Way Data binding 8. Two-Way Data binding**

>> Focus on HTML code cause HTML code has important keywords and I've commented what those keyword do and how.

Type Script
```TypeScript
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent{

  message: string = "This is the message";

  imgUrl: string = "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQrOCP7s0MdmZeE9eVE6xK-lqN1FlZnRy4Vdw&usqp=CAU";

  bool: boolean = true;

  title: String = "Title is inside app.component.ts";

  userName:string ="Two way binding";

  textValue: string = "One Way Binding";

  buttonClick(){
    console.log("Button Clicked")
  }
  
  enterKeyPressed(){
    console.log("Enter Key pressed");
  }

  getInputValue(username:string){
    console.log(username);
  }

  getInputValueInTwoWay(){
    console.log(this.userName);
  }
 
  getInputFromComp(){
    console.log(this.textValue);
  }

}

```
HTML
```HTML
<!-- String Interpulation -->
<p>{{ message }}</p>

<img src="{{ imgUrl }}" alt="">  <!--This is how we add image url normally-->

<h1 class="text-red">Pritam Sir</h1> <!--This is how we add class inside HTML text-->

<!-- Property Binding in Angular -->
<img [src]="imgUrl" alt="">

<!-- Class Binding in Angular -->
<h1 [class.text-red]="bool" >Pritam Sir</h1>

<!-- Style Binding in Angular (One line CSS)-->
<p [style.color]="bool? 'black' : 'blue'">Style Binding in Angular</p>

<!-- Event Binding in Angular -->
<button (click)="buttonClick()">Click Button</button>

<br>

<!-- Input Handling in Angular-->
<input type="text" (keyup.enter)="enterKeyPressed()">

<br>
<!-- Input Handling again but In this we are getting data from input and using it by a fuction after pressing on enter key -->
<input type="text" (keyup.enter)="getInputValue(username.value)" #username>

<br>
<!-- One-Way Data binding : In this binding we are able to display a variable data available inside our TS file -->
<input type="text" [value]="textValue" (keyup.enter)="getInputFromComp()">

<br>
<!-- Two-Way Data binding : In this binding we can get data from our TS file as well as we can get data from that Input section and use it any function-->
<input type="text" [(ngModel)] = "userName" (keyup.enter)="getInputValueInTwoWay()" >

```

CSS
```CSS
h2{
    color: aqua;
}
.text-red{
    color: rgb(197, 39, 39);
}
```

## Angular Directive
> An Angular Directive is special type of technology that can manipulate the DOM object. Like add HTML elements or remove HTML elements from DOM dynamically.

>> Basically Directives are Components without view or Angular Components are directives, with a view.

1. Component Directive
2. Structural //
3. Attribute  //
4. Custom     //

 **1. Component Directive**
> A directive with a template view.

**2. Structural Directive**
> A directive that can change DOM layout by adding and removing DOM elements.

**3. Attribute Directive**
> A directive that can change the appearence or behavior of an element, component or another directive.

**4. Custom Directive**
> A directive which can create custom directive from scratch.

### NgFor Directive
 1. We use NgFor directive to travers any array inside view(webpage)
 2. NgFor directive is a Structural Directive.
 3. With NgFor we can Manipulate the DOM.

 >Lets take an example, We need to print(show) array elements one by one.

TypeScript
 ```TypeScript
  postArray: Array<string> = ['post1','post2','post3','post4','post5'];
 ```

 There is 2 way to do that 
 > one-> We can print them one by one using un-ordered list. But this case can succed when we know length of the array

HTML
 ```HTML
<p>{{postArray}}</p>
<ul>
    <li>{{postArray[0]}}</li>
    <li>{{postArray[1]}}</li>
    <li>{{postArray[2]}}</li>
    <li>{{postArray[3]}}</li>
    <li>{{postArray[4]}}</li>
</ul>
```

-> Two-> Second way is using ngFore

HTML
```HTML
<ul>
    <li *ngFor="let post of postArray">{{post}}</li>
    <!-- let is an DataType, post is traversing variable name, of telling that 'post' will be an element of 'postArray', 'postArray' is an array -->
</ul>
```
>> Lets see how we can traverse an object array

TypeScript
```TypeScript
  objectArray: Array<object> = [
    {id: 1, name: 'pritam', email:'myemail1.com'},
    {id: 2, name: 'pritamji', email:'myemail2.com'},
    {id: 3, name: 'pritamsir', email:'myemail3.com'},
    {id: 4, name: 'shripritam', email:'myemail4.com'},
    {id: 5, name: 'shreeshreepritam', email:'myemail5.com'},
    
  ]// objectArray created and initialized
  ```
When need to show all objects one by one of this array

HTML
```HTML
<ul>
    <li *ngFor="let post of objectArray">{{post | json}}</li>
    <!-- it will show an array on web page, as we initialized it in TS file -->
    <!-- 
      { "id": 1, "name": "pritam", "email": "myemail1.com" }
      { "id": 2, "name": "pritamji", "email": "myemail2.com" }
      { "id": 3, "name": "pritamsir", "email": "myemail3.com" }
      { "id": 4, "name": "shripritam", "email": "myemail4.com" }
      { "id": 5, "name": "shreeshreepritam", "email": "myemail5.com" }
      Like this
    -->

</ul>
```
But what if we want only 'name' of every single array

HTML
```HTML
<ul>
    <li *ngFor="let post of objectArray">{{post.name}}</li>
    <!-- This code might show some error and dont know why this error is coming. To solve this error we need to change 'Array<object>' to 'Array<any>' -->
</ul>
```

## Change Detection in Angular
> In angular when we change somthing in our code Angular will automatically reflect it on our webpage. Let's try it by adding an object in an array of objects. We will add and delete an object in array by clicking a button

**Add Element In an array**

HTML
```HTML
<button (click)="addOneObject()">Add Object</button>
```

TypeScript
```TypeScript
  addOneObject(){
    this.objectArray.push({id:6,name:'pritamsaini',email:'myenail'});
  }
```
**Delete Element from an array**

HTML
```HTML
<ul>
    <li *ngFor="let post of objectArray">{{post.name}}
    <button (click)="deleteOne(post)">Delete</button>
    </li>
</ul>
<!-- according to this code every object or property that are visible on page will have a delete button with them to delete -->
```

TypeScript
```TypeScript
  deleteOne(postObj:object){
    let index = this.objectArray.indexOf(postObj);
    this.objectArray.splice(index, 1);  // Adding 1 with index cause if we tried to delete first object of that array thn whole array objects will be deleted. So by passing 1 we are telling that we want to delete one object at a time
  }
```

>> We can optimize this code by passing an index from HTML file 

HTML
```HTML
<ul>
    <li *ngFor="let post of objectArray; let i = index">{{post.name}}<!-- we have added one variable 'i' which will be initialized as an index of array -->
    <button (click)="deleteOne(i)">Delete</button> <!-- we are passing index so we can delete it easily and noew we dont need to extract index from its object -->
    </li>
</ul>
```

TypeScript
```TypeScript

  deleteOne(index:number){
    this.objectArray.splice(index, 1);
  }

```
## ngIf 
> NOW we got interesting thing that what if we have an empty list. How can we handle that situation

HTML
```HTML
<div *ngIf="postArray.length > 0">  <!-- if an array is has list of string we'll move forverd and render them on web page -->
    <ul>
        <li *ngFor="let post of postArray">{{post}}</li>
    </ul>
</div>
<div *ngIf="postArray.length == 0">   <!-- when we dont have thn we'll simply show an message that 'List was empty' -->
    <p>List was empty</p>
</div>
```

TypeScript
```TypeScript
  postArray: Array<string> = [];// empty list
```

## ng-template
To make above code more interesting we have ng-template
 
 HTML
 ```HTML
 <div *ngIf="postArray.length > 0;then dataAvailable else noData">
  <!-- this 'div' contain condition different different action on true and false of that
  if condition is true then run  'dataAvailable' template else run 'noData', its like we are calling different function based on conditions cool na? super cool : aag lga di aag-->
</div>

<ng-template #dataAvailable>
    <ul>
        <li *ngFor="let post of postArray">{{post}}</li>
    </ul>
</ng-template>

<ng-template #noData>
    <p>List was empty</p>
</ng-template>
```

## ng-Switch

> Ng switch nothing but contains same concept as switch cases in java

HTML
```HTML
<div><!-- On bases of different button different pages will get call-->
    <button (click)="callPage('btn1')">btn1</button>
    <button (click)="callPage('btn2')">btn2</button>
    <button (click)="callPage('btn3')">btn3</button>
</div>

<div [ngSwitch]="step"> <!-- 'step' is a string variable in TS file-->
    <div *ngSwitchCase="'btn1'">page1</div>
    <div *ngSwitchCase="'btn2'">page2</div>
    <div *ngSwitchCase="'btn3'">page3</div>
    <div *ngSwitchDefault>Something else</div>
</div>

```

## ngStyle Directive

HTML
```HTML
<!-- This is how we add CSS using 'Style Binding' -->
<!-- 'isActive' is a boolean variable in TS file-->
<h1 [style.color]="isActive ? 'red': 'black'"   
    [style.textTransform]="isActive ? 'uppercase': 'lowercase'"
>Working like this is my last day</h1>
```
Using [ngStyle]:
```HTML
<!-- This is how we add CSS using [ngStyle] it is same like we are adding a complete CSS as we were adding in seperate files-->
<!-- One thing we should remember that there should always be an value in 'else' part otherwise it will give error in compile-Time  -->
<!-- 'isActive' is a boolean variable in TS file-->
<h1 [ngStyle]="{
    color: isActive ? 'red': 'aqua',
    textTransform: isActive ? 'uppercase' : 'lowercase'
}"
>Working like this is my last day</h1>
```

> It is not a good idea to add Inline CSS like this we have a better approach for this.

## ngClass
> With the help of that class we can add multiple class's CSS inside on tag('H1' tag or 'div' tag). Here is an example.

CSS
```CSS
.main{
    color: aqua;
}
.tag{
    text-transform: uppercase;
}
```

HTML
```HTML
<!-- We have two class inside our CSS file 'main' or 'tag', their CSS will be applied to data inside 'h1' tag when 'isActive' is true. 
'isAvtive' is a boolean variable in TS file-->
<h1 [ngClass]="{    
    'main' : isActive,
    'tag' : isActive
}"
>Let's do it</h1>
```

## Task given in video
> We need to add some data like 'name','email' and 'address' and show the right below that input form.

HTML
```HTML
<div [ngClass]="{
    'center':true   
}">
    <input type="text" placeholder="Name" [(ngModel)]="name"><br><br>

    <input type="text" placeholder="Email" [(ngModel)]="email"><br><br>

    <textarea placeholder="Address" id="" cols="30" rows="10" [(ngModel)]="address"></textarea><br><br>

    <button (click)="addData()">Submit</button> <br><br><br>

    <table style="
    vertical-align: middle;
    margin-left: 600px;
    width: 40%;">

        <tr>
            <th>#</th>
            <th>name</th>
            <th>email</th>
            <th>address</th>
        </tr>

        <tr *ngFor="let data of dataArray; let i = index">
            <td>{{data.index}}</td>
            <td>{{data.name}}</td>
            <td>{{data.email}}</td>
            <td>{{data.address}}
                <button (click)="deleteData(i)"> Delete</button>
            </td>

        </tr>
    </table>
</div>
```

CSS
```CSS
table, th, td {
    border:1px solid black;
    text-align: center;
}
.tbl{
    margin-left: auto;
    margin-left: auto;
}
.center {
    padding: 70px 300;
    text-align: center;
}
```

TypeScript
```TypeScript
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})

export class AppComponent{

  title: string = "";

  index:number = 0;

  name: string = "";

  email: string = "";

  address: string = "";

  dataArray: Array<any> = [];

  addData(){
    this.dataArray.push({index:this.dataArray.length+1, name: this.name,email:this.email,address:this.address});
    console.log(this.dataArray);
    
  }

  deleteData(indexNumber:number){
    this.dataArray.splice(indexNumber,1);
  }

}
```

## Angular Pipes
>Pipes are simply a function that we can directly apply on any expression/value in a template to transform it into some other value.

### UpperCase and Lowercase Pipe, Number and decimal pipe, Currency Pipe
HTML
```HTML
<!-- By adding lowercase like this it will convert it into lowercase string, title is a string variable -->
<h1>{{ title | lowercase }}</h1> 

<!-- numbers is used to convert '123123' to '123,123' this it will add comas to make it more readable -->
<h1>{{ numbers | number }}</h1>

<!-- By '1.0-3' we are telling that we want a decimle value with 1 charachter before decimal and 0 to 3 character after dacimal-->
<h1>{{ decimals | number: '1.0-3' }}</h1>

<!-- by adding currency 99.99 changed into ₹99.99 -->
<h1>{{ price | currency: 'INR' }}</h1>

<!-- If you want to show country code thn add false after currency code... -->
<!-- Also if you want to fix number or decimal number in your amount hn add rest of the code -->
<h1>{{ price | currency: 'INR': false : '1.1-1'}}</h1>

<h1> {{ postObject }}</h1><!-- [object Object] <-- this will be the output -->
<h1> {{ postObject | json }}</h1><!-- { "id": 1, "name": "PritamSir", "email": "myemail.com" } -->

<!-- By percent pipe we can get percentage of any number -->
<h1>{{ 0.564 | percent}}</h1>
<!-- We can control digits also -->
<h1>{{ 0.564 | percent : '1.1-2'}}</h1>

<!-- slice pipe is used to remove elements in a range -->
<h1>{{ stringArray | slice :0:3 }}</h1>

```
## Custom Pipe
> We can create a our random PIPE which is very interesting part. we can create a PIPE easily.
>> Creating a PIPE is super easy. Here we will try to create an PIPE named "append" which will add 'My country' in front of Country name.

HTML
```HTML
<!-- append will be our PIPE name -->
<h1>{{ country | append}}</h1>
```
> Now we need ti create a seperate folder for our pipes so we can add PIPES in it.
> After creating a folder we need to create an TS file which can have any name like(pipes.append.ts) keep this name reamarkable which will denote our PIPE uniquely.
> Now create a class and import a 'Pipe' and 'PipeTransform' from application/core. And Implement 'PipeTransform' in your class
> 'PipeTransform' will offer an method. Which we need to Implement(modify) as per our requirement as shown in below code.

```TypeScript
import { Pipe, PipeTransform } from "@angular/core";

@Pipe({name : "append"})  // Don't forget to give name to your PIPE

export class AppendPipe implements PipeTransform{
    transform(value: any, ...args: any[]) {
        return "My country " + value;
    }
}
```
> One last this we need to do is. We need to tell Angular that I have created an PIPE and I want to use it.
>> For that just go to 'app.module.ts' file and add class name of your PIPE inside 'declarations' array.

## Creating Pipe using AngularCLI
> By Using AngularCLI we can easily create our PIPES. We just need to run a simple command.

>ng g pipe Pipes/appendCLI

>>>ng-> Angular

>>>g-> generate

>>>pipe-> it tells that we want to generate pipe

>>>Pipes/appendCLI-> pipes was the folder name which is already available inside my app. You can create new if it does'nt exist. appendCLI was name of file where we will be 'create' and 'implement' our PIPE. 

## Custom Pipe with Arguments
> By this way we can create a Pipe as well as pass a argument with this. Lets generate a PIPE which will trim our String length within 12 characters or as many character we want to trim and show them on webpage.

HTML
```HTML
<!-- trimText will be our PIPE name -->
<h1>{{ summary | trimText : 12 }}</h1>
```

TypeScript
```TypeScript
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'trimText'
})
export class TrimTextPipe implements PipeTransform {

  transform(value: string, length: number): unknown {
    return value.substring(0,length);
  }
}
```

# Angular Services
>It is basically a class that has a well-defined purpose to do something. You can create a service class for data or logic that is not associated with any specific view to share across components.

> We can create a service as we create a pipe. We need to create a folder thn we'll craete a TS-file with our service name. Then create a class of any name, declare an array of string in it.

```TypeScript
export class PostService{
    dataList: Array<string> = ['post1','post1','post1','post1','post1'];
}
```
> Now we need to use this service inside another component. So we will import and use it inside app.component.ts file.

TypeScript
```TypeScript
import { Component } from '@angular/core';
import { PostService } from './services/services.newService'; // Importing

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})

export class AppComponent{

  dataList: Array<string> = [];

  constructor () {
    let service = new PostService(); // Creating an instance of PostService
    this.dataList = service.dataList;

  }

  title: string = "Check Uppercase and Lowercase";
}
```
> There is one more better way to do that

TypeScript
```TypeScript
import { Component } from '@angular/core';
import { PostService } from './services/services.newService';// Importing PostService

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']

})

export class AppComponent{

  dataList: Array<string> = [];

  constructor (private service: PostService) {  // Injecting class
    // let service = new PostService();
    this.dataList = service.dataList; // Getting data

  }

  title: string = "Check Uppercase and Lowercase";
  // After doing this there is one more thing that we need to do. We'll go to the app.module.ts file and add 'PostService'(service name) inside 'providers' array, it will show you some error. It that will tell angular that this class is an injectable class.

}
```

> I tried it to inject it inside one more component. Which worked fine. It was almost the same process. At the end we need to add our this component HTML to main component HTML, to make it visible on webpage.

TypeScript
```TypeScript
import { Component } from '@angular/core';
import { PostService } from '../services/services.newService';

@Component({
  selector: 'app-post-list',
  templateUrl: './post-list.component.html',
  styleUrls: ['./post-list.component.css']
})
export class PostListComponent {

  postList: Array<string> = [];

  constructor(private service : PostService){
    this.postList = service.dataList;
  }

}
```
**Create a service class using Angular CLI**
> We can create a service class using Angular CLI. By running one command

>ng g s services/user

>>>ng-> Angular

>>>g-> generate

>>>s-> it tells that we want to generate service

>>>services/user-> services was the folder name which is already available inside my app. You can create new if it does'nt exist. user was name of file where we will be 'create' and 'implement' our service. 

> after creating an service we can see that service class has '@injectable' thing which is strange for us. No need to worry cause its the same thing that we do while adding our class to 'providers' in 'app.module.ts' file. Both helps injecting a service inside components.

TypeScript
```TypeScript
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class UserService {

  constructor() { }
}

```
**Use method of a service**
> To use a method from service I've added a new method inside 'PostService' class 'addData(data: string)'. It will take 'data'(string) as a parameter.

TypeScript -> Service 
```TypeScript
export class PostService{
    dataList: Array<string> = ['post1','post1','post1','post1','post1'];

    addData(data: string){  // Method to add data which will be called by a method inside any component
        this.dataList.push(data);
    }
}
```

TypeScript -> Component
```TypeScript
export class AppComponent{

addNewData() {    // This method will call 'addData()' method from PostService.
  this.service.addData("another post"); // calling service method and passing 'another post' as a parameter
}

  dataList: Array<string> = [];

  constructor (private service: PostService) {
    // let service = new PostService();
    this.dataList = service.dataList;
    
  }

  title: string = "Check Uppercase and Lowercase";

}
```
## Interface
> As we know interfaces are similar to types in TypeScript. Interfaces define properties, methods, and events, whose are the members of the interface. Interfaces contain only the declaration of the members. I've alrady written about it and not gonna write more about it. One more thing interfaces help us to reduce human errors while writing code like spelling mistake or passing half-completed objects.

## Angular Forms
> We have two types of forms in Angular.
1. Template-Driven form
2. Reactive form
> I've created a simple form which contains 2 inputs(name, email), 1 textarea(adress) and a button(submit).

HTML
```HTML
<div class="container mt-5"><!-- this container.. class gives this form required padding and keep the form in center of the page -->
    <div class="form-area">
        <form >
    
            <div class="form-group"><!-- 'form-group' class gives this 'div' margin to separate from other inputs-->
                <label> First Name </label>
                <input type="text" placeholder="FullName" class="form-control"><!-- this 'form-control' class added <br> tag so this form input will be positioned below label -->
            </div>
    
            <div class="form-group">
                <label> Email </label>
                <input type="text" placeholder="email" class="form-control">
            </div>
    
            <div class="form-group">
                <label> TextArea </label>
                <textarea placeholder="address" cols="30" rows="10" class="form-control"></textarea>
            </div>
    
        <button type="submit" class="btn btn-primary">Submit</button>
    
        </form>
    
    </div>

</div>
```

**Converting a simple form to Template-Driven form usingNgForm Directive**
> To convert this from to Template-Driven Form we just need to create a template name (as we know already) #form and inside <form> tag and assign it to 'ngform' that is it. 

HTML
```HTML
<!-- In above code you can see that we have a form tag. To make that form a Template-Driven form we need to add "#form = 'ngForm'" this I've given this template name 'form' you can give it any name.-->
        <form #form = 'ngForm' (submit)="onSubmit(form)"><!-- This '(submit)' will be called when we will submit this form. So we can process data of this form. For now we will print form-data. But But but we cannot print data from every field of form, we can print form but not data from its fields. To do that we need to add 'ngModel' in specific field tag and give it a name. Name is complesory else we'll get errors-->
```

HTML
```HTML
<div class="container mt-5">
    <div class="form-area">
        <form #form = 'ngForm' (submit)="onSubmit(form)"> <!-- We are printing the form with its whole properties in console -->
    
            <div class="form-group">
                <label> First Name </label>
                <input type="text" placeholder="FullName" class="form-control" name="fullName" ngModel> <!-- added name and ngModel in it -->
            </div>
    
            <div class="form-group">
                <label> Email </label>
                <input type="text" placeholder="email" class="form-control" name="email" ngModel>
            </div>
    
            <div class="form-group">
                <label> TextArea </label>
                <textarea placeholder="address" cols="30" rows="10" class="form-control" name="address" ngModel></textarea>
            </div>
    
            <button type="submit" class="btn btn-primary">Submit</button>
        </form>
    </div>
</div>
```

TypeScript
```TypeScript
export class AppComponent{

  title: string = "Check Uppercase and Lowercase";

  onSubmit(form:NgForm){
    console.log(form);    // Printing a form-- if you want to print name print 'form.value'
    
  }

  getFullName(f: NgModel){    // In youtube tutorial instructor was passing NgControl here but I was getting error and I passed Ngmodel and it   works fine
    console.log(f);       // Printing name field with its properties not 'name'
  }

}
```
> Okay! Now we are going to validate input field.

HTML
```HTML
<!-- From above code I've picked-up one input field 'fullname' and add changes in it-->
            <div class="form-group">
                <label> First Name </label>
                <input type="text"

                placeholder="FullName"
                class="form-control"
                name="fullName" 
                ngModel
                #fullname = 'ngModel' 
                (change)="getFullName(fullname)"
                required
                >
              <!-- #fullname is a template and we can access all properties related to this field. (change) is an event which will get activated when we click outside the field after clicking inside* the field. For example if we clicked inside the field to write 'name' but we didnt write any thing and then we tried to click on another field or any-where it must show an error cause a name field cannot be empty.-->
              <!-- To execute this plass we created a div and added a class in it 'alert alert-danger'. This class will show an error outside the field and error message will be same as we passed between 'div' tags-->

              <!-- We can show an error now but don't know when to show the message. Don't worry i got you, at this time 'fullname' has many properties 'invalid' and 'touched' are two of them. Invalid will be true if we have entered something inside field else it will be false. 'Touched' will be 'true' if we clicked once inside the field else false, Its name is telling what it means-->

                <div class="alert alert-danger" *ngIf="fullname.invalid && fullname.touched">Field cannot be empty..</div>

              <!-- Error will be thrown when 'fullname.invalid && fullname.touched' <- both codition are true. 1. Field should be clicked means 'Touched' is true. 2. Field is empty means 'Invalid' is true  -->
            </div>
```
**Lets add some style on Invalid Input**

HTML
```HTML
<!-- Same code again -->
            <div class="form-group">
                <label> First Name </label>
                <input type="text"

                placeholder="FullName"
                class="form-control"
                name="fullName" 
                ngModel
                #fullname = 'ngModel'
                (change)="getFullName(fullname)"
                required

                [ngClass]="{'is-invalid':fullname.invalid && fullname.touched}"
                >
                <!-- If you noticed we have added a class 'is-invalid' here which will be visible when given condition is true. Basically this class will show a red icon and highlight field border with red color.-->

                <div class="alert alert-danger" *ngIf="fullname.invalid && fullname.touched">Field cannot be empty..</div>
            </div>
```
**Let's add some more validations**

HTML
```HTML
            <div class="form-group">
                <label> First Name </label>
                <input type="text"

                placeholder="FullName"
                class="form-control"
                name="fullName" 
                ngModel
                #fullname = 'ngModel'
                (change)="getFullName(fullname)"
                required
                [ngClass]="{'is-invalid':fullname.invalid && fullname.touched}"
                minlength="5"
                maxlength="20"
                >
                <!-- added max and min-length -->

                <div *ngIf="fullname.errors?.['required']"><!-- We are cathing error here and showing error response according to error -->
                    <div class="alert alert-danger" *ngIf="fullname.invalid && fullname.touched">Field cannot be empty..</div>
                </div>
                <div *ngIf="fullname.errors?.['minlength']"><!-- Same here-->
                    <div class="alert alert-danger" *ngIf="fullname.invalid && fullname.touched">Enter atleast 5 charecters..</div>
                </div>
                <div *ngIf="fullname.errors?.['maxlength']"><!-- max length was limiting the number of characters but not throwing error when we pass less-->
                    <div class="alert alert-danger" *ngIf="fullname.valid && fullname.touched">You cannot enter more character then 20</div>
                </div>
            </div>
```

**Now we need to disble Submition of form if it contains no value or invalid values.**

HTML
```HTML
<!-- Its super easy just add this property binding. 'form' was a template name if you remember #form -->
        <button type="submit" class="btn btn-primary" [disabled]="form.invalid">Submit</button>
```

## REACTIVE FORMS
Reactive forms provide a model-driven approach to handling form inputs whose values change over time. And we have all control in TS file of a component not in HTML file.

HTML
```HTML
<div class="container mt-5">
    <div class="form-area">
<!--  -->
        <form [formGroup]="reactiveForm" (ngSubmit)="onSubmit()"> <!-- form group added and It was created already inside TS file -->
    
            <div class="form-group">  <!--   -->
                <label> Full Name </label>
                <input type="text"
                placeholder="FullName"
                class="form-control"
                name="fullName" 
                formControlName= "fullname" 
                
                >
                <!--  We are telling that this field will have form name 'fullname' basically we are connecting this field to 'fullname' formControl -->

                <div class="alert alert-danger" *ngIf="getFullName.touched && getFullName.invalid">   <!-- Same logic and css as previous form -->
                    <div *ngIf="getFullName.errors?.['required']">Field cannot be empty</div><!-- Here we need to give some attention cause 'getFullName' is a contructor here not a template as previous form cause we cannot call 'fullname' directly. And this constructor will help access the 'fullname'(formControl), which was inside 'reactiveForm'(formGroup). -->
                    <div *ngIf="getFullName.errors?.['minlength']">Full Name should be minimum 5 character long</div>
                </div>

            </div>
    
            <div class="form-group">
                <label> Email </label>
                <input 
                type="text" 
                placeholder="email" 
                class="form-control" 
                name="email" 
                formControlName= "email"
                >

                <div class="alert alert-danger" *ngIf="getEmail.touched && getEmail.invalid">
                    <div *ngIf="getEmail.errors?.['required']">Field cannot be empty</div>
                    <div *ngIf="getEmail.errors?.['email']">Invalid Email</div>
                </div>

            </div>
    
            <div class="form-group">
                <label> TextArea </label>
                <textarea 

                placeholder="address" 
                cols="30" 
                rows="10" 
                class="form-control" 
                name="address" 
                formControlName= "address"
                
                ></textarea>

                <div class="alert alert-danger" *ngIf="getAddress.touched && getAddress.invalid">
                    <div *ngIf="getAddress.errors?.['required']">Field cannot be empty</div>
                    <div *ngIf="getAddress.errors?.['minlength']">Please enter atleast 10 character</div>
                </div>

            </div>
    
        <button type="submit" class="btn btn-primary" [disabled]="reactiveForm.invalid">Submit</button>
    
        </form>
    </div>
</div>
```

TypeScript
```TypeScript
// Now this is the main and interesting part
import { Component } from '@angular/core';
import { FormControl, FormGroup, Validators } from '@angular/forms'; // Import them and remember you should have 'ReactiveFormsModule' this in your .app.module.ts'.

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']

})

export class AppComponent{

  reactiveForm: any;  // creating form with name 'reactiveForm'.

  constructor(){
    this.reactiveForm = new FormGroup({   // Initializing a from as well as adding formControls(fields) in it.
    // fullname: new FormControl()  <== this is how we create a normal field(formControl) 
      fullname: new FormControl('',[    // single quots are for initial value anything you will pass here will be visible inside field like* placeholder(you can say it will be default value)
        Validators.required,
        Validators.minLength(5) // These two lines are to add validations important we can access these validaters with get methods added below
      ]),
      email: new FormControl('',[
        Validators.required,
        Validators.email
      ]),
      address: new FormControl('',[
        Validators.required,
        Validators.minLength(10)
      ])
    });
  }

  get getFullName(){      // get methods will help to get formControl inside HTML file, so that we can add validation and show errors
    return this.reactiveForm.get('fullname');
  }

  get getEmail(){
    return this.reactiveForm.get('email');
  }

  get getAddress(){
    return this.reactiveForm.get('address');
  }

  onSubmit(){   // This submit is used to print 'reactiveForm' data inside console now we can use it anywhere.
    console.log(this.reactiveForm.value);
    
  }
}
```


## Nested REactive form

TypeScript
```TypeScript
import { Component } from '@angular/core';
import { FormControl, FormGroup, Validators } from '@angular/forms';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']

})

export class AppComponent{

  reactiveForm: any;

  constructor(){
    this.reactiveForm = new FormGroup({
      fullname: new FormControl('Pritam',[
        Validators.required,
        Validators.minLength(5)
      ]),
      email: new FormControl('sainipritam@gmail.com',[
        Validators.required,
        Validators.email
      ]),
      address: new FormControl('my adress is no-where',[
        Validators.required,
        Validators.minLength(10)
      ]),

    shippingDetails: new FormGroup({    // Here we are creating a form(Nested).
      pickupAddress: new FormControl('my adress is no-where',[  // There are form's controls as we have in normal form.
        Validators.required,            // same validation
        Validators.minLength(10)
      ]),
      deleverAddress: new FormControl('my adress is no-where',[
        Validators.required,
        Validators.minLength(10)
      ]),
      pinCode: new FormControl('123123',[
        Validators.required,
        Validators.maxLength(6),
        Validators.minLength(6)
      ])
    }),

    });
  }

  get getFullName(){
    return this.reactiveForm.get('fullname');
  }

  get getEmail(){
    return this.reactiveForm.get('email');
  }

  get getAddress(){
    return this.reactiveForm.get('address');
  }

  // getters for Shopping Details(nested forms)

  get getPickupAddress(){             // Same way creating get method
    return this.reactiveForm.get('shippingDetails.pickupAddress');    // Here we are getting 'pickup-address' from reacrive form with the help of 'shipping-details'. We cannot get 'pickupAddress' directly from 'shippingDetails'.
  }
  
  get getDeleverAddress(){
    return this.reactiveForm.get('shippingDetails.deleverAddress');
  }

  get getPincode(){
    return this.reactiveForm.get('shippingDetails.pinCode');
  }

  onSubmit(){
    console.log(this.reactiveForm.value);
    
  }
}
```

HTML
```HTML
<div class="container mt-5">
    <div class="form-area">

        <form [formGroup]="reactiveForm" (ngSubmit)="onSubmit()">

<!-- FULL NAME -->
    
            <div class="form-group">
                <label> Full Name </label>
                <input type="text"
                placeholder="FullName"
                class="form-control"
                name="fullName" 
                formControlName= "fullname"

                [ngClass]="{'is-invalid':getFullName.invalid && getFullName.touched}"
                
                >

                <div class="alert alert-danger" *ngIf="getFullName.touched && getFullName.invalid">
                    <div *ngIf="getFullName.errors?.['required']">Field cannot be empty</div>
                    <div *ngIf="getFullName.errors?.['minlength']">Full Name should be minimum 5 character long</div>
                </div>

            </div>

<!-- EMAIL -->
    
            <div class="form-group">
                <label> Email </label>
                <input 
                type="text" 
                placeholder="email" 
                class="form-control" 
                name="email" 
                formControlName="email"
                
                [ngClass]="{'is-invalid':getEmail.invalid && getEmail.touched}"
                >

                <div class="alert alert-danger" *ngIf="getEmail.touched && getEmail.invalid">
                    <div *ngIf="getEmail.errors?.['required']">Field cannot be empty</div>
                    <div *ngIf="getEmail.errors?.['email']">Invalid Email</div>
                </div>

            </div>

<!-- ADDRESS -->
    
            <div class="form-group">
                <label> TextArea </label>
                <textarea 

                placeholder="address" 
                cols="30" 
                rows="10" 
                class="form-control" 
                name="address" 
                formControlName= "address"

                [ngClass]="{'is-invalid':getAddress.invalid && getAddress.touched}"
                
                ></textarea>

                <div class="alert alert-danger" *ngIf="getAddress.touched && getAddress.invalid">
                    <div *ngIf="getAddress.errors?.['required']">Field cannot be empty</div>
                    <div *ngIf="getAddress.errors?.['minlength']">Please enter atleast 10 character</div>
                </div>

            </div>
<!-- ////////////////////////////////////////////////////////////   NESTED FORM   ////////////////////////////////////////////////////////////////// -->

        <div formGroupName="shippingDetails">   <!-- Here we are passing 'formGroupName' but not a [fromGroup] cause its not an independent from its nested -->

<!-- PICKUP ADDRESS -->

            <div class="form-group">
                <label>Pickup Address</label>
                <textarea 
                name="pickupAddress"
                cols="30" 
                rows="10" 
                class="form-control"
                formControlName="pickupAddress"

                [ngClass]="{'is-invalid':getPickupAddress.invalid && getPickupAddress.touched}"

                ></textarea>  
                <!-- Rest things are same but we need to pay attention in below lines -->

                <div class="alert alert-danger" *ngIf="getPickupAddress.invalid && getPickupAddress.touched">
                  <!-- In above line inside if condition we are calling get-Method not exact control -->
                    <div *ngIf="getPickupAddress.errors?.['required']" >Please enter somthing</div>
                    <div *ngIf="getPickupAddress.errors?.['minlength']" >Enter 10 charecters atleast</div>
                </div>
            </div>

<!-- DELEVER ADDRESS -->

            <div class="form-group">
                <label>Delever Address</label>

                <textarea 
                name="deleverAddress"
                cols="30" 
                rows="10" 
                class="form-control"
                formControlName="deleverAddress"

                [ngClass]="{'is-invalid':getDeleverAddress.invalid && getDeleverAddress.touched}"

                ></textarea>

                <div class="alert alert-danger" *ngIf="getDeleverAddress.invalid && getDeleverAddress.touched">
                    <div *ngIf="getDeleverAddress.errors?.['required']" >Please enter somthing</div>
                    <div *ngIf="getDeleverAddress.errors?.['minlength']" >Enter 10 charecters atleast</div>
                </div>
            </div>

<!-- PINCODE -->

            <div class="form-group">
                <label>Pincode</label>

                <input 
                type="text"

                class="form-control"
                name="pincode"
                placeholder="Enter Pincode"
                formControlName="pinCode"

                [ngClass]="{'is-invalid':getPincode.invalid && getPincode.touched}"

                >

                <div class="alert alert-danger" *ngIf="getPincode.invalid && getPincode.touched">
                    <div *ngIf="getPincode.errors?.['required']" >Pincode cannot be empty</div>
                    <div *ngIf="getPincode.errors?.['maxlength']" >Pincode digits cannot be more than 6</div>
                    <div *ngIf="getPincode.errors?.['minlength']" >Please enter 6 digit pincode</div>
                </div>
            </div>

        </div>

        <button type="submit" class="btn btn-primary" [disabled]="reactiveForm.invalid">Submit</button>
    
        </form>
    </div>
</div>
```

## Reactive Form Array

> Reactive from array is input filed by which we can multiple information inside an array. Suppose we need to get skills of a user, thn we will giv him an input field so we he can add multipe skills. When we type one skill we need to press enter button to add it in an array and input field will get empty so he can add more skills in it.

HTML
```HTML
<!--  -->
    <div class="form-group"><!-- class for basic css -->
        <label for="">Skills</label>
        <input type="text" class="form-control" #skill (keyup.enter)="addSkill(skill)"><!-- Input field to get skills and 'addSkill()' method which will add skills after pressing enter. -->

        <ul class="list-group"><!-- List to show data -->
            <li class="list-group-item" *ngFor="let skill of getSkills.controls; let i = index">
              <!-- Traversing data one by one class 'list-group-item' will help to show added skill below input field-->
                {{ skill.value }}
                <a class="btn btn-sm btn-danger" (click)="removeSkill(i)"> x </a><!-- This ancor tag will show a cross button will will call 'removeSkill()' method to remove existing skill with help of its index number -->

            </li>

        </ul>
    </div>
```

## Reactive Form Builder

> Reactive form builder is a builder which help us to build formGroup, formControll and formArray. Everything is same but it offers cleaner code only.

HTML code is same.. Problems thats i faced when i was commenting previous TS code and adding new code is that i forgot to initialize reative form to new form that was created using form-Builder. Also we beed to map every instance as it was. 

TypeScript
```TypeScript
  constructor(fb: FormBuilder){   // Create an instance of it

    this.reactiveForm = fb.group({    // This is how we declare a 'formGroup' using 'formBuilder' 
      fullname: ['',[Validators.required,Validators.minLength(5)]], // This is how we declare a 'formControl' using 'formBuilder' 
      email: ['',[Validators.required,Validators.email]],
      address: ['',[Validators.required, Validators.minLength(10)]],

      shippingDetails: fb.group({       // Form inside form
        pickupAddress: ['',[Validators.required,Validators.minLength(10)]],
        deleverAddress: ['', [Validators.required,Validators.minLength(10)]],
        pinCode: ['',[Validators.required,Validators.minLength(10),Validators.maxLength(40)]],
      }),
      skills: fb.array([])    // FormArray
    })
    // We can use constructor in creating getters of controls.. We will it inside custom Validators
  }
  ```

  ## Custom Validators

  > Custom Validators are nothing but a method inside another class which will run some operation on given input to check if the input is correctly typed or not. Here is an example.

  TypeScript -> Component class
  ```TypeScript
  import { Component } from '@angular/core';
import { FormBuilder, Validators } from '@angular/forms';
import { noSpace } from './Validators/noSpace.validators';// keep an on imported class

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']

})

export class AppComponent{
  newForm: any;     // creating a form

  constructor(fb: FormBuilder){   // Injecting formBuilder
    this.newForm = fb.group({

      username: ['',[Validators.required,Validators.minLength(9), noSpace.noSpaceValidation]],  // Added custom validator from class 'noSpace' -> "noSpaceValidation"
      password: ['',[Validators.required]]
    })
  };

  get ctrl(){       // This get method can be used to get all the controls
    return this.newForm.controls; // We can do this using with the help of 'FormBuilder' it reduces lots of lines
  }
}
```

TypeScript -> noSpace Class
```TypeScript
import { AbstractControl, ValidationErrors } from "@angular/forms";   // (.)

export class noSpace{

    static noSpaceValidation(control: AbstractControl): ValidationErrors | null{    // 
        let controlValue = control.value as string; // Converting input in string
        if(controlValue.indexOf(' ') >= 0){   // Checking is contained space is 1 or more than 1 : if yes it will 'noSpaceValidator' as true.
            return { noSpaceValidator: true};
        }else{
            return null;
        }

    }
}
```

HTML
```HTML
<!-- We did everything already excpect few line -->
<div class="container">
    <div>
        <form [formGroup]="newForm">

            <!-- USERNAME -->
            <div class="form-group">
                <label >username</label>
                <input type="text" class="form-control" formControlName="username">
                
                <div class="alert alert-danger" *ngIf="ctrl.username.invalid && ctrl.username.touched">
                    <div *ngIf="ctrl.username.errors?.['required']" >Please enter somthing</div>  <!-- this how we are using getter method from formBuilder -->
                    <div *ngIf="ctrl.username.errors?.['minlength']" >Enter 10 charecters atleast</div>
                    <div *ngIf="ctrl.username.errors?.['noSpaceValidator']" >Username cannot contain spaces</div> <!-- if 'noSpaceValidator' return true thn we'll throw an error -->
                </div>
            </div>

            <!-- PASSWORD -->
            <div class="form-group">
                <label > password</label>
                <input type="text" class="form-control" formControlName="password">
                
                <div class="alert alert-danger" *ngIf="ctrl.password.invalid && ctrl.password.touched">
                    <div *ngIf="ctrl.password.errors?.['required']" >Please enter somthing</div>
                </div>
            </div>

            <button type="submit" class="btn btn-primary">Submit</button>

        </form>
    </div>
</div>
```

## Router and Router-Link

> Route is nothing but a way to redirect user from one page to another page.

TypeScript -> app.module.ts
```TypeScript
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { AppComponent } from './app.component';
import { PostlistComponent } from './post/postlist/postlist.component';
import { RouterModule } from '@angular/router'; // We need to import RouterModule
import { HomeComponent } from './home/home.component';

@NgModule({
  declarations: [
    AppComponent,
    PostlistComponent,
    HomeComponent
  ],
  imports: [
    BrowserModule,
    RouterModule.forRoot([
      {path:'', component: HomeComponent},// This will take us to the home page
      {path:'posts', component: PostlistComponent}// This is the pass where we will be redirected after clicking a button(postList).
    ])

  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

HTML
```HTML
<h1>Angular Blog Spot</h1>

<button routerLink="/">Home</button>
<button routerLink="/posts">Post List</button><br><!-- Router links helps to redirect user by clicking any button -->
<!-- This is used to display components on the screen -->
<router-outlet></router-outlet> 
```

>> Now here is a question why do we use 'routerLink' to redirect on another page. Why dont we use anchor tag or 'href'.
We can use anchor tag but not 'href' cause when went on new page by routerLink it dont reload currunt page but when we use 'href' it reloads whole page. Which will cost lots of time an this is going angular.

**RouterLinkActive Directive**
>> routerLinkActive directive are used to add CSS on clicking on button and move to the given URL. CSS we need to define

HTML
```HTML
<h1>Angular Blog Spot</h1>

<button routerLink="/" routerLinkActive="active">Home </button>
<button routerLink="/posts" routerLinkActive="active">Post List</button><br>    

<router-outlet></router-outlet>
```

CSS
```CSS
.active{
    background-color: green;
    color: white;
}
```

>> Now we are going to create an array 'posts' of objects.

TypeScript
```TypeScript
import { Component } from '@angular/core';

@Component({
  selector: 'app-postlist',
  templateUrl: './postlist.component.html',
  styleUrls: ['./postlist.component.css']
})
export class PostlistComponent {

  posts: Array<any> = [
    {
      id:1,
      title: 'Title 1',
      content: 'this content is going to have a saparate page content number 1'
    },
    {
      id:2,
      title: 'Title 2',
      content: 'this content is going to have a saparate page content number 2'
    },
    {
      id:3,
      title: 'Title 3',
      content: 'this content is going to have a saparate page content number 3'
    },
    {
      id:4,
      title: 'Title 4',
      content: 'this content is going to have a saparate page content number 4'
    }
    
  ];
}
```
>> Frontend part

HTML
```HTML
<h1>Inside PostList</h1>
<!-- We'll taverse this array show its title only on web page, to show complete data we'll add one button with each title to re-direct it on another page  -->
<div *ngFor="let post of posts; let index = index"><br>
    <h1> {{ post.title }} </h1><br>     <!-- This will be a title --> 
    <button [routerLink]="['/post', index]">View more</button>    <!--By clicking on this button we will be redirect to another page on basis of URL. For Now URL looks like "localhost:4200/post/0" here 0 is an index which is index number of 'posts' array -->
    <hr>
</div>
```

TypeScript -> app.module.ts
```TypeScript
// Add path inside 'module.ts' file
  imports: [
    BrowserModule,
    RouterModule.forRoot([
      {path:'', component: HomeComponent},
      {path:'posts', component: PostlistComponent},
      {path: 'post/:id', component: SinglePostComponent}  // Adding URL path and component where this path will re-direct our user.
    ])
  ]
```

**Activated Route**
> Activated route is used to get data from a route. ActivatedRoute is an interface and it contains the information about a route associated with a component loaded into an outlet.

TypeScript
```TypeScript
import { Component, OnInit } from '@angular/core';
import { ActivatedRoute } from '@angular/router'; // Importing Activated route

@Component({
  selector: 'app-single-post',
  templateUrl: './single-post.component.html',
  styleUrls: ['./single-post.component.css']
})
export class SinglePostComponent implements OnInit{
  constructor( private route: ActivatedRoute) {   // Creating an instance of it
  }

  ngOnInit(): void {

    this.route.paramMap.subscribe(value => {    // paraMap contains values of 'route' by adding subscribe we will be able to access value of route
      let id = value.get('id');
      console.log(id);            // Printing id from value we've got
      
    })
  }
  
}
```
**Observable**
> An Observable will continuously observe a set of stream data & automatically update or track that sequence of data whenever there is something changed.

**subscribe**
> A subscriber function receives an Observer object, and can publish values to the observer's next() method.

**Next**
> Next. The observer's next method defines how to process the data sent by the observable. 

**Syncronization**
>  Syncronization is single-thread, so only one operation or program will run at a time.


TypeScript
```TypeScript
import { Component, OnInit } from '@angular/core';
import { Observable } from 'rxjs';    // Importing 'Observable' from 'RxJS' Its not from angular

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']

})

export class AppComponent implements OnInit{

  constructor(){}

  ngOnInit(): void {

    const obsTest$ = new Observable( observer => {    // Creating an object of 'Observable' 

      observer.next('First print statement');     // .next method helps in processing data
      observer.next('second print statement');
      // Code till now were following Syncronization cause it was running(one thereading) line by line. But now setTimeOut method will start running from here and it will take 2 second to print. But with help of 'asyncronization' we can execute rest lines of code. 
      setTimeout(() => {                            
        console.log('print by setTimeOut');
        
      }, 2000);
      observer.next('third print statment');    // This line will print before above print statement in 'setTimeOut' Function.

    }).subscribe( value => {      // subscribe function will be active all the time and keep detecting changes inside Observable(). 
      console.log(value);
      
    });

    obsTest$.unsubscribe();   // subscribe function will keep detecting changes utill we stop it. If we don't stop it will kept running and will consume space and it will make application slow. So to stop it we use 'unsubscribe()' funcion.

    // Observable method have one good feature that it can return multiple many data at a time
    const obsTest = function() {                          // This is a simple function that returns first return statement         
      return 'return from normal function first statement';
      return 'return from function second statement';
    }

    console.log(obsTest());  // This line will be printed before 'setTimeOut' print statement cause that was on hold for 2 second.
    
  }
}
``` 
**Asyncronization**
> Async is multi-thread, which means operations or programs can run in parallel.

**UnScubscribe**
> A Subscription essentially just has an unsubscribe() function to release resources or cancel Observable executions. To prevent these memory leaks we have to unsubscribe from the subscriptions when we are done with them.

**Multiple Parameters**
> We've already seen how we can pass single parameter in routerLink. Now we'll se multiple

TypeScript ->app.module.ts
```TypeScript
  imports: [
    BrowserModule,
    RouterModule.forRoot([
      {path:'', component: HomeComponent},
      {path:'posts', component: PostlistComponent},
      {path: 'post/:id/:title', component: SinglePostComponent}   // Passing one more parameter title
    ])
```

HTML
```HTML

<div *ngFor="let post of posts;let index = index"><br>
    <h1>{{ post.title }}</h1><br>
    <button [routerLink]="['/post', index, post.title]">View more</button><!-- added here too -->
    <hr>
</div>
```

TypeScript
```TypeScript
export class SinglePostComponent implements OnInit{
  constructor( private route: ActivatedRoute) {}

  ngOnInit(): void {
    this.route.paramMap.subscribe(value => {

      let id = value.get('id');
      console.log(id);
      console.log(value.get('title'));  // Printing another parameter
      
    })
  }
}
```

**Query Parameters**
> Query Parameters in Angular allow us to generate URLs with query strings. They allow us to pass optional parameters like page number, sorting & filter criteria to the component.

HTML
```HTML
<button routerLink="/" routerLinkActive="active">Home </button>
<!-- We can pass  query parameters like this -->
<button routerLink="/posts" routerLinkActive="active" [queryParams] = "{page:1, orderBy:'newest'}">Post List</button><br>   
```

TypeScript
```TypeScript
  constructor(private route: ActivatedRoute){}
  ngOnInit(): void {
    this.route.queryParamMap.subscribe( value => {    // We can use it like this
      // console.log(value);
      console.log(value.get('page'));
      console.log(value.get('orderBy'));
      
    })
  }
```

**SEPARATE FILE for Angular Routing**
> As we know we have added routes inside 'app.module.ts'

TypeScript
```TypeScript
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { AppComponent } from './app.component';
import { PostlistComponent } from './post/postlist/postlist.component';
import { RouterModule } from '@angular/router';
import { HomeComponent } from './home/home.component';
import { SinglePostComponent } from './single-post/single-post.component';
import { AppRoutingModule } from './app-routing.module';

@NgModule({
  declarations: [
    AppComponent,
    PostlistComponent,
    HomeComponent,
    SinglePostComponent
  ],
  imports: [
    BrowserModule,
    RouterModule.forRoot([    // Right here we were adding our 'routes'
      {path:'', component: HomeComponent},
      {path:'posts', component: PostlistComponent},
      {path: 'post/:id/:title', component: SinglePostComponent}
    ]),
    AppRoutingModule

  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```
> Now we will create a separate file with to add routes. To create a routing file we need to run a simple command. " ng g module app-routing --module app --flat " this one. It was giving me error after creating file. Than I delete file that was created already with name 'app-routing'. And thn i run this command "  ng g module app-routing --flat --moduleflat=app  "

File was created

> We need to delete all routes from app.module.ts file. Delete complete -> RouterModule.forRoot([]) from 'imports'

TypeScript
```TypeScript
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { HomeComponent } from './home/home.component';
import { PostlistComponent } from './post/postlist/postlist.component';
import { SinglePostComponent } from './single-post/single-post.component';

const routes: Routes = [    // Adding all the 

  {path:'', component: HomeComponent},
  {path:'posts', component: PostlistComponent},
  {path: 'post/:id/:title', component: SinglePostComponent}

];

@NgModule({
  // Delete 'Declaration' if it was generated already
  imports: [
    RouterModule.forRoot(routes)    // pass array here
    // Delete common module too, from imports also
  ],
  exports: [ RouterModule ]
})
export class AppRoutingModule {

 }
 ```

 > When we were creating an angular app in starting angular was asking that 'Would you like to add Angular Routing' If we give it YES at that time. Angular will auto create a SAPARATE FILE for angular routing.

 **NAVIATION PROGRAMITACILY**

 > Till now we were calling any route'path' by clicking one button using 'routerLink'. Now we will call a method 'onSubmit()' and that method will call a desiered link using 'navigate' keyword.

 HTML
 ```HTML
 <button (click)="onSubmit()" >Submit</button>
 ```

 TypeScript
 ```TypeScript
  constructor(private router: Router){} // We need to import and create an instance of 'Router'

   onSubmit(){

    // this.router.navigate(['/posts']);      // This s how we can call a normal route
    // this.router.navigate(['/post', 1, 'postTitle']);   // This is how we can call 'route' with multiple parameter
    this.router.navigate(['/posts'], {queryParams: {page: 1, order: 'newest'}});    // This how we can pass 'queryParameter' 

  }
  ```

  **FourNotFour**

  > What if we called any random URL that does not exist inside our 'Router' list. To handle that situation we will create a component 'fournotfour' which mean 'bad request'. 

TypeScript
```TypeScript
const routes: Routes = [
  {path:'', component: HomeComponent},
  {path:'posts', component: PostlistComponent},
  {path: 'post/:id/:title', component: SinglePostComponent},
  {path: '**', component: FournotfourComponent}   // this is how we can handle any anonymos request. And never put it in top of router list else it will show 'fournotfour' for every request. 

];
```

# Congretulations 







































 


