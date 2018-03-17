<template>
    <div class="table">
        <div class="crumbs">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item><i class="el-icon-menu"></i>登录日志列表管理</el-breadcrumb-item>
                <el-breadcrumb-item>登录日志列表信息</el-breadcrumb-item>
            </el-breadcrumb>
        </div>
        <div class="handle-box">
            <el-input v-model="select_word" placeholder="用户ID" class="handle-input mr10" style="width: 100px;height: 5px;"></el-input>
            <el-input v-model="select_word" placeholder="IP" class="handle-input mr10" style="width: 100px;height: 5px;"></el-input>
            <el-input v-model="select_word" placeholder="URL" class="handle-input mr10" style="width: 120px;height: 5px;"></el-input>

            <el-select v-model="select_cate" placeholder="访问方法" class="handle-select mr10" style="width: 150px;height: 5px;">
                <el-option key="1" label="广东省" value="广东省"></el-option>
                <el-option key="2" label="湖南省" value="湖南省"></el-option>
            </el-select>
            <!--<el-input v-model="select_word" placeholder="访问时间" class="handle-input mr10" style="width: 120px;height: 5px;"></el-input>-->
            <el-date-picker
                v-model="value1"
                type="datetime"
                placeholder="访问时间">
            </el-date-picker>
            <el-button type="primary" icon="search" @click="search">搜索</el-button>
            <!--<el-button type="primary"  @click="dialogFormVisible = true">新增人员</el-button>-->
        </div>
        <el-table :data="loginLogsInfo" border style="width: 100%" ref="multipleTable" @selection-change="handleSelectionChange">
            <el-table-column type="selection" width="55"></el-table-column>
            <!--<el-table-column prop="date" label="日期" sortable width="150">-->
            <!--</el-table-column>-->
            <el-table-column prop="yhId" label="用户ID" width="150">
            </el-table-column>
            <el-table-column prop="ip" label="IP" width="150">
            </el-table-column>
            <el-table-column prop="url" label="URL" width="150">
            </el-table-column>
            <el-table-column prop="method" label="访问方法" width="150">
            </el-table-column>
            <el-table-column prop="createTime" label="访问时间" width="150">
            </el-table-column>
            <!--<el-table-column prop="job" label="职位电话" width="120">-->
            <!--</el-table-column>-->
            <!--<el-table-column prop="state" label="状态" width="120">-->
            <!--</el-table-column>-->
            <!--<el-table-column prop="address" label="地址" :formatter="formatter">-->
            <!--</el-table-column>-->
            <!--<el-table-column label="操作" width="180">-->
                <!--<template scope="scope">-->
                    <!--&lt;!&ndash;<el-button size="small"&ndash;&gt;-->
                    <!--&lt;!&ndash;@click="handleEdit(scope.$index, scope.row)">编辑修改</el-button>&ndash;&gt;-->
                    <!--<el-button size="small" type="danger"-->
                               <!--@click="handleDelete(scope.$index, scope.row)">删除</el-button>-->
                <!--</template>-->
            <!--</el-table-column>-->
        </el-table>

        <div class="pagination">
            <el-pagination
                @current-change ="handleCurrentChange"
                layout="prev, pager, next"
                :total="1000">
            </el-pagination>
        </div>
        <!--<el-dialog title="新增人员" :visible.sync="dialogFormVisible">-->
            <!--<el-form :model="form" ref="form">-->
                <!--<el-form-item label="用户名" :label-width="formLabelWidth">-->
                    <!--<el-input v-model="form.username" auto-complete="off"></el-input>-->
                <!--</el-form-item>-->
                <!--<el-form-item label="名称" :label-width="formLabelWidth">-->
                    <!--<el-input v-model="form.name" auto-complete="off"></el-input>-->
                <!--</el-form-item>-->
                <!--<el-form-item label="密码" :label-width="formLabelWidth">-->
                    <!--<el-input v-model="form.password" auto-complete="off"></el-input>-->
                <!--</el-form-item>-->
                <!--<el-form-item label="用户职位" :label-width="formLabelWidth">-->
                    <!--<el-input v-model="form.job" auto-complete="off"></el-input>-->
                <!--</el-form-item>-->
                <!--<el-form-item label="性别" :label-width="formLabelWidth">-->
                    <!--<el-input v-model="form.gender" auto-complete="off"></el-input>-->
                <!--</el-form-item>-->
                <!--<el-form-item label="手机号" :label-width="formLabelWidth">-->
                    <!--<el-input v-model="form.phone" auto-complete="off"></el-input>-->
                <!--</el-form-item>-->
                <!--<el-form-item label="邮箱" :label-width="formLabelWidth">-->
                    <!--<el-input v-model="form.email" auto-complete="off"></el-input>-->
                <!--</el-form-item>-->
                <!--<el-form-item label="活动区域" :label-width="formLabelWidth">-->
                <!--<el-select v-model="form.region" placeholder="请选择活动区域">-->
                <!--<el-option label="区域一" value="shanghai"></el-option>-->
                <!--<el-option label="区域二" value="beijing"></el-option>-->
                <!--</el-select>-->
                <!--</el-form-item>-->
            <!--</el-form>-->
            <!--<div slot="footer" class="dialog-footer">-->
                <!--<el-button @click="dialogFormVisible = false">取 消</el-button>-->
                <!--<el-button type="primary" @click="addUser('form')">确 定</el-button>-->
            <!--</div>-->
        <!--</el-dialog>-->
    </div>
