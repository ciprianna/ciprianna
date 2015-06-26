---
layout: post
title: "Definitions"
date: 2015-06-25
categories: technical code ruby omaha-code-school
---

## What is a Class?
A Class is a collection of methods. A Class is used when certain methods will need to be applied continually on the same *kind* of Objects. Classes help keep the Ruby Main clear and act as a way of organizing methods that will be used continually.

## What is a Model?
A Model is also a Class, but it is a Class which is specifically created to deal with a given table in a database. The Model, or Class, is intended for the handling the table alone. The Model's methods deal with CRUD operators for the table, with additional methods for handling logic specific to the needs of the program.

## What is a Method?
A Method is a function that runs commands when called. They can be defined to run a block of code when they're called. Methods can either be Class or Instance. Class methods are functions that affect the entire Class behavior. In contrast, Instance methods are called on a Class Object and only affect the Object itself.

## What is a Variable?
A variable is a String which can store information of any type. A variable is named and defined in a program, and can be overwritten. The variable can be recalled when the information inside of it is needed.

## What is a Request?
A request is a call sent by the user on a webpage to get a specific view back. The request must be defined in the controller in order for it to mean anything. It is unique within the web application.

## What is a Route?
A route defines what happens with a request. For every request, there should be a corresponding route handler that defines what the user receives back.

## What is a Response?
A response is what the user is shown as a result of the definition in the route handler. The route handler takes the request from the user and defines what response is shown to the user on the web.
