
<template>
    <div class="post-list">
        <!--<div style="width: 200px;margin:auto">-->
           <!--用户名:-->
            <!--<el-input v-model="name" placeholder="请输入内容" ></el-input>-->
        <!--</div>-->

        <el-form :model="ruleForm2" status-icon :rules="rules2" ref="ruleForm2" label-width="100px" class="demo-ruleForm" style="width: 300px;margin: auto">
            <el-form-item label="用户编号" prop="id" style="display: none">
                <el-input v-model.number="ruleForm2.id"></el-input>
            </el-form-item>
            <el-form-item label="原密码" prop="oldPassword">
                <el-input type="password" v-model="ruleForm2.oldPassword" auto-complete="off"></el-input>
            </el-form-item>
            <el-form-item label="新密码" prop="newPassword">
                <el-input type="password" v-model="ruleForm2.newPassword" auto-complete="off"></el-input>
            </el-form-item>
            <el-form-item label="确认密码" prop="checkPass">
                <el-input type="password" v-model="ruleForm2.checkPass" auto-complete="off"></el-input>
            </el-form-item>
            <el-form-item>
                <el-button type="primary" @click="submitForm('ruleForm2')">提交</el-button>
                <el-button @click="resetForm('ruleForm2')">重置</el-button>
            </el-form-item>
        </el-form>

        <!--<author v-if="authorId" :id="authorId"></author>-->
    </div>
</template>

<script>

    export default {
        data() {
//            var checkAge = (rule, value, callback) => {
//                if (!value) {
//                    return callback(new Error('不能为空'));
//                }
//                setTimeout(() => {
//                    if (!Number.isInteger(value)) {
//                        callback(new Error('请输入数字值'));
//                    } else {
//                        if (value < 18) {
//                            callback(new Error('必须年满18岁'));
//                        } else {
//                            callback();
//                        }
//                    }
//                }, 1000);
//            };
//             var validateoldPass = (rule, value, callback) => {
//                if (!value) {
//                    return callback(new Error('不能为空'));
//                }
//            };
//            var validateoldPass = (rule, value, callback) => {
//                if (value === '') {
//                    callback(new Error('请输入密码'));
//                } else {
//                    if (this.ruleForm2.num1 !== '') {
//                        this.$refs.ruleForm2.validateField('num1');
//                    }
//                    callback();
//                }
//            };
            var validatePass = (rule, value, callback) => {
                if (value === '') {
                    callback(new Error('请输入密码'));
                } else {
                    if (this.ruleForm2.checkPass !== '') {
                        this.$refs.ruleForm2.validateField('checkPass');
                    }
                    callback();
                }
            };
            var validatePass2 = (rule, value, callback) => {
                if (value === '') {
                    callback(new Error('请再次输入密码'));
                } else if (value !== this.ruleForm2.newPassword) {
                    callback(new Error('两次输入密码不一致!'));
                } else {
                    callback();
                }
            };
            return {
                name: '',
                ruleForm2: {
                    oldPassword:'',
                    newPassword: '',
                    checkPass: '',
                    id:''
                },
                rules2: {
                    newPassword: [
                        { validator: validatePass, trigger: 'blur' }
                    ],
                    checkPass: [
                        { validator: validatePass2, trigger: 'blur' }
                    ],
//                    num1: [
//                        { validator: validateoldPass, trigger: 'blur' }
//                    ]
                }
            };
        },
        created(){
            this.getname();
        },
        methods: {
            getname(){
                this.name= localStorage.getItem('ms_username')
                this.ruleForm2.id=localStorage.getItem('uid');
            },

            submitForm(formName) {
                this.$refs[formName].validate((valid) => {
                    if (valid) {
                        delete this.ruleForm2.checkPass
                        console.log('6666',this.ruleForm2)
//                        alert('submit!');
                        let {id,newPassword,oldPassword}=this.ruleForm2
                        let data= {id,newPassword,oldPassword}
                        this.$ajax(
                            {
                                method: 'put', //请求方式
                                url: 'http://10.103.243.94:8080/userInfo/newPassword',
//                                http://10.103.243.94:8080/userInfo/newPassword?id=5&num1=111111&num2=123456
                                data:data,
                                headers:{"Authorization":localStorage.getItem('token')},
                            }).then((res)=>{
//                            this.permissionsInfo=[],
//                                this.permissionsInfo=res.data.data.results;
                            console.log('修改密码结果',res)
                        })

                    } else {
                        console.log('error submit!!');
                        return false;
                    }
                });
            },
            resetForm(formName) {
                this.$refs[formName].resetFields();
            }
        }
    }
</script>
