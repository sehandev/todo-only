<script>
    import Home from "./components/Home.svelte"
    import Login from "./components/Login.svelte"

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
        // authDomain: "todo-only.firebaseapp.com",
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

<svelte:head>
    <meta name="viewport" content="width=device-width, initial-scale=1" />

    <link type="text/css" rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bulma@0.9.0/css/bulma.min.css" />
    <link type="text/css" rel="stylesheet" href="https://www.gstatic.com/firebasejs/ui/4.3.0/firebase-ui-auth.css" />

    <script type="text/javascript" defer src="https://use.fontawesome.com/releases/v5.3.1/js/all.js">

    </script>
</svelte:head>

<nav class="navbar" role="navigation" aria-label="main navigation">
    <div class="navbar-brand">
        <a class="navbar-item" href="https://bulma.io">
            <img alt="logo" src="https://bulma.io/images/bulma-logo.png" width="112" height="28" />
        </a>

        <a href="#home" role="button" class="navbar-burger burger" aria-label="menu" aria-expanded="false" data-target="navbarBasicExample">
            <span aria-hidden="true" />
            <span aria-hidden="true" />
            <span aria-hidden="true" />
        </a>
    </div>

    <div id="navbarBasicExample" class="navbar-menu">
        <div class="navbar-start">
            <a href="#home" class="navbar-item">Home</a>

            <a href="#home" class="navbar-item">Documentation</a>

            <div class="navbar-item has-dropdown is-hoverable">
                <a href="#home" class="navbar-link">More</a>

                <div class="navbar-dropdown">
                    <a href="#home" class="navbar-item">About</a>
                    <a href="#home" class="navbar-item">Jobs</a>
                    <a href="#home" class="navbar-item">Contact</a>
                    <hr class="navbar-divider" />
                    <a href="#home" class="navbar-item">Report an issue</a>
                </div>
            </div>
        </div>

        <div class="navbar-end">
            <div class="navbar-item">
                <div class="buttons">
                    <a href="#home" class="button is-primary">
                        <strong>Sign up</strong>
                    </a>
                    <a href="#home" class="button is-light">Log in</a>
                </div>
            </div>
        </div>
    </div>
</nav>

{#if selected.router === ''}
    <Home {changePage} firebaseAppMain={firebase} />
{:else if selected.router === 'home'}
    <Home {changePage} firebaseAppMain={firebase} />
{:else if selected.router === 'login'}
    <Login {changePage} firebaseAppMain={firebase} />
{/if}
<!-- <svelte:component this={selected.component} {changePage} firebaseAppMain={firebase} /> -->
