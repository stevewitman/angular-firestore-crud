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


[3 of 3] https://www.youtube.com/watch?v=onVp-8BYUSM



