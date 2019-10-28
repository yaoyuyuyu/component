<style scoped>
.expand-row {
  margin-bottom: 16px;
  z-index: 999;
  position: relative;
}

.expand-input {
  width: 200px;
}

.item-span {
  padding-right: 20px;
}
</style>
<template>
    <div>
      <Row class="expand-row">
        <Col span="6">
          <div>
            <span>昵称: </span>
            <Input v-model="editNickName" v-if="edit && editItem === 'nickName'" class="expand-input" @on-blur="handleVerify()" @on-enter="handleVerify()"/>
            <span v-else @click="showInput(row, 'nickName')" class="item-span">{{ row.nickName }}</span>
          </div>
        </Col>
        <Col span="6">
          <span>个人简介: </span>
          <Input v-model="editDescription" v-if="edit && editItem === 'description'" class="expand-input" @on-blur="handleVerify()" @on-enter="handleVerify()"/>
          <span v-else @click="showInput(row, 'description')" class="item-span">{{ row.description }}</span>
        </Col>
        <Col span="6">
          <span>所在省市: </span>
          <Cascader :data="data" v-model="editAddress" :transfer="true" v-if="edit && editItem === 'address'" @on-change="handleChangeAddress" @visible-change="edit = false" filterable clearable style="width:230px;display: inline-block;"></Cascader>
          <span v-else @click="showInput(row, 'address')">{{ row.address }}</span>
        </Col>
        <Col span="6">
          <span>评论: </span>
          <Input v-model="editRemark" v-if="edit && editItem === 'remark'" class="expand-input" @on-blur="handleVerify()" @on-enter="handleVerify()"/>
          <span v-else @click="showInput(row, 'remark')" class="item-span">{{ row.remark }}</span>
        </Col>
      </Row>
      <Row class="expand-row">
        <Col span="6">
          <span>创建者: </span>
          <Input v-model="editCreatedBy" v-if="edit && editItem === 'createdBy'" class="expand-input" @on-blur="handleVerify()" @on-enter="handleVerify()"/>
          <span v-else @click="showInput(row, 'createdBy')" class="item-span">{{ row.createdBy }}</span>
        </Col>
      </Row>
    </div>
</template>
<script>
import { apiUserSave } from '@/api/index'
import { citys } from './citys'
export default {
  name: 'expandRow',
  props: {
    row: Object,
    index: Number
  },
  data () {
    return {
      editNickName: this.row.nickName,
      editDescription: this.row.description,
      editRemark: this.row.remark,
      editCreatedBy: this.row.createdBy,
      editIndex: '',
      data: citys,
      editAddress: [],
      edit: false,
      editItem: ''
    }
  },
  created () {
    this.init()
  },
  methods: {
    init () {
      this.editAddress = this.row.address.split('/')
    },
    showInput (row, editItem) {
      this.edit = true
      this.editItem = editItem
    },
    handleChangeAddress (value, selectedData) {
      this.row.address = value.join('/')
      // apiUserSave(this.row).then(res => {
      //   if (parseInt(res.status) === 200 && parseInt(res.data.code) === 200) {
      //     this.$Message.success(res.data.message)
      //   } else {
      //     this.$Message.error(res.data.message)
      //   }
      // })
      this.edit = false
    },
    handleVerify () {
      this.row.nickName = this.editNickName
      this.row.description = this.editDescription
      this.row.remark = this.editRemark
      this.row.createdBy = this.editCreatedBy
      // apiUserSave(this.row).then(res => {
      //   if (parseInt(res.status) === 200 && parseInt(res.data.code) === 200) {
      //     this.$Message.success(res.data.message)
      //   } else {
      //     this.$Message.error(res.data.message)
      //   }
      // })
      this.edit = false
    }
  }
}
</script>
