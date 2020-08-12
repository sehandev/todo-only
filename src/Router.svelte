<script>
    import Home from "./components/App.svelte"
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

{#if selected.router === ''}
    <Home {changePage} firebaseAppMain={firebase} />
{:else if selected.router === 'home'}
    <Home {changePage} firebaseAppMain={firebase} />
{:else if selected.router === 'login'}
    <Login {changePage} firebaseAppMain={firebase} />
{/if}
<!-- <svelte:component this={selected.component} {changePage} firebaseAppMain={firebase} /> -->
