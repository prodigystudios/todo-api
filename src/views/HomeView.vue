<template>
  <div>
    <input id="inputId" v-on:keydown.enter="saveTodo()" class="input-field" type="text"
      placeholder="Vad behöver du göra?" v-model="title" />
  </div>
  <div>
    <HelloWorld :key="todoKey" />
  </div>
</template>

<script>
import HelloWorld from '@/components/HelloWorld.vue'
export default {
  name: 'HomeView',

  components: {
    HelloWorld
  },
  data() {
    return {
      title: '',
      todoKey: 0
    }
  },
  methods: {
    saveTodo() {
      const todo = {
        title: this.title
      }
      fetch('http://localhost:5262/api/Posts', {
        method: 'POST',
        body: JSON.stringify(todo),
        headers: { 'content-type': 'application/json' },
      })
        .then(() => {
          this.title = ''
          this.todoKey++
        })
    },
  }
}
</script>
<style scoped>
.input-field
{
  margin-top: 150px;
  width: 20%;
  padding-bottom: 20px;
  padding-top: 6px;
  padding-left: 10px;
  border-radius: 4px;
  box-shadow: 3px 4px;
}

.input-field:focus
{
  outline: none;
}
</style>
