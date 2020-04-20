---
title: "iOS - persistence - user defaults & core data"
date: 2020-04-10 10:13:33
---

## Persistence

There are many different ways to persist data in iOS. Here are some examples:

* UserDefaults
* Core Data
    * SQLite
    * a binary store
    * in memory store
* Property Lists
* File System
* SQLite
* Keychain
* Firebase
* Realm


## User Defaults

### What

* a dictionary that saves its contents to your device’s permanent storage (SSD)
* caches the information in to avoid having to open the user’s defaults database each time you need a default value. When you set a default value, it’s changed synchronously within your process, and asynchronously to persistent storage and other processes. 
* only the following types are allowed to be persisted:
    * NSData, NSString, NSNumber, NSDate, NSArray, or NSDictionary
* When storing (writing) the file or accessing (reading) the file, UserDefaults does it all at once, possibly creating long/unnecessary I/O time.
* It’s a good idea to keep under 1MB.


### Why

* Great for persisting user preferences
* Great for storing user preferences and other simple things

## Core Data

### What

* a framework for managing an app's data layer.
    * a framework is a library or bundle of code that can be included in an app and used to perform specific tasks
* core Data != persistence
    * persistence is just one aspect of data layer management. But it is an important one!
* abstracts the persistent store's details
    * core data provdies a common interface no matter what the underlying data store is used
* examples of core data underlying storage:
    * SQLite
        * relational database
    * in memory
        * appropriate when you have a small data model that can fit in memory all at once and that doesn’t need to be saved to disk – for example a cache
    * binary
        * appropriate when you always need the database to be read and written in its entirety—for example if you are using a file format such as CSV

### Two Sided Relationships

* instead of using `id`s to manage relationships, core data uses `two-sided relationships`
* it traverses the web of entity class instances (also known as the “object graph”) and make sure all affected references are updated. This is called referential integrity.

#### Deletion Rules

* **Cascade**
    * when an object is deleted with this rule, then its reference nodes are also deleted
        * Notebook has a cascade deletion rule. Since it has a reference to Note, then Notes will be also be deleted
* **Nullify**
    * when an object is deleted with this rule, then its reference nodes are NOT deleted
        * Note has a nullify deletion rule. Notes will be also be deleted but not its reference to Notebook
* **Deny**
    * when an object is deleted with this rule, then it can only be deleted if there are no other references to it
        * Notebook has a deny deleltion rule. Since it has a reference to Note, then Notebook can only be deleted if there are no Note references to it

### Why

* heavier data persistence
* 
