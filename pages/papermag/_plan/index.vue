<template>
  <div class="subscribe-magazine-page">
    <SubscribeStepProgress :currentStep="2" />

    <SubscribeForm
      v-if="orderStatus === 'normal' || orderStatus === 'loading'"
      :currentChoosedPlanId="currentChoosedPlanId"
      :proceedOrderPayment="proceedOrderPayment"
      :validateOn="validateOn"
    />

    <!-- loading mask -->
    <div
      v-if="orderStatus === 'loading'"
      class="subscribe-magazine-page__loading"
    >
      <div class="subscribe-magazine-page__loading_icon">
        <img :src="require('~/assets/loading.gif')" alt="" />
      </div>
    </div>
  </div>
</template>

<script>
import SubscribeStepProgress from '~/components/SubscribeStepProgress.vue'
import SubscribeForm from '~/components/SubscribeForm.vue'

export default {
  components: {
    SubscribeStepProgress,
    SubscribeForm,
  },
  middleware({ route, redirect }) {
    if (
      process.server &&
      route.params.plan !== '1' &&
      route.params.plan !== '2'
    ) {
      redirect(`/papermag`)
    }
  },
  data() {
    return {
      orderStatus: 'normal', // normal, loading
      validateOn: true,
    }
  },
  computed: {
    currentChoosedPlanId() {
      const plan = this.$route.params.plan
      return parseInt(plan) - 1
    },
  },
  methods: {
    async proceedOrderPayment(orderPayload) {
      // save orderInfo for successPage
      this.$store.dispatch('subscribe/updateOrderInfo', orderPayload)
      this.orderStatus = 'loading'

      try {
        const paymentPayload = await this.$axios.$post(
          `/api/v2/subscribe-magazine/payload`,
          orderPayload
        )

        const infoPayload = {
          JwtToken: paymentPayload.JwtToken,
          MerchantOrderNo: paymentPayload.MerchantOrderNo,
        }

        this.$store.dispatch('subscribe/updatePaymentPayload', paymentPayload)
        this.$store.dispatch('subscribe/updateInfoPayload', infoPayload)

        /*
         * save paymentPayload to store
         * then jump to redirect page
         */
        this.$store.dispatch('subscribe/updateReadyToPay', true)

        this.$router.push(`/papermag/redirect`)
      } catch (err) {
        console.error(err)

        // payment fail
        this.$store.dispatch('subscribe/updateResultStatus', 'order-fail')
        this.$router.push(`/papermag/result`)
      }
    },

    setValidateOn() {
      this.validateOn = !this.validateOn
    },
  },
}
</script>

<style lang="scss" scoped>
.subscribe-magazine-page {
  position: relative;
  &__loading {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.6);
    display: flex;
    justify-content: center;
    align-items: center;
    &_icon {
      width: 100px;
      height: 100px;
      img {
        width: 100%;
        height: 100%;
        object-fit: cover;
      }
    }
  }
}
</style>
