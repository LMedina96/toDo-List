<template>
  <div id="body">
    <main>
      <h1>toDo List</h1>

      <input
        type="text"
        v-model="toDoItem"
        v-on:keyup.enter="addToDo"
        placeholder="Add a pending topic"
      />
      <button @click="addToDo">ToDo</button>

      <div id="list">
        <div id="pendingList">
          <h2>Pending list</h2>
          <ul>
            <div class="listItem">
              <li v-for="item in list" key="item.id">
                <div class="itemText">
                  <span @click="markItem(item, 1)">
                    <img src="../src/assets/square.svg" />
                    {{ item }}
                  </span>
                </div>
                <div class="itemIcons">
                  <img
                    src="../src/assets/grid-gap.svg"
                    alt="icon-for-choose-colors"
                    class="iconColors"
                    @click="openColorPalette"
                  />
                  <i @click="deleteItem(item, 1)">✘</i>
                </div>
              </li>
            </div>
          </ul>
        </div>

        <div id="doneList">
          <div>
            <h2>Done list</h2>
            <ul>
              <div class="listItem">
                <li v-for="itemDone in doneList" key="itemDone.id">
                  <span @click="markItem(itemDone, 2)">
                    <img
                      src="../src/assets/check-square.svg"
                      alt="checkBox-Unchecked"
                    />
                    {{ itemDone }}
                  </span>
                  <div class="itemIcons">
                    <img
                      src="../src/assets/grid-gap.svg"
                      alt="icon-for-choose-colors"
                      class="iconColors"
                      @click="openColorPalette"
                    />
                    <i @click="deleteItem(itemDone, 2)">✘</i>
                  </div>
                  
                </li>
              </div>
            </ul>
          </div>
          
          <p class="tip">Click an item in Peding List to mark as done</p>
        </div>
      </div>
      
      <button @click="cleanToDoList">Clean list</button>
    </main>

    <div class="colorPalette" v-if="isVisible">
      <i @click="closeColorPalette">X</i>
      <ul>
        <li v-for="button in buttons" key="button.id">
          <button :style="button.color" @click="setColor(button.color)"></button>
        </li>
      </ul>
    </div>

  </div>
</template>


<script>

export default {

  data() {
    return {
      listStorage: window.localStorage,
      list: JSON.parse(localStorage.getItem("pendingListStore")) || [],
      doneList: JSON.parse(localStorage.getItem("doneListStore")) || [],
      toDoItem: "",
      buttons: [
        {
          id: 1,
          color: "background-color: red",
        },
        {
          id: 2,
          color: "background-color: blue",
        },
        {
          id: 3,
          color: "background-color: yellow",
        },
        {
          id: 4,
          color: "background-color: green",
        },
        {
          id: 5,
          color: "background-color: orange",
        }
      ],

      isVisible: false,

    };
  },

  methods: {
    addToDo() {
      if (this.toDoItem != "") {
        this.list.push(this.toDoItem);
        this.toDoItem = "";
        this.listStorage.setItem("pendingListStore", JSON.stringify(this.list));
      }
    },

    markItem(item, category) {
      if (category == 1) {
        this.list = this.list.filter((itemList) => itemList !== item);
        this.doneList.push(item);
        this.listStorage.setItem("pendingListStore", JSON.stringify(this.list));
        this.listStorage.setItem(
          "doneListStore",
          JSON.stringify(this.doneList)
        );
      } else if (category == 2) {
        this.doneList = this.doneList.filter((itemList) => itemList !== item);
        this.list.push(item);
        this.listStorage.setItem("pendingListStore", JSON.stringify(this.list));
        this.listStorage.setItem(
          "doneListStore",
          JSON.stringify(this.doneList)
        );
      }
    },

    cleanToDoList() {
      this.listStorage.clear();
      this.list = [];
      this.doneList = [];
      this.toDoItem = "";
    },

    deleteItem(item, category) {
      if (category == 1) {
        this.list = this.list.filter((itemList) => itemList !== item);
        this.listStorage.setItem("pendingListStore", JSON.stringify(this.list));
      } else if (category == 2) {
        this.doneList = this.doneList.filter((itemList) => itemList !== item);
        this.listStorage.setItem(
          "doneListStore",
          JSON.stringify(this.doneList)
        );
      }
    },

    openColorPalette() {
      this.isVisible = true
    },

    closeColorPalette() {
      this.isVisible = false
    },

    setColor (color) {
      console.log("Seteando: ", color)
    }
  },
};
</script>

<style lang="scss" scoped>

main {
  min-width: 350px;
  min-height: 600px;
  background-color: white;
  padding: 1rem;
  border-radius: 1rem;
}

#body {
  display: flex;
  justify-content: center;
  flex-direction: column;
  align-items: center;
  text-align: center;
  color: black;
  
  h1 {
    margin: 1rem;
    background-color: rgb(230, 230, 230);
  }

  h2 {
    font-size: large;
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
    span {
      display: flex;

      img {
        margin-right: 0.5rem;
      }
    }

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
    .listItem {
      display: flex;
      flex-direction: column;
    }

    .itemIcons { 
      display: flex;

      .iconColors {
        display: none;
      }
    }

    ul {
      padding: 1rem 0.5rem;
      text-align: start;

      li {
        color: black;
        display: flex;
        justify-content: space-between;
        margin: 0.2rem 0;
        overflow: auto;

        span {
          font-weight: 600;
        }

        i {
          color: red;
        }

        i:hover {
          cursor: pointer;
        }
      }

      li:hover {
        background-color: rgb(175, 175, 175, 0.2);
        cursor: pointer;

        .iconColors {
        display: flex;
        }
      }
    }

  }
}
.colorPalette {
  display: flex;
  background-color: white;
  flex-direction: column;
  align-items: flex-end;
  padding: 0.5rem;
  border-radius: 3px;

  ul {
    list-style: none;
    padding: 0;
  }

  button{
    width: 100px;
    display: block;
    border-radius: 5px;
  }

  i:hover {
    cursor: pointer;
  }
}


</style>
