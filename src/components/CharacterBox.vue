<template>
    <div class="box">
        <img :src="img">
        <div class="hpWrapper">
            <div class="hpBar" :style="{width: hp+'%'}"></div>
        </div>
    </div>
</template>



<script>
export default {
    props:['hp', 'img']
}
</script>



<style scoped>
.box {
    padding: 10px;
    width: 300px;
    background-color: rgb(207, 197, 197);
    margin-bottom: 10px;
}

.box img {
    height:400px;
    width:100%;
    object-fit: cover;
}

.hpWrapper{
    height: 28px;
    width:100%;
    background-color: rgb(187, 163, 163);
    font-weight: 700;
}


.hpBar{
    height: 100%;
    background-color: rgba(53, 170, 53, 0.657);
}
</style>