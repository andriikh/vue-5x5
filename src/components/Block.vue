<template>
    <div class="block" :style="'background-color:#' + color">
        <span class="colorText">#{{color}}</span>
    </div>
</template>
<script>
export default {
    name: "block",
    props: {
        color: String,
        status: Boolean,
    },
    created: function(){
        console.log(this.color)
    }
}
</script>
<style scoped>
    .block{
        width: 100px;
        height: 100px;
        border: 2px solid #fff;
        text-align: center;
        font-weight: bold;
    }
    .colorText{
        position: relative;
        top: calc(50% - 12px);
        text-shadow: 0 0 3px #fff;
    }
</style>



