<script setup>
import { ref, onMounted, watch } from "vue";
import { db } from "./data/guitarsData";
import Guitar from "./components/Guitar.vue";
import Header from "./components/Header.vue";
import Footer from "./components/Footer.vue";

const guitarsState = ref([]);
const cart = ref([]);
const guitar = ref({});

watch(
  cart,
  () => {
    saveToLocalStorage();
  },
  {
    deep: true,
  }
);

onMounted(() => {
  guitarsState.value = db;
  guitar.value = db[3];

  const storedCart = localStorage.getItem("cart");
  if (storedCart) {
    cart.value = JSON.parse(storedCart);
  }
});

const saveToLocalStorage = () => {
  localStorage.setItem("cart", JSON.stringify(cart.value));
};

const addToCart = (guitar) => {
  const itemExists = cart.value.findIndex((item) => item.id === guitar.id);
  if (itemExists >= 0) {
    cart.value[itemExists].quantity++;
  } else {
    guitar.quantity = 1;
    cart.value.push(guitar);
  }
};

const increaseQty = (id) => {
  const index = cart.value.findIndex((item) => item.id === id);
  if (cart.value[index].quantity >= 5) return;
  cart.value[index].quantity++;
};
const decreaseQty = (id) => {
  const index = cart.value.findIndex((item) => item.id === id);
  if (cart.value[index].quantity <= 1) return;
  cart.value[index].quantity--;
};

const removeItem = (id) => {
  console.log(`eliminar el item con el id ${id}`);

  cart.value = cart.value.filter((item) => item.id !== id);
};

const emptyCart = () => {
  cart.value = [];
};
</script>

<template>
  <Header
    :guitar="guitar"
    :cart="cart"
    @increase-qty="increaseQty"
    @decrease-qty="decreaseQty"
    @add-to-cart="addToCart"
    @remove-item="removeItem"
    @empty-cart="emptyCart"
  />

  <main class="container-xl mt-5">
    <h2 class="text-center">Nuestra Colección</h2>

    <div class="row mt-5">
      <!-- Guitar component-->

      <Guitar
        v-for="guitar in guitarsState"
        :guitar="guitar"
        :key="guitar.id"
        @add-to-cart="addToCart"
      />
    </div>
  </main>

  <Footer />
</template>

<style lang="scss" scoped></style>
