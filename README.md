# Zwannah API Structure Documentation (Actors-Supporters Arrangement)

## Introduction
The **Zwannah API Structure (Actors-Supporters Arrangement)** (ZAS or ASA) specifies how files in an API (that's based on scripting language) should be arranged to enhance security, maintenance, and tools usage. Tools usage means using tools such as file includers and others efficiently. With this structure, it is easy to write and create tools that ensure development meets organizational conventions and standards.

## ZAS Naming Conventions
1. **directory names:** All directory names should be lowercase with no dividers such as underscores or dashes.
2. **filenames:** all file names should be lowercase with no dividers such as underscores or dashes.
3. **extensions:** the file extension should reflect its purpose. for example, "user.php" that holds a user class code should be named "user.class.php" or any preceeding extension as chosen by the organization before the language's extension. For example, "user.class.php" could be "user.c.php".

## Terminologies
- **process:** consist of one or many files to perform a common function.

## Overview
![Zwannah API Structure](https://user-images.githubusercontent.com/56189552/154696567-88ccfe0b-8fa4-4058-af94-caece91ebda7.png)
*All rectangles with rounded corners are processes, circles have no meaning, others are directories.*

## Structure
### API (root directory)
The API directory is the root directory from which the structure begins. If the API is deployed, the exposed directory doesn't necessarily have to be the api directory. It could, and should mostly be the api/actors/foreground repository. This ensures that no request can be made to the supporters without further security on "apache web server"; This directory could be called any name.

### setup (process)
The setup is a process that is responsible to set up dependencies used by the lower processes. For example, in PHP, you want to include the vendor autoload in this process. Other includers such as your own autoloading should be done here. This process could be a single file or multiple files. However, the process will be included in every (process) below it (your own initiative).

### authenticator (process)
The authenticator process checks if the request is authenticated and updates a global state variable accordingly. This state variable could be anything. For example, it could be a userId whose default is set to 0. If a person is logged in, then it is set to 1. This global variable, as the name suggests will be available to every process below it. The authenticator here should not be used to block access to the service, it should only be used to update the state of the request. Let the lower processes handle this state accordingly.  
For example, a login process will check if the userId is not 0 and respond with an "already logged in" message. However, for an update process, this the response will be "not logged it".



