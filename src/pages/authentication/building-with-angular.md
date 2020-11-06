---
title: Building an app with Angular
description: Learn how to integrate authentication within an Angular application
experience: beginners
duration: 30 minutes
tags:
  - tutorial
# images:
#   large: /images/pages/todo-app.svg
#   sm: /images/pages/todo-app-sm.svg
---

# Building an with Angular

<!-- ![What you'll be creating in this tutorial](/images/todo-list-app.png) -->

## Getting started with Angular

In this tutorial, you'll learn how to work with Stacks Connect when using [Angular](https://angular.io/) as your framework of choice. It builds on what you've learnt in the [Authentication Overview](/authentication/overview).

This article presumes some familiarity with [Angular](https://angular.io/), as well as [Reactive Extensions (RxJS)](https://rxjs.dev/).

### Prerequisites

We'll be using the [Angular CLI](https://cli.angular.io/) to scaffold the project, so make sure you've got the latest version installed. We're using version `10.2.0`.

```sh
npm install --global @angular/cli
```

## 1. Scaffold & Run

Use the `ng new` command to scaffold a new project. We've named ours `ng-stacks-connect`.

```sh
ng new --minimal --inline-style --inline-template
```

Inside the newly created `ng-stacks-connect` directory, we can boot up the development server on [localhost:4200](http://localhost:4200).

```sh
ng serve
```

## 2. Install Stacks Connect
