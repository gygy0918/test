<template>
    <div class="table">
        <div class="crumbs">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item><i class="el-icon-menu"></i> 权限管理</el-breadcrumb-item>
                <el-breadcrumb-item>权限信息</el-breadcrumb-item>
            </el-breadcrumb>
        </div>
        <div class="handle-box">
            <el-input v-model="select_word" placeholder="编号" class="handle-input mr10" style="width: 120px;height: 5px;"></el-input>
            <el-input v-model="select_word" placeholder="名称" class="handle-input mr10" style="width: 120px;height: 5px;"></el-input>
            <el-select v-model="select_cate" placeholder="类型" class="handle-select mr10" style="width: 120px;height: 5px;">
                <el-option key="1" label="广东省" value="广东省"></el-option>
                <el-option key="2" label="湖南省" value="湖南省"></el-option>
            </el-select>
            <el-select v-model="select_cate" placeholder="权限值" class="handle-select mr10">
                <el-option key="1" label="广东省" value="广东省"></el-option>
                <el-option key="2" label="湖南省" value="湖南省"></el-option>
            </el-select>
            <el-select v-model="select_cate" placeholder="路径" class="handle-select mr10">
                <el-option key="1" label="广东省" value="广东省"></el-option>
                <el-option key="2" label="湖南省" value="湖南省"></el-option>
            </el-select>
            <el-button type="primary" icon="search" @click="search">搜索</el-button>
        </div>
        <el-table :data="permissionsInfo" border  ref="multipleTable" @selection-change="handleSelectionChange">
            <el-table-column type="selection" width="55"></el-table-column>
            <el-table-column prop="id" label="编号" sortable width="50">
            </el-table-column>
            <el-table-column prop="name" label="名称" width="150">
            </el-table-column>
            <el-table-column prop="resourceType" label="类型" width="150">
            </el-table-column>
            <el-table-column prop="permission" label="权限值" width="150">
            </el-table-column>
            <el-table-column prop="url" label="路径" width="150">
            </el-table-column>
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
    </div>
</template>

<script>
    export default {
        data() {
            return {
                permissionsInfo: [],
                cur_page: 1,
                multipleSelection: [],
                select_cate: '',
                select_word: '',
                del_list: [],
                is_search: false,
                dialogFormVisible:false,
                formLabelWidth: '120px'
            }
        },
        created(){
//            this.getData();
            this.$ajax(
                {
                    method: 'get', //请求方式
                    url: 'http://10.103.243.94:8080/permission/page',
                    params:{
                        page:1,
                        size:5
                    },
                    headers:{"Authorization":localStorage.getItem('token')},
                }).then((res)=>{
                this.permissionsInfo=[],
                    this.permissionsInfo=res.data.data.results;
                console.log('结果',this.permissionsInfo)
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
            handleDelete(index, row) {
                this.$message.error('删除第'+(index+1)+'行');
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
