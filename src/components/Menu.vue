<template>
    <button>
        <span class="material-icons-outlined">
            menu
        </span>
    </button>
</template>

<script>
export default {
    name: 'Menu',    
}
</script>

<style scoped>
    button {
        z-index: 3;
        border: none;
        background-color: transparent;
        width: max-content;
        align-items: center;
        margin-left: 30px;
        margin-right: auto;
        cursor: not-allowed; /* --- TEMP --- */
    }
    button span {
        color: var(--white);
        font-size: 40px;
    }
</style>