<template>
    <view>
        <view
                style="float: left;width: 49%;background-color: #fff;margin-top: 4px;margin-right: 2px"
                @click="open(itemid)">
            <view style="padding: .3em .3em .3em;position: relative">
                <image :src="itempic" style="border-radius: 5px;width: 94%;height: 100px;margin-left: 3%;"></image>
            </view>
            <view>
                <view style="display: flex">
                    <view class="twolist-hidden"
                          style="height: 40px;font-size: 14px;margin-right: 2px;margin-left: 10px;width: 95%;max-lines:2;overflow: hidden">
                        {{ itemtitle }}
                    </view>
                </view>
                <view style="margin-top: 4px">
                    <view style="margin-bottom: 4px;font-size: 12px;color: grey;display: flex">
                        <view style="width: 50%;margin-left: 10px;text-align: left">更新至</view>
                    </view>
                </view>
            </view>
        </view>
    </view>
</template>

<script>
    export default {
        name: "broadcast-card",
        props: [
            "itempic", "itemtitle", "itemprice", "itemsale", "itemendprice", "couponmoney", "itemid", "logo","tkmoney"
        ],
        methods: {
            open(itemid) {
               let relation_id = this.$queue.getData("relation_id");
                if (relation_id) {
                    uni.navigateTo({
                        url: "/pages/detail/goodsinfo?itemid=" + itemid + "&relation_id=" + relation_id,
                    })
                } else {
                    uni.navigateTo({
                        url: "/pages/detail/goodsinfo?itemid=" + itemid,
                    })
                }
            },
        }
    }
</script>

<style>
    .quan {
        z-index: 1;
        zoom: 1;
        top: 0;
        overflow: hidden;
        float: right;
        background: -moz-linear-gradient(left, #FC3F78 0, #FF927C 100%);
        background: -webkit-gradient(linear, left top, left right, color-stop(0, #FC3F78), color-stop(100%, #FF927C));
        background: -webkit-linear-gradient(left, #FC3F78 0, #FF927C 100%);
        background: -o-linear-gradient(left, #FC3F78 0, #FF927C 100%);
        background: -ms-linear-gradient(left, #FC3F78 0, #FF927C 100%);
        background: linear-gradient(to left, #FC3F78 0, #FF927C 100%);
        /*background: #FC3F78;*/
        position: relative;
        font-style: normal;
        display: block;
        border-radius: 3px;
        -moz-border-radius: 3px;
        -webkit-border-radius: 3px;
        -o-border-radius: 3px;
        -ms-border-radius: 3px;
        font-size: 14px;
        margin-left: 10px;
        min-width: 20px;
        text-align: center;
        padding: 1px 10px;
        margin-bottom: 4px;
        color: #fff;
    }

    .quan:before {
        position: absolute;
        z-index: 1;
        zoom: 1;
        top: 50%;
        margin-top: -3px;
        background: #fff;
        display: block;
        width: 5px;
        height: 5px;
        content: "";
        border-radius: 10px;
        border: 1px solid #fff;
        left: auto;
        right: -4px;
    }

    .quan:after {
        position: absolute;
        z-index: 1;
        zoom: 1;
        top: 50%;
        margin-top: -3px;
        background: #fff;
        display: block;
        width: 5px;
        height: 5px;
        content: "";
        border-radius: 10px;
        border: 1px solid #fff;
        left: -4px;
    }

    .twolist-hidden {
        max-height: 40px;
        color: #333;
        font-weight: 400;
        text-overflow: ellipsis;
        display: -webkit-box;
        -webkit-line-clamp: 2;
        -webkit-box-orient: vertical;
        overflow: hidden;
    }
</style>
