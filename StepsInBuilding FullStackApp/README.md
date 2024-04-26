<!-- Building a Full-Stack Application with Angular, Node.js, and MySQL -->

Creating a full-stack application can be a challenging endeavour, especially for newcomers to web development. This guide provides a clear, step-by-step process for setting up a basic full-stack application using Angular for the frontend, Node.js with Express for the backend server, and MySQL for the database. By the end, you’ll have a simple application that retrieves data from the backend and displays it on the frontend.

Prerequisites

Before we dive into the technical steps, ensure you have the following installed on your machine:

    Node.js (Long Term Support - LTS version recommended for better compatibility with Angular)
    npm (Node Package Manager, typically installed with Node.js)
    Angular CLI (Command Line Interface)
    MySQL Server

Note: While Node.js is available in both LTS and Current versions, it is advisable to use the LTS version when working with Angular. Some users have reported compatibility issues when using the Current version of Node.js with Angular. The LTS version tends to have better stability and wider support within the developer community, making it the preferred choice for development, especially for those who are new to these technologies.
Backend Setup with Node.js and MySQL
Step 1: Initialize Your Node.js Project

Open your terminal or command prompt and follow these commands:

mkdir my-fullstack-app        // Create a new directory for your project

cd my-fullstack-app           // Navigate into your project directory

npm init -y                   // Initialize a new Node.js project

The npm init -y command creates a new package.json file in your project directory, which will keep track of your Node.js project's details and dependencies.
Step 2: Install Backend Dependencies

Install the necessary Node.js packages for our backend.

bash npm install express mysql cors –save

Here's what each package is for:

    express: A web framework for Node.js that simplifies routing and middleware integration.
    mysql: A package to enable communication with MySQL databases.
    cors: A package that allows your server to accept requests from different origins, which is essential for a frontend hosted on a different port or domain.

Step 3: Set Up the Express Server

Create a server.js file in the root of your project directory and add the following code:

// server.js

// Require the necessary modules for the server

const express = require('express');

const mysql = require('mysql');

const cors = require('cors');

const app = express();        // Create an instance of the express application

app.use(cors());              // Enable CORS policy for all routes


// Set up the connection to the MySQL database

const connection = mysql.createConnection({

 host: 'localhost',

 user: 'yourUsername',       // Replace with your MySQL username

 password: 'yourPassword',   // Replace with your MySQL password

 database: 'yourDatabase'    // Replace with your database name

});

 

// Establish a connection to the database

connection.connect((err) => {

 if (err) throw err;

 console.log('Connected to the MySQL server.'); // Confirmation message

});

 

// Define a route to retrieve data from the database

app.get('/data', (req, res) => {

 connection.query('SELECT * FROM yourTable', (err, results) => {

 if (err) throw err;

 res.json(results);        // Send query results back to the client

 });

});

 

// Start the server on port 3000

const port = 3000;

app.listen(port, () => {

 console.log(`Server running on port ${port}`);

});

Each line of code is commented to explain its purpose. This server setup includes a route to fetch data from a MySQL database and send it to the frontend.
Frontend Setup with Angular
Step 4: Create an Angular Project

Generate a new Angular application with the Angular CLI.

ng new my-angular-frontend --routing=false --style=css

cd my-angular-frontend       // Move into the Angular project directory

 

The ng new command scaffolds a new Angular project. We've disabled routing for simplicity and chosen CSS for styling.
Step 5: Create a Data Service

Angular services are singleton objects used for sharing data and functionality across components. Generate a service for data retrieval:

ng generate service data

This command creates a new data.service.ts file in the src/app directory.
Step 6: Implement the Data Service

Open src/app/data.service.ts and add the following:

// src/app/data.service.ts

 

import { Injectable } from '@angular/core';

import { HttpClient } from '@angular/common/http';

import { Observable } from 'rxjs';

 

@Injectable({

 providedIn: 'root'  // This service is provided at the root level

})

export class DataService {

 private dataUrl = 'http://localhost:3000/data';  // The URL to the backend endpoint

 

 constructor(private http: HttpClient) {}        // Inject HttpClient to make HTTP requests 

 getData(): Observable<any[]> {

 return this.http.get<any[]>(this.dataUrl);    // Fetch data from the backend

 }

}

The DataService class is marked with the @Injectable decorator, indicating that it can be injected into other classes, such as Angular components. The HttpClient is a service provided by Angular for making HTTP requests and it's injected into the DataService through the constructor.
Step 7: Fetch and Display Data in the AppComponent

Now, let's use the DataService to fetch data from the backend and display it in the AppComponent.
Update app.component.ts

 

// src/app/app.component.ts

 

import { Component, OnInit } from '@angular/core';

import { DataService } from './data.service'; // Import the data service

 

@Component({

 selector: 'app-root',

 templateUrl: './app.component.html',

 styleUrls: ['./app.component.css']

})

export class AppComponent implements OnInit {

 data: any[] = []; // Property to store the data

 

 constructor(private dataService: DataService) {} // Inject the data service

 

 ngOnInit(): void {

 this.dataService.getData().subscribe((data) => {

 this.data = data; // Assign the received data to the property

 }, (error) => {

 console.error('There was an error retrieving data:', error);

 });

 }

}

 

The AppComponent implements the OnInit lifecycle hook to fetch data when the component initializes. The getData method of DataService is called, and the result is subscribed to. The received data is then stored in the data property of the component.
Update app.component.html 

<!-- src/app/app.component.html --> 

<ul>

 <!-- Use *ngFor directive to loop over the data array -->

 <li *ngFor="let item of data">

 <h3>{{ item.name }}</h3> <!-- Display the name of the item -->

 <p>{{ item.description }}</p> <!-- Display the item's description -->

 <!-- Use Angular date pipe to format the created_at value -->

 <small>Created at: {{ item.created_at | date: 'medium' }}</small>

 </li>

</ul>

 

The app.component.html template uses the Angular *ngFor directive to iterate over the data array and display each item's name, description, and formatted created_at value.
Step 8: Run Your Full-Stack Application

With both the backend and frontend set up, it's time to run your full-stack application.
Start the Node.js Backend

 

node server.js

This command starts your Node.js/Express server, which listens for requests on port 3000.
Serve the Angular Frontend

Open a new terminal window and navigate to the my-angular-frontend directory:

 

ng serve

The ng serve command compiles the Angular application and starts the development server, typically on port 4200.
View the Application in a Browser

Open your browser and navigate to http://localhost:4200. You should see your Angular application displaying the data fetched from your Node.js backend server.
Conclusion





Congratulations! You've set up a full-stack application with Angular, Node.js, and MySQL. This setup is a foundation that you can build upon with more features and complexity as you grow more comfortable with these technologies. Continue exploring, adding new functionalities, and refining your application. Happy coding!
