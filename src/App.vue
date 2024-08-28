<template>
  <div id="app">
    <UserSelection v-if="!selectedUser" @user-selected="selectUser" />
    <ChatWindow
      v-else
      :selectedUser="selectedUser"
      :currentUser="currentUser"
      @back-to-users="deselectUser"
    />
  </div>
</template>

<script>
import UserSelection from "./components/UserSelection.vue";
import ChatWindow from "./components/ChatWindow.vue";

export default {
  data() {
    return {
      selectedUser: null,
      currentUser: { name: "User1" },
    };
  },
  components: {
    UserSelection,
    ChatWindow,
  },
  methods: {
    selectUser(user) {
      this.selectedUser = user;
    },
    deselectUser() {
      this.selectedUser = null;
    },
  },
};
</script>
