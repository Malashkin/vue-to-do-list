<template>
  <div class="app">
    <div class="card">
      <h1
        class="card__title"
        :style="{
          color: notes.length < 3 ? 'green' : 'orange',
        }"
      >
        {{ title
        }}{{
          notes.length > 4
            ? ". A lot of things to do. You need to do something now!"
            : ""
        }}
      </h1>
      <div class="form-control">
        <input
          class="input"
          type="text"
          :placeholder="placeholderString"
          v-model="inputValue"
          @keypress.enter="addNewNote"
        />
      </div>
      <button class="btn" @click="addNewNote">Add</button>

      <hr />
      <ul class="list" v-if="notes.length !== 0">
        <li class="list-item" v-for="(note, idx) of notes" :key="idx">
          <span
            :class="{
              'list-item_bold': note.length < 6,
              'list-item_normal': note.length > 5,
            }"
          >
            {{ toUpperCase(note) }}
          </span>
          <button class="btn-delete" @click="deleteNote(idx)">Delete</button>
        </li>
        <li>
          <strong>Total amount of notes: {{ notes.length }}</strong> | Doubling
          with 1 render:
          {{ doubleCount }}
        </li>
      </ul>

      <div class="caution" v-if="notes.length === 0">
        Notes list is empty now, use input and button "Add" / press "Enter" to
        create note.
      </div>
      <hr />
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      title: "To Do List",
      placeholderString: "Enter note title",
      inputValue: "",
      notes: [],
    };
  },
  methods: {
    addNewNote() {
      if (this.inputValue !== "") {
        this.notes.push(this.inputValue);
        this.inputValue = "";
      }
    },
    deleteNote(idx) {
      this.notes.splice(idx, 1);
    },
    toUpperCase(item) {
      const newItem = item[0].toUpperCase() + item.slice(1);
      return newItem;
    },
  },
  computed: {
    doubleCount() {
      return this.notes.length * 2;
    },
  },
  watch: {
    inputValue() {
      if (this.inputValue.length > 20) {
        this.inputValue = "";
      }
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

.app {
  padding: 20px;
}

.card {
  display: flex;
  flex-direction: column;
}

.card__title {
  font-size: 20px;
}
form {
  display: flex;
  flex-direction: column;
}

.form-control {
  margin: 0;
}
.input {
  width: 100%;
  border: 2px solid teal;
  padding: 10px 15px;
  margin-top: 15px;
}

.btn {
  margin-bottom: 10px;
  padding: 10px 15px;
  background: none;
  color: teal;
  cursor: pointer;
  align-self: flex-start;
  margin-top: 15px;
}

.list {
  list-style-type: none;
}

.list-item {
  display: flex;
  justify-content: space-between;
  padding: 10px;
  border: 1px solid teal;
  margin: 5px 0 5px;
  align-items: baseline;
}

.list-item_bold {
  font-weight: bold;
  color: green;
}

.list-item_normal {
  color: teal;
}

.btn-delete {
  padding: 10px 15px;
  background: none;
  color: teal;
  cursor: pointer;
}

.caution {
  margin: 20px auto 20px;
  font-size: 14px;
  color: teal;
  justify-self: center;
}
</style>
