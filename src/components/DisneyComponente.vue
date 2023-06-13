<template>
    <div class="pagina">
        <div class="buscar">            
            <img alt="Disney" src="https://static-mh.content.disney.io/matterhorn/assets/goc/disney_logo_dark@2x-45d70f7dd57b.png" class="img">            
            <input 
                class="caixaTexto"
                type="text"
                placeholder="Procure um personagem"
                v-model="search"
                @input="onSearchChange"
            >
        </div>
        <ul class="container">
            <li v-for="(character, key) in characters" :key="key" class="card">
                <div class="content">
                    <a :href="character.imageUrl" target="_blank">
                        <img :src="character.imageUrl">
                    </a>
                    <h2>{{ character.name }}</h2>
                </div>
            </li>
        </ul>
    </div>
</template>

<script>
    import api from '../services/api.js'
    export default {
        name: "DisneyComponente",
        components: {
            api
        },
        data() {
            return {
                search: "",
                characters: [],
                timeout: null
            }
        },
        mounted(){
            this.buscar()
        },
        methods: {
            buscar() {                
                let url = "&name=" + this.search
                api.get(url)
                    .then(ret => {
                        if (Array.isArray(ret.data.data)) {
                            this.characters = ret.data.data;
                        } else {
                            this.characters = [ret.data.data];
                        }
                        console.log(this.characters);
                    });
            },
            onSearchChange() {
                this.characters = [];
                clearTimeout(this.timeout);                
                this.timeout = setTimeout(() => {
                    this.buscar();
                }, 300);
            }
        }
    }
</script>

<style scoped>
    @import url('https://fonts.googleapis.com/css?family=Poppins:200,300,400,500,600,700,800,900&display=swap');


    .buscar {
        position: relative;
        display: flex;
        flex-direction: column;
        top: 50px;
        color: #fff;
        width: 100%;
        margin-bottom: 50px;
        justify-content: center;
        align-items: center;
    }
    .buscar img {
        filter: invert();
        margin-bottom: 15px;
    }
    .buscar .caixaTexto {
        border: 1px solid #fff;
        border-radius: 5px;
        height: 25px;
        padding-left: 10px;        
        color: gray;
        width: 60%;
    }
    .pagina {
        display: absolute;
        min-height: 100%;
        min-width: 100%;
        background-color: black;
        justify-content: center;
        align-items: center;
        flex-direction: column;
    }
        .container
    {
        position: relative;
        display: flex;
        justify-content: center;
        align-items: center;
        max-width: 1200px;
        flex-wrap: wrap;
    }
        .container .card
    {
        position: relative;
        width: 280px;
        height: 400px;
        margin: 30px;
        border-radius: 15px;
        background: rgba(255, 255, 255, 0.1);
        overflow: hidden;
        display: flex;
        justify-content: center;
        text-align: center;
        border: 1px solid #FFF;
    }
</style>