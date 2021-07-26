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
        <div class="about-content">
            <h3>O que é a Técnica Pomodoro?</h3>
            <p>Criada em meados de 1980 por <span>Francesco Cirillo</span>, essa é uma técnica de gerenciamento de tempo focada em aumentar a produtividade. Curiosamente o nome vem do relógio de cozinha que o próprio <span>Cirillo</span> usava para cronometrar seus períodos de trabalho e descanso, que tinha o formato de um tomate (pomodoro, em italiano). Leia mais <a target="_blank" href="https://pt.wikipedia.org/wiki/T%C3%A9cnica_pomodoro"> <span>aqui</span></a>.</p> 
        </div>
        <div class="about-content">
            <h3>E como ela funciona?</h3>
            <p>Intercalando momentos de foco no trabalho ou estudo (chamados pomodoros) com momentos de descanso:</p>
            <ul>
                <li>&nbsp;<span>Inicie</span> o timer</li>
                <li>Foque na sua tarefa por <span>25 minutos</span></li>
                <li>Descanse por cinco minutos, <span>o alarme vai te avisar quando</span>!</li>
                <li>Repita os passos <span>b</span> e <span>c</span> até <span>completar um ciclo e…</span></li>
                <li>Descanse um pouco mais, <span>você mereceu!</span></li>
                <li>Repita, repita e <span>repita</span></li>
            </ul>
        </div>
        <div class="about-content">
            <h3>O que o PomoJota lhe oferece?</h3>
            <ul>
                <li>Você pode configurar seus próprios intervalos livremente</li>
                <li>Design <span>minimalista</span> que não desvia seu foco</li>
                <li>Abra em uma aba e deixe ele lá, não precisa ficar checando o celular</li>
                <li>É de graça e <span>sempre será!</span></li>
            </ul>
        </div>
        <div class="about-content">
            <h3>Curtindo o site?</h3>
            <p>O PomoJota é de graça e sempre será, mas se você gostou, talvez queira <span><a href="https://www.buymeacoffee.com/jeanrufino">me pagar um café ☕</a></span>.<br>Desejo sinceramente que você <span>#ContinueFocando</span>!</p>
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
        background: var(--white);
        overflow-y: auto;
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
        grid-template-columns: 20% 60% 20%;
        padding: 30px 0 30px 0;
        background: var(--grey);
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
    /* ---- CONTENT STYLING BEGINS HERE ---- */
    .about-content {
        color: var(--text-black);
        width: 80vw;
        margin: 0 auto;
        text-align: start;
    }
    .about-content:last-child {
        margin-bottom: 60px;
    }
    .about-content span {
        font-weight: 700;
    }
    h3 {
        font-size: 20px;
        margin: 30px 0 10px;
    }
    p {
        font-size: 17px;
    }
    p a {
        color: var(--text-black);
        -webkit-transition: 500ms;
        -moz-transition: 500ms;
        -o-transition: 500ms;
        transition: 500ms;
    }
    p a:hover {
        opacity: 0.9;
        letter-spacing: 2px;
    }
    .about-content:nth-child(3) ul {
        list-style: decimal;
    }
    .about-content:nth-child(4) ul {
        list-style: disc;
    }
    li {
        list-style-position: inside;
    }
    li:first-child {
        margin-top: 10px;
    }
    li:not(:last-of-type) {
        margin-bottom: 2px;
    }
    /* ---- CONTENT STYLING ENDS HERE ---- */
    /* ---- MEDIA-QUERIES ---- */
    @media screen and (min-width: 500px) {
        .aboutHeader {
            grid-template-columns: 30% 40% 30%;
        }
        span.menu, span.menuTitle {
            font-size: 24px;
            display: block;
        }
        span.menuM {
            font-size: 24px;
            display: none;
        }
    }
    @media screen and (min-width: 1024px) {
        .about-content {
            width: 50%;
        }
    }
</style>