<template>
  <div class="dynamic-attribute">
    <div class="dynamic-attribute-header">
      <el-row>
        <el-col :span="9" style="padding-left: 20px"><i class="el-icon-edit"></i> <span>{{title}}</span></el-col>
        <el-col :span="13">&nbsp;</el-col>
        <el-col :span="2" style="text-align: center">
          <el-tooltip :content="'添加'+attributeName" effect="dark" placement="bottom">
            <el-button type="success" icon="el-icon-plus" circle size="mini" @click="addDynamicAttributeForm"></el-button>
          </el-tooltip>
        </el-col>
      </el-row>
    </div>
    <div class="dynamic-attribute-body">
      <el-row
        v-for="(value, index) in values"
        :key="value.key">
        <el-col :span="9">
          <el-form-item
            :label="attributeName"
            :label-width="labelWidth"
            :prop="valuesName+'.' + index + '.code'"
            :rules="{ required: true, message: '请选择'+attributeName, trigger: 'change' }"
          >
            <el-select v-model="value.code" :placeholder="'请选择'+attributeName" value-key="code" @change="attributeChange($event,value)">
              <el-option v-for="item in attributeOptions" :key="item.code" :label="item.name" :value="item.code" :disabled="item.disabled"/>
            </el-select>
          </el-form-item>
        </el-col>
        <el-col :span="13">
          <el-form-item
            label="值"
            label-width="50px"
            :prop="valuesName+'.' + index + '.value'"
            :rules="{ required: true, message: '请填写'+attributeName+'值', trigger: 'blur' }"
          >
            <el-input v-if="isStringType(value.code)" v-model="value.value" :placeholder="'请填写'+attributeName+'值'"></el-input>
            <el-date-picker v-if="isDateType(value.code)"  v-model="value.value" type="date" value-format="yyyy-MM-dd"/>
            <el-date-picker v-if="isDateTimeType(value.code)"  v-model="value.value" type="datetime" value-format="yyyy-MM-dd HH:mm:ss"/>
            <el-select v-if="isSelect(value.code)" v-model="value.value" :placeholder="'请选择'+attributeName+'值'">
              <el-option v-for="item in getSelectAttributeOptionsByCode(value.code)" :key="item.key" :label="item.label" :value="item.value" />
            </el-select>
          </el-form-item>
        </el-col>
        <el-col :span="2" style="text-align: center">
          <el-button size="mini" type="danger" icon="el-icon-delete" circle @click.prevent="removeDynamicAttributeFromValues(value)"></el-button>
        </el-col>
      </el-row>
    </div>
  </div>
</template>

<script>
    /*
     * 动态KEY-VALUE属性添加组件
     */
    export default {
        name: 'DynamicAttributesFormItem',
        model: {
            prop: 'values',
            event: 'change'
        },
        props: {
            title: {
                default: '动态属性',
                type: String,
                required: false
            },
            values: {
                type: Array,
                default: function() {
                    return []
                }
            },
            attributeName: {
                type: String,
                default: '属性'
            },
            attributeOptions: {
                type: Array,
                required: true,
                default: function() {
                    return []
                }
            },
            valuesName: {
                type: String,
                default: 'values'
            },
            labelWidth: {
                type: String
            }
        },
        data() {
            return {
                // internalValues: this.values
            }
        },
        mounted() {
            if (!this.values) {
                this.values = []
            }
            this.changeAttributeOptionsDisabled()
        },
        methods: {
            addDynamicAttributeForm() {
                if (this.values.length === this.attributeOptions.length) {
                    this.$emit('all-attributes-added')
                    return
                }
                this.values.push({
                    code: undefined,
                    value: undefined,
                    key: Date.now()
                })
            },
            attributeChange(choseValue, value) {
                this.changeAttributeOptionsDisabled()
                value.value = undefined
            },
            changeAttributeOptionsDisabled() {
                this.attributeOptions.forEach((option, index) => {
                    const choseOption = this.values.some(value => {
                        return value.code === option.code
                    })
                    if (choseOption) {
                        option.disabled = true
                    } else {
                        option.disabled = false
                    }
                })
            },
            removeDynamicAttributeFromValues(value) {
                const index = this.values.indexOf(value)
                if (index !== -1) {
                    this.values.splice(index, 1)
                }
                this.changeAttributeOptionsDisabled()
            },
            isStringType(code) {
                const option = this.getAttributeOptionByCode(code)
                return option && option.type === 'String'
            },
            isDateType(code) {
                const option = this.getAttributeOptionByCode(code)
                return option && option.type === 'Date'
            },
            isDateTimeType(code) {
                const option = this.getAttributeOptionByCode(code)
                return option && option.type === 'DateTime'
            },
            isSelect(code) {
                const option = this.getAttributeOptionByCode(code)
                return option && option.type === 'Select'
            },
            getAttributeOptionByCode(code) {
                return this.attributeOptions.find((option) => {
                    return option.code === code
                })
            },
            getSelectAttributeOptionsByCode(code) {
                const option = this.getAttributeOptionByCode(code)
                return option ? option.options : []
            }
        }
    }
</script>

<style scoped>
  .dynamic-attribute .dynamic-attribute-header{
      border-bottom: 1px solid #dfe6ec;
      padding-bottom: 2px;
      margin-bottom: 10px;
      font-weight: bold;
  }
</style>
