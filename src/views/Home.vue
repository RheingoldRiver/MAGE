<template>
  <div> <router-link to="/login"><button v-bind:data-id="user.key" @click="login" class="button hover:bg-purple-100">Login</button></router-link> </div>
</template>

<script>
export default {
  name: "Home",
  data: () => ({ users: [], name: null, wiki: null }),
  methods: {
    login(event) {
      const userKey = event.target.dataset.id;
      window.api.remote("loginUser", userKey).then(data => {
        this.$store.state.current_user = { ...data.cacheUser, ...data.cacheSite };
      });
    },
    logout(event) {
      window.api.remote("logoutUser").then(data => {
        this.$store.state.current_user = { ...data.cacheUser, ...data.cacheSite };
      });
    },
    disconnect(event) {
      const userKey = event.target.dataset.id;
      window.api.remote("disconnectServer", userKey).then(data => {
        this.$store.state.current_user = { ...data.cacheUser, ...data.cacheSite };
      });
    }
  },
  created() {
    window.api.remote("getUserLists").then(data => {
      this.users = data;
    });
  }
};
</script>
