<template>
  <div v-loading="loading">
    <p style="color: red;font-size: 12px;">如果存在校验不通过的列则不允许新增</p>
    <p style="color: red;font-size: 12px;">name字段（校验必填，校验3-10个字符）nickname字段（校验5-20个字符）sex字段（校验必填，校验手机号码）age字段（校验必填，自定义校验，18-28之间）phone字段（校验必填，校验手机号码）rate字段（校验必填，校验最少选中2颗星）url（校验必填，校验URL路径）attr1（校验数字）attr2（校验整数）attr3（校验小数）</p>
    <p style="color: red;font-size: 12px;">自定义列头：attr4字段、attr5字段</p>

    <p>
      <el-button type="success" size="mini" @click="insertEvent">新增</el-button>
      <el-button type="danger" size="mini" @click="$refs.editable.removeSelecteds()">删除选中</el-button>
      <el-button type="info" size="mini" @click="$refs.editable.revert()">放弃更改</el-button>
      <el-button type="info" size="mini" @click="$refs.editable.clear()">清空数据</el-button>
      <el-button type="warning" size="mini" @click="validEvent">校验</el-button>
      <el-button type="warning" size="mini" @click="submitEvent">校验&保存</el-button>
      <el-button type="primary" size="mini" @click="getInsertEvent">获取新增数据</el-button>
      <el-button type="primary" size="mini" @click="getUpdateEvent">获取已修改数据</el-button>
      <el-button type="primary" size="mini" @click="getRemoveEvent">获取已删除数据</el-button>
      <el-button type="primary" size="mini" @click="getAllEvent">获取所有数据</el-button>
    </p>

    <el-editable
      ref="editable"
      class="my-table11"
      stripe
      border
      size="medium"
      height="480"
      :editRules="validRules" :editConfig="{trigger: 'dblclick', showIcon: false, showStatus: false}"
      style="width: 100%">
      <el-editable-column type="index" width="55">
        <template slot="head">
          <i class="el-icon-setting" @click="dialogVisible = true"></i>
        </template>
      </el-editable-column>
      <template v-for="(item, index) in columnConfigs">
        <template v-if="item.customShow">
          <el-editable-column v-if="index === 0" :key="index" v-bind="item">
            <template slot="head" slot-scope="scope">
              <i class="editable-required-icon"></i>
              <i class="el-icon-edit editable-header-icon"></i>
              {{ scope.column.label }}
            </template>
            <template slot="valid" slot-scope="scope">
              <span class="error-msg">自定义校验提示语的样式：<br>{{ scope.rule.message }}<br>名称为必填字段<br><a href="https://github.com/xuliangzhan/vue-element-extends" target="_blank">参考API说明</a></span>
            </template>
          </el-editable-column>
          <el-editable-column v-else-if="item.prop === 'attr4'" :key="index" v-bind="item">
            <template slot="head" slot-scope="scope">
              <i class="editable-required-icon"></i>
              {{ scope.column.label }}
              <el-tooltip class="item" placement="top">
                <div slot="content">说明：<br>attr4字段为自定义列头<br>校验必填字段</div>
                <i class="el-icon-question"></i>
              </el-tooltip>
            </template>
          </el-editable-column>
          <el-editable-column v-else-if="item.prop === 'attr5'" :key="index" v-bind="item">
            <template slot="head" slot-scope="scope">
              {{ scope.column.label }}
              <el-tooltip class="item" content="说明：attr5字段为自定义列头" placement="top">
                <i class="el-icon-warning"></i>
              </el-tooltip>
            </template>
          </el-editable-column>
          <el-editable-column v-else :key="index" v-bind="item">
            <template slot="head" slot-scope="scope">
              <i class="el-icon-edit editable-header-icon"></i>
              {{ scope.column.label }}
            </template>
          </el-editable-column>
        </template>
      </template>
      <el-editable-column label="操作">
        <template slot-scope="scope">
          <el-button size="mini" type="danger" @click="removeEvent(scope)">删除</el-button>
        </template>
      </el-editable-column>
    </el-editable>

    <el-dialog title="自定义列" :visible.sync="dialogVisible" width="300px" @open="openCustomEvent">
      <ul>
        <li v-for="(item, index) in columnConfigs" :key="index">
          <el-checkbox v-model="item.customChecked">{{ item.label }}</el-checkbox>
        </li>
      </ul>
      <span slot="footer" class="dialog-footer">
        <el-button @click="resetCustomEvent">重 置</el-button>
        <el-button @click="dialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="saveCustomEvent">保 存</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
