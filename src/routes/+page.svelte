<script lang="ts">
    let count = $state(0);
    let counter = $state({ count: 0});
    let doubled = $derived(count * 2);

    let numbers = $state([1, 2, 3]);
    let numbers2 = $state.raw([1, 2, 3]);

    let total = $derived.by(() => {
        let total = 0;
        for (const n of numbers) {
            total += n;
        }
        return total;
    }); 

    let size = $state(50);
    let color = $state('#ff3e00');

    let canvas;

    let images = [
        {
            src: "https://picsum.photos/200/300",
            width: 200,
            height: 300,
            caption: "Random image 1",
            href: "https://example.com/image1"
        },
        {
            src: "https://picsum.photos/300/200",
            width: 300,
            height: 200,
            caption: "Random image 2"
        },
        {
            src: "https://picsum.photos/250/250",
            width: 250,
            height: 250,
            caption: "Random image 3",
            href: "https://example.com/image3"
        }
    ];

    let { message = `It's great to see you!` } = $props();

    class Todo {
        done = $state(false);
        text = $state();

        constructor(text: string) {
            this.text = text;
        }
    }

    let todos = $state([
        new Todo("Buy groceries"),
        new Todo("Take out the trash"),
        new Todo("Mow the lawn"),
    ]);

    function addTodo() {
        todos = [...todos, new Todo("New task")];
    }

    function removeTodo(index: number) {
        todos = todos.filter((_, i) => i !== index);
    }
    
    function onclick() {
        console.log($state.snapshot(counter));
    }

    $effect(() => {
        const context = canvas.getContext('2d');
        context.clearRect(0, 0, canvas.width, canvas.height);
        context.fillStyle = color;
        setTimeout(() => {
            context.fillRect(0, 0, size, size);
        }, 0);
    });
</script>

<div>
    <h1>Svelte 5 Testing</h1>
    <p>
        Visit <a href="https://svelte-5-preview.vercel.app/docs/introduction"
            >Vercel</a
        > to read the documentation
    </p>
</div>

<br />
<hr />
<br />

<div>
    <button onclick={() => count++}>
        clicks: {count}
    </button>

    <button onclick={() => count++}>
        doubled: {doubled}
    </button>
</div>

<br />
<hr />
<br />

<div>
    <div>
        <h2>Todo List</h2>
        <ul>
            {#each todos as todo, index}
                <li>
                    <input type="checkbox" bind:checked={todo.done} />
                    <input type="text" bind:value={todo.text} />
                    <button onclick={() => removeTodo(index)}>Remove</button>
                </li>
            {/each}
        </ul>
    </div>
    <div>
        <button onclick={addTodo}>Add Todo</button>
    </div>
</div>

<br />
<hr />
<br />

<div>
    <p>
        $state: For simple counters or toggles, medium-sized forms, or local
        component state that doesn't need to be shared.
    </p>

    <button onclick={() => numbers.push(numbers.length + 1)}>push</button>

    <button onclick={() => numbers.pop()}>pop</button>

    <p>
        {numbers.join(" + ") || 0} = {numbers.reduce((a, b) => a + b, 0)}
    </p>
</div>

<div>
    <p>
        $state.raw: For large datasets or complex objects, when integrating with
        external libraries that expect immutable data, state that needs to be
        shared or passed down to multiple components, or when you need to track
        previous state for comparison.
    </p>

    <button onclick={() => (numbers2 = [...numbers2, numbers2.length + 1])}>
        push
    </button>

    <button onclick={() => (numbers2 = numbers2.slice(0, -1))}> pop </button>

    <p>
        {numbers2.join(" + ") || 0} = {numbers2.reduce((a, b) => a + b, 0)}
    </p>
</div>

<div>
    <p>
        In general, use $state for simpler, self-contained state that doesn't
        require careful control over updates. Use $state.raw when you need more
        explicit control over state updates, are working with larger or more
        complex data structures, or need to ensure immutability for performance
        or integration reasons. (tl;dr: Shared vs non-shared data.) Use
        $state.snapshot to get a copy of the state.
    </p>
</div>

<div>
    <button onclick={() => numbers.push(numbers.length + 1)}>
        {numbers.join(" + ")} = {total}
    </button>
</div>

<div>
    <canvas bind:this={canvas} width="100" height="100" />
</div>

    {#snippet figure(image)}
        <figure>    
            <img 
                src={image.src} 
                width={image.width}
                height={image.height}  
                alt={image.caption}
            />
            <figcaption>
                {image.caption}
            </figcaption>
        </figure>
    {/snippet}

    {#each images as image}
        {#if image.href}
            <a href={image.href}>
                {@render figure(image)}
            </a>
        {:else}
            {@render figure(image)}
        {/if}
    {/each}

    {#snippet hello(name)}
            <p>Hello {name}! {message}</p>
    {/snippet}

    {@render hello('Bob')}
    {@render hello('Nancy')}
    {@render hello('Jermaine')}

    {#snippet blastoff()}
        <span>Blastoff!</span>
    {/snippet}

    {#snippet countdown(n)}
        {#if n > 0}
            <span>{n}...</span>
            {@render countdown(n - 1)}
        {:else}
            {@render blastoff()}
        {/if}
    {/snippet}

    {@render countdown(10)}