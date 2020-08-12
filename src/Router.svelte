<script>
    import Home from "./components/Home.svelte"
    import Login from "./components/Login.svelte"
    import FirebaseApp from "./firebase/FirebaseApp.svelte"
    import User from "./firebase/User.svelte"

    const options = [
        { router: "", component: Home },
        { router: "home", component: Home },
        { router: "login", component: Login },
    ]

    let selected = options[0]

    const changePage = (index) => {
        selected = options[index]
    }

    import firebase from "firebase/app"
    let firebaseConfig = {
        apiKey: "AIzaSyCAqX9fElYHowq4QNXZSScE9BiMpTQ8VOo",
        authDomain: "todo-only.firebaseapp.com",
        databaseURL: "https://todo-only.firebaseio.com",
        projectId: "todo-only",
        storageBucket: "todo-only.appspot.com",
        messagingSenderId: "231703038378",
        appId: "1:231703038378:web:6adf5e7fbf9ebd495f83d3",
        measurementId: "G-WFJ95YKTXK",
    }
    firebase.initializeApp(firebaseConfig)
</script>

<style>
    main {
        font-family: "Noto Sans KR", sans-serif;
    }

    a {
        text-decoration: none;
    }

    .unselectable {
        -moz-user-select: none; /* These user-select properties are inheritable, used to prevent text selection */
        -webkit-user-select: none;
        -ms-user-select: none; /* From IE10 only */
        user-select: none; /* Not valid CSS yet, as of July 2012 */
        -webkit-user-drag: none; /* Prevents dragging of images/divs etc */
    }
</style>

<svelte:head>
    <meta name="viewport" content="width=device-width, initial-scale=1" />

    <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+KR&display=swap" rel="stylesheet" />
    <link type="text/css" rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bulma@0.9.0/css/bulma.min.css" />
    <link type="text/css" rel="stylesheet" href="https://www.gstatic.com/firebasejs/ui/4.3.0/firebase-ui-auth.css" />

    <script defer src="https://use.fontawesome.com/releases/v5.3.1/js/all.js">

    </script>
</svelte:head>

<main>
    <FirebaseApp {firebase}>
        <User let:user let:auth>

            <div slot="signed-out">
                <Login firebaseAppMain={firebase} />
            </div>

            <!-- Navbar -->
            <nav class="navbar is-primary" role="navigation" aria-label="main navigation">
                <div class="container">
                    <div class="navbar-brand">
                        <div class="navbar-item unselectable">
                            <i class="fas fa-check-double" />
                            <h4 class="title is-4 px-2">Todo Only</h4>
                        </div>

                        <div class="navbar-item has-text-black unselectable">{user.email}</div>

                        <a href="#login" class="navbar-item" on:click={() => auth.signOut()}>Logout</a>
                    </div>
                </div>
            </nav>

            <section class="section">
                <div class="container">

                    <!-- Pages -->
                    {#if selected.router === ''}
                        <Home {user} />
                    {:else if selected.router === 'home'}
                        <Home {user} />
                    {/if}
                    <!-- <svelte:component this={selected.component} firebaseAppMain={firebase} /> -->

                </div>
            </section>
        </User>
    </FirebaseApp>
</main>
