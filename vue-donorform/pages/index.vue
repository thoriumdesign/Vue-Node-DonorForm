<template>
  <v-container>
    <v-row class="py-16">
      <v-col class="text-center">
        <PrepareContribution @price="v => price = v" />
        <StripeCheckout
          ref="checkoutRef"
          :pk="pk"
          :mode="mode"
          :line-items="items"
          :successUrl="successUrl"
          :cancelUrl="cancelUrl"
          :submitType="submit_type"
        />
        <v-btn @click="checkout">Checkout</v-btn>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import { StripeCheckout } from '@vue-stripe/vue-stripe'

export default {
  components: {
    StripeCheckout,
  },
  data() {
    return {
      pk: process.env.STRIPE_PK,
      mode: "payment",
      submit_type: "donate",
      price: null,
      items: [
        {
          price: 'price_1Jy4REKZWYBVfmCspLawINdH',
          quantity: 1,
        },
      ],
      successUrl: 'http://localhost:3030/thank-you/',
      cancelUrl: 'http://localhost:3030/canceled/',
    }
  },
  methods: {
    checkout () {
      this.preparePrice().then(() => {
        this.$refs.checkoutRef.redirectToCheckout()
      })
    },

    async preparePrice() {
      const url = process.env.NODE_SERVER_URL
      const options = {
        method: "POST",
        headers: {
          "Content-Type": "application/json"
        },
        body: JSON.stringify({price: this.price})
      }
      const price_id = "price_1Jy4REKZWYBVfmCspLawINdH"
      this.items = [{ price: price_id, quantity: 1 }]
    },
  },
}
</script>