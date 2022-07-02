<template>
    <section class="main">
        <div class="card-action">
            <button @click="execCommand('Italic')" :class="{'active': activeCard.italicActive}" class="card-btn">
                Курсив
            </button>
            <button @click="addCard" class="card-btn">Добавить еще</button>
        </div>
        <div class="card-group">
            <item-cards :cards="cards" :activeCard="activeCard" />
        </div>
    </section>
</template>
<script>
    import axios     from "axios";
    import ItemCards from './items-card.vue'

    export default {
        name: 'MainPage',
        data() {
            return {
                'cards': [],
                'activeCard': [],
                'lastIndex': 1
            }
        },
        methods: {
            request: function (id) {
                axios.get(`https://jsonplaceholder.typicode.com/photos/${id}`)
                    .then((response) => {
                        this.cards.push(this.getStructure(response.data));
                    })
                    .catch((response) => {
                        alert(`status: ${response.code}, message: ${response.message}`)
                    })
            },
            addCard: function () {
                this.request(this.lastIndex++)
            },
            execCommand: function (commandName) {
                window.tinyMCE.activeEditor.execCommand(commandName);
            },
            getStructure: function (card) {
                return {
                    'id': card.id,
                    'thumbnailUrl': card.thumbnailUrl,
                    'title':  card.title,
                    'active': false,
                    'italicActive': false,
                    'body': `quia et suscipit suscipit recusandae consequuntur expedita et cum reprehenderit
                                    molestiae ut ut quas totam nostrum rerum est autem sunt rem eveniet architecto`
                };
            },
        },
        components: {
            ItemCards
        },
        mounted: function () {
            for (let i = 0; i < 3; i++) {
                this.addCard();
            }
        }
    }
</script>

<style>
    .main {
        display: flex;
        flex-flow: row nowrap;
        justify-content: center;;
        margin: 50px 0;
    }
    .card-group {
        display: flex;
        flex-flow: column wrap;
        width: 500px;
    }
    .card {
        margin-bottom: 50px;
        box-shadow: 0px 17px 42px rgb(0 0 0 / 15%);
        padding: 25px;
    }
    .card.active {
        border: 1px solid;
    }
    .card-title {
        font-size: 24px;
        margin-bottom: 25px;
    }
    .card-img {
        max-width: 100%;
        margin-bottom: 25px;
        max-height: 200px;
        width: 100%;
        object-fit: cover;
    }
    .card-body {
        font-size: 18px;
    }
    .card-action {
        width: 300px;
    }
    .card-btn {
        background: #fff;
        color: #bbb;
        border: 1px solid #bbbbbb;
        border-radius: 4px;
        padding: 5px 10px;
        margin: 0px 5px;
        cursor: pointer;
    }
    .card-btn:hover, .card-btn.active {
        background: #bbb;
        color: #fff;
    }
    .card .tinymce__init-area {
        height: auto !important;
        outline: none;
    }
</style>
