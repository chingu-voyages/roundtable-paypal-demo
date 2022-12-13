<script>
  import { loadScript } from "@paypal/paypal-js"
  import Header from '../components/header.svelte'

  let quantity = 0
  let unitPrice = 34.99
  let totalCost = 0
  let resultData

  let isPaymentVisible = false
  let isPaymentSuccessful = false

  const handleCheckout = () => {
    totalCost = quantity * unitPrice
    isPaymentVisible = true

    loadScript({ 
        "client-id": `${ import.meta.env.VITE_PAYPAL_CLIENT_ID }`, 
        "disable-funding": "paylater"
      }).then((paypal) => {
        paypal
          .Buttons({
            style: {
              color: "blue",
              shape: "rect",
              label: "paypal",
              layout: "vertical"
            },
            onInit: function(data, actions) {
              // Set the z-index of the iframe injected by Paypal so it won't
              // overlay the top nav bar
              const iframes = document.getElementsByTagName("iframe")
              if (iframes.length === 0) {
                throw new Error("PayPal iframe not found")
              }
              iframes[0].style.zIndex = 5
            },
            createOrder: function (data, actions) {
              // Set up the transaction
              console.log('Create order: ', data)

              // Process the payment if no errors were detected
              return actions.order.create({
                purchase_units: [
                  {
                    amount: {
                      value: totalCost,
                    },
                  },
                ],
              })
            },
            onApprove: function (data, actions) {
              // Capture order after payment approved
              resultData = data
              return actions.order.capture().then(function (details) {
                resultDetails = details
                console.log("Captured order: ", details)
                isPaymentSuccessful = true
              })
            },
            
            onError: function (err) {
              // Log error if something goes wrong during approval
              console.log("Something went wrong", err)
            },
          })
          .render("#paypal-button-container")
        })
  }
</script>

<Header/>

<section class="pt-20 ml-6 mr-8">
  <h2 class="mt-4 text-2xl font-bold">How to integrate PayPal REST in your apps</h2>
  <p class="mt-4 text-xl">
    This app was built to provide a test platform to show how to integrate PayPal 
    payments into your applications. It utilizes the PayPal REST API and 
    `@paypal/paypal-js` to add payment options to a checkout form.

    This will be referenced in an upcoming weekly Chingu Roundtable in our
    <a class="underline italic text-blue-500" target="_blank noreferrer" 
      href="https://www.youtube.com/playlist?list=PLmnPfbM4_HyszkLRkLtlUgoMCHer3jpg7">YouTube channel.</a>
  </p>
</section>

<!-- Mock Checkout Form -->
<section class="ml-6 mr-8">
  <h2 class="mt-4 text-2xl font-bold">Mock Checkout Form</h2>
  <div class="mt-4 ml-8 text-lg">
    <span class="font-bold">Item:</span>
    <span>Left handed Monkey wrench</span>
    <div class="mt-4">
      <label for="quantity">
        Quantity:
        <input class="border w-12" id="quantity" name="quantity" type="number"
        bind:value={ quantity } on:input={ quantity }>
      </label>
      <label for="unitPrice">
        Unit price:
        <input class="border w-16" id="unitPrice" name="unitPrice" type="number"
        bind:value={ unitPrice } disabled>
      </label>
      <div class="mt-4">
        Total cost: { totalCost }
      </div>
      <div class="flex flex-col items-center mt-4 w-1/6 bg-green-500 border rounded-lg">
        <button class="font-bold" on:click={ handleCheckout }>Checkout</button>
      </div>
    </div>
  </div>

  {#if isPaymentVisible}
    <div class="flex flex-col items-center bg-white">
      <div id="paypal-button-container" />
    </div>
  {/if}
</section>