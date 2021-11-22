<template>
  <div>
    <div>
      <v-btn
        to="/"
        text
      ><v-icon>mdi-chevron-left</v-icon>Home</v-btn>
    </div>
    <h1>Thank You</h1>
    <p>Name: {{ name }}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      name: "",
    }
  },
  mounted() {
    this.loadSession()
  },
  methods: {
    async loadSession() {
      const session_id = this.$route.params.session_id || this.$route.query.session_id
      if (!session_id) throw "We have no session ID"
      const url = `${process.env.CF_WORKER_URL}/customer`
      const options = {
        method: "GET",
        headers: {
          "Content-Type": "application/json",
          "X-Stripe-Session-ID": session_id
        },
      }
      const response = await fetch(url, options)
      const results = await response.json()
      this.name = results.customer_name
    }
  },
}
</script>