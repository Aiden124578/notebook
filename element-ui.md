# Element-ui

基于Vue的第三方组件框架



## 栅格布局

- 分为24列

- el-row为行 ，属性:gutter="20"，行与行之间的距离
- el-clo为列，属性:span="6"，列所占的多少



## 表单

- el-form    :model="addForm"  表单数据    ，  :rules=""

- prop表单验证与rules结合使用，放在el-form-item
- ref表单实例对象，放在el-form，用于表单重置或者提交表单时校验是否正确
  - 调用this.$refs.表单实例对象的名字.resetFields()
- rules表单校验，放在el-form



## 表格

- el-table  :data数据    stripe  border
  - el-table-column    prop数据字段的名字   label表格列名字   type=“index”增加索引列



## 作用域插槽

- template   slot-scope=“scope”
- {{scope.row}}可以拿到当前行的数据



## token

项目中除了登录之外的其他API接口，必须在登录后才能访问，将token保存到客户端

- localStorage为持久化的存储
- sessionStorage回话式的存储，网站打开期间生效

```javascript
window.sessionStorage.setItem("token",this.data.token)
//清除token
window.sessionStorage.clear()
```



## 导航跳转页面

this.$router.push('/home')



## 菜单

- el-menu
  - unique-opened
  - collapse-transition
  - router路由跳转
  - default-active点击链接高亮效果



## Tooltip提示

文字提示，:enterable="false"



## switch开关

change事件拿状态值，状态改变时触发