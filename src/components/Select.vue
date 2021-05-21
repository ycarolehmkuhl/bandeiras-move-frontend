<template>
  <select
    v-on:change="onClick(selected)"
    v-model="selected"
    required
    name="name"
    :id="id"
    class="select-field"
  >
    <option v-for="item in options" :key="item.value" :value="item.value">
      {{ item.option }}
    </option>
  </select>
</template>

<script>
export default {
  name: "Select",
  props: ["name", "id", "options"],
  data: function () {
    return {
      showDefault: 1,
      selected: "",
    };
  },
  methods: {
    onClick(value) {
      this.$emit("selected", value);
    },
  },
};
</script>

<style scoped>
.select-field{
  padding: 10px;
  border: 1px solid #1f1f1f;
  border-radius: 3px;
  width: 95%;
}

.select-field:last-child{
  float: right;
}

select:required:invalid {
  color: gray;
}
option[value=""] {
  display: none;
}
option {
  color: black;
}
</style>
