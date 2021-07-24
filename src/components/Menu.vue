<template>
    <button>
        <span class="material-icons-outlined">
            menu
        </span>
        <span class="about">Sobre</span>
    </button>
</template>

<script>
export default {
    name: 'Menu',    
}
</script>

<style scoped>
    button {
        display: flex;
        align-items: center;
        border: none;
        background-color: transparent;
        width: max-content;
        align-items: center;
        margin-left: auto;
        cursor: not-allowed; /* --- TEMP --- */
        transition: 300ms;
    }
    button:hover { 
        opacity: 0.6;
    }
    button > .about {
        display: none;
    }
    button > .material-icons-outlined {
        display: initial;
    }
    button span {
        color: var(--white);
        font-size: 30px;
    }
    /* ---- MEDIA-QUERIES ---- */
    @media screen and (min-width: 500px) {
        button > .about {
            display: block;
            font-size: 24px;
        }
        button > .material-icons-outlined {
            display: none;
        }
    }
</style>