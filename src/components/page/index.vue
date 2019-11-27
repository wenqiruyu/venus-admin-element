<template>
    <div>
        <div class="crumbs">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item><i class="iconfont icon-wen-home"></i> 系统首页</el-breadcrumb-item>
            </el-breadcrumb>
        </div>
        <div class="container">
            <div class="index-title">
                <span>书斋在线书店后台管理系统</span>
            </div>
            <div class="index-content">
                <el-card class="box-card">
                    <div>
                        <span>系统性质</span>
                    </div>
                    <div>
                        <span>毕业设计：在线书店的设计与实现  后台管理系统</span>
                    </div>
                    <div>
                        <span>采用技术</span>
                    </div>
                    <div>
                        <span>vue.js + element(ui)</span>
                    </div>
                    <div>
                        <span>技术官网</span>
                    </div>
                    <div>
                        <a href=""><i class="iconfont icon-wen-lianjie"></i>vue</a>
                        <a href=""><i class="iconfont icon-wen-lianjie"></i>element</a>
                    </div>
                </el-card>
            </div>
            <div class="index-content">
                <el-card class="box-card">
                    <div>
                        <span>毕设门户</span>
                    </div>
                    <div>
                        <span>书斋在线书店</span>
                    </div>
                    <div>
                        <span>使用技术</span>
                    </div>
                    <div>
                        <span>vue.js + iview(ui)</span>
                    </div>
                    <div>
                        <span>技术官网</span>
                    </div>
                    <div>
                        <a href=""><i class="iconfont icon-wen-lianjie"></i>vue</a>
                        <a href=""><i class="iconfont icon-wen-lianjie"></i>iview</a>
                    </div>
                </el-card>
            </div>
            <div class="index-content">
                <el-card class="box-card">
                    <div>
                        <span>毕设后台</span>
                    </div>
                    <div>
                        <span>使用技术</span>
                    </div>
                    <div>
                        <span>使用java语言、采用springcloud微服务框架搭建、使用MySQL数据库、使用redis缓冲降低数据库压力</span>
                    </div>
                    <div>
                        <span>技术官网</span>
                    </div>
                    <div>
                        vue   element
                    </div>
                </el-card>
            </div>
            <div class="index-content">
                <el-card class="box-card">
                    <div>
                        <span>联系作者</span>
                    </div>
                    <div>
                        <div>
                            <a href=""><i class="iconfont icon-wen-qq"></i></a>
                        </div>
                        <div>
                            <a href=""><i class="iconfont icon-wen-weixin"></i></a>
                        </div>
                        <div>
                            <a href=""><i class="iconfont icon-wen-weibo"></i></a>                            
                        </div>
                        <div>
                            <a href=""><i class="iconfont icon-wen-github"></i></a>
                        </div>
                    </div>
                </el-card>
            </div>
        </div>
        <div class="copyright">
          <p>Copyright © 2019 书斋网 by 刘应兴 让阅读成为习惯</p>
        </div>
    </div>
</template>

<script>
    export default {

    }
</script>

<style>
    .container{
        height: 510px;
        /* background-image: url(../../assets/img/index-bg.jpg); */
    }
    .index-title{
        text-align: center;
        font-size: 18px;
        font-weight: bold;
        color: #ff0000;
    }
    .index-content{
        margin-top: 60px;
        margin-left: 80px;
        width: 40%;
        float: left;
    }
    .copyright {
        width: 100%;
        color: #666;
        line-height: 30px;
        text-align: center;
        height:30px;
        position:absolute;
        top:100%;
        margin-top:-30px;
    }
</style>