import XEUtils from 'xe-utils'
import { MessageBox } from 'element-ui'
import regionData from '@/common/json/editable/region.json'
import sexData from '@/common/json/editable/sex.json'
import columnsData from '@/common/json/editable/columns.json'

export default {
  data () {
    const checkAge = (rule, value, callback) => {
      if (!value) {
        return callback(new Error('年龄不能为空'))
      }
      setTimeout(() => {
        if (!Number.isInteger(value)) {
          callback(new Error('请输入数字值'))
        } else {
          if (parseInt(value) < 18 || parseInt(value) > 28) {
            callback(new Error('年龄必须在18-28之间'))
          } else {
            callback()
          }
        }
      }, 50)
    }
    const checkRate = (rule, value, callback) => {
      if (parseInt(value || 0) < 2) {
        callback(new Error('最小选择2颗星'))
      } else {
        callback()
      }
    }
    const checkAttr2 = (rule, value, callback) => {
      if (!value || XEUtils.isInteger(Number(value))) {
        callback()
      } else {
        callback(new Error('请输入整数'))
      }
    }
    const checkAttr3 = (rule, value, callback) => {
      if (!value || XEUtils.isFloat(Number(value))) {
        callback()
      } else {
        callback(new Error('请输入小数'))
      }
    }
    return {
      loading: false,
      dialogVisible: false,
      columnConfigs: [],
      sexList: [],
      regionList: [],
      validRules: {
        name: [
          { required: true, message: '名称必须填写', trigger: 'change' },
          { min: 3, max: 10, message: '名称长度在 3 到 10 个字符', trigger: 'change' }
        ],
        nickname: [
          { min: 5, max: 20, message: '名称长度在 5 到 20 个字符', trigger: 'blur' }
        ],
        sex: [
          { required: true, message: '请选择性别', trigger: 'blur' }
        ],
        age: [
          { required: true, message: '年龄必须填写', trigger: 'blur' },
          { validator: checkAge, trigger: 'blur' }
        ],
        phone: [
          { required: true, message: '手机号码必须填写', trigger: 'blur' },
          { pattern: /^1[34578]\d{9}$/, message: '请输入正确的手机号码', trigger: 'blur' }
        ],
        birthdate: [
          { message: '请选择日期', trigger: 'blur' }
        ],
        rate: [
          { required: true, message: '请选择评分', trigger: 'blur' },
          { validator: checkRate, trigger: 'blur' }
        ],
        url: [
          { required: true, message: 'URL必须填写', trigger: 'change' },
          { pattern: /^((ht|f)tps?):\/\/([\w-]+(\.[\w-]+)*\/)*[\w-]+(\.[\w-]+)*\/?(\?([\w\-.,@?^=%&:/~+#]*)+)?/, message: '格式：http(s)://xxx.com', trigger: 'change' }
        ],
        attr1: [
          { type: 'number', message: '请输入数字', trigger: 'change' }
        ],
        attr2: [
          { validator: checkAttr2, trigger: 'change' }
        ],
        attr3: [
          { validator: checkAttr3, trigger: 'change' }
        ],
        attr4: [
          { required: true, message: 'attr4必须填写', trigger: 'blur' }
        ]
      }
    }
  },
  created () {
    this.init()
  },
  methods: {
    init () {
      let sexPromise = this.getSexJSON()
      let regionPromise = this.getRegionJSON()
      this.findList()
      this.getColumnConfigs().then(data => {
        this.columnConfigs = data.map(column => {
          let defaultShow = ['name', 'nickname', 'sex', 'region', 'phone', 'rate', 'attr1', 'attr2', 'attr3', 'attr4', 'attr5'].includes(column.prop)
          let editRender = column.editRender
          column.customDefault = defaultShow
          column.customChecked = defaultShow
          column.customShow = defaultShow
          column.minWidth = '150'
          editRender.attrs = {
            placeholder: `请输入${column.label}`
          }
          switch (column.prop) {
            case 'sex':
              editRender.options = []
              sexPromise.then(rest => {
                editRender.options = rest
              })
              break
            case 'region':
              editRender.attrs = {options: []}
              regionPromise.then(rest => {
                editRender.attrs.options = rest
              })
              break
            case 'birthdate':
              editRender.attrs = {
                type: 'date',
                format: 'yyyy-MM-dd'
              }
              break
            case 'rate':
              editRender.type = 'visible'
              break
          }
          return column
        })
      })
    },
    findList () {
      this.loading = true
      this.getDataJSON().then(data => {
        this.$refs.editable.reload(data)
        this.loading = false
      }).catch(e => {
        this.loading = false
      })
    },
    insertEvent () {
      if (!this.$refs.editable.checkValid().error) {
        let row = this.$refs.editable.insert()
        // 对新增行进行校验提示
        this.$nextTick(() => this.$refs.editable.validateRow(row).catch(e => e))
      }
    },
    removeEvent (scope) {
      MessageBox.confirm('确定删除该数据?', '温馨提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        this.$refs.editable.remove(scope.row)
      }).catch(e => e)
    },
    validEvent () {
      this.$refs.editable.validate().then(valid => {
        alert('通过')
      }).catch(valid => {
        console.log('error submit!!')
      })
    },
    submitEvent () {
      this.$refs.editable.validate().then(valid => {
        let { insertRecords, removeRecords, updateRecords } = this.$refs.editable.getAllRecords()
        this.postJSON('url', { insertRecords, removeRecords, updateRecords }).then(data => {
          this.findList()
        })
      }).catch(valid => {
        console.log('error submit!!')
      })
    },
    openCustomEvent () {
      this.columnConfigs.forEach(column => {
        column.customChecked = column.customShow
      })
    },
    resetCustomEvent () {
      this.columnConfigs.forEach(column => {
        column.customChecked = column.customDefault
      })
    },
    saveCustomEvent () {
      this.dialogVisible = false
      this.columnConfigs.forEach(column => {
        column.customShow = column.customChecked
      })
    },
    getInsertEvent () {
      let rest = this.$refs.editable.getInsertRecords()
      MessageBox({ message: JSON.stringify(rest), title: `获取新增数据(${rest.length}条)` })
    },
    getUpdateEvent () {
      let rest = this.$refs.editable.getUpdateRecords()
      MessageBox({ message: JSON.stringify(rest), title: `获取已修改数据(${rest.length}条)` })
    },
    getRemoveEvent () {
      let rest = this.$refs.editable.getRemoveRecords()
      MessageBox({ message: JSON.stringify(rest), title: `获取已删除数据(${rest.length}条)` })
    },
    getSelectedEvent () {
      let rest = this.$refs.editable.getSelecteds()
      MessageBox({ message: JSON.stringify(rest), title: `获取已选中数据(${rest.length}条)` })
    },
    getAllEvent () {
      let rest = this.$refs.editable.getRecords()
      MessageBox({ message: JSON.stringify(rest), title: `获取所有数据(${rest.length}条)` })
    },
    postJSON (data) {
      return new Promise(resolve => {
        setTimeout(() => {
          resolve('保存成功')
        }, 300)
      })
    },
    getSexJSON () {
      return new Promise(resolve => {
        setTimeout(() => resolve(XEUtils.clone(sexData, true)), 100)
      })
    },
    getColumnConfigs () {
      return new Promise(resolve => {
        setTimeout(() => resolve(XEUtils.clone(columnsData, true)), 100)
      })
    },
    getDataJSON () {
      return new Promise(resolve => {
        setTimeout(() => resolve([]), 350)
      })
    },
    getRegionJSON () {
      return new Promise(resolve => {
        setTimeout(() => resolve(XEUtils.clone(regionData, true)), 200)
      })
    }
  }
}
</script>

<style scoped>
.my-table11 .error-msg {
  display: block;
  color: #fff;
  background-color: red;
  border-radius: 8px;
  font-size: 12px;
  line-height: 1;
  padding: 10px;
  position: absolute;
  top: calc(100% + 10px);
  left: 10px;
  z-index: 9;
}
.my-table11 .error-msg:before {
  content: "";
  position: absolute;
  border: 4px solid;
  top: -8px;
  left: 20%;
  border-color: transparent transparent red transparent;
}
</style>
