# Zwannah API Structure Specification(Actors-Supporters Arrangement)

## Introduction
The **Zwannah API Structure (Actors-Supporters Arrangement)** (ZAS or ASA) specifies how files in an API (that's based on scripting language) should be arranged to enhance security, maintenance, and tools usage. Tools usage means using tools such as file includers, package managers, and others efficiently. With this structure, it is easy to write and create tools that ensure development meets organizational conventions and standards. For example, a file includer to automatically include classes, interfaces, or traits.   

In the ZAS, an API could be any backend that powers some set of frontend. For example, an app that has a mobile, desktop, and web application frontends running on a single backend. The ZAS can work with APIs that use routing to make public their resources. The routing logic should be placed in the actors directory since that's where HTTP requests/responses are allowed.

## ZAS Naming Conventions
1. **directory names:** All directory names should be lowercase with no dividers such as underscores or dashes.
2. **filenames:** all file names should be lowercase with no dividers such as underscores or dashes.
3. **extensions:** the file extension should reflect its purpose. for example, "user.php" that holds a user class code should be named "user.class.php" or any preceeding extension as chosen by the organization before the language's extension. For example, "user.class.php" could be "user.c.php".

## Terminologies
- **process:** consist of one or many files to perform a common function.

## Overview
![Zwannah API Structure](https://user-images.githubusercontent.com/56189552/154704650-4edfda60-4ada-41a1-889d-24b5cb4db515.png)

*All rectangles with rounded corners are processes, circles have no meaning, others are directories.*

## Structure
### 1.0 API (root directory)
The API directory is the root directory from which the structure begins. If the API is deployed, the exposed directory doesn't necessarily have to be the api directory. It could, and should mostly be the api/actors/foreground directory. This ensures that no request can be made to the supporters without further security on "apache web server"; This directory could be called any name.

#### 1.1 setup (process)
The setup is a process that is responsible to set up dependencies used by the lower processes. For example, in PHP, you want to include the vendor autoload in this process. Other includers such as your own autoloading should be done here. This process could be a single file or multiple files. However, the process will be included in every (process) below it (your own initiative).
#### 2.0 actors (directory)
Actors directory contains all processes (files) that can interact directly with clients. These are the processes that perform the work on the server. All http requests are made to processes in this directory. An HTTP request should not or cannot be made to processes in the supporters directory. You should set up security rules or only expose the actors directory to HTTP requests. 

##### 2.0.1 authenticator (process)
The authenticator process checks if the request is authenticated and updates a global state variable accordingly. This state variable could be anything. For example, it could be a `userId` whose default is 0. If a user is logged in, then it is set to the user's ID. This global variable, as the name suggests will be available to every process below it. The authenticator here should not be used to block access to the service, it should only be used to update the state of the request. Let the lower processes handle this state accordingly.
For example, a `login process` will check if the `userId` is not 0 and respond with an "already logged in" message. However, for an `update process`, this state will lead to the response "not logged in... or blab.. with code 403".  

The authenticator must include the setup process. For example, if you have a database managed session. You want to ensure that when you create a database manager object in the authenticator to query the database, you don't get a "Class not exist" error. This is why the Authenticator needs the setup processes.

##### 2.0.2 foreground (directory)
The foreground directory holds the processes that interact through http request. Their executions are initiated but not necessarily terminated by an HTTP request. This means, every file that does some work for the client on the server and needs http requests to be initiated should be placed in this directory. If a router is needed, it should be placed in this directory. 

###### 2.0.2.1 process groups (directories)
At this level, processes that perform similar functions should be put together in a single directory. This will make development faster as every process directory can have it's own master includer which will include everything from the root setup to the authenticator or the state of the requests. This is open to interpretation.  

In an MVC framework setup, Controllers can be placed in this directory. This will assume that the router is placed in the foregrounds directory.

##### 2.0.3 background (directory)
As the name suggests, this directory holds processes that run in the background. HTTP requests cannot be made to processes(scripts) in the background directory. All the processes in the background didrectory must either be initiated by a script in the foreground directory or the server itself. For example, a background script that verifies payment can be deployed by a foreground script to continuously check if the initiated payment (by the foreground script) completed successfully. Also, scripts that are run by cron jobs are placed in this directory.  
  
The communication between the foreground and background scripts are made through CLI. This means, every script in the background folder are Command-Line Application. With this in mind, you can have a C++ or Java program in the background directory that can be called from a foreground php file with all the required arguments.








