<template>
    <!-- 首页右下角弹层，可放广告、欢迎语等。使用sessionStorage缓存 -->
    <div class="welcome">
        <h2 class="h2">
            <span class="span"></span>
        </h2>
        <p class="p"><br/></p>
        <div class="div">
            <!-- <button class="button" @click="($event) => { closeToast && closeToast($event) }">免费领取</button> -->
            <a class="button" href="" target="_blank">免费领取</a>
            <a class="a" href="" target="_blank"><img class="image" src="" alt="" /></a>
        </div>
            
    </div>
</template>

<script lang="js" setup>
const props = defineProps({
    closeToast: {
        type: Function,
        default: null
    },
    toastProps: {
        type: Object,
        default: () => ({})
    }
})

</script>

<style scoped>
.welcome {
    position: relative;
    /* width: 416px; */
    height: 194px;
    display: flex;
    flex-direction: column;
    justify-content: space-around;

    .h2 {
        display: flex;
        font-size: .875rem;
        line-height: 1.5rem;
        font-weight: 600;
        
        .span {
            opacity: .6;
        }
    }

    .p {
        margin-top: 0.5rem;
        line-height: 1.5rem;
        opacity: .6;
    }

    .div {
        display: flex;
        margin-top: 1.5rem;
        position: relative;
        .button{
            background-color: #F35543;
            color: #ffffff;
            width:100px;
            height:36px;
            line-height: 36px;
            text-align: center;
            border-radius: 8px;
            display: block;
        }

        .image {
            width:160px;
            position: absolute;
            right:-10px;
            bottom:-70px;
        }
    }
}
</style>
