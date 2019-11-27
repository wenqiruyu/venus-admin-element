<template>
    <div>
        <div class="crumbs">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item><i class="iconfont icon-wen-book"></i> 图书管理</el-breadcrumb-item>
                <el-breadcrumb-item>图书详情</el-breadcrumb-item>
                <el-breadcrumb-item>添加详情</el-breadcrumb-item>
            </el-breadcrumb>
        </div>
        <div class="container">
            <div class="plugins-tips">
                <p>
                    图书详情必须包含：<span style="color: #FF0036;">开 本、纸 张、包 装、是否套装、国际标准书号ISBN相关信息</span>
                    可包含：<span style="color: #515A6E;">图书特色、编辑推荐、内容简介、作者简介、目录、前言等</span>
                </p>
            </div>
            <div class="form-box">
                <el-form ref="form" :rules="rules" :model="form" label-width="120px">
                    <el-form-item prop="name" label="图书名称">
                        <el-input v-model="form.name"></el-input>
                    </el-form-item>
                    <el-form-item prop="format" label="图书开张">
                        <el-select v-model="form.format" placeholder="选择图书开张">
                            <el-option key="1" label="16开" value="16开"></el-option>
                            <el-option key="2" label="32开" value="32开"></el-option>
                        </el-select>
                    </el-form-item>
                    <el-form-item prop="paper" label="图书纸型">
                        <el-select v-model="form.paper" placeholder="选择图书纸型">
                            <el-option key="1" label="胶版纸" value="胶版纸"></el-option>
                            <el-option key="2" label="轻型纸" value="轻型纸"></el-option>
                        </el-select>
                    </el-form-item>
                    <el-form-item prop="pack" label="图书包装">
                        <el-select v-model="form.pack" placeholder="选择图书纸型">
                            <el-option key="1" label="平装-胶订" value="平装-胶订"></el-option>
                            <el-option key="2" label="轻型纸" value="轻型纸"></el-option>
                        </el-select>
                    </el-form-item>
                    <el-form-item prop="isSuit" label="是否套装">
                        <el-radio-group v-model="form.isSuit">
                            <el-radio label="1">是</el-radio>
                            <el-radio label="0">否</el-radio>
                        </el-radio-group>
                    </el-form-item>
                    <el-form-item prop="bookNumber" label="国际标准书号">
                        <el-input v-model="form.bookNumber"></el-input>
                    </el-form-item>
                </el-form>
            </div>
            <quill-editor ref="myTextEditor" v-model="content" :options="editorOption"></quill-editor>
            <el-button class="editor-btn" type="primary" @click="submit('form')">提交</el-button>
            <el-button @click="toBack">取消</el-button>
        </div>
    </div>
</template>

<script>
    import 'quill/dist/quill.core.css';
    import 'quill/dist/quill.snow.css';
    import 'quill/dist/quill.bubble.css';
    import { quillEditor } from 'vue-quill-editor';
    export default {
        name: 'editor',
        data: function(){
            return {
                content: '',
                editorOption: {
                    placeholder: '填写图书特色、编辑推荐、内容简介、作者简介、目录、前言等'
                },
                bookId: '',
                form: {},
                rules: {
                    name: [
                        { required: true, message: '书本名称不能为空哦！', trigger: 'blur' }
                    ],
                    format: [
                        { required: true, message: '书本开本不能为空哦！', trigger: 'blur' }
                    ],
                    paper: [
                        { required: true, message: '书本纸张类型不能为空哦！', trigger: 'blur' }
                    ],
                    pack: [
                        { required: true, message: '书本包装类型不能为空哦！', trigger: 'blur' }
                    ],
                    isSuit: [
                        { required: true, message: '请选择书本是否为套装！', trigger: 'blur' }
                    ],
                    bookNumber: [
                        { required: true, message: '书本国际标准书号不能为空哦！', trigger: 'blur' }
                    ]
                }
            }
        },
        components: {
            quillEditor
        },
        methods: {
            toBack(){
                this.$router.push('/bookDetail')
            },
            onEditorChange({ editor, html, text }) {
                this.content = html;
            },
            submit(formName){
                this.$refs[formName].validate((valid) => {
                    if (valid) {
                        if(this.content == ""){
                            this.$notify.error({
                                title: '内容为空',
                                message: '不能提交空的详情哟'
                            });
                        }else{
                            // 根据商品名称查询id
                            this.$axios.get("/product-server/product/name/" + this.form.name).then((res) => {
                                if(res.data.status == 1){
                                   this.bookId = res.data.data.id
                                }else{
                                    this.$notify.error({
                                        title: '书名错误',
                                        message: '书名相关的书籍不存在哟！请检查书名'
                                    });
                                }
                            })
                            console.log(this.bookId)
                            this.$axios.post('/product-server/product/newDetail',{
                                productId: this.bookId,
                                format: this.form.format,
                                paper: this.form.paper,
                                pack: this.form.pack,
                                isSuit: this.form.isSuit,
                                bookNumber: this.form.bookNumber,
                                content: this.content
                            }).then((res) => {
                                if(res.data.status == 1){
                                    this.$message.success(res.data.msg)
                                    this.$router.push('/bookDetail')
                                }else{
                                    this.$notify.error({
                                        title: '错误提示',
                                        message: 'res.data.msg'
                                    });
                                }
                            })
                        }
                    }
                })
            }
        }
    }
</script>
<style scoped>
    .editor-btn{
        margin-top: 20px;
    }
</style>