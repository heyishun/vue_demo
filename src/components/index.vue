<template>
    <div>
        <el-button size="primary" style="float: left" type="" @click="handleAdd()">添加学生</el-button>
        <el-table :data="tableData.filter(data => !search || data.name.toLowerCase().includes(search.toLowerCase()))"
            style="width: 100%">
            <el-table-column label="sid" prop="sid">
            </el-table-column>
            <el-table-column label="name" prop="name">
            </el-table-column>
            <el-table-column label="age" prop="age">
            </el-table-column>
            <el-table-column label="birthday" prop="birthday">
            </el-table-column>

            <el-table-column align="right">
                <template slot="header" slot-scope="scope">
                    <el-input placeholder="输入关键字搜索" size="primary" v-model="search" clearable>
                    </el-input>
            
                </template>
                <template slot-scope="scope">
                    <el-button size="primary" @click="handleEdit(scope.row)">修改</el-button>
                    <el-button size="primary" type="danger" @click="handleDelete(scope.sid)">删除</el-button>
                </template>


            </el-table-column>

        </el-table>


        <el-dialog title="修改学生" :visible.sync="dialogTableVisible" :data="templateList">
            <template scope='scope'>
                <el-form :model="templateList" ref="templateList" style="padding:0 1vw;">
                    <el-row>
                        <el-input v-model="templateList.sid" readonly>
                            <template slot="prepend">sid</template>
                        </el-input>
                    </el-row>
                    <el-row>
                        <el-input v-model="templateList.name">
                            <template slot="prepend">name</template>
                        </el-input>
                    </el-row>
                    <el-row>
                        <el-input v-model="templateList.age">
                            <template slot="prepend">age</template>
                        </el-input>
                    </el-row>
                    <el-row>
                        <el-date-picker v-model="templateList.birthday" type="date" placeholder="选择日期"
                            value-format="yyyy-MM-dd">
                        </el-date-picker>
                    </el-row>

                </el-form>
            </template>
            <span slot="footer" class="dialog-footer">
                <el-button type="primary" @click="submitEdit(templateList)">确定</el-button>
            </span>
        </el-dialog>
        <!-- 添加学生 -->
        <el-dialog title="添加学生" :visible.sync="dialogTableVisibleAdd" :data="templateListAdd" @close="closeDialog">
            <template scope='scope'>
                <el-form :model="templateListAdd" ref="templateListAdd" style="padding:0 1vw;">
                    <el-row>
                        <el-input v-model="templateListAdd.name">
                            <template slot="prepend">name</template>
                        </el-input>
                    </el-row>
                    <el-row>
                        <el-input v-model="templateListAdd.age">
                            <template slot="prepend">age</template>
                        </el-input>
                    </el-row>
                    <el-row>
                        <el-date-picker v-model="templateListAdd.birthday" type="date" placeholder="选择日期"
                            value-format="yyyy-MM-dd">
                        </el-date-picker>
                    </el-row>

                </el-form>
            </template>
            <span slot="footer" class="dialog-footer">
                <el-button type="primary" @click="submitAdd(templateListAdd)">确定</el-button>
            </span>
        </el-dialog>
    </div>
</template>
  
<script>
import axios from "axios";
export default {
    mounted() {
        //页面加载完成发送异步请求，查询数据
        axios.get("http://localhost/student/queryAll")
            .then(res => {
                if (res.data.success) {
                    //alert("查询成功~");
                    this.tableData = res.data.data
                }
            });
    },

    data() {
        return {
            //表格数据
            tableData: [],
            search: '',
            loading: false,
            //修改弹框
            templateList: {
                sid: "",
                name: "",
                age: "",
                birthday: ""
            },
            templateListAdd: {
                name: "",
                age: "",
                birthday: ""
            },
            dialogTableVisible: false,
            dialogTableVisibleAdd: false
        }
    },
    methods: {
        handleEdit(row) {
            console.log(row);
            this.dialogTableVisible = true;
            this.templateList = row;

        },
        handleAdd() {
            this.dialogTableVisibleAdd = true;
        },
        submitEdit(stu) {
            console.log(stu);
            axios.post("http://localhost/student/update", {
                sid: this.templateList.sid,
                name: this.templateList.name,
                age: this.templateList.age,
                birthday: this.templateList.birthday
            })
                .then(res => {
                    if (res.data.success) {
                        alert("修改成功~");
                        axios.get("http://localhost/student/queryAll")
                            .then(res => {
                                if (res.data.success) {
                                    this.tableData = res.data.data
                                }
                            });
                    } else {
                        alert("修改出错~");
                    }
                });
            this.dialogTableVisible = false
        },
        submitAdd(stu) {
            console.log(stu);
            axios.post("http://localhost/student/add", {
                name: this.templateListAdd.name,
                age: this.templateListAdd.age,
                birthday: this.templateListAdd.birthday
            })
                .then(res => {
                    if (res.data.success) {
                        alert("添加成功~");
                        axios.get("http://localhost/student/queryAll")
                            .then(res => {
                                if (res.data.success) {
                                    this.tableData = res.data.data
                                }
                            });
                    } else {
                        alert("用户已存在~");
                    }
                });
            this.dialogTableVisibleAdd = false

        },
        handleDelete(index, row) {
            console.log(index, row);
        },
        //关闭弹框的事件
        closeDialog() {
            this.templateListAdd = '';//清空数据
        },

    },
}
</script>

<style>
.el-row {
    margin-bottom: 20px;
}
</style>