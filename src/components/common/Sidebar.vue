<template>
    <div class="sidebar" v-show="collapse">
        <div class="logo"  style="background-color: #232329">
            <span>
                企业员工信息管理系统
            </span>   
        </div>
        <div class="sidebar-menu">
            <el-menu
                class="sidebar-el-menu"
                :default-active="onRoutes"
                background-color="#232329"
                text-color="#bfcbd9"
                active-text-color="#20a0ff"
                unique-opened
                router
            >
                <template v-for="item in items">
                    <template v-if="item.subs">
                        <el-submenu :index="item.index" :key="item.index">
                            <template slot="title">
                                <i :class="item.icon"></i>
                                <span slot="title">{{ item.title }}</span>
                            </template>
                            <template v-for="subItem in item.subs">
                                <el-submenu
                                    v-if="subItem.subs"
                                    :index="subItem.index"
                                    :key="subItem.index"
                                >
                                    <template slot="title">{{ subItem.title }}</template>
                                    <el-menu-item
                                        v-for="(threeItem,i) in subItem.subs"
                                        :key="i"
                                        :index="threeItem.index"
                                    >{{ threeItem.title }}</el-menu-item>
                                </el-submenu>
                                <el-menu-item
                                    v-else
                                    :index="subItem.index"
                                    :key="subItem.index"
                                >{{ subItem.title }}</el-menu-item>
                            </template>
                        </el-submenu>
                    </template>
                    <template v-else>
                        <el-menu-item :index="item.index" :key="item.index">
                            <span slot="title">{{ item.title }}</span>
                        </el-menu-item>
                    </template>
                </template>
            </el-menu>
        </div>
    </div>
</template>

<script>
import bus from "../common/bus";
export default {
    data() {
        return {
            collapse: true,
            items: [
                {
                    icon: "iconfont icon-wen-home",
                    index: "index",
                    title: "系统首页"
                },
                {
                    icon: "iconfont icon-wen-team",
                    index: "user",
                    title: "会员管理",
                    subs: [
                        {
                            index: "userList",
                            title: "会员列表"
                        }
                    ]
                },
                {
                    icon: "iconfont icon-wen-book",
                    index: "book",
                    title: "图书管理",
                    subs: [
                        {
                            index: "bookList",
                            title: "图书列表"
                        },
                        {
                            index: "addBook",
                            title: "图书上架"
                        },
                        {
                            index: "bookDetail",
                            title: "图书详情"
                        },
                        {
                            index: "managerRecommend",
                            title: "店长推荐"
                        }
                    ]
                },
                {
                    icon: "iconfont icon-wen-lihuoshangjia",
                    index: "order",
                    title: "订单管理",
                    subs: [
                        {
                            index: "orderList",
                            title: "订单列表"
                        }
                    ]
                },
                {
                    icon: "iconfont icon-wen-lihuoshangjia",
                    index: "chat",
                    title: "聊天室",
                    subs: [
                        {
                            index: "weChat",
                            title: "在线聊天"
                        }
                    ]
                }
            ]
        };
    },
    computed: {
        onRoutes() {
            return this.$route.path.replace("/", "");
        }
    },
    created() {
        // 通过 Event Bus 进行组件间通信，来折叠侧边栏
        bus.$on("collapse", msg => {
            this.collapse = msg;
        });
    }
};
</script>

<style scoped>
.sidebar{
    float: left;
}
.logo {
    width: 195px;
    height: 50px;
    float: left;
    margin-top: 10px;
    color: rgb(19, 18, 18);
    font-size: 20px;
}
.sidebar-menu {
    /* 声明块级元素 */
    display: block;
    /* 声明定位 绝对定位 */
    position: absolute;
    left: 0;
    top: 50px;
    bottom: 0;
    /* 裁剪内容可提供滚动 */
    overflow-y: scroll;
}
.sidebar-menu::-webkit-scrollbar {
    width: 0;
}
.sidebar-el-menu {
    width: 195px;
}
.sidebar-menu > ul {
    height: 100%;
}
</style>
