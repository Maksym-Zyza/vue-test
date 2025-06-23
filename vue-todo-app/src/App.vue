<template>
  <div class="container">
    <h1>{{ title }}</h1>
    <input v-model="newItem" @keyup.enter="addItem" placeholder="Add item" />
    <button @click="addItem">Add</button>

    <ul>
      <li v-for="(item, index) in filteredItems" :key="index">
        {{ item }}
        <button @click="removeItem(index)">❌</button>
      </li>
    </ul>

    <p v-if="itemCount > 5" class="warning">Too many items!</p>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      title: "Vue 3 To-Do List",
      items: [],
      newItem: "",
      filter: "",
    };
  },
  mounted() {
    console.log("App mounted!");
    const saved = localStorage.getItem("todo-items");
    if (saved) {
      this.items = JSON.parse(saved);
    }
  },
  computed: {
    itemCount() {
      return this.items.length;
    },
    filteredItems() {
      if (!this.filter) return this.items;
      return this.items.filter((item) =>
        item.toLowerCase().includes(this.filter.toLowerCase())
      );
    },
  },
  watch: {
    items: {
      handler(newVal) {
        localStorage.setItem("todo-items", JSON.stringify(newVal));
      },
      deep: true,
    },
  },
  methods: {
    addItem() {
      if (this.newItem.trim()) {
        this.items.push(this.newItem.trim());
        this.newItem = "";
      }
    },
    removeItem(index) {
      this.items.splice(index, 1);
    },
  },
};
</script>

<style scoped>
.container {
  padding: 2rem;
  font-family: sans-serif;
  max-width: 600px;
  margin: auto;
}
input {
  padding: 0.5rem;
  margin-right: 0.5rem;
}
.warning {
  color: red;
  font-weight: bold;
}
</style>
