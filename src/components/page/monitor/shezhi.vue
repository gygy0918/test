<template>


    <div id="control" style="width: 300px">
       物品名称： <el-input
            placeholder="请输入内容"
            v-model="form.lightName "
            :disabled="true">
        </el-input>
        物品位置： <el-input
        placeholder="请输入内容"
        v-model="form.location"
        :disabled="true">
        </el-input>
            电压： <el-input
            placeholder="请输入内容"
            v-model="form.power"
            :disabled="true">
        </el-input>
            功率： <el-input
            placeholder="请输入内容"
            v-model="form.electricity"
            :disabled="true">
        </el-input>
        <p style="margin-top: 20px;">开关</p>
       <el-switch
        v-model="value2"
        active-color="#13ce66"
        inactive-color="#ff4949"
    style="margin-top: 20px">
    </el-switch>
        <p style="margin-top: 20px;">亮度控制</p>
        <el-slider
            v-model="value8"
            :step="0.1"
            :max="1"
            show-input>
        </el-slider>

            <el-button type="primary" @click="onSubmit" style="margin-top: 20px;">确定并返回</el-button>
            <el-button>取消</el-button>

    </div>

</template>

<script>
    export default {
        data: function(){
            return {
              value1: true,
              value2: true,
              value8: 0,
                form: {
                    lightId:'',
                    lightName:'',
                    power:'',
                    electricity:'',
                    location
                },
                ruleForm: {
                    username: 'root',
                    password: '123456'
                },
                rules: {
                    username: [
                        { required: true, message: '请输入用户名', trigger: 'blur' }
                    ],
                    password: [
                        { required: true, message: '请输入密码', trigger: 'blur' }
                    ]
                }
            }
        },
        created(){
            this.form.lightId= window.localStorage.getItem('lightId')
            this.form.lightName= window.localStorage.getItem('lightName')
            this.form.power= window.localStorage.getItem('power')
            this.form.electricity= window.localStorage.getItem('electricity')
            this.form.location= window.localStorage.getItem('location')

//            console.log('electricity',data)
        },
        methods: {
            onSubmit() {
                console.log('submit!');
                this.$router.push('/lamp')
            },
            submitForm(formName) {
                const self = this;
                self.$refs[formName].validate((valid) => {
                    if (valid) {
                        console.log('formName',this.ruleForm)
                        const {username,password}=this.ruleForm;
                        let data={username,password};
                        this.$ajax({
                            method:'post',
                            url:'http://10.103.243.94:8080/login',
                            data:data
                        }).then((res)=>{
                            console.log('test',res)
                            localStorage.setItem('ms_username',self.ruleForm.username);
                            localStorage.setItem('token',res.data.token);
                            localStorage.setItem('msg',res.data.msg);
                            self.$router.push('/readme');
                        })
                    } else {
                        console.log('error submit!!');
                        return false;
                    }
                });
            }
        }
    }
</script>

<style scoped>
    .login-wrap{
        position: relative;
        width:100%;
        height:100%;
    }
    .ms-title{
        position: absolute;
        top:50%;
        width:100%;
        margin-top: -230px;
        text-align: center;
        font-size:30px;
        color: #fff;

    }
    .ms-login{
        position: absolute;
        left:50%;
        top:50%;
        width:300px;
        height:160px;
        margin:-150px 0 0 -190px;
        padding:40px;
        border-radius: 5px;
        background: #fff;
    }
    .login-btn{
        text-align: center;
    }
    .login-btn button{
        width:100%;
        height:36px;
    }
</style>
