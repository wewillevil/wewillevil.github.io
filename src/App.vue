<template>
  <div id="app">
    <ContactsTable :list="list"/>
    <ModalForm :list="list" @submitForm="onFormSubmit"/>
  </div>
</template>

<script>
import ContactsTable from '@/components/ContactsTable'
import ModalForm from '@/components/ModalForm'

export default {
  name: 'App',
  components: {
    ContactsTable,
    ModalForm
  },
  data: () => ({
    list: [{
      leader: '',
      name: 'Silvia',
      number: +54321236576,
      id: 17,
      children: [{
        leader: 17,
        name: 'Joe',
        number: +87653459809,
        id: 191,
        children: []
      }]
    },
    {
      leader: '',
      name: 'Victor',
      number: +98765434560,
      id: 42,
      children: [{
        leader: 42,
        name: 'Sam',
        number: +145735643798,
        id: 202,
        children: []
      }]
    }]
  }),
  mounted () {
    if (localStorage.getItem('list')) {
      try {
        this.list = JSON.parse(localStorage.getItem('list'))
      } catch (e) {
        alert('Data was removed due to an error!')
        localStorage.removeItem('list')
      }
    }
  },
  methods: {
    onFormSubmit (data) {
      let newId = Math.floor(Math.random() * 100) + data.leader
      let newObj = {}
      newObj = {
        ...data,
        id: newId
      }
      if (!data.leader) {
        this.list.push(newObj)
        this.saveList()
      } else {
        for (let i = 0; i < this.list.length; i++) {
          if (this.list[i].id === newObj.leader) {
            this.list[i].children = [newObj]
            this.saveList()
          }
        }
      }
    },
    saveList () {
      const parsed = JSON.stringify(this.list)
      localStorage.setItem('list', parsed)
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin: auto;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  max-width: 60%;
  min-width: 300px;
}

input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}
</style>
