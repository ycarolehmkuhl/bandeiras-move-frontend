<template>
  <button v-on:click="$emit('clicked')" class="btn">{{txt}}</button>
</template>

<script>
export default {
  name: "Button",
    props: ['txt']
};
</script>

<style scoped>
.btn {
    transition: 0.2s;
    cursor: pointer;
    padding: 10px 20px;
    border: 1px solid #ffffff;
    border-radius: 10px;
    background-color: #6D2080;
    color: rgb(255, 255, 255);
}

.btn:hover {
    cursor: pointer;
    background-color: #9c2fb8;
}

</style>
