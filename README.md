In Angular, services are a core feature that allows you to organize and reuse code efficiently. A service in Angular is typically a class that encapsulates business logic, such as fetching data from an API, performing calculations, or managing state across different components. This logic is separated from the view or component to ensure that the application remains modular and easier to maintain.

#### Why Use Angular Services?

1. **Separation of Concerns**: Services allow you to offload logic from components. Components are mostly responsible for handling the view, while services deal with application logic, such as retrieving data or handling complex operations. This helps keep your code clean and organized​(
    
    [Angular](https://angular.io/guide/architecture-services))​([Angular](https://angular.dev/tutorials/first-app/09-services/)).
    
2. **Reusability**: Once you define a service, it can be injected and used across multiple components. This avoids repeating the same code in different parts of your application, making it easier to update and maintain​(
    
    [FreeCodeCamp](https://www.freecodecamp.org/news/angular-services-and-dependency-injection-explained/))​([DEV Community](https://dev.to/this-is-angular/introduction-to-angular-services-j55)).
    
3. **Dependency Injection**: Angular has a built-in Dependency Injection (DI) system, making it easy to use services across different parts of your app. You can inject services directly into your components or other services by declaring them in the constructor and marking them with `@Injectable` decorator​([DEV Community](https://dev.to/this-is-angular/introduction-to-angular-services-j55))​([TekTutorialsHub](https://www.tektutorialshub.com/angular/angular-services/)).
    

#### How to Create a Service

1. **Define the Service**: A service is essentially a class marked with the `@Injectable` decorator. For example, a simple logging service could look like this:
    
    ```bash
    import { Injectable } from '@angular/core';
    
    @Injectable({
      providedIn: 'root',
    })
    export class LoggingService {
      log(message: string): void {
        console.log(message);
      }
    }
    ```
    
    In this example, the `LoggingService` logs messages to the console.
    
2. **Injecting the Service**: Once your service is created, you can inject it into a component by adding it to the constructor:
    
    ```bash
    import { Component } from '@angular/core';
    import { LoggingService } from './logging.service';
    
    @Component({
      selector: 'app-root',
      templateUrl: './app.component.html',
    })
    export class AppComponent {
      constructor(private loggingService: LoggingService) {}
    
      logMessage(): void {
        this.loggingService.log('Hello from AppComponent!');
      }
    }
    ```
    
    This ensures that the `LoggingService` can be accessed from within the `AppComponent`​([Angular](https://angular.dev/tutorials/first-app/09-services/))​([FreeCodeCamp](https://www.freecodecamp.org/news/angular-services-and-dependency-injection-explained/)).
    
3. **Using Services with Dependency Injection**: The `providedIn: 'root'` setting in the `@Injectable` decorator makes the service available globally, meaning Angular creates a single instance (singleton) of the service across the entire application​(
    
    [FreeCodeCamp](https://www.freecodecamp.org/news/angular-services-and-dependency-injection-explained/)).
    

#### Common Use Cases for Angular Services

* **Fetching Data**: Services are often used to make HTTP requests to APIs using Angular’s `HttpClient`. For instance, you can create a service to fetch data from an external API and then share that data across different components.
    
* **State Management**: Services can maintain the state of your application (such as user data or configuration settings), ensuring that the state is consistent across different components.
    
* **Logging and Error Handling**: Services can handle centralized logging or error reporting, which simplifies the monitoring and debugging of your application​([Angular](https://angular.io/guide/architecture-services))​([DEV Community](https://dev.to/this-is-angular/introduction-to-angular-services-j55)).
    

Angular services allow you to build modular, scalable, and maintainable applications by organizing business logic into reusable units. By leveraging dependency injection, services reduce code duplication and help keep components focused on managing the view.

### **EXAMPLE:**

Imagine you have a simple app that displays a list of universities. You want to add new universities, edit existing ones, and delete them. While you can handle all the logic in your component, using a **service** allows you to separate concerns and keep your code organized.

## Creating the Component

Now, let's create our Angular component.

### `app.component.ts`

```java
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css'],
})
export class AppComponent implements OnInit {
  universityList: string[] = [];
  newUniversity = '';

  ngOnInit(): void {
    // Initialization logic will go here
  }
}
```

## Setting Up the HTML

```java
<h2>Universities</h2>

<h3>Add a New University</h3>
<input placeholder="Enter university name" />
<button>Add University</button>
<br /><br />
<table border="1" cellpadding="5" cellspacing="0">
  <thead>
    <tr>
      <th>#</th>
      <th>University Name</th>
      <th>Actions</th>
    </tr>
  </thead>
  <tbody>
    <tr *ngFor="let university of universityList; let i = index">
      <td>{{ i + 1 }}</td>
      <td>{{ university }}</td>
      <td>
        <button>Edit</button>
        <button>Delete</button>
      </td>
    </tr>
  </tbody>
</table>
```

OUTPUT:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1728048486457/2a7e8ded-9fd5-4148-9eef-69d00d97cc9d.png align="center")

At this point, the HTML is set up, but the functionality isn't there yet. The buttons don't do anything, and the list doesn't display any data because `universityList` is not defined.

In this component:

* We declare a `universityList` array to hold our list of universities.
    
* We have a `newUniversity` string to bind to our input field.
    
* We implement the `OnInit` interface to perform initialization logic.
    

1. Generate Service
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1728046674797/9d133b9c-5d97-4b7b-bcf9-1d9ab5fa2d0d.png)

in angular UniversityService

```typescript
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class UniversityService {
  private universityList: string[] = ["University of the Philippines", "UST"];

  // Fetch all universities
  getUniversities(): string[] {
    return this.universityList;
  }

  // Add a new university
  addUniversity(university: string): void {
    this.universityList.push(university);
  }

  // Update a university by index
  updateUniversity(index: number, newUniversity: string): void {
    if (index >= 0 && index < this.universityList.length) {
      this.universityList[index] = newUniversity;
    }
  }

  // Remove a university by index
  deleteUniversity(index: number): void {
    if (index >= 0 && index < this.universityList.length) {
      this.universityList.splice(index, 1);
    }
  }
}
```

In this service:

* We have a private `universityList` array that stores the universities.
    
* Methods to get, add, update, and delete universities.
    
* By injecting this service into our component, we can manage the data efficiently.
    

## Connecting the Service to the Component

Now that we have our service, let's use it in our component.

### Import the Service

```java
import {UniversityService} from './services/university-service.service';
```

### Inject the Service

Modify the constructor to inject the service:

```java
constructor(private universityService: UniversityService) {}
```

### Initialize the Data

Update the `ngOnInit` method:

```java
ngOnInit(): void {
  this.universityList = this.universityService.getUniversities();
}
```

## Implementing the Functions

Let's make our buttons functional by implementing the methods for adding, editing, and deleting universities.

### Add University

Update the HTML input and button to bind to `newUniversity` and call `addUniversity()`:

```java
<input [(ngModel)]="newUniversity" placeholder="Enter university name" />
<button (click)="addUniversity()">Add University</button>
```

In the component:

```java
addUniversity(): void {
  if (this.newUniversity.trim()) {
    this.universityService.addUniversity(this.newUniversity.trim());
    this.newUniversity = '';
  }
}
```

* We check if the input is not empty.
    
* Use the service's `addUniversity` method to add the new university.
    
* Reset the `newUniversity` variable to clear the input field.
    

### Edit University

Update the "Edit" button in the HTML:

```java
<button (click)="editUniversity(i)">Edit</button>
```

In the component:

```java
editUniversity(index: number): void {
  const currentName = this.universityList[index];
  const newName = prompt('Enter new name for the university:', currentName);
  if (newName !== null && newName.trim() !== '') {
    this.universityService.updateUniversity(index, newName.trim());
  }
}
```

* When "Edit" is clicked, we get the current name and prompt the user for a new name.
    
* If the user provides a valid name, we update the university using the service.
    

### Delete University

Update the "Delete" button in the HTML:

```java
<button (click)="deleteUniversity(i)">Delete</button>
```

In the component:

```java
deleteUniversity(index: number): void {
  this.universityService.deleteUniversity(index);
}
```

**Let's put it all together.**

#### `app.component.html`

```java
<h2>Universities</h2>

<h3>Add a New University</h3>
<input [(ngModel)]="newUniversity" placeholder="Enter university name" />
<button (click)="addUniversity()">Add University</button>
<br /><br />
<table border="1" cellpadding="5" cellspacing="0">
  <thead>
    <tr>
      <th>#</th>
      <th>University Name</th>
      <th>Actions</th>
    </tr>
  </thead>
  <tbody>
    <tr *ngFor="let university of universityList; let i = index">
      <td>{{ i + 1 }}</td>
      <td>{{ university }}</td>
      <td>
        <button (click)="editUniversity(i)">Edit</button>
        <button (click)="deleteUniversity(i)">Delete</button>
      </td>
    </tr>
  </tbody>
</table>
```

### `app.component.ts`

```typescript
import { Component, OnInit } from '@angular/core';
import { UniversityService } from './university.service';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css'],
})
export class AppComponent implements OnInit {
  universityList: string[] = [];
  newUniversity = '';

  constructor(private universityService: UniversityService) {}

  ngOnInit(): void {
    this.universityList = this.universityService.getUniversities();
  }

  addUniversity(): void {
    if (this.newUniversity.trim()) {
      this.universityService.addUniversity(this.newUniversity.trim());
      this.newUniversity = '';
    }
  }

  deleteUniversity(index: number): void {
    this.universityService.deleteUniversity(index);
  }

  editUniversity(index: number): void {
    const currentName = this.universityList[index];
    const newName = prompt('Enter new name for the university:', currentName);
    if (newName !== null && newName.trim() !== '') {
      this.universityService.updateUniversity(index, newName.trim());
    }
  }
}
```

### `university.service.ts`

```java
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root',
})
export class UniversityService {
  private universityList: string[] = ['University of the Philippines', 'UST'];

  // Fetch all universities
  getUniversities(): string[] {
    return this.universityList;
  }

  // Add a new university
  addUniversity(university: string): void {
    this.universityList.push(university);
  }

  // Update a university by index
  updateUniversity(index: number, newUniversity: string): void {
    if (index >= 0 && index < this.universityList.length) {
      this.universityList[index] = newUniversity;
    }
  }

  // Remove a university by index
  deleteUniversity(index: number): void {
    if (index >= 0 && index < this.universityList.length) {
      this.universityList.splice(index, 1);
    }
  }
}
```

**OUTPUT**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1728049309465/460d2b7f-94b6-4d63-a2e2-b03d1e09a129.gif)

You've built a simple Angular app that uses a service to manage data. Let's recap what we've learned:

* **Angular Services**: A way to encapsulate and share code across components. They help keep your components lean and focused on the view logic.
    
* **Data Binding**: Using `[(ngModel)]` for two-way data binding between the input field and the component property.
    
* **Event Binding**: Using `(click)` to bind button clicks to component methods.
    
* **Dependency Injection**: Injecting the `UniversityService` into the component's constructor to access its methods.
    

REF:

[Angular.io](https://angular.io/guide/architecture-services) and [FreeCodeCamp](https://www.freecodecamp.org/news/angular-services-and-dependency-injection-explained/).

#### WEBSITE

[AngularServices (thirdy-angular-service.web.app)](https://thirdy-angular-service.web.app/)

**GITHUB REPO:**
