<template>
  <div class="order-success">
    <div class="order-success__wrapper">
      <p>您已完成付款，以下為本次訂購資訊，已同步寄送至您的信箱。</p>

      <SubscribeSuccessOrderInfo
        :orderInfo="orderInfo"
        :orderInfoPurchasedList="orderInfoPurchasedList"
        :customerInfo="customerInfo"
      />

      <UiSubscribeInfo type="papermag" :infoList="infoList" />
    </div>
  </div>
</template>

<script>
import UiSubscribeInfo from '~/components/UiSubscribeInfo.vue'
import SubscribeSuccessOrderInfo from '~/components/SubscribeSuccessOrderInfo.vue'
export default {
  components: {
    UiSubscribeInfo,
    SubscribeSuccessOrderInfo,
  },
  props: {
    orderInfo: {
      type: Object,
      isRequired: true,
      default: () => {
        return {}
      },
    },
    orderInfoPurchasedList: {
      type: Array,
      default: () => [],
    },
    customerInfo: {
      type: Object,
      default: () => {
        return {
          pur_name: '',
          pur_mail: '',
          pur_cell: '',
          rec_name: '',
          rec_cell: '',
          rec_addr: '',
        }
      },
    },
  },
  data() {
    return {
      infoList: [
        {
          id: 0,
          text: '完成訂購後，開始寄送期數將由服務人員提供。',
        },
        {
          id: 1,
          text:
            '雜誌每週三出刊，將於週四(含)前完成寄送，若當週五仍未收到，請來電 02-6633-3882 查詢。',
        },
        {
          id: 2,
          text:
            '若需更改收件地址，請於每週四中午 12:00 前來電，我們將於下週寄送至新址。',
        },
        {
          id: 3,
          text:
            '訂單成立後，會收到我們 email 給您的確認信。手機號碼及電子信箱請務必正確填寫。',
        },
        {
          id: 4,
          text: '雜誌的取消/退訂注意事項',
          child: [
            {
              id: 0,
              text:
                '雜誌未配送前可全額退款。雜誌如已開始配送，退費金額=原訂金額-(零售價 $75 x 訂閱已配送期數)-原訂金額之 5% 手續費',
            },
            {
              id: 1,
              text: '註1：訂閱已配送期數包含已進入配送作業流程的期數。',
            },
            {
              id: 2,
              text: '如有疑問請來電 02-6633-3882 詢問。',
            },
          ],
        },
      ],
    }
  },
}
</script>

<style lang="scss" scoped>
.order-success {
  position: relative;
  padding: 13px 8px;
  @include media-breakpoint-up(sm) {
    padding: 18px 8px;
  }
  &__wrapper {
    max-width: 990px;
    margin: auto;
    p {
      padding: 0 17px;
      margin-bottom: 13px;
      font-size: 18px;
      line-height: 1.87;
      text-align: justify;
      color: #4a4a4a;
      @include media-breakpoint-up(sm) {
        padding: 0 25px;
        margin-bottom: 18px;
      }
    }
    .order-info {
      margin-bottom: 18px;
      @include media-breakpoint-up(sm) {
        margin-bottom: 36px;
      }
    }
  }
}
</style>
