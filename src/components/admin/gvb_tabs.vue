<template>
    <div class="gvb_tabs">
        <swiper :slides-per-view="12" class="mySwiper">
            <swiper-slide v-for="(item, index) in tabList" :key="item.name">
                <span :class="{ gvb_tab: true, active: route.name === item.name }" @click="clickTab(item)"
                    @click.middle="closeTab(item)">
                    {{ item.title }}
                    <IconClose @click.stop="closeTab(item)" v-if="item.name !== 'home'"></IconClose>
                </span>
            </swiper-slide>
        </swiper>
        <span @click="closeAllTab" class="gvb_tab close_all_tab">全部关闭</span>
    </div>
</template>

<script setup lang="ts">
import { IconClose } from "@arco-design/web-vue/es/icon";
import type { Ref } from "vue";
import { ref, watch, onMounted } from "vue";
import { useRoute, useRouter } from "vue-router";
import { Swiper, SwiperSlide } from 'swiper/vue';

const route = useRoute()
const router = useRouter()

interface tabType {
    name: string
    title: string
}

const tabList: Ref<tabType[]> = ref([
    { name: "home", title: "首页" },
])


function clickTab(item: tabType) {
    router.push({ name: item.name })
}

// 持久化存储
function persistence() {
    localStorage.setItem("tabList", JSON.stringify(tabList.value))
}

function loadTabs() {
    let val = localStorage.getItem("tabList")
    if (val === null) {
        return
    }
    let tabs = []
    try {
        tabs = JSON.parse(val)
    } catch (e) {
        return;
    }
    tabList.value = tabs
}

loadTabs()
watch(() => tabList.value.length, () => {
    persistence()
})

function closeTab(item: tabType) {
    // 点的home，不关
    if (item.name === "home") {
        return
    }
    // 找当前tab在列表里面的索引
    let index = tabList.value.findIndex((tab) => item.name === tab.name)
    tabList.value.splice(index, 1)
    // 点的是当前页面的tab
    // 判断删除的这个name是不是和我当前的路由是一样的
    if (route.name === item.name) {
        let beforeIndex = index - 1 // 因为首页一定会有
        let beforeItem = tabList.value[beforeIndex]
        clickTab(beforeItem)
    }

    // 点的是其他页面的tab
}

function closeAllTab() {
    tabList.value = [
        { name: "home", title: "首页" },
    ]

    router.push({ name: "home" })
}

watch(() => route.name, () => {
    if (!inList(route.name as string)) {
        // 当前路由不在列表里面，那就将当前路由加到列表里面
        tabList.value.push({
            name: route.name as string,
            title: route.meta.title as string,
        })
    }
}, { immediate: true })


function inList(name: string): boolean {
    for (const tab of tabList.value) {
        if (tab.name === name) {
            return true
        }
    }
    return false
}

const slidesPerView = ref(12)

onMounted(() => {
  // 总宽度
  let mySwiperWith = document.querySelector(".mySwiper")!.clientWidth
  // 实际宽度
  let actualWidth = document.querySelector(".swiper-wrapper")!.scrollWidth
  if (actualWidth <= mySwiperWith) {
    return
  }
  let swiperSlideList = document.querySelectorAll(".swiper-wrapper .swiper-slide")

  let sum = 0
  let count = 0

  for (const slide of swiperSlideList) {
    sum += slide.clientWidth
    if (sum > mySwiperWith) {
      break
    }
    count++
  }
  slidesPerView.value = count
})

</script>

<style lang="scss">
.gvb_tabs {
    height: 30px;
    width: 100%;
    border-bottom: 1px solid var(--bg);
    padding: 0 20px;
    display: flex;
    align-items: center;

    .mySwiper {
        width: calc(100% - 94px);
        overflow: hidden;
        white-space: nowrap;
        height: 100%;
        display: flex;
        align-items: center;

        .swiper-wrapper {
            display: flex;
            justify-content: start;
            width: 100%;

            .swiper-slide {
                width: auto !important;
            }
        }
    }

    .gvb_tab {
        border-radius: 5px;
        border: 1px solid var(--bg);
        padding: 2px 8px;
        margin-right: 10px;
        cursor: pointer;
        position: relative;

        &.active {
            background-color: var(--active);
            color: white;
            border: none;

            svg:hover {
                background-color: rgb(var(--arcoblue-4));
            }

        }

        svg {
            font-size: 12px;
            border-radius: 50%;

            &:hover {
                background-color: var(--bg);
            }
        }

    }


    .close_all_tab {
        position: absolute;
        right: 20px;
        margin-right: 0;
    }
}
</style>