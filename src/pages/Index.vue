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
                <label class="form-label t-title" htmlFor="full_name">Name</label>
                <div class="input-wrap">
                  <input id="full_name" class="form-input" name="full_name" type="text" placeholder="Full Name" />
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
                  <input id="month" class="form-input" name="month" type="text" maxLength={2} placeholder="XX" />
                  <input id="year" class="form-input" name="year" type="text" maxLength={4} placeholder="XXXX" />
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
          {{dataSample}}
          <!-- <vue-json-pretty :path="'res'" :data="{ key: JSON.parse(dataSample)}" @click="handleClick"> </vue-json-pretty> -->
          <!-- <vue-json-pretty
            :path="'res'"
            :data="{ key: 'dataSample' }"
            @click="handleClick">
          </vue-json-pretty> -->
          <!-- <JSONPretty id="json-pretty" data=""></JSONPretty> -->
        </div>
      </div>
  </main>
</template>

<script>
  // import Vue from 'vue'
  // import VueJsonPretty from 'vue-json-pretty';
  // import 'vue-json-pretty/lib/styles.css';

  const billsbyTokens = window.billsbyTokens;
  let dataVal = [];

  billsbyTokens.init("billsby-number", "billsby-cvv");

  billsbyTokens.on("ready", function () {
    const submitButton = document.getElementById("submit-button");
    submitButton.disabled = false;
  });

  billsbyTokens.on("errors", function (errors) {
    for (var i = 0; i < errors.length; i++) {
      var error = errors[i];
      console.log(error);
    }
  });

  billsbyTokens.on("paymentMethod", function (token, pmData) {
    let pmDataStringify = JSON.stringify(pmData, null, 2)
    var pmDataParse = JSON.parse(pmDataStringify.replace(/n;/g,'"'))
    dataVal.push(pmDataParse)
    console.log(token);
    console.log(pmData)
  });

  console.log(dataVal)
  export default {
    name: "Index",
    data: function() {
      return {
        submitPaymentForm: function(evt) {
          evt.preventDefault();
          const requiredFields = {};
          // Get required, non-sensitive, values from host page
          requiredFields["full_name"] = document.getElementById("full_name").value;
          requiredFields["month"] = document.getElementById("month").value;
          requiredFields["year"] = document.getElementById("year").value;

          billsbyTokens.tokenizeCreditCard(requiredFields);
          console.log(dataVal)
        },
        dataSample: dataVal
      }
    },
    // components: {
    //   VueJsonPretty,
    // }
  }
</script>
