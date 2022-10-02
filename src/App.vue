
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
              <span @click="markItem(item, 1)">
                <img src="../src/assets/square.svg" />
                {{item}}
              </span>
              <i @click="deleteItem(item, 1)">✘</i>
            </li>
          </ul>
        </div>


        <div id="doneList">
          <div>
            <h2>Done list</h2>
            <ul>
              <li v-for="itemDone in doneList" key="itemDone.id" >
                <span @click="markItem(itemDone, 2)">
                  <img src="../src/assets/check-square.svg" />
                  {{itemDone}}
                </span>
                <i @click="deleteItem(itemDone, 2)">✘</i>
              </li>
            </ul>
          </div>

          <p class="tip">Click an item in Peding List to mark as done</p>
        </div>
      </div>

      <button @click="cleanToDoList">Clean list</button>
    </main>

  </div>
</template>


<script>
  import ItemList from './components/ItemList.vue';

export default {
  components: {
    ItemList
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
      /* display: flex;
      flex-direction: column; */

      #pendingList {
        min-height: 250px;
      }

      #doneList {
        min-height: 200px;
        display: flex;
        flex-direction: column;
        justify-content: space-between;
        
        span {
          text-decoration: line-through;
        }

        .tip {
          color: gray;
          font-size: small;
        }
      }

      ul {
        padding: 1rem;
        text-align: start;

        li {
        color: black;
        display: flex;
        justify-content: space-between;
        margin: 0.2rem 0;
        overflow:auto;

          span {
            font-weight: 600;
          }
        
          i{  
            color: red;
          }

          i:hover {
            cursor: pointer;
          }
        }

        li:hover {
          background-color: rgb(175, 175, 175, 0.2);
          cursor: pointer
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
