<template>
  <div class="app">
    <div class="card" v-cloak>
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
          :placeholder="placeholderInputText"
          v-model="inputValueText"
          @keypress.enter="addNewNote"
          required
        />
        <input
          class="input"
          type="number"
          :placeholder="placeholderInputTime"
          v-model="inputValueTime"
          @keypress.enter="addNewNote"
          required
        />
      </div>
      <div class="buttons">
        <button class="button button__add" @click="addNewNote">Add</button>
        <button
          class="button button__filter"
          @click="filterShorts"
          v-if="notes.length !== 0"
        >
          Filter shorts
        </button>
      </div>
      <hr />
      <ul class="list" v-if="notes.length !== 0">
        <li class="list-item" v-for="(note, idx) of notes" :key="note">
          <span
            :class="{
              'list-item_bold': note.length < 6,
              'list-item_normal': note.length > 5,
            }"
          >
            {{ note }}
          </span>
          <div>
            <p>
              <button class="button button__delete" @click="deleteNote(idx)">
                Delete
              </button>
            </p>
          </div>
        </li>
        <li>
          <strong>Total amount of notes: {{ notes.length }}</strong> | Doubling
          with 1 render:
          {{ doubleCount }}
        </li>
      </ul>
      <div class="caution" v-show="notes.length === 0">
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
      placeholderInputText: "Enter note title",
      placeholderInputTime: "Enter duration",
      inputValueText: "",
      inputValueTime: "",
      editButtonIsClicked: false,
      notes: [],
    };
  },
  methods: {
    addNewNote() {
      if (this.inputValueText !== "" && this.inputValueTime !== "") {
        const data = [
          this.inputValueText + ". I will spend",
          this.inputValueTime + " min",
        ];
        const res = data.join(": ");
        this.notes.push(res);
        this.inputValueText = "";
        this.inputValueTime = "";
      }
    },
    filterShorts() {
      const res = this.notes.filter((i) => {
        const duration = i.split(": ");
        const minutes = duration[1].split(" ");
        return minutes[0] < 15;
      });
      return (this.notes = res);
    },
    deleteNote(idx) {
      this.notes.splice(idx, 1);
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

.buttons {
  display: flex;
  align-items: center;
}

.button {
  margin-bottom: 10px;
  padding: 10px 15px;
  background: none;
  color: teal;
  cursor: pointer;
  align-self: flex-start;
  margin-top: 15px;
}

.button__add {
  margin-right: 15px;
}

.button__filter {
  color: blue;
}

.button__edit {
  margin: 0 15px 0 0;
}

.button__delete {
  color: orange;
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

.caution {
  margin: 20px auto 20px;
  font-size: 14px;
  color: teal;
  justify-self: center;
}
</style>
