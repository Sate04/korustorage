<script type="ts">
	import { initializeApp } from 'firebase/app';
	import { collection, getDocs, doc, getFirestore, onSnapshot } from 'firebase/firestore';

	// TODO: Replace the following with your app's Firebase project configuration
	// See: https://firebase.google.com/docs/web/learn-more#config-object
	const firebaseConfig = {
		apiKey: 'AIzaSyB0OVwuis39aMzUxTQ5EBFXvIDs7dhALs0',
		authDomain: 'korustorage-6ae74.firebaseapp.com',
		projectId: 'korustorage-6ae74',
		storageBucket: 'korustorage-6ae74.appspot.com',
		messagingSenderId: '949934571747',
		appId: '1:949934571747:web:54fceeb6474741f5e345d6',
		measurementId: 'G-65JZM9L42Z',
	};

	// Initialize Firebase
	let app = initializeApp(firebaseConfig);

	// Initialize Cloud Firestore and get a reference to the service
	let db = getFirestore(app);

	const buttons = [];

	const q = collection(db, 'buttons');
	const unsubscribe = onSnapshot(q, (querySnapshot) => {
		let buttonNames = [];

		buttons.forEach((button) => {
			buttonNames.push(button[0]);
		});

		querySnapshot.forEach((doc) => {
			if (!buttonNames.includes(doc.id)) {
				buttons.push([doc.id, doc.data()['amount']]);
			} else {
				buttons[buttonNames.indexOf(doc.id)][1] = doc.data()['amount'];
			}
		});
		buttons = buttons;
	});

	function add(name) {
		let buttonDoc = doc(db, 'buttons', name);
		console.log(buttonDoc.id);
	}

	add('teal');
</script>

<div class="w-screen h-screen flex justify-evenly">
	{#each buttons as button}
		<div>
			<h1>
				{button[0]}
				{button[1]}
			</h1>
			<button on:click={add(button[0])}>Add</button>
			<button>Subtract</button>
		</div>
	{/each}
</div>
