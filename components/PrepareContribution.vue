<template>
  <div>
    <PriceOptions
      :amounts="amounts"
      :allow_custom="true"
      @change="setPrice"
    />
    <ChooseRecurring
      :allow_recurring="true"
      label="Make this donation monthly"
      @change="setRecurring"
    />
    <CheckoutButton
      :price="price"
      :recurring="recurring"
    />
  </div>
</template>

<script>
export default {
  data() {
    return {
      amounts: [1500, 500, 200, 76, 35, 18, 14, 5, 3],
      allow_custom: true,
      custom: false,
      price: null,
      recurring: false,
    }
  },
  methods: {
    setPrice(new_price) {
      this.price = new_price
      // this.$emit("price", new_price)
    },

    setRecurring(is_recurring) {
      this.recurring = is_recurring
      // this.$emit("recurring", is_recurring)
    },
  },
}
</script>