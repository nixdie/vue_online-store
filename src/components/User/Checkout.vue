<template>
 <v-container>
   <v-layout row>
     <v-flex xs12 class="text-xs-center pt-5" v-if="loading">
      <v-progress-circular
        :size="100"
        :width="4"
        color="green"
        indeterminate
      ></v-progress-circular>
    </v-flex>
     <v-flex xs12 sm6 offset-sm3 v-else-if="!loading && orders.length !== 0">
       <h1 class="text-secondary mb-3">Orders</h1>
        <v-list
          subheader
          three-line
          >
            <v-list-tile
            avatar
            v-for="(order, i) in orders"
            :key="i"
            >
              <v-list-tile-action>
                <v-checkbox
                  :input-value="order.done"
                  color="success"
                  @change="markDone(order)"
                ></v-checkbox>
              </v-list-tile-action>

              <v-list-tile-content>
                <v-list-tile-title>{{ order.name }}</v-list-tile-title>
                <v-list-tile-sub-title>{{ order.phone }}</v-list-tile-sub-title>
              </v-list-tile-content>
              <v-list-tile-action>
                <v-btn
                  :to="'/product/' + order.productId"
                  class="primary ml-3"
                >Open</v-btn>
              </v-list-tile-action>
            </v-list-tile>
          </v-list>
     </v-flex>
     <v-flex xs12 class="text-xs-center" v-else>
        <h1 class="text--secondary">You have no orders.</h1>
    </v-flex>
   </v-layout>
 </v-container>
</template>

<script>
export default {
  computed: {
    loading () {
      return this.$store.getters.loading
    },
    orders () {
      return this.$store.getters.orders
    }
  },
  methods: {
    markDone (order) {
      this.$store.dispatch('markOrderDone', order.id)
      .then(() => {
        order.done = true
      })
      .catch(() => {})
    }
  },
  created () {
    this.$store.dispatch('fetchOrders')
  }

}
</script>

