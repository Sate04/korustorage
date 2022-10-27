<script type="ts">


import { initializeApp } from "firebase/app"
import { collection, doc, getFirestore } from "firebase/firestore";


// TODO: Replace the following with your app's Firebase project configuration
// See: https://firebase.google.com/docs/web/learn-more#config-object
const firebaseConfig = {
  apiKey: "AIzaSyB0OVwuis39aMzUxTQ5EBFXvIDs7dhALs0",
  authDomain: "korustorage-6ae74.firebaseapp.com",
  projectId: "korustorage-6ae74",
  storageBucket: "korustorage-6ae74.appspot.com",
  messagingSenderId: "949934571747",
  appId: "1:949934571747:web:dea5a16ede3c07bbe345d6",
  measurementId: "G-5HNG6H0FS3"
};

// Initialize Firebase
let app = initializeApp(firebaseConfig);


// Initialize Cloud Firestore and get a reference to the service
let db = getFirestore(app)


db.collection("buttons").get().then((querySnapshot) => {
    querySnapshot.forEach((doc) => {
        // doc.data() is never undefined for query doc snapshots
        console.log(doc.id, " => ", doc.data());
    });
});

</script>

<div class="w-screen h-screen flex justify-evenly">

<button>
	Add
</button>

<button>
	Subtract
</button>

</div>