<template>
  <div>
    <v-form @submit.prevent="">
      <v-row>
        <v-col>
          <v-chip-group
            active-class="primary--text"
            column
            @change="setPrice"
          >
            <v-chip
              v-for="(amount, index) in amounts" 
              v-bind:key="index"
            >
              {{ currencyFormat(amount) }}
            </v-chip>
            <v-chip v-if="allow_custom">Custom Amount</v-chip>
          </v-chip-group>
        </v-col>
      </v-row>
      <v-row v-show="custom">
        <v-col>
          <v-text-field
            label="Custom Amount"
            type="number"
            @input="setCustom"
          ></v-text-field>
        </v-col>
      </v-row>
    </v-form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      amounts: [1500, 500, 200, 76, 35, 18, 14, 5, 3],
      allow_custom: true,
      custom: false,
    }
  },
  methods: {
    returnPrice(new_price) {
      this.$emit("price", new_price)
    },

    setPrice(chip_index) {
      if (chip_index >= this.amounts.length) return this.custom = true
      this.custom = false
      console.log(this.amounts[chip_index])
      this.returnPrice(this.amounts[chip_index])
    },

    setCustom(new_price) {
      this.returnPrice(new_price)
    },

    currencyFormat(value) {
      if (!value) return ''
      return Number(value).toLocaleString('en', {
        style: 'currency', currency: 'USD'
      })
    },
  },
}
</script>