<script>
import { mapActions, mapWritableState, mapState } from "pinia";
import axios from "axios";
import { useCounterStore } from "../stores/counter";
import CartRow from "../components/CartRow.vue";
import Swal from "sweetalert2";

export default {
  components: {
    CartRow,
  },

  methods: {
    ...mapActions(useCounterStore, ["fetchCart", "addOrderHandler"]),

    async midtrans() {
      try {
        const { data } = await axios.post(
          this.baseUrl + "/midtrans",
          {
            amount: this.totalPrice,
          },
          {
            headers: {
              access_token: localStorage.access_token,
            },
          }
        );

        const cb = this.addOrderHandler;
        await window.snap.pay(data.transactionToken, {
          onSuccess() {
            cb();
          },
          onClose() {
            Swal.fire({
              icon: "error",
              title: "Need Payment!",
              timer: 1500,
              showConfirmButton: false,
            });
          },
        });
      } catch (error) {
        Swal.fire({
          icon: "error",
          title: "Your Cart is Empty!",
          timer: 1500,
          showConfirmButton: false,
        });
      }
    },
  },
  computed: {
    ...mapWritableState(useCounterStore, ["carts", "totalPrice", "baseUrl"]),
  },
  created() {
    this.totalPrice = 0;
    this.fetchCart();
  },
};
</script>
<template>
  <section>
    <div class="flex max-h-full w-full pt-6">
      <div class="flex-1 bg-hero bg-cover bg-center bg-no-repeat">
        <div
          class="max-w-screen-xl px-4 py-8 mx-auto sm:px-6 sm:py-12 lg:px-8 bg-white bg-opacity-90"
        >
          <div class="max-w-3xl mx-auto">
            <header class="text-center">
              <h1 class="text-xl font-bold text-gray-900 sm:text-3xl">
                Your Cart
              </h1>
            </header>

            <div class="mt-8">
              <CartRow v-for="cart in carts" :key="cart.id" :cart="cart" />
              <div class="flex justify-end pt-8 mt-8 border-t border-gray-100">
                <div class="w-screen max-w-lg space-y-4">
                  <dl class="space-y-0.5 text-sm text-gray-700">
                    <div class="flex justify-between !text-base font-medium">
                      <dt>Total</dt>
                      <dd>Rp.{{ Number(totalPrice).toLocaleString() }},-</dd>
                    </div>
                  </dl>

                  <div class="flex justify-end">
                    <button
                      @click="midtrans"
                      href=""
                      class="block px-5 py-3 text-sm text-gray-100 font-medium transition bg-[#b08968] rounded hover:bg-[#7f5539]"
                    >
                      Checkout
                    </button>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>
