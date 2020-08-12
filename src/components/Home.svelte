<script>
    export let user
    import FirebaseApp from "../firebase/FirebaseApp.svelte"
    import User from "../firebase/User.svelte"
    import Doc from "../firebase/Doc.svelte"
    import Collection from "../firebase/Collection.svelte"
    import "firebase/firestore"
    import "firebase/auth"

    let todo_text = ""
</script>

<h1 class="title is-1">
    <i class="fas fa-check" />
    TODO LIST
</h1>

<Doc path={`todo_list/${user.uid}`} let:ref={postRef} log>

    <span slot="loading">Loading post...</span>

    <span slot="fallback">
        <button class="button" on:click={() => postRef.set({ createdAt: Date.now() })}>Create Document</button>
    </span>

    <Collection path={postRef.collection('todo')} query={(ref) => ref.orderBy('createdAt')} let:data={todos} let:ref={todosRef} log>

        <div class="field has-addons">
            <div class="control">
                <input class="input" type="text" placeholder="New TODO" bind:value={todo_text} />
            </div>
            <div class="control">
                <button
                    class="button is-primary"
                    on:click={() => {
                        if (todo_text) {
                            todosRef.add({ yn: false, text: todo_text, createdAt: Date.now() })
                        }
                        todo_text = ''
                    }}>
                    ADD
                </button>
            </div>
        </div>

        <span slot="loading">Loading todos...</span>

        {#if !todos.length}No todos yet...{/if}

        {#each todos as todo}
            <!-- <p>{todo.ref.id}</p> -->
            <div class="block">
                <span class="tag is-success is-large">
                    <label class="checkbox" on:click={() => (todo.ref.yn = todo.yn)}>
                        <input type="checkbox" bind:checked={todo.yn} />
                        {todo.text}
                    </label>
                    <button class="delete is-small" on:click={() => todo.ref.delete()} />
                </span>
            </div>
        {/each}

    </Collection>
</Doc>

<!-- Styles -->
