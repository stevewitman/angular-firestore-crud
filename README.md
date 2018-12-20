DEMO: tidy-brothers.surge.sh (permission disabled ... owner reset db rules to run)

Tutorial from ...
[1 of 3] https://www.youtube.com/watch?v=gUmItHaVL2w
did all these notes from comments ...
- install firebase @angular/fire --save
- import { Observable } from 'rxjs/Observable'; -->  import { Observable } from 'rxjs';
- The *ngFor directive should be placed on <li> not <ul>
- Sergiu Șveț Yes I did, don't put the subscribe in the constructor, put it on a get method instead.
- constructor(public afs: AngularFireService) {}
(Working example wired up to read and display firestore fields. Currently we cannot get the id using valueChanges() so next we will need to use SnapshotChanges().)

[2 of 3] https://www.youtube.com/watch?v=cwqeyOFcaoA

notes from comments ...
- I had an error with the 'map' operator. For me it helped to issue the command 'npm install rxjs-compat'. After that I just imported map like this; import 'rxjs/add/operator/map'; And that did it for me.﻿

- update!! With Angular 7, this is what it looks like
import { Observable } from 'rxjs';
import { map } from "rxjs/operators";

- I know its 2 Months ago but you need to use pipe instead of map (in first) 

Something like this: 
.snapshotChanges().pipe(
      map(actions => actions.map(a => {
        const data = a.payload.doc.data() as Item;
        data.id = a.payload.doc.id;
        return data;
      }))
    );

- If I navigate to another screen where the items component is not visible and return to items component the list is empty, until I manually refresh the page. Any clue ?
answer - you need to unsubscribe from the observable that you get from getItems. so you first assign it to a variable this.subscription = this.moviesService.getMovies().subscribe(movies => {
          this.movies = movies;
    }); and then you implement onDestroy  ngOnDestroy(): void {
    this.subscription.unsubscribe();
  }﻿

- question : sir i got an error like ./src/app/app.module.ts
Module not found: Error: Can't resolve 'rxjs/add/operator/map'.
 In  file 'items.service.ts'. it shows  " [ts] Property 'map' does not exist on type 'Observable<DocumentChangeAction<Item>[]>'."
 answer - The angularfire2 updated the way you stream data from "snapshotChanges()" to use the .pipe() method instead of .map() like the following:

  this.items = this.afs.collection('items').snapshotChanges().pipe(
    map(changes => changes.map(a => {
      const data = a.payload.doc.data() as Item;
      const id = a.payload.doc.id;
      return { id, ...data}
    })))

- Hi Traversy, I finished the tutorial, thanks a lot, very helpful! Just to help everyone, I came across an issue, I guess because I´m using Ang6 and the latest AngularFirestore version(?). Around 2 min, when you change to snapshotChanges(), I got an error related to the .map method. It took me a while to find the solution ("pipe" must be used before the "map"), so to fix it: 
1) Import the map operator: 
       import { map } from 'rxjs/operators';  
2) ... then write:   
this.items = this.itemsCollection.snapshotChanges().pipe(map(changes => {
      return changes.map(a => {
        const data = a.payload.doc.data() as Item;
        data.id = a.payload.doc.id;
        return data;
      })
    }))

-, ref => ref.orderBy('title', 'asc')
is not working!﻿
The solution is to write the order to another collection as follows:
this.itemsCollection = this.afs.collection('items');
    this.items = this.afs.collection('items', ref => ref.orderBy('title', 'desc')).snapshotChanges().map(changes => {
      return changes.map(a => {
        const data = a.payload.doc.data() as Item;
        data.id = a.payload.doc.id;
        return data;
      });
    });

- Does anybody else have issues with the double click delete?
'use back-ticks' he is saying that in the video,
see the code from Brad's repository, deleteItem(item: Item){
    this.itemDoc = this.afs.doc(`items/${item.id}`);
    this.itemDoc.delete();
  }  - this should work

  [3 of 3] https://www.youtube.com/watch?v=onVp-8BYUSM

  comments ...
  updateItem(item: Item){
    this.itemDoc = this.afs.doc(`items/${item.id}`);
    delete item.id;
    this.itemDoc.update(item);
 }

 15:52 Don't change the import to evironment.prod. Angular will see which environment you are using and naming that file to environment.ts . That means you can leave the import and it will work when you're using prod and dev while you're using another firebase project﻿




