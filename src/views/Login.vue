<template>
    <el-form :model="ruleForm2" :rules="rules2" ref="ruleForm2" label-position="left" label-width="0px"
             class="demo-ruleForm login-container">
        <h3 class="title">系统登录</h3>
        <el-form-item prop="account" >
            <el-input type="text" v-model="ruleForm2.username" auto-complete="off" placeholder="账号"></el-input>
        </el-form-item>
        <el-form-item prop="checkPass" >
            <el-input type="password" v-model="ruleForm2.password" auto-complete="off" placeholder="密码"></el-input>
        </el-form-item>
        <el-checkbox v-model="checked" checked class="remember">记住密码</el-checkbox>
        <el-form-item style="width:100%;">
            <el-button type="primary" style="width:45%;" @click.native.prevent="handleSubmit2" :loading="logining">登录
            </el-button>
            <el-button style="width:45%;" @click.native.prevent="handleReset2">注册</el-button>
        </el-form-item>
    </el-form>
</template>

<script>
    import {requestLogin,getBingPic} from '../api/api';
    export default {
        data() {
            return {
                logining: false,
                ruleForm2: {
                    username: 'admin',
                    password: '1'
                },
                rules2: {
                    username: [
                        {required: true, message: '请输入账号', trigger: 'blur'},
                    ],
                    password: [
                        {required: true, message: '请输入密码', trigger: 'blur'},
                    ]
                },
                checked: true
            };
        },
        methods: {
            handleReset2() {
                this.$refs.ruleForm2.resetFields();
            },
            getBackgroupPic(){
                getBingPic(null).then(res => {
                    console.log(res)
                    this.logining = false;
                    if (res.status == 1) {
                        this.$message({
                            message: res.msg,
                            type: 'error'
                        });
                    } else {
                        sessionStorage.setItem('user', JSON.stringify(res.data));
                        this.$router.push({path: '/table'});
                    }
                });

            },
            handleSubmit2(e) {
                this.$refs.ruleForm2.validate((valid) => {
                    if (valid) {
                        this.logining = true;
                        let para = Object.assign({}, this.ruleForm2);
                        requestLogin(para).then(res => {
                            this.logining = false;
                            if (res.status == 1) {
                                this.$message({
                                    message: res.msg,
                                    type: 'error'
                                });
                            } else {
                                sessionStorage.setItem('user', JSON.stringify(res.data));
                                this.$router.push({path: '/table'});
                            }
                        });
                    } else {
                        console.log('check your username or password!!');
                        return false;
                    }
                });
            }
        },
        mounted() {
            // this.getBackgroupPic();
        }
    }

</script>

<style lang="scss" scoped>
    .login-container {
        /*box-shadow: 0 0px 8px 0 rgba(0, 0, 0, 0.06), 0 1px 0px 0 rgba(0, 0, 0, 0.02);*/
        -webkit-border-radius: 5px;
        border-radius: 5px;
        -moz-border-radius: 5px;
        background-clip: padding-box;
        margin: 180px auto;
        width: 350px;
        padding: 35px 35px 15px 35px;
        background: #fff;
        /*background-image: '';*/
        border: 1px solid #eaeaea;
        box-shadow: 0 0 25px #cac6c6;
        .title {
            margin: 0px auto 40px auto;
            text-align: center;
            color: #505458;
        }
        .remember {
            margin: 0px 0px 35px 0px;
        }
    }
</style>