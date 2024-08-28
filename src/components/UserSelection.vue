<template>
  <div class="user-selection-container">
    <h2 class="header">Select a User</h2>
    <ul class="user-list">
      <li
        v-for="user in users"
        :key="user.id"
        @click="selectUser(user)"
        class="user-item"
      >
        {{ user.name }}
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      users: [
        { id: 1, name: "User1" },
        { id: 2, name: "User2" },
        { id: 3, name: "User3" },
        { id: 4, name: "User4" },
      ],
    };
  },
  methods: {
    selectUser(user) {
      this.$emit("user-selected", user);
    },
  },
};
</script>

<style scoped>
.user-selection-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
  background-color: #f5f5f5;
  padding: 20px;
  border-radius: 10px;
}

.header {
  font-size: 24px;
  margin-bottom: 20px;
  color: #0088cc;
}

.user-list {
  list-style-type: none;
  padding: 0;
  width: 100%;
  max-width: 300px;
}

.user-item {
  background-color: #fff;
  border: 1px solid #ddd;
  padding: 10px 15px;
  margin-bottom: 10px;
  border-radius: 5px;
  cursor: pointer;
  text-align: center;
  transition: background-color 0.3s, transform 0.3s;
}

.user-item:hover {
  background-color: #0088cc;
  color: #fff;
  transform: translateY(-2px);
}

.user-item:active {
  background-color: #0077b3;
  transform: translateY(0);
}
</style>
