<template>
    <v-container>
        <div class="theme--dark">
            <div v-for="category1 of itemCategories">
                <v-layout text-xs-center wrap>
                    <div v-for="category2 of itemCategories">
                        <v-card dark :width=cardSize :height=cardSize
                                @mouseenter="mouseEnter(category1,category2)"
                                @mouseleave="mouseLeave(category1,category2)"
                                @mousemove="onmousemove"
                                :img="createUrl(category1,category2)">
                        </v-card>
                    </div>
                </v-layout>
            </div>
        </div>
        <div id="tooltip" v-show="this.isShowToolTip" :style="style">
            <v-card dark width="200px" height="200px" >
                <v-card-title>{{toolTipData.name}}</v-card-title>
                <v-img :src="toolTipData.img" class="fill-height repeating-gradient" ></v-img>
                <v-layout row v-show="toolTipData.isMixed" >
                        <v-img  :src="toolTipData.category1.img"></v-img>
                        <v-img  :src="toolTipData.category2.img"></v-img>
                </v-layout>
            </v-card>
        </div>
    </v-container>
</template>

<script>
    const  itemData = require('../Data/ItemData.js')
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
        },
        methods: {
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
                this.toolTipData = {
                    name: '',
                    img: '',
                    category1: {
                        img: '',
                    },
                    category2: {
                        img: '',
                    },

                }
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
<style>
</style>
