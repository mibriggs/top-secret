<script lang="ts">
    import { fade } from "svelte/transition";

    type MODE = "INPUT" | "FIRST_CONFIRMATION" | "SECOND_CONFIRMATION" | "QUESTION" | "ACCEPT";
    type KeyDownEvent = KeyboardEvent & {
        currentTarget: EventTarget & HTMLInputElement;
    };

    let shakeInput = $state(false);
    let input: string = $state("");
    let mode: MODE = $state("INPUT");
    let noButtonPos = $state({ x: 0, y: 0 });
    let noButtonClicked = $state(false);

    const moveNoButton = () => {
        noButtonClicked = true;
        noButtonPos = {
            x: Math.random() * (window.innerWidth - 80),
            y: Math.random() * (window.innerHeight - 40),
        };
    };

    const login = (e: KeyDownEvent) => {
        if (e.key === "Enter") {
            const inputIsMichelle = input.toLowerCase() === "michelle";
            mode = inputIsMichelle ? "FIRST_CONFIRMATION" : "INPUT";
            shakeInput = !inputIsMichelle;
        }
    };
</script>

<main>
    {#if mode === "INPUT"}
        <div out:fade>
            <img
                in:fade={{ delay: 415 }}
                alt="spy"
                src="https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExd3JlamR3YXlqZGo5MGY3c3gydnh1dmZ5bjYyamt2bzY1MHM5cjJwbCZlcD12MV9naWZzX3NlYXJjaCZjdD1n/VOIXfpInNbgcU2i3QD/giphy.gif"
            />
            <div>You're not michelle!</div>
            <div>Enter super duper top secret password</div>
            <input
                class:error={shakeInput}
                onanimationend={() => (shakeInput = false)}
                bind:value={input}
                type="text"
                placeholder="Enter password (HINT: Your name)"
                onkeydown={login}
            />
        </div>
    {:else if mode === "FIRST_CONFIRMATION"}
        <div in:fade={{ delay: 415 }} out:fade>
            <img
                alt="val day"
                src="https://media.giphy.com/media/v1.Y2lkPWVjZjA1ZTQ3aG5tcnU1MHd3YzE1ZDdtZWI0dmJxaWl6MWU0eTM3em1tNmZ1YTBrdCZlcD12MV9naWZzX3NlYXJjaCZjdD1n/DDfeeqecSEeLgHahO0/giphy.gif"
            />
            <div>Hi Babe, so I have a really important question. Are you ready?</div>
            <div>
                <button onclick={() => (mode = "SECOND_CONFIRMATION")} class="yes">Yes</button>
                <button class="no">No</button>
            </div>
        </div>
    {:else if mode === "SECOND_CONFIRMATION"}
        <div in:fade={{ delay: 415 }} out:fade>
            <img
                alt="val day"
                src="https://media4.giphy.com/media/v1.Y2lkPTc5MGI3NjExZ2o1OHZjamxsenhkdGNlMHlhZXFkcnhiOWtuZTJmM2pyaHk5ZmhqdiZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/26xBBjZ35Q6CMtuI8/giphy.gif"
            />
            <div>Are you really sure you're ready?</div>
            <div>
                <button onclick={() => (mode = "QUESTION")} class="yes">Yes</button>
                <button class="no">No</button>
            </div>
        </div>
    {:else if mode === "QUESTION"}
        <div>
            <img
                in:fade={{ delay: 415 }}
                alt="pleaseeeeeee"
                src="https://media.giphy.com/media/v1.Y2lkPWVjZjA1ZTQ3cm9sdjcwaHAzYnp0bngyeG13cXF3ajZpaTdueGRqeGd0aWs1N21hYyZlcD12MV9naWZzX3NlYXJjaCZjdD1n/35qdMmtFWqdeErlhDm/giphy.gif"
            />
            <img
                in:fade={{ delay: 415 }}
                alt="pleaseeeeeee"
                src="https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExaTNwcTZoamNndmV6eTZ1enR0eG1uZ3lvcXF3ejZmdndkbHg0cW8zYyZlcD12MV9naWZzX3NlYXJjaCZjdD1n/9Ppbna5Si2jQ7t9gi5/giphy.gif"
            />
        </div>
        <div in:fade={{ delay: 415 }}>Okay, here goes...</div>
        <div in:fade={{ delay: 1200 }}>Will you be my valentine?</div>
        <button in:fade={{ delay: 1700 }} onclick={() => (mode = "ACCEPT")} class="yes">Yes</button>
        <button
            in:fade={{ delay: 1700 }}
            onclick={moveNoButton}
            class="no"
            class:runaway={noButtonClicked}
            style={noButtonClicked ? `left: ${noButtonPos.x}px; top: ${noButtonPos.y}px;` : ""}
            >No</button
        >
    {:else if mode === "ACCEPT"}
        <div>YAYYYYY</div>
    {/if}
</main>

<style>
    /*@keyframes shake {
        0% {
            margin-left: 0rem;
        }
        25% {
            margin-left: 0.5rem;
        }
        75% {
            margin-left: -0.5rem;
        }
        100% {
            margin-left: 0rem;
        }
    }*/

    @keyframes shake {
        10%,
        90% {
            transform: translate3d(-1px, 0, 0);
        }

        20%,
        80% {
            transform: translate3d(2px, 0, 0);
        }

        30%,
        50%,
        70% {
            transform: translate3d(-4px, 0, 0);
        }

        40%,
        60% {
            transform: translate3d(4px, 0, 0);
        }
    }

    main {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        padding: 20%;
    }

    input {
        border-radius: 6px;
        width: 100%;
        padding: 4px;
    }

    button {
        cursor: pointer;
        font-size: 24px;
        box-shadow: 3px 6px 6px inherit;
    }

    .yes {
        background-color: #4ade80;
        color: #14532d;
        border: none;
        border-radius: 4px;
        padding: 8px;
        box-shadow: 2.5px 5px 5px hsl(0deg 0% 0% / 0.42);
    }

    .no {
        background-color: #f87171;
        color: #991b1b;
        border: none;
        border-radius: 4px;
        padding: 8px;
        box-shadow: 2.5px 5px 5px hsl(0deg 0% 0% / 0.42);
    }

    .runaway {
        position: fixed;
    }

    .error {
        /*animation: shake 0.2s ease-in-out 0s 2;*/
        animation: shake 0.82s cubic-bezier(0.36, 0.07, 0.19, 0.97) both;
    }
</style>
