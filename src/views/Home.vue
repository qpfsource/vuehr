<template>
    <div>
        <el-container>
            <el-header class="homeHeader">
                <div class="title">微人事</div>
                <el-dropdown  @command="commandhandler">
                    <span type="primary" class="addiconpicture">
                        {{user.name}}<i><img :src="user.userface" alt=""></i>
                    </span>
                    <el-dropdown-menu slot="dropdown">
                        <el-dropdown-item command="userinfo">个人中心</el-dropdown-item>
                        <el-dropdown-item command="setting">设置</el-dropdown-item>
                        <el-dropdown-item divided command="logout">注销登录</el-dropdown-item>
                    </el-dropdown-menu>
                </el-dropdown>
            </el-header>
            <el-container>
                <el-aside width="200px">
                    <el-menu router unique-opened>
                        <!-- 此处的报错忽略 -->
                        <el-submenu :index="index+''" v-for="(item,index) in routes"  v-if="!item.hidden"   :key="index">
                            <template slot="title" >
                                <i style="color:#409eff;margin-right:10px" :class="item.iconCls" ></i>
                                <span>{{item.name}}</span>
                            </template>
                                <el-menu-item :index="child.path" v-for="(child,indexj) in item.children" :key="indexj">{{child.name}}</el-menu-item>
                                
                        </el-submenu>
                    </el-menu>
                </el-aside>

                <el-main>
                    <el-breadcrumb separator-class="el-icon-arrow-right" v-if="this.$router.currentRoute.path!='/home'">
                    <el-breadcrumb-item :to="{ path: '/home' }">首页</el-breadcrumb-item>
                    <el-breadcrumb-item>{{this.$router.currentRoute.name}}</el-breadcrumb-item>
                    </el-breadcrumb>
                    <div class="welcomehome" v-if="this.$router.currentRoute.path=='/home'">
                        欢迎来到微人事！
                    </div>
                    <router-view/>
                </el-main>
            </el-container>
        </el-container>
    </div>
</template>

<script>
    //  var tetopp;
    //  tetopp = this.$router.options.routers
    //  console.log(tetopp)
    import {getRequest} from "../utils/api";

    export default {
        name: "Home",
        data(){
            return {
                user:JSON.parse(window.sessionStorage.getItem("user"))
            }
        },
        computed:{
            routes(){
                return this.$store.state.routes;
            }
        },
        methods:{
            commandhandler(cmd){
                if(cmd=='logout'){
                    this.$confirm('此操作将注销登录, 是否继续?', '提示', {
                        confirmButtonText: '确定',
                        cancelButtonText: '取消',
                        type: 'warning'
                    }).then(() => {
                        this.getRequest('/logout');
                        window.sessionStorage.removeItem("user")
                        this.$router.replace("/")
                        this.$store.commit("initRoutes",[])
                    }).catch(() => {
                        this.$message({
                            type: 'info',
                            message: '已取消操作'
                        });
                    });
                }


            }
        }
    }
</script>

<style >
    .welcomehome{
        text-align: center;
        color:#409eff;
        font-size: 30px;
        font-family: 华文行楷;
        padding-top: 50px;

    }
    .homeHeader{
        background-color: #409eff;
        display: flex;
        align-items: center;
        justify-content: space-between;
        padding: 0px 15px;
        box-sizing: border-box;
    }
    .homeHeader .title{
        font-size: 30px;
        font-family: 华文行楷;
        color: #ffffff;
    }

    .addiconpicture img{
        width: 48px;
        height: 48px;
        border-radius: 24px;
        margin-left:15px ;
    }

    .addiconpicture{
        display: flex;
        align-items: center;

    }

</style>