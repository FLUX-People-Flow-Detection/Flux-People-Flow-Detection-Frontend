<template>
  <div>
    <nav>
      <button @click="logout">Logout</button>
    </nav>
    <RouterView></RouterView>
  </div>
</template>

<script>
import { onMounted, reactive } from "vue";
import { useRouter } from "vue-router";
import { useStore } from "vuex";
import axios from "axios";

export default {
  name: "App",

  setup() {
    const router = useRouter();
    const store = useStore();
    let data = reactive({ user: null, items: [] });

    const fetchData = async () => {
      try {
        const response = await axios.get("https://api.example.com/data");
        data.items = response.data;
      } catch (error) {
        console.error("Error fetching data:", error);
      }
    };

    const logout = () => {
      store.dispatch("logout");
      router.push("/login");
    };

    onMounted(() => {
      if (!store.state.isAuthenticated) {
        router.push("/login");
      } else {
        fetchData();
      }
    });

    return {
      data,
      logout,
    };
  },
};
</script>

<style scoped>
@media (max-width: 1224px) {
  nav {
    display: flex;
    justify-content: space-between;
  }
  button {
    padding: 10px;
    font-size: 16px;
  }
}
</style>
