<template>
  <v-row>
    <v-col class="text-center">
      <v-btn @click="checkout">Checkout</v-btn>
    </v-col>
  </v-row>
</template>

<script>
export default {
  props: ["price", "recurring"],

  data() {
    return {
      test_mode: false,
      successUrl: 'http://localhost:3030/thank-you?session_id={CHECKOUT_SESSION_ID}',
      cancelUrl: 'http://localhost:3030/canceled/',
    }
  },

  computed: {
    synced_price () {
      return this.price * 100 // Convert to cents
    },
    mode () {
      return this.recurring ? "subscription" : "payment"
    },
  },
  methods: {
    checkout () {
      this.preparePrice().then((url) => {
        window.location.replace(url)
      })
    },

    async preparePrice() {
      const url = `${process.env.CF_WORKER_URL}/stripe`
      const options = {
        method: "POST",
        headers: {
          "Content-Type": "application/json"
        },
        body: JSON.stringify({
          price: this.synced_price,
          mode: this.mode,
          successUrl: this.successUrl,
          cancelUrl: this.cancelUrl,
        })
      }
      const response = await fetch(url, options)
      if (response.status != 200) throw "Could not get price ID from server"
      const results = await response.json()
      const return_url = results.url
      return return_url
    },
  },
}
</script>