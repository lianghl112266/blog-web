<template>
    <div class="gvb_admin">
        <aside>
            <Gvb_logo></Gvb_logo>
            <Gvb_menu></Gvb_menu>
        </aside>
        <main>
            <div class="gvb_head">
                <Gvb_bread_crumb></Gvb_bread_crumb>
                <div class="gvb_function_area">
                    <IconMenu class="action_icon"></IconMenu>
                    <div class="gvb_theme">
                        <IconSun class="action_icon"></IconSun>
                    </div>
                    <div class="gvb_user_info_menu">
                        <a-dropdown>
                            <div class="gvb_user_info_menu_dropdown">
                                <img src="/image/profile.jpg" alt="">
                                <span class="gvb_user_info_menu_dropdown_span">lianghl</span>
                                <IconDown></IconDown>
                            </div>
                            <template #content>
                                <a-doption>Option 1</a-doption>
                                <a-doption disabled>Option 2</a-doption>
                                <a-doption>Option 3</a-doption>
                                <a-doption>Option 4</a-doption>
                            </template>
                        </a-dropdown>
                    </div>
                </div>
            </div>

            
            <Gvb_tabs></Gvb_tabs>
            <div class="gvb_container">
                <router-view v-slot="{Component}">
                    <transition name="fade" mode="out-in">
                    <component :is="Component"></component>
                    </transition>
                </router-view>
            </div>
            
        </main>
    </div>
</template>


<script setup lang="ts">
import {
    IconDown,
    IconMenu,
    IconUser,
    IconSun,
    IconApps,
    IconBug,
    IconBulb,
} from '@arco-design/web-vue/es/icon';


import Gvb_menu from "@/components/admin/gvb_menu.vue"
import Gvb_bread_crumb from "@/components/admin/gvb_bread_crumb.vue"
import Gvb_logo from "@/components/admin/gvb_logo.vue"
import Gvb_tabs from "@/components/admin/gvb_tabs.vue"
</script>

<style lang="scss">
.gvb_admin {
    display: flex;
    color: var(--color-text-1);
    height: 100vh;

    aside {
        width: 240px;
        border-right: 1px solid var(--bg);
        height: 100vh;
    }

    main {
        width: calc(100% - 240px);
        overflow-x: hidden;
        overflow-y: auto;

        .gvb_head {
            width: 100%;
            height: 60px;
            border-bottom: 1px solid var(--bg);
            display: flex;
            justify-content: space-between;
            padding: 0 20px;
            align-items: center;

            .gvb_function_area {
                display: flex;
                align-items: center;


                .action_icon {
                    margin-right: 10px;
                    cursor: pointer;
                    font-size: 16px;
                }

                .gvb_user_info_menu {
                    img {
                        width: 30px;
                        height: 30px;
                        border-radius: 50%;
                    }

                    .gvb_user_info_menu_dropdown {
                        display: flex;
                        cursor: pointer;
                        align-items: center;

                        .gvb_user_info_menu_dropdown_span {
                            margin: 0 5px;
                        }
                    }
                }
            }
        }



        .gvb_container {
            background-color: var(--bg);
            min-height: calc(100vh - 90px);
            padding: 20px;
        }



    }
}


.fade-leave-to {
    opacity: 0;
    transform: translateX(30px);
}

.fade-enter-active {
    opacity: 0;
    transform: translateX(-30px);
}

.fade-enter-to {
    opacity: 11;
    transform: translateX(30px);
}

.fade-leave-active, .fade-enter-active  {
    transition: all 0.3s ease-out;
}


</style>