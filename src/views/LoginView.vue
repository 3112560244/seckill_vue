<template>
    <div class="login-wrap">
        <div class="ms-login">
            <div class="ms-title">后台管理系统</div>
            <el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="0px" class="ms-content">
                <el-form-item prop="username">
                    <el-input v-model="ruleForm.username" placeholder="username">
                        <el-button slot="prepend" icon="el-icon-lx-people"></el-button>
                    </el-input>
                </el-form-item>
                <el-form-item prop="password" >
                    <el-input type="password" placeholder="password" v-model="ruleForm.password"
                              @keyup.enter.native="submitForm('ruleForm')">
                        <el-button slot="prepend" icon="el-icon-lx-lock"></el-button>
                    </el-input>
                </el-form-item>
                <div class="login-btn">
                    <el-button type="primary" @click="submitForm('ruleForm')">登录</el-button>
                </div>
                <p class="login-tips">Tips : 用户名和密码随便填。</p>
            </el-form>
        </div>
<!--        <el-button @click="test">test</el-button>-->
    </div>
</template>

<script>
    import httpServer from "@/util/Axios";
    import axios from "axios";


    export default {
        data: function () {
            return {
                ruleForm: {
                    username: '',
                    password: ''
                },

                rules: {
                    username: [
                        {required: true, message: '请输入用户名', trigger: 'blur'}
                    ],
                    password: [
                        {required: true, message: '请输入密码', trigger: 'blur'}
                    ]
                }
            }
        },




      methods: {
            submitForm(formName) {
                this.$refs[formName].validate((valid) => {
                    if (valid) {


                        httpServer({
                            url: '/v1/user/login',
                            method: 'post',
                            data: {
                              username: this.ruleForm.username,
                              password: this.ruleForm.password
                            },
                          isForm: true
                        }).then(res => {
                            if (res.code===200){
                              console.log(res)
                              // localStorage.setItem('user', "123");
                              console.log(JSON.stringify(res.data))
                              localStorage.setItem('user_token',  JSON.stringify(res.data) );
                              this.$router.push('/home');
                            }else {
                              //报错信息弹窗
                              this.$message.error(res.msg);
                            }

                        })
                    } else {
                        console.log('error submit!!');
                        return false;
                    }
                });


            },
            // submitForm(formName) {
            //     this.$refs[formName].validate((valid) => {
            //         if (valid) {
            //
            //             localStorage.setItem('ms_username', this.ruleForm.username);
            //             this.$router.push('/');
            //         } else {
            //             console.log('error submit!!');
            //             return false;
            //         }
            //     });
            // }
        }
    }
</script>

<style scoped>
    .login-wrap {
        position: relative;
        width: 100%;
        height: 100vh;
        /*background-image: url(../../assets/img/BG.jpg);*/
      background-image: url("@/assets/img/BG.jpg");
        background-size: 100%;
    }

    .ms-title {
        width: 100%;
        line-height: 50px;
        text-align: center;
        font-size: 20px;
        color: #fff;
        border-bottom: 1px solid #ddd;
    }

    .ms-login {
        position: absolute;
        left: 50%;
        top: 50%;
        width: 350px;
        margin: -190px 0 0 -175px;
        border-radius: 5px;
        background: rgba(255, 255, 255, 0.3);
        overflow: hidden;
    }

    .ms-content {
        padding: 30px 30px;
    }

    .login-btn {
        text-align: center;
    }

    .login-btn button {
        width: 100%;
        height: 36px;
        margin-bottom: 10px;
    }

    .login-tips {
        font-size: 12px;
        line-height: 30px;
        color: #fff;
    }
</style>