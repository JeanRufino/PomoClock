<template>
    <span class="material-icons-outlined menuM" @click="toggleWindow">
        menu_book
    </span>
    <span class="menu" @click="toggleWindow">Sobre</span>
    <div class="about" :class="{active: aboutActive}">
        <div class="aboutHeader">
            <span class="material-icons-outlined menuM" @click="toggleWindow">
                arrow_back
            </span>
            <span class="menu" @click="toggleWindow">Voltar</span>
            <span class="menuTitle">Sobre</span>
            <img src="../assets/italianTomato.png" alt="Um tomate italiano">
        </div>
    </div>
</template>

<script>
export default {
    name: 'Menu',
    props: {
        aboutActive: { type: Boolean, required: true, }
    },
    methods: {
        toggleWindow() {
            this.aboutActive ? this.$emit('update:aboutActive', false) : this.$emit('update:aboutActive', true);
        }
    }
}
</script>

<style scoped>
    button {
        display: flex;
        align-items: center;
        border: none;
        background-color: transparent;
        align-items: center;
        margin-left: auto;
        cursor: pointer; 
        font-family: 'Poppins', sans-serif;
        color: var(--white);
        -webkit-transition: 300ms ease-out;
        -moz-transition: 300ms ease-out;
        -o-transition: 300ms ease-out;
        transition: 300ms ease-out;
    }
    button:hover { 
        opacity: 0.6;
    }
    .about {
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background: var(--blue);
        box-shadow: 10px 0 var(--dark-blue);
        -webkit-transition: 300ms ease-out;
        -moz-transition: 300ms ease-out;
        -o-transition: 300ms ease-out;
        transition: 300ms ease-out;
        transform: translateX(-110%);
        -o-transform: translateX(-110%);
        -webkit-transform: translateX(-110%);
        z-index: 2;
    }
    .about.active {
        transform: translateX(0);
        -o-transform: translateX(0);
        -webkit-transform: translateX(0);
    }
    .aboutHeader {
        display: grid;
        align-items: center;
        justify-content: center;
        grid-template-columns: 30% 40% 30%;
        padding: 30px 0 30px 0;
        background: var(--dark-blue);
    }
    .aboutHeader img {
        margin-left: 0;
    }
    span.menu, span.menuTitle {
        font-size: 20px;
        font-weight: normal;
    }
    span.menu {
        display: none;
        -webkit-transition: 300ms ease-out;
        -moz-transition: 300ms ease-out;
        -o-transition: 300ms ease-out;
        transition: 300ms ease-out;
    }
    span.menu:hover { 
        letter-spacing: 5px;
    }
    span.menuM {
        color: var(--white);
        font-size: 24px;
        display: block;
    }
    span.menu, span.menuM { 
        margin-left: auto;
        cursor: pointer;
    }
    /* ---- MEDIA-QUERIES ---- */
    @media screen and (min-width: 500px) {
        span.menu, span.menuTitle {
            font-size: 24px;
            display: block;
        }
        span.menuM {
            font-size: 24px;
            display: none;
        }
    }
</style>