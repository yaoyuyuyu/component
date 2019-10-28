<template>
  <div>
    <Form ref="formDynamic" :model="formDynamic" class="formDynamic" inline @keydown.native.enter ="handleSubmit('formDynamic')">
      <div class="form-title">
        <FormItem class="form-title-button">
          <Button @click="handleAdd" icon="md-add"></Button>
        </FormItem>
        <FormItem v-for="(item, index) in formTitle" :key="index" class="form-title-item">
          <label>{{item.value}}</label>
        </FormItem>
      </div>
      <div v-for="(item, index) in formDynamic.items" :key="index" v-if="item.status" class="dynamic-form">
        <FormItem class="form-item-button">
          <Button @click="handleAdd" icon="md-add" style="margin-right: 10px;"></Button>
          <Button @click="handleRemove(item, index)" icon="md-remove"></Button>
        </FormItem>
        <FormItem :prop="'items.' + index + '.relation'" :rules="formValidate.relation" class="form-item">
          <Input v-if="isEdit && editCell === 'relation' || item.relation === ''" type="text" v-model="item.relation" @on-blur="changeEidt('relation', item.relation)"></Input>
          <span v-else  @click="changeEidt('relation', item.relation)" class="form-item-span">{{item.relation}}</span>
        </FormItem>
        <FormItem :prop="'items.' + index + '.mobile'" :rules="formValidate.mobile" class="form-item">
          <Input v-if="!isEdit && editCell === 'mobile' || item.mobile === ''" type="text" v-model="item.mobile" @on-blur="changeEidt('mobile', item.mobile)"></Input>
          <span v-else @click="changeEidt('mobile', item.mobile)" class="form-item-span">{{item.mobile}}</span>
        </FormItem>
        <FormItem :prop="'items.' + index + '.age'" :rules="formValidate.age" class="form-item">
          <Input v-if="!isEdit && editCell === 'age' || item.age === ''" type="text" v-model="item.age" @on-blur="handleSubmit('formDynamic')"></Input>
          <span v-else @click="changeEidt('age', item.age)" class="form-item-span">{{item.age}}</span>
        </FormItem>
      </div>
      <!-- <FormItem class="form-button">
        <Button type="primary" @click="handleSubmit('formDynamic')">提交</Button>
        <Button @click="handleReset('formDynamic')" style="margin-left: 8px">重置</Button>
      </FormItem> -->
    </Form>
  </div>
</template>

<script>
import { validateMobile, validateAge } from '@/libs/validate'
export default {
  name: 'formDynamic',
  data () {
    return {
      formTitle: [
        {
          index: 1,
          value: '关系'
        },
        {
          index: 2,
          value: '电话'
        },
        {
          index: 3,
          value: '年龄'
        }
      ],
      isEdit: false,
      editCell: '',
      index: 1,
      formDynamic: {
        items: [
          {
            relation: '',
            mobile: '',
            age: '',
            index: 1,
            status: 1
          }
        ]
      },
      formValidate: {
        relation: [{ required: true, message: '关系不能为空', trigger: 'blur' }],
        mobile: [
          { required: true, message: '手机号不能为空', trigger: 'blur' },
          { validator: validateMobile, trigger: 'blur' }
        ],
        age: [
          { required: true, message: '年龄不能为空', trigger: 'blur' },
          { validator: validateAge, trigger: 'blur' }
        ]
      }
    }
  },
  props: {
    relations: {
      type: Array,
      default: function () { return [] }
    }
  },
  mounted () {
    this.init()
  },
  methods: {
    init () {
      if (this.relations.length === 0) {
        this.isEdit = true
      } else {
        this.formDynamic.items = this.relations
        this.isEdit = false
      }
    },
    handleSubmit (name) {
      this.$refs[name].validate((valid) => {
        if (valid) {
          this.$Message.success('Success!')
        } else {
          this.$Message.error('Fail!')
        }
      })
      this.isEdit = false
      this.editCell = ''
      this.$emit('on-change', this.formDynamic.items)
    },
    changeEidt (editCell, value) {
      if (editCell === 'relation' && this.isEdit === false) {
        this.isEdit = true
        this.editCell = editCell
      } else if (editCell === 'mobile' && this.isEdit === false) {
        this.isEdit = true
        this.editCell = editCell
      } else if (editCell === 'age' && this.isEdit === false) {
        this.isEdit = true
        this.editCell = editCell
      } else {
        this.isEdit = false
      }
    },
    handleReset (name) {
      this.$refs[name].resetFields()
    },
    handleAdd () {
      this.index++
      this.isEdit = true
      this.formDynamic.items.push({
        relation: '',
        mobile: '',
        age: '',
        index: this.index,
        status: 1
      })
    },
    handleRemove (item, index) {
      this.formDynamic.items[index].status = 0
      this.index = this.formDynamic.items.indexOf(item)
      if (index !== -1) {
        this.formDynamic.items.splice(index, 1)
      }
    }
  }
}
</script>

<style>
  .formDynamic {
    width: 400px;
  }
  .form-title {
    margin-bottom: 10px;
  }
  .form-title-button {
    width: 100px;
    text-align: center;
  }
  .form-title-item {
    width: 80px;
    text-align: center;
  }
  .dynamic-form {
    margin-bottom: 10px;
  }
  .form-item-button {
    width: 100px;
    display: inline-block;
    text-align: center;
    margin-right: 20px;
  }
  .form-item {
    width: 80px;
    display: inline-block;
    text-align: center;
  }
  .form-button {
    float: right;
    margin-right: 30px !important;
  }
</style>
