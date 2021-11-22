<template>
  <v-row v-if="allow_recurring">
    <v-col>
      <v-checkbox
        :label="label"
        @change="setRecurring"
      ></v-checkbox>
    </v-col>
  </v-row>
</template>

<script>
export default {
  props: ["allow_recurring", "label"],
  methods: {
    setRecurring(new_is_recurring) {
      this.$emit("change", new_is_recurring)
    },
  },
}
</script>