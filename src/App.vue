<template>
  <div id="app">
    <h1>
      <a href="https://github.com/xuliangzhan/vue-element-extends">Editable</a>
    </h1>
    <div class="apimenu-header">
      <div class="apimenu-row">
        <a class="apimenu-title">手动模式</a>
        <router-link class="apimenu-link" :to="{name: 'EditableManual1'}" replace>行编辑</router-link>
        <router-link class="apimenu-link" :to="{name: 'EditableManual2'}" replace>行编辑+校验</router-link>
      </div>
      <div class="apimenu-row">
        <a class="apimenu-title">单击模式</a>
        <router-link class="apimenu-link" :to="{name: 'EditableClick1'}" replace>列编辑</router-link>
        <router-link class="apimenu-link" :to="{name: 'EditableClick2'}" replace>行样式+自定义渲染+分页</router-link>
        <router-link class="apimenu-link" :to="{name: 'EditableClick3'}" replace>行编辑+自定义渲染+排序</router-link>
        <router-link class="apimenu-link" :to="{name: 'EditableClick4'}" replace>多级表头+校验</router-link>
        <router-link class="apimenu-link" :to="{name: 'EditableClick5'}" replace>动态列渲染</router-link>
        <router-link class="apimenu-link" :to="{name: 'EditableClick6'}" replace>动态列渲染+自定义列</router-link>
        <router-link class="apimenu-link" :to="{name: 'EditableClick7'}" replace>表尾合计行</router-link>
        <router-link class="apimenu-link" :to="{name: 'EditableClick8'}" replace>合并行或列</router-link>
        <router-link class="apimenu-link" :to="{name: 'EditableClick9'}" replace>基本校验</router-link>
        <router-link class="apimenu-link" :to="{name: 'EditableClick10'}" replace>行编辑+排序+禁用行编辑</router-link>
        <router-link class="apimenu-link" :to="{name: 'EditableClick11'}" replace>列编辑+排序+禁用列编辑+唯一下拉选项</router-link>
      </div>
      <div class="apimenu-row">
        <a class="apimenu-title">双击模式</a>
        <router-link class="apimenu-link" :to="{name: 'EditableDblclick1'}" replace>列编辑</router-link>
        <router-link class="apimenu-link" :to="{name: 'EditableDblclick2'}" replace>行编辑+自定义渲染+筛选+分页</router-link>
        <router-link class="apimenu-link" :to="{name: 'EditableDblclick3'}" replace>个性化列+自定义渲染+自定义校验</router-link>
        <router-link class="apimenu-link" :to="{name: 'EditableDblclick4'}" replace>个性化列+唯一下拉选项+完整校验</router-link>
        <router-link class="apimenu-link" :to="{name: 'EditableDblclick5'}" replace>个性化列+自定义列头+完整校验</router-link>
      </div>
      <div class="apimenu-row">
        <a class="apimenu-title">Excel表格</a>
        <router-link class="apimenu-link" :to="{name: 'EditableExcel1'}" replace>带状态</router-link>
        <router-link class="apimenu-link" :to="{name: 'EditableExcel2'}" replace>排序</router-link>
        <router-link class="apimenu-link" :to="{name: 'EditableExcel3'}" replace>校验+排序+筛选</router-link>
        <router-link class="apimenu-link" :to="{name: 'EditableExcel4'}" replace>校验+排序+筛选+个性化列</router-link>
      </div>
    </div>
    <router-view/>
  </div>
</template>

<script>
export default {
  name: 'App',
  computed: {
    activeName: {
      get () {
        return this.$route.name
      },
      set (value) {
        this.$router.replace({name: value})
      }
    }
  }
}
</script>

<style>
#app {
  font-family: "Helvetica Neue",Helvetica,"PingFang SC","Hiragino Sans GB","Microsoft YaHei","微软雅黑",Arial,sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
.apimenu-header {
  margin-bottom: 10px;
}
.apimenu-title {
  font-weight: 700;
  font-size: 14px;
}
.apimenu-link {
  margin: 2px;
  padding: 2px 4px;
  color: #606266;
  border: 1px solid #dcdfe6;
  display: inline-block;
  font-size: 12px;
  border-radius: 2px;
  text-decoration: none;
}
.apimenu-link:hover {
  color: #409eff;
  border-color: #c6e2ff;
  background-color: #ecf5ff;
}
.apimenu-link.router-link-exact-active,
.apimenu-link.router-link-exact-active:hover {
  color: #fff;
  background-color: #409eff;
  border-color: #409eff;
}
</style>
