---
title: angular observables
date: 2018-03-25 11:56:43
---

## What is an Observable?
- an **observable** a function that handles a set of values represented over time
- an **observer** is an object that the observable uses to perform actions on the data set based on whether the event is **next**, **complete**, or **error**.

- related: [observables vs promises](promises-observables) 

    ```
    const myObservable: Observable<string> = Observable.create((observer: Observer<string>) => {
        setTimeout(() => {
            observer.next('first package');
        }, 2000);
        setTimeout(() => {
            observer.next('second package');
        }, 4000);
        setTimeout(() => {
            observer.complete();
        }, 6000);
        setTimeout(() => {
            observer.next('third package');
        }, 8000);
        });
        myObservable.subscribe(
        (data: string) => { console.log(data);},
        (error: string) => { console.log(error);},
        () => { console.log('completed');},
        );
    ```

* the above will output:

    ```

    first package
    second package
    completed

    ```
* the `observer.complete()` function will terminate the subscription to the observable, therefore the 'third package' console log will never get executed. 


```
    const myObservable: Observable<string> = Observable.create((observer: Observer<string>) => {
        setTimeout(() => {
            observer.next('first package');
        }, 2000);
        setTimeout(() => {
            observer.next('second package');
        }, 4000);
         setTimeout(() => {
        observer.error('error');
        }, 6000);
        setTimeout(() => {
            observer.next('third package');
        }, 8000);
        });
        myObservable.subscribe(
        (data: string) => { console.log(data);},
        (error: string) => { console.log(error);},
        () => { console.log('completed');},
        );
    ```

* the above will output:

    ```

    first package
    second package
    error

    ```
* the `observer.error()` function will terminate and error out the subscription to the observable, therefore the 'third package' console log will never get executed. 