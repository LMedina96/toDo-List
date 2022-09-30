
<template>
  <div id="body">
    
    <main>
      <h1>toDo List</h1>
      

      <input type="text" v-model="toDoItem">
      <button @click="addToDo">ToDo</button>

      <div id="list">
        <ul>
          <li v-for="item in list" key="item.id">
            {{item}}
            <i @click="deleteItem">✘</i>
          </li>
        </ul>
      </div>

      <button @click="cleanToDoList">Limpiar Lista</button>
    </main>

  </div>
</template>


<script>

export default {
  components: {

  },

  data() {
    return {
      listStorage: window.localStorage,
      list: JSON.parse(localStorage.getItem('Lista')) || [],
      toDoItem: "",
    };
  },

  methods: {
    addToDo () {
      if (this.toDoItem != "") {
        this.list.push(this.toDoItem)
        this.listStorage.setItem("Lista", JSON.stringify(this.list))
        this.toDoItem = ""
      }
    },

    cleanToDoList () {
      this.listStorage.clear()
      this.list = []
      this.toDoItem = ""
    },

    deleteItem () {
      console.log("HOLA MUNDO")
    }

  }

}
</script>

<style lang="scss" scoped>
  #body {
    display: flex;
    justify-content: center;
    text-align: center;

    h1 {
      color: black;
      margin: 1rem;
    }

    input {
      width: 250px;
      padding: 0.5rem;
      margin: 1rem 0;
    }

    button {
      padding: 0.5rem;
    }

    #list {
      ul{
        padding: 1rem;
        text-align: start;
        min-height: 400px;

        li {
        color: black;
        list-style: square;

          i:hover {
            cursor: pointer;
          }
        }
      }
    }
  }

  main{
    min-width: 350px;
    min-height: 600px;
    background-color: white;
    padding: 1rem;
  }
</style>
