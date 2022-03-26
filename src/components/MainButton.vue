<template>
    <button class="MainButton">
      {{ time }}
    </button>
</template>

<script>
export default {
    props: ['time']
}
</script>

<style>
.MainButton {
  background-color: #fff;
  height: 40%;
  width: 100%;
  border-radius: 15px;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 2rem;
}
</style>