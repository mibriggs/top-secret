<script lang="ts">
    import { fade } from "svelte/transition";

    type MODE = "INPUT" | "FIRST_CONFIRMATION" | "SECOND_CONFIRMATION" | "QUESTION" | "ACCEPT";
    type KeyDownEvent = KeyboardEvent & {
        currentTarget: EventTarget & HTMLInputElement;
    };

    const noTexts = [
        "No",
        "I know you didn't just click no",
        "Surely that was a mistake?",
        "Are you sure?",
        "Wait think this through...",
        "Wait really?",
        "One more chance...",
        "Okay but fr this is your last chance",
        "I won't ask again",
        "Okay thats just mean",
        "Oh nah you don't have a choice :)",
        "I can do this all day 😈",
    ] as const;

    let shakeInput = $state(false);
    let shouldShow = $state(true);
    let input: string = $state("");
    let mode: MODE = $state("INPUT");
    let noButtonPos = $state({ x: 0, y: 0 });
    let noButtonClicked = $state(false);
    let textIndex: number = $state(0);
    let padding: number = $state(8);

    const handleClick = () => {
        if (textIndex < noTexts.length - 1) {
            textIndex++;
        }
        moveNoButton();
    };
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
                <button
                    onclick={() => (mode = "SECOND_CONFIRMATION")}
                    class="yes"
                    style={`padding: ${padding}px`}>Yes</button
                >
                <button class="no" onclick={() => (padding += 8)}>No</button>
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
                <button onclick={() => (mode = "QUESTION")} class="yes" style="padding: 8px"
                    >Yes</button
                >
                {#if shouldShow}
                    <button class="no" onclick={() => (shouldShow = false)}>No</button>
                {/if}
            </div>
        </div>
    {:else if mode === "QUESTION"}
        <div class="flex">
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
        <div in:fade={{ delay: 1700 }}>
            <button onclick={() => (mode = "ACCEPT")} class="yes" style="padding: 8px;">Yes</button>
            <button
                onclick={handleClick}
                class="no"
                class:runaway={noButtonClicked}
                style={noButtonClicked ? `left: ${noButtonPos.x}px; top: ${noButtonPos.y}px;` : ""}
                >{noTexts[textIndex]}</button
            >
        </div>
    {:else if mode === "ACCEPT"}
        <img
            alt="pleaseeeeeee"
            src="https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExd28zcjJtZjVyazlleW9xdmI5a3EzN2M0a29lczNiZ2Y2cWhkdjhlaCZlcD12MV9naWZzX3NlYXJjaCZjdD1n/ukmZRuEqc2Rbi/giphy.gif"
        />
        <img
            alt="pleaseeeeeee"
            src="https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExZTJreWIyeDcza2V1MW5vaHd5cWttOW1uYXRsamR6dTdxcm9jNml6eiZlcD12MV9naWZzX3NlYXJjaCZjdD1n/Zl7u48zLVFgLpRwq6f/giphy.gif"
        />
        <div>YAYYYYY</div>
        <div>
            Hope you have an amazing day today! See you soon and make sure your calendars empty 😗
        </div>
        <img
            alt="pleaseeeeeee"
            src="https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExd28zcjJtZjVyazlleW9xdmI5a3EzN2M0a29lczNiZ2Y2cWhkdjhlaCZlcD12MV9naWZzX3NlYXJjaCZjdD1n/YFIn0ICJFwGNa/giphy.gif"
        />
        <img
            alt="pleaseeeeeee"
            src="https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExd28zcjJtZjVyazlleW9xdmI5a3EzN2M0a29lczNiZ2Y2cWhkdjhlaCZlcD12MV9naWZzX3NlYXJjaCZjdD1n/dvdcBNbAiNVT9Z0iwP/giphy.gif"
        />
    {/if}
</main>

<style>
    @import url("https://fonts.googleapis.com/css2?family=Chewy&display=swap");

    :global(body) {
        background-color: rgb(256, 246, 229);
        font-family: "Chewy", system-ui;
        font-weight: 400;
        font-style: normal;
        font-size: 28px;
    }
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
        max-height: 100vh;
        padding: 1rem;
        box-sizing: border-box;
        text-align: center;
    }

    main > div {
        display: flex;
        flex-direction: column;
        align-items: center;
        gap: 0.5rem;
    }

    div:has(> button) {
        display: flex;
        flex-direction: row;
        gap: 1rem;
    }

    img {
        max-width: 100%;
        height: auto;
    }

    input {
        border-radius: 6px;
        width: 100%;
        max-width: 300px;
        padding: 8px;
        box-sizing: border-box;
    }

    button {
        cursor: pointer;
        font-size: 24px;
        box-shadow: -2px 2px 2px currentColor;
    }

    .yes {
        background-color: #4ade80;
        color: #14532d;
        border: none;
        border-radius: 4px;
    }

    .no {
        background-color: #f87171;
        color: #991b1b;
        border: none;
        border-radius: 4px;
        padding: 8px;
    }

    .runaway {
        position: fixed;
    }

    .error {
        animation: shake 0.82s cubic-bezier(0.36, 0.07, 0.19, 0.97) both;
    }

    .flex {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
    }
</style>
