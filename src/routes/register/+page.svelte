<script>
	// register/+page.svelte
	import { auth } from '$lib/firebase.client';
	import { createUserWithEmailAndPassword } from 'firebase/auth';
	import { goto } from '$app/navigation';
	import { session } from '$store/session';

	let email = '';
	let password = '';

	async function handleRegister() {
		await createUserWithEmailAndPassword(auth, email, password)
			.then((result) => {
				const { user } = result;
				session.update((cur) => {
					return {
						...cur,
						user,
						loggedIn: true,
						loading: false
					};
				});
				goto('/');
			})
			.catch((error) => {
				throw new Error(error);
			});
	}
</script>

<div class="register-form">
	<form on:submit={handleRegister}>
		<h2>Register</h2>
		<input bind:value={email} type="text" placeholder="Email" />
		<input bind:value={password} type="password" placeholder="Password" />
		<button type="submit">Register</button>
	</form>
</div>
