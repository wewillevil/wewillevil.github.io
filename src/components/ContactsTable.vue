<template>
  <div>
    <el-card :header="header" shadow="never">
      <template v-if="!isEmpty">
        <el-table :data="list" row-key="id" :tree-props="{children: 'children', hasChildren: 'hasChildren'}" border>
          <el-table-column prop="name" label="Name" sortable/>
          <el-table-column prop="number" label="Phone" sortable/>
          <el-table-column label="Operations">
            <template slot-scope="scope">
              <el-button
                @click.native.prevent="deleteRow(scope.row, list)"
                type="text"
                size="small">
                Delete
              </el-button>
            </template>
          </el-table-column>
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
  methods: {
    deleteRow (row, list) {
      if (!row.leader) {
        for (let i = 0; i < list.length; i++) {
          if (list[i].id === row.id) {
            list.splice(i, 1)
            return
          }
        }
      }

      if (row.leader) {
        for (let j = 0; j < list.length; j++) {
          if (row.leader === list[j].id) {
            const children = list[j].children
            for (let k = 0; k < children.length; k++) {
              if (children[k].id === row.id) {
                list[j].children.splice(k, 1)
                return
              }
            }
          }
        }
      }
    }
  },
  computed: {
    isEmpty () {
      return !Object(this.list).length
    }
  }
}
</script>
