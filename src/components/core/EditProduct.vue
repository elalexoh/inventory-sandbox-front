<template>
  <button class="btn" title="Crear Nuevo producto">Nuevo</button>
</template>

<script>
export default {}
</script>

<style lang="scss">
.btn {
  // height: 32px;
  // width: 32px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: orange;
  padding: 5px 10px;
  font-weight: bold;
  color: white;
  font-size: 0.75rem;
  border-radius: 5px;
  transition: all 0.5s ease;
  border: 0;
  &:hover {
    color: orange;
    background-color: white;
  }
}
</style>