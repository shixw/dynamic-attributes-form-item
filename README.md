# 基于VUE+ElementUI的添加动态属性的表单组件

> 此组件为在使用VUE+ElementUI开发项目过程中总结出一个动态属性添加的表单组件,可以直接在依赖ElementUI开发的项目中使用

 [![vue2](https://img.shields.io/badge/vue-2.x-brightgreen.svg)](https://vuejs.org/)


## [示例地址]（https://shixw.github.io/dynamic-attributes-form-item/demo/)



### 示例



### 使用方式

1. 需首先安装elementUI依赖
2. 安装

```bash
npm i dynamic-attributes-form-item
```

3. 示例代码

```vue
  <template>
    <h1>动态属性表单组件</h1>
    <div class='container'>
      <el-form :model="form" ref="createForm" label-width="100px" class="demo-dynamic">
        <dynamic-attributes-form-item
          title="添加动态属性"
          v-model="form.values"
          attribute-name="动态属性"
          :attribute-options="extOptions"
          v-on:all-attributes-added="doSom"></dynamic-attributes-form-item>
      </el-form>
      <el-button @click="submitForm">提交</el-button>
    </div>
  </template>

<script>

    new Vue({
        el: '#app',
        data: {
            form: {
                values: [
                    {
                        code: 'attribute1',
                        value: '属性1value'
                    },
                    {
                        code: 'attribute2',
                        value: '2019-01-01'
                    }
                ]
            },
            extOptions: [
                {
                    code: 'attribute1',
                    name: '测试属性1',
                    type: 'String'
                },
                {
                    code: 'attribute2',
                    name: '测试属性2',
                    type: 'Date'
                },
                {
                    code: 'attribute3',
                    name: '测试属性3',
                    type: 'DateTime'
                },
                {
                    code: 'attribute4',
                    name: '测试属性4',
                    type: 'Select',
                    options: [
                        {
                            key: '1',
                            value: '1',
                            label: 'tt-1'
                        },
                        {
                            key: '2',
                            value: '2',
                            label: 'tt-2'
                        }
                    ]
                }
            ]
        },
        methods: {
            doSom(){
                this.$notify({
                    title: '提醒',
                    message: '没有可以添加的',
                    type: 'warning',
                    duration: 2000
                })
            },
            submitForm() {
                this.$refs['createForm'].validate((valid) => {
                    if (valid) {
                        this.$notify({
                            title: '提醒',
                            message: '验证通过，可以提交表单',
                            type: 'success',
                            duration: 2000
                        })
                    }
                })
            }
        },
    })
</script>
```



### 参数说明

| 属性             | 描述                     | 类型   | 默认值   |
| ---------------- | ------------------------ | ------ | -------- |
| title            | 标题                     | String | 动态属性 |
| values           | 绑定值                   | Array  | []       |
| attributeName    | 属性的名称               | String | 属性     |
| attributeOptions | 动态属性可选择值（必填） | Array  | []       |

