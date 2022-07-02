<template>
    <div @click="actCard = card" :class="{'active': actCard.id == card.id}" v-for="(card, index) of cards" :key="index" class="card">
        <div class="card-title">
            {{ card.title }}
        </div>
        <img class="card-img" :src="card.thumbnailUrl">
        <div class="body">
            <editor @selectionChange="handleChange" :init="{menubar: false, statusbar: false, toolbar: false}" :inline=true tag-name="div" v-model="card.body"/>
        </div>
    </div>
</template>

<script>
    import Editor from '@tinymce/tinymce-vue';

    export default {
        name: 'ItemCards',
        methods: {
            handleChange: function () {
                this.actCard.italicActive = window.tinymce.activeEditor.queryCommandState('Italic')
            }
        },
        model: {
            prop: 'activeCard',
            event: 'active'
        },
        computed: {
            actCard: {
                get: function() {
                    return this.activeCard
                },
                set: function(value) {
                    this.$emit('active', value)
                }
            }
        },
        components: {
            Editor
        },
        props: ['cards', 'activeCard']
    }
</script>

<style>
    .card .mce-content-body.mce-edit-focus {
        height: auto !important;
        outline: none;
    }
    .tox.tox-silver-sink.tox-tinymce-aux {
        display: none;
    }
</style>
