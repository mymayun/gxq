<template>
    <Modal @on-visible-change="openModal" v-model="evalModal" title="服务报告" width="40%" :closable='false' :mask-closable="false">
        <div style="text-align:center">
            <Form ref="evalData" :model="evalData" :rules="ruleValidate" :label-width="70" class="clearfix">
                <Row :gutter="40">
                    <Col span="24">
                        <FormItem label="时间" label-position="left" prop="date">
                            <Input v-model="evalData.date" disabled></Input>
                        </FormItem>
                    </Col>
                     <Col span="24">
                        <FormItem label="服务过程" :label-width="72" prop="process">
                            <Input v-model="evalData.process" type="textarea" :autosize="{minRows: 2,maxRows: 5}" style="resize:none;" disabled></Input>
                        </FormItem>
                    </Col>
                     <Col span="24">
                        <FormItem label="结果" :label-width="72" prop="results">
                            <Input v-model="evalData.results" type="textarea" :autosize="{minRows: 2,maxRows: 5}" style="resize:none;" disabled></Input>
                        </FormItem>
                    </Col>
                     <Col span="24">
                        <FormItem label="备注" :label-width="72" prop="note">
                            <Input v-model="evalData.note" type="textarea" :autosize="{minRows: 2,maxRows: 5}" style="resize:none;" disabled></Input>
                        </FormItem>
                    </Col>
                    <Col span="18">
                        <FormItem label="服务报告" prop="man">
                            <Input v-model="evalData.man" disabled></Input>
                        </FormItem>
                    </Col>
                    <Col span="6">
                        <a class="download" type="button">下载</a>
                    </Col>
                    <Col span="18">
                        <FormItem label="服务成果" prop="man">
                            <Input v-model="evalData.man" disabled></Input>
                        </FormItem>
                    </Col>
                    <Col span="6">
                        <a class="download" type="button">下载</a>
                    </Col>
                </Row>
            </Form>
        </div>
        <div slot="footer">
            <Button class="modalBtn" type="default" @click="closeModal" size="large">关闭</Button>
        </div>
    </Modal>
</template>

<script>
export default {
    data(){
        return{
            evalData: {
                safetyLevel: '3',
            },
            ruleValidate: {
            }
        }
    },
    props:{
        evalModal:{
            type: Boolean,
            default: false
        },
        closeModal: Function,
    },
    methods:{
        openModal(status) {
            if(!status){
                this.$refs['evalData'].resetFields();
            }
        },
        setEvalReport(response, file, fileList) {
            this.evalData.evalReport = 'ok';
            this.validForm();
        },
        setReformReport(response, file, fileList) {
            this.evalData.reformReport = 'ok';
            this.validForm();
        },
        removeEvalReport(file, fileList) {
            if(fileList.length) {
                this.evalData.evalReport = 'ok';
            } else {
                this.evalData.evalReport = '';
            }
            this.validForm();
        },
        removeReformReport(file, fileList) {
            if(fileList.length) {
                this.evalData.reformReport = 'ok';
            } else {
                this.evalData.reformReport = '';
            }
            this.validForm();
        },
        handleSubmit() {
            this.$refs['evalData'].validate((valid) => {
                if(valid) {
                    this.save();
                } else {
                    this.$Message.error('表单验证失败!');
                }
            })
        },
        validForm() {
            this.$refs['evalData'].validate();
        },
    }
}
</script>

<style lang="less" scoped>
.modalBtn{
    margin: 0 10%;
    width: 80%;
}
.download,.download:hover{
    width: 100px;
    height: 32px;
    line-height: 30px;
    color: #000;
    float: left;
}
</style>
