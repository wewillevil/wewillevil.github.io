<template>
  <div>
    <el-card :header="header" shadow="never">
      <template v-if="!isEmpty">
        <el-table :data="list" row-key="id" :tree-props="{children: 'children', hasChildren: 'hasChildren'}" border>
          <el-table-column prop="name" label="Name" sortable/>
          <el-table-column prop="number" label="Phone" sortable/>
        </el-table>
      </template>
      <el-alert v-else type="info" :title="emptyTitle" :description="alertDescription" :closable="false" show-icon center/>
    </el-card>
  </div>
</template>

<script>
export default {
  name: 'ContactsTable',
  props: {
    list: {
      type: Array,
      default: () => ({})
    }
  },
  data: () => ({
    header: 'Phonebook',
    emptyTitle: 'There are no numbers yet :(',
    alertDescription: 'please add the first ! :)'
  }),
  computed: {
    isEmpty () {
      return !Object(this.list).length
    }
  }
}
</script>
