<script type="ts">


import { initializeApp } from "firebase/app"
import { collection, getDocs, getDoc, doc, getFirestore, setDoc, onSnapshot } from "firebase/firestore";
import { Button } from 'svelte-materialify'


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


const buttons = [];


const q = collection(db, "buttons");
const unsubscribe = onSnapshot(q, (querySnapshot) => {

    let buttonNames = [];

    buttons.forEach(button => {
        buttonNames.push(button[0])
    })

    querySnapshot.forEach((doc) => {
        if(!buttonNames.includes(doc.id)){
            buttons.push([doc.id, doc.data()['amount']]);
        }

        else {

            buttons[buttonNames.indexOf(doc.id)][1] = doc.data()['amount']

        }
    });
    buttons = buttons
}); 

async function add(name){
    let buttonDoc = await getDoc(doc(db, "buttons", name))


    await setDoc(doc(db, "buttons", name), {
        amount: buttonDoc.data()['amount'] + 1
    })

}   

async function subtract(name){
    let buttonDoc = await getDoc(doc(db, "buttons", name))


    await setDoc(doc(db, "buttons", name), {
        amount: buttonDoc.data()['amount'] - 1 
    })

}   


</script>

<div class="w-screen h-screen flex justify-evenly">
{#each buttons as button}
<div>
    <p class='text-center'>
        {button[0]} {button[1]}
    </p>
    <Button on:click={add(button[0])}>
    	Add
    </Button>
    <Button on:click={subtract(button[0])}>
        Subtract
    </Button>
</div>
{/each}


</div>