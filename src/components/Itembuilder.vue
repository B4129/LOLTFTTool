<template>
    <v-container>
        <div class="theme--dark">
            <v-layout text-xs-center>
                <div v-for="category1 of itemCategories">
                    <v-flex>
                        <div v-for="category2 of itemCategories" class="item">
                            <v-card dark :width=cardSize :height=cardSize
                                    @mouseenter="mouseEnter(category1,category2)"
                                    @mouseleave="mouseLeave(category1,category2)"
                                    @mousemove="onmousemove"
                                    :img="createUrl(category1,category2)">
                            </v-card>
                        </div>
                    </v-flex>
                </div>
            </v-layout>
        </div>
        <v-layout>
            <div id="tooltip" v-show="this.isShowToolTip" :style="style">
                <v-card dark width="200px">
                    <v-card-title>{{toolTipData.name}}</v-card-title>
                    <v-flex lg12>
                        <v-img :src="toolTipData.img" class="fill-height repeating-gradient"></v-img>
                    </v-flex>
                    <v-layout row lg12 v-show="toolTipData.isMixed">
                        <v-flex lg6>
                            <v-img :src="toolTipData.category1.img"></v-img>
                        </v-flex>
                        <v-flex lg6>
                            <v-img :src="toolTipData.category2.img"></v-img>
                        </v-flex>
                    </v-layout>
                </v-card>
            </div>
        </v-layout>
    </v-container>
</template>

<script>
    const itemData = require('../Data/ItemData.js')
    export default {
        data: () => ({
            cardSize: '70px',
            cardName: '',
            allItem: {},
            toolTipData: {
                name: '',
                img: '',
                isMixed: true,
                category1: {
                    img: '',
                },
                category2: {
                    img: '',
                }

            },
            isShowToolTip: false,
            style: {
                position: 'absolute',
                top: '0',
                left: '0',
            },
        }),
        created() {
            this.allItem = itemData.getAllItem()
            this.itemCategories = this.getItemCategories()
            this.toolTipData = this.initToolTipData()
        },
        methods: {
            initToolTipData() {
                return {
                    name: '',
                    img: '',
                    category1: {
                        img: '',
                    },
                    category2: {
                        img: '',
                    }
                }
            },
            mouseEnter(c1, c2) {
                this.isShowToolTip = true
                this.toolTipData = {
                    name: this.allItem[c1][c2].name,
                    img: this.createUrl(c1, c2),
                    isMixed: this.isWeaponMixed(c1, c2),
                    category1: {
                        img: this.createUrl(c1, 'none')
                    },
                    category2: {
                        img: this.createUrl(c2, 'none')
                    }
                }
            },
            mouseLeave(c1, c2) {
                this.isShowToolTip = false
                this.toolTipData = this.initToolTipData()
            },
            createUrl(category1, category2) {
                const version = '9.13.1'
                const itemUrl = 'http://ddragon.leagueoflegends.com/cdn/' + version + '/img/item/'
                const extension = '.png'

                //ddragonに画像urlが無いものはiconに直接urlを入れる
                if (this.allItem[category1][category2].icon.match('http')) {
                    return this.allItem[category1][category2].icon
                }
                return itemUrl + this.allItem[category1][category2].icon + extension
            },

            onmousemove(e) {
                this.style.left = parseInt(e.pageX) + 10 + 'px'
                this.style.top = parseInt(e.pageY) - 45 + 'px'
            },
            //カテゴリを取得する
            getItemCategories() {
                return ['none', 'ad', 'ap', 'as', 'mana', 'ar', 'mr', 'hp', 'other']
            },
            isWeaponMixed(c1, c2) {
                if (c1 === 'none') return false
                if (c2 === 'none') return false
                return 'ok'
            },
        }
    }

</script>
<style scoped>
    .item {
        display: table;
        padding: 0.5em 1em;
        font-weight: bold;
        border-collapse: collapse;
        border: solid 1px #acacac;


    }
</style>
