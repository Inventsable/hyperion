<template>
  <div class="fixed-center text-center">
    <p>
      <sadface />
    </p>
    <p class="text-faded">Sorry, nothing here...<strong>(404)</strong></p>
    <q-btn
      color="secondary"
      style="width:200px;"
      :to="{ name: 'Home' }"
      label="Go back"
    />
  </div>
</template>

<script>
export default {
  name: "Error404",
  components: {
    sadface: require("src/assets/sadLogo.vue").default
  }
};
</script>
