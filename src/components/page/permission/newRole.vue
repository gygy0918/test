<template>
    <div>
        <div class="crumbs">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item><i class="el-icon-date"></i>新增入库表单</el-breadcrumb-item>
                <el-breadcrumb-item>新增角色表单</el-breadcrumb-item>
            </el-breadcrumb>
        </div>
        <div class="form-box">
            <el-form ref="form" :model="form" label-width="80px">
                <el-form-item label="角色ID">
                    <el-input v-model="form.id"></el-input>
                </el-form-item>
                <el-form-item label="角色名称">
                    <el-input v-model="form.role"></el-input>
                </el-form-item>
                <el-form-item label="角色名称">
                    <el-input v-model="form.description"></el-input>
                </el-form-item>
                <!--<el-form-item label="商品ID">-->
                    <!--<el-input v-model="form.spId"></el-input>-->
                <!--</el-form-item>-->
                <!--<el-form-item label="入库商品数量">-->
                    <!--<el-input v-model="form.rkCount"></el-input>-->
                <!--</el-form-item>-->
                <!--<el-form-item label="备注">-->
                <!--<el-input v-model="form.remark"></el-input>-->
                <!--</el-form-item>-->
                <!--<el-form-item label="表单名称">-->
                <!--<el-input v-model="form.name"></el-input>-->
                <!--</el-form-item>-->
                <!--<el-form-item label="选择器">-->
                <!--<el-select v-model="form.region" placeholder="请选择">-->
                <!--<el-option key="bbk" label="步步高" value="bbk"></el-option>-->
                <!--<el-option key="xtc" label="小天才" value="xtc"></el-option>-->
                <!--<el-option key="imoo" label="imoo" value="imoo"></el-option>-->
                <!--</el-select>-->
                <!--</el-form-item>-->
                <!--<el-form-item label="日期时间">-->
                <!--<el-col :span="11">-->
                <!--<el-date-picker type="date" placeholder="选择日期" v-model="form.date1" style="width: 100%;"></el-date-picker>-->
                <!--</el-col>-->
                <!--<el-col class="line" :span="2">-</el-col>-->
                <!--<el-col :span="11">-->
                <!--<el-time-picker type="fixed-time" placeholder="选择时间" v-model="form.date2" style="width: 100%;"></el-time-picker>-->
                <!--</el-col>-->
                <!--</el-form-item>-->
                <!--<el-form-item label="选择开关">-->
                <!--<el-switch on-text="" off-text="" v-model="form.delivery"></el-switch>-->
                <!--</el-form-item>-->
                <!--<el-form-item label="多选框">-->
                <!--<el-checkbox-group v-model="form.type">-->
                <!--<el-checkbox label="步步高" name="type"></el-checkbox>-->
                <!--<el-checkbox label="小天才" name="type"></el-checkbox>-->
                <!--<el-checkbox label="imoo" name="type"></el-checkbox>-->
                <!--</el-checkbox-group>-->
                <!--</el-form-item>-->
                <!--<el-form-item label="单选框">-->
                <!--<el-radio-group v-model="form.resource">-->
                <!--<el-radio label="步步高"></el-radio>-->
                <!--<el-radio label="小天才"></el-radio>-->
                <!--<el-radio label="imoo"></el-radio>-->
                <!--</el-radio-group>-->
                <!--</el-form-item>-->
                <el-form-item label="备注">
                    <el-input type="textarea" v-model="form.remark"></el-input>
                </el-form-item>
                <el-form-item>
                    <el-button type="primary" @click="onSubmit">提交</el-button>
                    <el-button>取消</el-button>
                </el-form-item>
            </el-form>
        </div>

    </div>
</template>

<script>
    export default {
        data: function(){
            return {
                roleInfo:[],
                form: {
                    description: '',
                    roel: '',
                    id: ''
                }
            }
        },
        methods: {
            onSubmit() {
                let data=Object.assign({},this.form);
                console.log('ttt',data)
                this.$ajax(
                    {
                        method: 'post', //请求方式
                        url: 'http://10.103.243.94:8080/role',
                        data:data,
                        headers:{"Authorization":localStorage.getItem('token')},
                    }).then((res)=>{
                    this.roleInfo=[],
                        this.roleInfo=res.data.data.results;
                    console.log('结果',this.roleInfo)
                });
                this.$message.success('提交成功！');
                self.$router.push('/vueeditor');
            }
        }
    }
</script>
