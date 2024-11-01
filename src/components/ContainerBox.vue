<script>
export default {
  name: "ContainerBox"
}
</script>

<template>
  <main class="mt-5 container-fluid w-50 border border-secondary-subtle rounded-3 d-flex flex-column justify-content-center
         align-items-center gap-5 shadow p-5 bg-body-tertiary">
    <slot />
  </main>
</template>

<style scoped>

</style>