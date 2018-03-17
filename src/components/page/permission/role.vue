<template>
    <div class="table">
        <div class="crumbs">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item><i class="el-icon-menu"></i> 角色管理</el-breadcrumb-item>
                <el-breadcrumb-item>角色信息</el-breadcrumb-item>
            </el-breadcrumb>
        </div>
        <div class="handle-box">
            <el-input v-model="select_word" placeholder="编号" class="handle-input mr10" style="width: 120px;height: 5px;">></el-input>
            <el-input v-model="select_word" placeholder="名称" class="handle-input mr10" style="width: 120px;height: 5px;">></el-input>
            <el-input v-model="select_word" placeholder="描述" class="handle-input mr10" style="width: 120px;height: 5px;">></el-input>
            <el-button type="primary" icon="search" @click="search">搜索</el-button>
            <el-button type="primary"  @click="dialogFormVisible1 = true">新增角色</el-button>
        </div>
        <el-table :data="rolesInfo" border style="width: 100%" ref="multipleTable" @selection-change="handleSelectionChange">
            <el-table-column type="selection" width="55"></el-table-column>
            <!--<el-table-column prop="date" label="日期" sortable width="150">-->
            <!--</el-table-column>-->
            <el-table-column prop="id" label="编号" sortable width="50">
            </el-table-column>
            <el-table-column prop="role" label="名称" width="120">
            </el-table-column>
            <el-table-column prop="description" label="描述" width="120">
            </el-table-column>
            <!--<el-table-column prop="address" label="地址" :formatter="formatter">-->
            <!--</el-table-column>-->
            <el-table-column label="操作" width="280">
                <template scope="scope">
                    <el-button size="small"
                    @click="handleEdit(scope.$index, scope.row)">编辑修改</el-button>
                    <el-button size="small"
                               @click="checkPerssion(scope.$index, scope.row)">查看权限</el-button>
                    <el-button size="small" type="danger"
                               @click="handleDelete(scope.$index, scope.row)">删除</el-button>
                </template>
            </el-table-column>
        </el-table>
        <div class="pagination">
            <el-pagination
                @current-change ="handleCurrentChange"
                layout="prev, pager, next"
                :total="1000">
            </el-pagination>
        </div>
        <el-dialog title="新增角色" :visible.sync="dialogFormVisible1">
            <el-form ref="newRole" :model="newRole" label-width="80px">
                <!--<el-form-item label="角色ID">-->
                    <!--<el-input v-model="form.id"></el-input>-->
                <!--</el-form-item>-->
                <el-form-item label="角色名称">
                    <el-input v-model="newRole.role"></el-input>
                </el-form-item>
                <el-form-item label="角色描述">
                    <el-input v-model="newRole.description"></el-input>
                </el-form-item>
                <el-form-item>
                    <el-button type="primary" @click="submitRole(newRole)">提交</el-button>
                    <el-button   @click="dialogFormVisible1 = false">取消</el-button>
                </el-form-item>
            </el-form>
        </el-dialog>

        <el-dialog title="权限信息" :visible.sync="dialogTableVisible" style="width: 50%;margin: auto" >
            <el-table
                :data="permission"
                stripe
                style="width: 100%">
                <el-table-column
                    prop="name"
                    label="该用户操作权限如下"
                    width="280">
                </el-table-column>
            </el-table>
        </el-dialog>


        <el-dialog title="编辑角色信息" :visible.sync="dialogFormVisible">
            <el-form :model="form">
                <el-form-item label="角色编号" :label-width="formLabelWidth">
                    <el-input v-model="form.id" auto-complete="off"></el-input>
                </el-form-item>
                <el-form-item label="角色名称" :label-width="formLabelWidth">
                    <el-input v-model="form.role" auto-complete="off"></el-input>
                </el-form-item>
                <el-form-item label="描述" :label-width="formLabelWidth">
                    <el-input v-model="form.description" auto-complete="off"></el-input>
                </el-form-item>
                <!--<el-form-item label="权限" :label-width="formLabelWidth">-->
                    <!--<el-input v-model="form.ckHgCount" auto-complete="off"></el-input>-->
                <!--</el-form-item>-->
                <!--<el-form-item label="属性" :label-width="formLabelWidth">-->
                    <!--<el-input v-model="form.ckAttribute" auto-complete="off"></el-input>-->
                <!--</el-form-item>-->
                <!--<el-form-item label="状态" :label-width="formLabelWidth">-->
                    <!--<el-input v-model="form.ckStatus" auto-complete="off"></el-input>-->
                <!--</el-form-item>-->
                <el-form-item label="权限" :label-width="formLabelWidth">
                <!--<el-select v-model="form.region" placeholder="请选择操作权限">-->
                <!--<el-option label="区域一" value="shanghai"></el-option>-->
                <!--<el-option label="区域二" value="beijing"></el-option>-->
                <!--</el-select>-->
                    <el-checkbox-group v-model="addpermission" @change="handleCheckedCitiesChange">
                        <el-checkbox v-for="permission in allpermissions" :label="permission.id" :key="permission.id">{{permission.name}}</el-checkbox>
                    </el-checkbox-group>
                </el-form-item>
            </el-form>
            <div slot="footer" class="dialog-footer">
                <el-button @click="dialogFormVisible = false">取 消</el-button>
                <el-button type="primary" @click="submitUpdatarole('form')">确 定</el-button>
            </div>
        </el-dialog>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                newRole:{},
                permission:[],
                allpermissions:[],
                addpermission:[],
                rolesInfo: [],
                cur_page: 1,
                multipleSelection: [],
                select_cate: '',
                select_word: '',
                del_list: [],
                is_search: false,
                form: {},
                dialogTableVisible: false,
                dialogFormVisible:false,
                formLabelWidth: '180px',
                dialogFormVisible1: false
            }
        },
        created(){
//            this.getData();
            this.$ajax(
                {
                    method: 'get', //请求方式
                    url: 'http://10.103.243.94:8080/role/page',
                    params:{
                        page:1,
                        size:5
                    },
                    headers:{"Authorization":localStorage.getItem('token')},
                }).then((res)=>{
                this.rolesInfo=[],
                    this.rolesInfo=res.data.data.results;
                console.log('结果',this.rolesInfo)
            })
            this.$ajax(
                {
                    method: 'get', //请求方式
                    url: 'http://10.103.243.94:8080/permission/page',
                    params:{
                        page:1,
                        size:20
                    },
                    headers:{"Authorization":localStorage.getItem('token')},
                }).then((res)=>{
                this.allpermissions=[],
                this.allpermissions=res.data.data.results;
                console.log('权限',this.allpermissions)
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
            handleCheckedCitiesChange(value) {
                console.log('fuxuan',value)
//                let obj={}
//                obj.id=value
//                console.log('22',obj)
                let checkedCount = value.length;
                this.addpermission=[];
                this.addpermission=value;
            },
            submitRole(newRole){
                let {role,description}=newRole
                let data= {role,description}
                console.log('submit',newRole)
                this.dialogFormVisible1=false;
                this.$ajax(
                    {
                        method: 'post', //请求方式
                        url: 'http://10.103.243.94:8080/role',
                        data:data,
                        headers:{"Authorization":localStorage.getItem('token')},
                    }).then((res)=>{
                    console.log('结果qqq',res)
                });


            },
            checkPerssion(index, row){
                this.dialogTableVisible = true;
                this.$ajax(
                    {
                        method: 'get', //请求方式
                        url: 'http://10.103.243.94:8080/role',
                        params:{
                            id:row.id
                        },
                        headers:{"Authorization":localStorage.getItem('token')},
                    }).then((res)=>{
                    let permission=[];
                    this.permission=res.data.data.permissions
                    console.log('查看权限',res.data.data.permissions)
                });



             },
            handleCurrentChange(val){
                this.cur_page = val;
                this.getData();
            },
//            getData(){
//                let self = this;
//                if(process.env.NODE_ENV === 'development'){
//                    self.url = '/ms/table/list';
//                };
//                self.$axios.post(self.url, {page:self.cur_page}).then((res) => {
//                    self.tableData = res.data.list;
//                })
//            },
            handleClose(done) {
                this.$confirm('确认关闭？')
                    .then(_ => {
                        done();
                    })
                    .catch(_ => {});
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
                this.dialogFormVisible = true;
//                this.$message('编辑第'+(index+1)+'行');
                this.form=row;
            },
            handleDelete(index, row) {
                this.$message.error('删除第'+(index+1)+'行');
            },
            submitUpdatarole(form){
                let data=Object.assign({},this.form);
                let permission=[]
                this.addpermission.map((item)=>{
                    item={id:item}
                    permission.push(item)
                })
                data.permission=permission
                delete data.permissions
                console.log('编辑提交信息',data)
                this.$ajax(
                    {
                        method: 'put', //请求方式
                        url: 'http://10.103.243.94:8080/warehouse',
                        data:data,
                        headers:{"Authorization":localStorage.getItem('token')},
                    }).then((res)=>{
                    console.log('编辑结果',res)
                });
                this.dialogFormVisible = false;
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