</template>

<script>
    export default {
        data() {
            return {
                loginLogsInfo: [],
                cur_page: 1,
                multipleSelection: [],
                select_cate: '',
                select_word: '',
                del_list: [],
                is_search: false,
                dialogFormVisible: false,
                form: {
                    username: '',
                    name:'',
                    password: '',
                    job: '',
                    gender: '',
                    phone: '',
                    email: ''
                },
                formLabelWidth: '120px'
            }
        },
        created(){
//            this.getData();
            this.$ajax(
                {
                    method: 'get', //请求方式
                    url: 'http://10.103.243.94:8080/loginLog/page',
                    params:{
                        page:1,
                        size:5
                    },
                    headers:{"Authorization":localStorage.getItem('token')},
                }).then((res)=>{
                this.loginLogsInfo=[],
                    this.loginLogsInfo=res.data.data.results;
                console.log('结果22',res.data.data.results)
            })
        },
        computed: {
            data(){
                const self = this;
                return self.tableData.filter(function(d){
                    let is_del = false;
                    for (let i = 0; i < self.del_list.length; i++) {
                        if(d.name === self.del_list[i].name){
                            is_del = true;
                            break;
                        }
                    }
                    if(!is_del){
                        if(d.address.indexOf(self.select_cate) > -1 &&
                            (d.name.indexOf(self.select_word) > -1 ||
                                d.address.indexOf(self.select_word) > -1)
                        ){
                            return d;
                        }
                    }
                })
            }
        },
        methods: {
            addUser(formName){
                let data=Object.assign({},this.form);
                console.log('ttt',data)
                this.$ajax(
                    {
                        method: 'post', //请求方式
                        url: 'http://10.103.243.94:8080/userInfo',
                        data:data,
                        headers:{"Authorization":localStorage.getItem('token')},
                    }).then((res)=>{
                    this.usersInfo=[],
                        this.usersInfo=res.data.data.results;
                    console.log('结果',this.usersInfo)
                });
                this.dialogFormVisible = false;
            },
            handleCurrentChange(val){
                this.cur_page = val;
                this.getData();
            },
            getData(){
                let self = this;
                if(process.env.NODE_ENV === 'development'){
                    self.url = '/ms/table/list';
                };
                self.$axios.post(self.url, {page:self.cur_page}).then((res) => {
                    self.tableData = res.data.list;
                })
            },
            search(){
                this.is_search = true;
            },
            formatter(row, column) {
                return row.address;
            },
            filterTag(value, row) {
                return row.tag === value;
            },
            handleEdit(index, row) {
                this.$message('编辑第'+(index+1)+'行');
            },
//            handleDelete(index, row) {
//                this.$message.error('删除第'+(index+1)+'行');
//                this.$ajax(
//                    {
//                        method: 'post', //请求方式
//                        url: 'http://10.103.243.94:8080/userInfo/{uid}',
//                        data:data
//                        headers:{"Authorization":localStorage.getItem('token')},
//                    }).then((res)=>{
//                    this.usersInfo=[],
//                        this.usersInfo=res.data.data.results;
//                    console.log('结果',this.usersInfo)
//                });
//
//            },
            handleDelete (index, row) {
                this.usersInfo.splice(index, 1);
                console.log('ddddd',row.uid)
                let uid=row.uid;
                this.$ajax({
                    method: 'delete', //请求方式
                    url: 'http://10.103.243.94:8080/userInfo',
                    params:{
                        uid
                    },
                    headers:{"Authorization":localStorage.getItem('token')}
                }).then(
                    (res) => {
                        console.log(res);
                    });
                this.$message({
                    message: "操作成功！",
                    type: 'success'
                });
            },
            delAll(){
                const self = this,
                    length = self.multipleSelection.length;
                let str = '';
                self.del_list = self.del_list.concat(self.multipleSelection);
                for (let i = 0; i < length; i++) {
                    str += self.multipleSelection[i].name + ' ';
                }
                self.$message.error('删除了'+str);
                self.multipleSelection = [];
            },
            handleSelectionChange(val) {
                this.multipleSelection = val;
            }
        }
    }
</script>

<style scoped>
    .handle-box{
        margin-bottom: 20px;
    }
    .handle-select{
        width: 120px;
    }
    .handle-input{
        width: 300px;
        display: inline-block;
    }
</style>
