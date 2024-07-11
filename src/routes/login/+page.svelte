<script>
	// login/+page.svelte
	import { session } from '$store/session';
	import { auth } from '$lib/firebase.client';
	import {
		GoogleAuthProvider,
		FacebookAuthProvider,
		signInWithPopup,
		signInWithEmailAndPassword
		// type UserCredential
	} from 'firebase/auth';
	import { goto } from '$app/navigation';

	let email = '';
	let password = '';

	async function loginWithMail() {
		await signInWithEmailAndPassword(auth, email, password)
			.then((result) => {
				const { user } = result;
				session.set({
					loggedIn: true,
					user: {
						displayName: user?.displayName,
						email: user?.email,
						photoURL: user?.photoURL,
						uid: user?.uid
					}
				});
				goto('/');
			})
			.catch((error) => {
				return error;
			});
	}

	async function loginWithGoogle() {
		const provider = new GoogleAuthProvider();
		await signInWithPopup(auth, provider)
			.then((result) => {
				const { displayName, email, photoURL, uid } = result?.user;
				session.set({
					loggedIn: true,
					user: {
						displayName,
						email,
						photoURL,
						uid
					}
				});

				goto('/');
			})
			.catch((error) => {
				return error;
			});
	}

	async function loginWithFacebook() {
		const provider = new FacebookAuthProvider();
		await signInWithPopup(auth, provider)
			.then((result) => {
				const { displayName, email, photoURL, uid } = result?.user;
				session.set({
					loggedIn: true,
					user: {
						displayName,
						email,
						photoURL,
						uid
					}
				});

				goto('/');
			})
			.catch((error) => {
				return error;
			});
	}
</script>

<div class="login-form">
	<h1>Login</h1>
	<form on:submit={loginWithMail}>
		<input bind:value={email} type="text" placeholder="Email" />
		<input bind:value={password} type="password" placeholder="Password" />
		<button type="submit">Login</button>
	</form>

	<div>or</div>

	<button on:click={loginWithGoogle}>Login with Google</button>
	<button on:click={loginWithFacebook}>Login with Facebook</button>
	<div>Don't you have an account? <a href="/register"> Register</a></div>
</div>
