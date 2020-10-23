<script lang="ts">
	export let loggedIn;

	const auth = firebase.auth();
	const googleProvider = new firebase.auth.GoogleAuthProvider();

	const Anonymous = () => {
		auth.signInAnonymously();
	};
	const Google = () => {
		auth.signInWithPopup(googleProvider);
	};
</script>

<main>
	<button id="white" on:click={Anonymous}>Sign In Anonymously</button>
	<button id="red" on:click={Google}>Sign In with Google</button>
</main>

<style lang="scss">
	main {
		display: flex;
		flex-direction: column;
		align-items: center;
	}
	button {
		width: 210px;
		padding: 15px 0;
		cursor: pointer;
		border-radius: 10px;
		display: block;

		&:not(:nth-child(1)) {
			margin-top: 20px;
		}
		&#white {
			background-color: #333;
			color: #fff;

			&:hover {
				background-color: #000;
			}
		}
		&#red {
			background-color: #f66;
			color: #fff;

			&:hover {
				background-color: #f33;
			}
		}
		&:hover {
			width: 220px;
			border-radius: 15px;
		}
	}
</style>
