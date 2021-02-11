<template>
  <main class="index">
    <div class="container">
      <div class="content">
        <div class="form-wrap">
          <form id="payment-form" @submit="submitPaymentForm">
            <input
              type="hidden"
              name="payment_method_token"
              id="payment_method_token"
            />
            <div class="form-container">
              <label class="form-label t-title" for="full_name">Name</label>
              <div class="input-wrap">
                <input id="full_name" class="form-input" type="text" v-model="formData.full_name" placeholder="Full Name" />
              </div>
            </div>
            <div class="form-container">
              <label class="form-label t-title">Credit Card Number</label>
              <div class="input-wrap">
                <div id="billsby-number" class="form-input custom-input"></div>
              </div>
            </div>
            <div class="form-container">
              <label class="form-label t-title" htmlFor="month">Expiration Date</label>
              <div class="input-wrap dual-form">
                <input type="number" class="form-input" name="month" v-model="formData.month" maxLength={2} placeholder="XX" />
                <input type="year" class="form-input" v-model="formData.year" name="year" maxLength={4} placeholder="XXXX" />
              </div>
            </div>
            <div class="form-container">
              <label class="form-label t-title">CVV</label>
              <div class="input-wrap">
                <div id="billsby-cvv" class="form-input custom-input"></div>
              </div>
            </div>
            <div class="reminder-wrap">
              <p class="t-text">For test accounts, use <span>4111111111111111</span> with any future dated expiry date and any three digit CVV. The tokenizer does not validate cards.</p>
            </div>
            <input id="submit-button" class="btn-orange w-100" type="submit" value="Submit and tokenize" disabled />
          </form>
        </div>
      </div>
    </div>
    <div class="console-wrap">
      <div class="console-header">
        <h6 class="t-title">Console</h6>
      </div>
      <div class="console-body">
        <div v-if="dataSample.length !== 0">
          <vue-json-pretty :path="'res'" :data="{ key: dataSample }" @change="submitPaymentForm"> </vue-json-pretty>
        </div>
      </div>
    </div>
  </main>
</template>

<script>
  import VueJsonPretty from 'vue-json-pretty';
  import 'vue-json-pretty/lib/styles.css';

  export default {
    name: "Index",
    components: {
      VueJsonPretty,
    },
    mounted() {
      this.billsbyTokens = window.billsbyTokens;
      this.billsbyTokens.on('paymentMethod', this.eventListerPaymentMethod);
      this.billsbyTokens.init("billsby-number", "billsby-cvv");
      this.billsbyTokens.on("ready", function () {
        const submitButton = document.getElementById("submit-button");
        submitButton.disabled = false;
      });
      this.billsbyTokens.on("errors", function (errors) {
        for (var i = 0; i < errors.length; i++) {
          errors[i];
        }
      });
    },
    beforeDestroy() {
      if (!this.billsbyTokens) return; // ignore if billsby from data is null
      this.billsbyTokens.off('paymentMethod');
    },
    data() {
      return {
        billsbyTokens: null,
        formData: {
          full_name: '',
          month: '',
          year: '',
        },
        dataSample: ''
      }
    },
    methods: {
      submitPaymentForm(event) {
        event.preventDefault();
        const { full_name, month, year } = this.formData;
        const requiredField = {
          full_name,
          month,
          year
        }
        this.billsbyTokens.tokenizeCreditCard(requiredField);
      },
      eventListerPaymentMethod(token, pmData) {
        this.dataSample = pmData
        // JSON.stringify(JSON.parse(this.dataSample), null, 2)
      }
    },
  }
</script>
