// '费用账单' - 房屋租赁费
<template lang="pug">
    #BuildingFee.bill--backgroundColor
        BillHeader( v-bind:contentObj="BillHeaderObj" )
        // v-on:watchDetailInfo="setDetailIndex"
        CostList(
            v-bind:briefListObj="CostListBrief"
        )
        #vueLoading.vuee-loading
            vue-loading(
                type="bars"
                color="#20a0ff"
            )
</template>

<script>
    /* global require: true */
    import { mapGetters }   from 'vuex'
    import vueLoading       from 'vue-loading-template'

    import BillHeader       from '../../components/Bill/BillHeader'
    import ContentNull      from '../../components/common/ContentNull'
    import CostList         from '../../components/Bill/CostList'
    const components = { BillHeader, CostList, vueLoading, ContentNull }

    export default {
        name: 'BuildingFee',
        methods: {
            // 目的: 请求 - 账单详情未交
            requireBuildingFee() {
                this.$store.dispatch({
                    type: 'bill/REQUIRE_RENT_FEE',
//                    clientNum: 'o0CuEuD9L-YlwNO5nKEMUjK2zauY'
                    clientNum: this.$store.state.bill.clientNum
                })
            },
            // 目的: 请求 - 账单详情已交
            requireBuildingFeeH() {
                this.$store.dispatch({
                    type: 'bill/REQUIRE_RENT_FEE_H',
                    clientNum: 'o0CuEuD9L-YlwNO5nKEMUjK2zauY'
                })
            }
        },
        data() {
            return {
                BillHeaderObj: {
                    title: '未交金额合计',
                    money: ''
                },
                // 费用列表 - 简要( 收起样式 展示 )
                CostListBrief: {
                    showNullBill: false,                            // 是否显示 '空账单'
                    listIcon: require( '../../assets/images/iconHousTitle@2x.png' ),
                    listIconColor: 'rgb( 255, 181, 0 )',
                    hasDetailList: false,                           // 是否显示 '详情列表': 否
                    // 列表数组
                    listArr: [

                    ],
                    // 已交列表数组
                    listArrH: [

                    ]
                }

            }
        },
        computed: mapGetters({
            getterRentInfo: 'getterRentInfo',
            getterRentInfoH: 'getterRentInfoH',
            getterSumRent: 'getterSumRent'
        }),
        watch: {
            getterRentInfo: function() {
                this.$data.CostListBrief.listArr = this.getterRentInfo
                this.$data.BillHeaderObj.money = this.getterSumRent
                let loading = document.getElementById( 'vueLoading' )
                loading.style.display = 'none'
                if( this.$data.CostListBrief.listArr.length === 0 ) {
                    let notbill = document.getElementById( 'notbill' )
                    notbill.style.display = 'block'
                }
//                console.log( this.$data.CostListBrief.listArr )
//                console.log( this.$data.BillHeaderObj.money )
            },
            getterRentInfoH: function() {
                this.$data.CostListBrief.listArrH = this.getterRentInfoH
                console.log( this.$data.CostListBrief.listArrH )
                if( this.$data.CostListBrief.listArrH.length === 0 ) {
                    let twobill = document.getElementById( 'twobill' )
                    twobill.style.display = 'block'
                }
            }
        },
        mounted: function() {
            this.requireBuildingFee()
            this.requireBuildingFeeH()
        },
        components: components
    }
</script>
<style lang="sass">
    @import "../../sass/main"
    .vuee-loading
        width: 100%
        height: 100%
        background-color: #ffffff
        line-height: 100%
        z-index: 999
        padding-top: 70%
        position: absolute
        top: 0
        left: 0
</style>
