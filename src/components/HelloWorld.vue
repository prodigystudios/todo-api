<template>
  <ol>
    <div class="wrapper" v-for="t in todos" :key="t.id">
      <li v-if="!t.isCompleted" @click="setCompleted(t.id)" class="list-items">
        <h4 class="title-text">{{ t.title }}</h4>
        <img class="checkmark checkmark-grayscale" src="../assets/CheckMark.png" width="30" height="30"
          alt="Checkmark" />
        <button class="delete-btn" @click="deleteTodo(t.id)"><img src="../assets/cross.jpg" width="30" height="30"
            alt="cross" /></button>
      </li>
      <li v-else @click="setCompleted(t.id)" class="list-items list-items-completed">
        <h4 class="title-text">{{ t.title }}</h4>
        <img class="checkmark" src="../assets/CheckMark.png" width="30" height="30" alt="Checkmark" />
        <button class="delete-btn" @click="deleteTodo(t.id)"><img src="../assets/cross.jpg" width="30" height="30"
            alt="cross" /></button>
      </li>
    </div>
  </ol>
  <button class="deleteAll-btn" @click="deleteAll()">Ta bort alla</button>
</template>

<script>

export default {
  name: 'HelloWorld',
  data() {
    return {
      todos: [],
    }
  },
  name: 'HelloWorld',
  props: {
    msg: String
  },
  created() {
    fetch('http://localhost:5262/api/Posts')
      .then(response => response.json())
      .then(data => {
        this.todos = data
        console.log(this.todos)
      })
  },
  methods: {

    getObjFromApi() {
      fetch('http://localhost:5262/api/Posts')
        .then(response => response.json())
        .then(data => {
          this.todos = data
        })
    },

    deleteTodo(id) {
      fetch('http://localhost:5262/api/Posts/' + id, {
        method: 'DELETE'
      })
        .then(() => {
          this.getObjFromApi()
        })
    },
    deleteAll() {
      this.todos.forEach(todo => {
        fetch('http://localhost:5262/api/Posts/' + todo.id, {
          method: 'DELETE'
        })
          .then(() => {
            this.getObjFromApi()
          })
      })
    },

    setCompleted(id) {
      const singelPost = this.todos.find(todo => todo.id == id)

      if (singelPost.isCompleted == true) {
        singelPost.isCompleted = false
      } else {
        singelPost.isCompleted = true
      }

      fetch('http://localhost:5262/api/Posts/' + id, {
        method: 'PUT',
        body: JSON.stringify(singelPost),
        headers: { 'Content-Type': 'application/json' }
      })
    }
  },
}


</script>
<style scoped>
ol
{
  display: flex;
  flex-direction: column;
}

.wrapper
{
  display: list-item;
}

.list-items
{
  display: grid;
  width: 100%;
  gap: 20px;
  margin-bottom: 20px;
  grid-template-columns: 1fr;
  grid-template-rows: 1fr;
  grid-template-areas: "title-text checkmark crossmark";
  cursor: pointer;
  border: solid black;
  border-radius: 10px;
  box-shadow: 3px 4px black;
}

.list-items-completed
{
  text-decoration-line: line-through;
  text-decoration-thickness: 3px;
  text-decoration-color: black;
}

.title-text
{
  grid-area: title-text;
  display: flex;
  flex-wrap: wrap;
  width: fit-content;
  color: #42b983;
  font-size: 20px;
  padding-left: 15px;
}

.checkmark
{
  grid-area: checkmark;
  display: flex;
  justify-content: center;
  margin: auto;
}

.delete-btn
{
  grid-area: crossmark;
  display: flex;
  justify-content: right;
  padding-top: 10%;
  filter: grayscale(100);
  background-color: white;
  border: none;
  cursor: pointer;
}

.delete-btn:hover
{
  filter: grayscale(0);
}

.deleteAll-btn
{
  background: #42b983;
  color: white;
  border: none;
  cursor: pointer;
  border-radius: 10px;
  width: 300px;
  padding: 20px;
  font-size: 18px;
  box-shadow: 3px 4px black;
}

.deleteAll-btn:hover
{
  opacity: .75;
}

.checkmark-grayscale
{
  filter: grayscale(100);
}

::marker
{
  font-size: 30px;
}
</style>