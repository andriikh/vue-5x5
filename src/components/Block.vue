<template>
    <div @click="onclick()" class="block" :style="style">
        <span class="colorText">{{data.status ? '&#10003;' : data.active ? `#${data.color}`  : '?'}}</span>
    </div>
</template>
<script>
export default {
    name: "block",
    props: {
        data: Object,
        onclick: Function,
    },
    created: function(){
        
    },
    computed: {
        style: function(){
            let style;
            if(this.data.active == true){
                style = `background-color: #${this.data.color}`
            }
            return style;
        }
    }
}
</script>
<style lang="scss" scoped>
    .block{
        width: 100px;
        height: 100px;
        border: 2px solid #eee;
        text-align: center;
        font-weight: bold;
        cursor: pointer;
    }
    .colorText{
        position: relative;
        top: calc(50% - 12px);
        text-shadow: 0 0 3px #fff;
    }
</style>



