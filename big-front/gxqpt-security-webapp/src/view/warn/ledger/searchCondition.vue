<template>
  <div>
    <Form
      ref="formValidate"
      inline
      :label-width="100"
      :model="searchCondition">
      <FormItem label="预警级别：">
        <Select v-model="searchCondition.level" style="width:200px">
          <Option v-for="item in levels" v-model="item.value" :key="item.value">{{item.label}}</Option>
        </Select>
      </FormItem>
      <FormItem label="预警类型：">
        <div class="ivu-form-item-content" @click="showTreeModal()">
          <div class="ivu-input-wrapper ivu-input-type">
            <i class="ivu-icon ivu-icon-load-c ivu-load-loop ivu-input-icon ivu-input-icon-validate"></i>
            <div class="ivu-input" style="width: 160px;line-height: 22px;">{{typeName}}</div>
          </div>
        </div>
      </FormItem>
      <FormItem label="预警标题：">
        <Input
          type="text"
          v-model="searchCondition.title">
        </Input>
      </FormItem>
      <FormItem label="应用程序名称：">
        <Input
          type="text"
          v-model="searchCondition.appName">
        </Input>
      </FormItem>
      <FormItem label="预警状态：" v-if="!excepts.includes('status')">
        <Select v-model="searchCondition.status" style="width:200px">
          <Option v-for="item in allStatus" v-model="item.value" :key="item.value">{{item.label}}</Option>
        </Select>
      </FormItem>
      <FormItem label="预警时间：">
        <DatePicker
          v-model="warntime"
          :editable="false"
          format="yyyy-MM-dd"
          type="daterange"
          style="width: 200px">
        </DatePicker>
      </FormItem>
      <FormItem :label-width="20">
        <Button type="primary" @click="preSearch">查询</Button>
      </FormItem>
    </Form>
    <Modal
      v-model="typeModal"
      title="类型选择"
      width="40%">
      <typeTree ref="typeTree" />
      <div slot="footer">
        <Button class="modalBtn" type="primary" @click="selectType" size="large">确定</Button>
        <Button class="modalBtn" type="default" @click="typeModal = false" size="large">取消</Button>
      </div>
    </Modal>
  </div>
</template>

<script>
import {levels, allStatus} from './constant'
import typeTree from './typeTree.vue'

function getDate(time) {
  if (!time) {
    return ''
  }
  // 结束日期
  const date = new Date(time)
  return `${date.getFullYear()}-${date.getMonth() + 1}-${date.getDate()}`
}

export default {
  props: {
    excepts: {
      type: Array,
      default: () => {
        return []
      }
    }
  },
  components: {
    typeTree
  },
  data () {
    return {
      // 预警级别
      levels,
      // 预警状态
      allStatus,
      // 预警时间
      warntime: [],
      // 预警搜索条件
      searchCondition: {
        level: '-1',
        title: '',
        appName: '',
        status: '-1',
        typeId: ''
      },
      // 选择类型的模态框控制
      typeModal: false,
      // 选择的类型名称
      typeName: '全部'
    }
  },
  methods: {
    // 预备搜索
    preSearch () {
      const vm = this
      const condition = vm.getCondition()
      vm.$emit('searchClick', condition)
    },
    // 打开类型选择模态框
    showTreeModal () {
      this.typeModal = true
      this.$refs.typeTree.initTreeData()
    },
    selectType () {
      const vm = this
      const selection = vm.$refs.typeTree.getSelection()
      const {id, title} = selection.length > 0 ? selection[0] : {id: '', title: ''}
      vm.searchCondition.typeId = id
      vm.typeName = title
      vm.typeModal = false
    },
    getCondition () {
      const vm = this
      const endTime = vm.warntime.length > 0 ? getDate(vm.warntime[1]) : ''
      const startTime = vm.warntime.length > 0 ? getDate(vm.warntime[0]) : ''
      const condition = {
        warntimeEnd: endTime ? `${endTime} 23:59:59` : '',
        warntimeStart: startTime ? `${startTime} 00:00:00` : '',
        ...vm.searchCondition
      }
      return condition
    }
  }
}
</script>
