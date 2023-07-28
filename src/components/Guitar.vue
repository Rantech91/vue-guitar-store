<script setup>
const props = defineProps({
  guitar: {
    type: Object,
    required: true,
  },
});

const emit = defineEmits(["add-to-cart"]);
</script>

<template>
  <div class="col-md-6 col-lg-4 my-4 row align-items-center">
    <div class="col-4">
      <img
        class="img-fluid"
        :src="`/img/${props.guitar.image}.jpg`"
        :alt="`imagen guitarra ${props.guitar.name}`"
      />
    </div>
    <div class="col-8">
      <h3 class="text-black fs-4 fw-bold text-uppercase">
        {{ props.guitar.name }}
      </h3>
      <p>
        {{ props.guitar.description }}
      </p>
      <p class="fw-black text-primary fs-3">${{ props.guitar.price }}</p>
      <button
        type="button"
        class="btn btn-dark w-100"
        @click="emit('add-to-cart', props.guitar)"
      >
        Agregar al Carrito
      </button>
    </div>
  </div>
  <!-- FIN GUITARRA -->
</template>

<style lang="scss" scoped></style>
