
<template>
  <div id="body">
    
    <main>
      <h1>toDo List</h1>
      

      <input type="text" v-model="toDoItem" v-on:keyup.enter="addToDo" placeholder="Add a pending topic">
      <button @click="addToDo">ToDo</button>

      <div id="list">
        <div id="pendingList">
          <h2>Pending list</h2>
          <ul>
            <li v-for="item in list" key="item.id">
              <span @click="markItem(item, 1)">{{item}}</span>
              <i @click="deleteItem(item, 1)">✘</i>
            </li>
          </ul>

        </div>


        <div id="doneList">
          <h2>Done list</h2>
          <ul>
            <li v-for="itemDone in doneList" key="itemDone.id">
              <span @click="markItem(itemDone, 2)">{{itemDone}}</span>
              <i @click="deleteItem(itemDone, 2)">✘</i>
            </li>
          </ul>
        </div>
      </div>

      <button v-if="doneList.length > 0 || list.length > 0" @click="cleanToDoList">Clean list</button>
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
      list: JSON.parse(localStorage.getItem('pendingListStore')) || [],
      doneList: JSON.parse(localStorage.getItem('doneListStore')) || [],
      toDoItem: "",
    };
    
  },

  methods: {
    addToDo () {
      if (this.toDoItem != "") {
        this.list.push(this.toDoItem)
        this.toDoItem = ""
        this.listStorage.setItem("pendingListStore", JSON.stringify(this.list))
      }
    },

    markItem (item, category) {
      if (category == 1) {
        this.list = this.list.filter((itemList) => itemList !== item)
        this.doneList.push(item)
        this.listStorage.setItem("pendingListStore", JSON.stringify(this.list))
        this.listStorage.setItem("doneListStore", JSON.stringify(this.doneList))
      } else if (category == 2) { 
        this.doneList = this.doneList.filter((itemList) => itemList !== item)
        this.list.push(item)
        this.listStorage.setItem("pendingListStore", JSON.stringify(this.list))
        this.listStorage.setItem("doneListStore", JSON.stringify(this.doneList))
      }
    },

    cleanToDoList () {
      this.listStorage.clear()
      this.list = []
      this.doneList = []
      this.toDoItem = ""
    },

    deleteItem (item, category) {
      if(category == 1) {
        this.list = this.list.filter((itemList) => itemList !== item)
        this.listStorage.setItem("pendingListStore", JSON.stringify(this.list))
      } else if (category == 2) {
        this.doneList = this.doneList.filter((itemList) => itemList !== item)
        this.listStorage.setItem("doneListStore", JSON.stringify(this.doneList))
      }
    }

  }

}
</script>

<style lang="scss" scoped>

  main {
    border-radius: 1rem;
  }

  #body {
    display: flex;
    justify-content: center;
    text-align: center;
    color: black;

    h1 {
      margin: 1rem;
    }

    h2 {
      font-size:large;
      text-align: left;
      text-decoration-line: underline;
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

      #pendingList {
        min-height: 250px;
      }

      #doneList {

        min-height: 200px;
        
        span {
          text-decoration: line-through;
        }
      }

      ul{
        padding: 1rem;
        text-align: start;

        li {
        color: black;
        display: flex;
        justify-content: space-between;
        
          i{  
            color: red;
          }

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
