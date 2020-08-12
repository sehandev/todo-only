<script>
    export let changePage
    export let firebaseAppMain
    import * as firebaseui from "firebaseui"

    // Initialize the FirebaseUI Widget using Firebase.
    let ui = firebaseui.auth.AuthUI.getInstance() || new firebaseui.auth.AuthUI(firebaseAppMain.auth())

    let uiConfig = {
        callbacks: {
            signInSuccessWithAuthResult: function (authResult, _) {
                // User successfully signed in.
                // Return type determines whether we continue the redirect automatically
                // or whether we leave that to developer to handle.
                console.log("SUCCESS signInSuccessWithAuthResult")
                console.log(authResult)
                return true
            },
            uiShown: function () {
                // The widget is rendered.
                // Hide the loader.
                document.getElementById("loader").style.display = "none"
            },
        },
        // Will use popup for IDP Providers sign-in flow instead of the default, redirect.
        signInFlow: "popup",
        signInSuccessUrl: "/",
        signInOptions: [
            // Leave the lines as is for the providers you want to offer your users.
            firebaseAppMain.auth.GoogleAuthProvider.PROVIDER_ID,
            firebaseAppMain.auth.EmailAuthProvider.PROVIDER_ID,
        ],
        // Terms of service url.
        // tosUrl: "<your-tos-url>",
        // Privacy policy url.
        // privacyPolicyUrl: "<your-privacy-policy-url>",
    }

    // The start method will wait until the DOM is loaded.
    // ui.start("#firebaseui-auth-container", uiConfig)
    setTimeout(() => {
        ui.start("#firebaseui-auth-container", uiConfig)
    }, 1)
</script>

<!-- The surrounding HTML is left untouched by FirebaseUI.
    Your app may use that space for branding, controls and other customizations.-->

<main>
    <button on:click={() => changePage(0)}>Home</button>
    <h1>Welcome to My Awesome App</h1>
    <div id="firebaseui-auth-container" />
    <div id="loader">Loading...</div>
</main>
