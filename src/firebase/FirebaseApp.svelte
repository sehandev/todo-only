<script>
    import { onMount, setContext, createEventDispatcher } from "svelte"
    export let firebase

    // Ready required to use the app event,
    // prevents child components from running before init
    let ready = false

    // Emit firebase
    const dispatch = createEventDispatcher()

    // Must be a function to ensure changes after initialization are caught
    setContext("firebase", {
        getFirebase: () => firebase,
    })

    onMount(() => {
        // Set firebase context from window if needed
        firebase = firebase || (window && window.firebase)
        if (!firebase) {
            throw Error("No firebase app was provided. You must provide an initialized Firebase app or make it available globally.")
        } else {
            // Optional event to set additional config
            dispatch("initializeApp", {
                firebase,
            })
            ready = true
        }
    })
</script>

{#if ready}
    <slot />
{/if}
