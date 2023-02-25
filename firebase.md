Firebase Cheatsheet:

- Initialize Firebase SDK: 
    `firebase.initializeApp(config)`

- Authenticate users:
    ```
    firebase.auth().signInWithEmailAndPassword(email, password)
    firebase.auth().createUserWithEmailAndPassword(email, password)
    firebase.auth().signOut()
    ```

- Access Firestore database:
    ```
    const db = firebase.firestore()
    db.collection('collectionName').doc('docId').get()
    db.collection('collectionName').add(data)
    db.collection('collectionName').doc('docId').set(data)
    db.collection('collectionName').doc('docId').update(data)
    db.collection('collectionName').doc('docId').delete()
    ```

- Use Firebase Storage:
    ```
    firebase.storage().ref('path/to/file').put(file)
    firebase.storage().ref('path/to/file').getDownloadURL()
    ```

- Use Firebase Cloud Functions:
    ```
    firebase.functions().httpsCallable('functionName')(data)
    exports.functionName = functions.https.onCall((data, context) => {...})
    ```

- Use Firebase Hosting:
    ```
    firebase deploy --only hosting
    ```

- Use Firebase Emulator Suite:
    ```
    firebase emulators:start
    firebase emulators:export ./path/to/export/dir
    firebase emulators:import ./path/to/import/dir
    firebase emulators:exec [emulator name] [command]
    ```

- Use Firebase CLI to manage your Firebase project:
    ```
    firebase init
    firebase login
    firebase deploy
    ```
