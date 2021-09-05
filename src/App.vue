<template>
  <main id="todolist">
    <h1>Todo List <span>Get things done, one item at a time.</span></h1>
    <div class="filter">
      <span>Filter </span>
      <!-- <i aria-hidden="true" class="material-icons">check_box_outline_blank</i>
      <i aria-hidden="true" class="material-icons">indeterminate_check_box </i> -->
      <button class="btn-picto" type="button" @click="filterTodo()">
        <i aria-hidden="true" class="material-icons">{{ filterValue }}</i>
      </button>
    </div>
    <div class="items" v-if="items.length > 0">
      <li
        v-for="(item, index) in filteredItems"
        :key="index"
        :class="{ done: item.done }"
      >
        <span class="label">{{ item.title }}</span>
        <div class="actions">
          <button class="btn-picto" type="button" @click="doneTodo(index)">
            <i aria-hidden="true" class="material-icons">{{
              item.done ? "check_box" : "check_box_outline_blank"
            }}</i>
          </button>
          <button
            class="btn-picto"
            type="button"
            aria-label="Delete"
            title="Delete"
            @click="deleteTodo(index)"
          >
            <i aria-hidden="true" class="material-icons">delete</i>
          </button>
        </div>
      </li>
    </div>

    <p class="emptylist" v-else>Your todo list is empty.</p>

    <form @submit.prevent="addNewTodo">
      <label for="newitem">Add to the todo list</label>
      <input type="text" v-model="newTodoTitle" name="newitem" id="newitem" />
      <button type="submit">Add item</button>
    </form>
  </main>
</template>
<script>
export default {
  data() {
    return {
      newTodoTitle: "",
      filter: 0,
      items: [],
      filteredItems: [],
    };
  },
  methods: {
    addNewTodo() {
      if (this.newTodoTitle === "") {
        return;
      } else {
        this.items.push({
          title: this.newTodoTitle,
          done: false,
        });
        this.newTodoTitle = "";
      }
    },
    deleteTodo(todo) {
      this.items.splice(todo, 1);
    },
    doneTodo(index) {
      this.items[index].done = !this.items[index].done;
    },
    filterTodo() {
      this.filter++;
      if (this.filter === 0) {
        this.filteredItems = this.items;
      }
      if (this.filter === 1) {
        this.filteredItems = this.items.filter((t) => t.done === false);
      } else if (this.filter === 2) {
        this.filteredItems = this.items.filter((t) => t.done === true);
      } else if (this.filter === 3) {
        this.filteredItems = this.items;
        this.filter = 0;
      }
    },
  },
  mounted() {
    const todos = localStorage.getItem("todos");
    const items = JSON.parse(todos ? todos : "[]");
    this.items = items;
    this.filteredItems = items;
  },
  watch: {
    items: {
      deep: true,
      handler() {
        const items = JSON.stringify(this.items);
        localStorage.setItem("todos", items);
      },
    },
  },
  computed: {
    filterValue() {
      return this.filter === 0
        ? "check_box_outline_blank"
        : this.filter === 1
        ? "check_box"
        : "indeterminate_check_box";
    },
  },
};
</script>
<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
html,
body {
  background: #f7f1f1;
  font-size: 1.1rem;
  font-family: "Quicksand", sans-serif;
  height: 100%;
}
@keyframes strikeitem {
  to {
    width: calc(100% + 1rem);
  }
}
#todolist {
  margin: 4rem auto;
  padding: 2rem 3rem 3rem;
  max-width: 500px;
  background: #ff6666;
  color: #fff;
  box-shadow: -20px -20px 0px 0px rgba(100, 100, 100, 0.1);
}
#todolist h1 {
  /*text-align:center;*/
  font-weight: normal;
  font-size: 2.6rem;
  letter-spacing: 0.05em;
  border-bottom: 1px solid rgba(255, 255, 255, 0.3);
}
#todolist h1 span {
  display: block;
  font-size: 0.8rem;
  margin-bottom: 0.7rem;
  margin-left: 3px;
  margin-top: 0.2rem;
}
#todolist .emptylist {
  margin-top: 2.6rem;
  text-align: center;
  letter-spacing: 0.05em;
  font-style: italic;
  opacity: 0.8;
}
#todolist ul {
  margin-top: 2.6rem;
  list-style: none;
}
#todolist .todolist-move {
  transition: transform 1s;
}
#todolist li {
  display: flex;
  margin: 0 -3rem 4px;
  padding: 1.1rem 3rem;
  justify-content: space-between;
  align-items: center;
  background: rgba(255, 255, 255, 0.1);
}
#todolist .actions {
  flex-shrink: 0;
  padding-left: 0.7em;
}
#todolist .label {
  position: relative;
  transition: opacity 0.2s linear;
}
#todolist .done .label {
  opacity: 0.6;
}
#todolist .done .label:before {
  content: "";
  position: absolute;
  top: 50%;
  left: -0.5rem;
  display: block;
  width: 0%;
  height: 1px;
  background: #fff;
  animation: strikeitem 0.3s ease-out 0s forwards;
}
#todolist .btn-picto {
  border: none;
  background: none;
  -webkit-appearance: none;
  cursor: pointer;
  color: #fff;
}
/* FORM */
form {
  margin-top: 3rem;
  display: flex;
  flex-wrap: wrap;
}
form label {
  min-width: 100%;
  margin-bottom: 0.5rem;
  font-size: 1.3rem;
}
form input {
  flex-grow: 1;
  border: none;
  background: #f7f1f1;
  padding: 0 1.5em;
  font-size: initial;
}
form button {
  padding: 0 1.3rem;
  border: none;
  background: #ff6666;
  color: white;
  text-transform: uppercase;
  font-weight: bold;
  border: 1px solid rgba(255, 255, 255, 0.3);
  margin-left: 5px;
  cursor: pointer;
  transition: background 0.2s ease-out;
}
form button:hover {
  background: #ff5e5e;
}
form input,
form button {
  font-family: "Quicksand", sans-serif;
  height: 3rem;
}
.filter {
  display: flex;
  align-items: center;
}
.filter button {
  margin-left: auto;
}
</style>
