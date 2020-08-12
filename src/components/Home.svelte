<script>
    export let changePage
    export let firebaseAppMain
    import FirebaseApp from "../firebase/FirebaseApp.svelte"
    import User from "../firebase/User.svelte"
    import Doc from "../firebase/Doc.svelte"
    import Collection from "../firebase/Collection.svelte"
    import Login from "./Login.svelte"
    import "firebase/firestore"
    import "firebase/auth"
</script>

<style>
    main {
        text-align: center;
        padding: 1em;
        max-width: 240px;
        margin: 0 auto;
    }
    h1,
    em {
        color: #ff3e00;
    }
    hr {
        height: 1px;
        border: none;
        background: rgb(195, 195, 195);
    }
    @media (min-width: 640px) {
        main {
            max-width: none;
        }
    }
</style>

<section class="section">
    <div class="container">
        <h1 class="title">Hello World</h1>
        <p class="subtitle">
            My first website with
            <strong>Bulma</strong>
            !
        </p>
    </div>
</section>

<main>

    <!-- 1. 🔥 Firebase App -->
    <FirebaseApp firebase={firebaseAppMain}>

        <h1>💪🔥 Mode Activated</h1>

        <p>
            <strong>Tip:</strong>
            Open the browser console for development logging.
        </p>

        <!-- 2. 😀 Get the current user -->
        <User let:user let:auth>
            Howdy 😀! User
            <em>{user.uid}</em>

            <button on:click={() => auth.signOut()}>Sign Out</button>

            <div slot="signed-out">
                <button class="button is-primary is-outlined" on:click={() => changePage(2)}>Login</button>

                <button on:click={() => auth.signInAnonymously()}>Sign In Anonymously</button>
            </div>

            <hr />

            <!-- 3. 📜 Get a Firestore document owned by a user -->
            <Doc path={`posts/${user.uid}`} let:data={post} let:ref={postRef} log>

                <h2>{post.title}</h2>

                <p>
                    Document created at
                    <em>{new Date(post.createdAt).toLocaleString()}</em>
                </p>

                <span slot="loading">Loading post...</span>

                <span slot="fallback">
                    <button on:click={() => postRef.set({ title: '📜 I like Svelte', createdAt: Date.now() })}>Create Document</button>
                </span>

                <!-- 4. 💬 Get all the comments in its subcollection -->

                <h3>Comments</h3>
                <Collection path={postRef.collection('comments')} query={(ref) => ref.orderBy('createdAt')} let:data={comments} let:ref={commentsRef} log>

                    {#if !comments.length}No comments yet...{/if}

                    {#each comments as comment}
                        <p>
                            ID:
                            <em>{comment.ref.id}</em>
                        </p>
                        <p>
                            {comment.text}
                            <button on:click={() => comment.ref.delete()}>Delete</button>
                        </p>
                    {/each}

                    <button on:click={() => commentsRef.add({ text: '💬 Me too!', createdAt: Date.now() })}>Add Comment</button>

                    <span slot="loading">Loading comments...</span>

                </Collection>
            </Doc>
        </User>
    </FirebaseApp>

</main>

<!-- Styles -->
