<template>
    <div class="gvb_logo">
        <img src="/image/logo.jpg" alt="">
        <div class="logo_head">
            <div>指尖</div>
            <div>dance at fingers</div>
        </div>
    </div>
</template>

<script setup lang="ts">
</script>

<style lang="scss">
.gvb_logo {
    height: 90px;
    display: flex;
    padding: 20px;
    align-items: center;
    border-bottom: 1px solid var(--bg);

    img {
        width: 60px;
        height: 60px;
    }

    .logo_head {
        margin-left: 20px;

        >div:nth-child(1) {
            font-size: 22px;
            font-weight: 600;
            margin-bottom: 2px;
        }

        >div:nth-child(2) {
            font-size: 13px;
        }
    }
}
</style>