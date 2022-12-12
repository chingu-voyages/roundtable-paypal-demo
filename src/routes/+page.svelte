<script>
  let quantity = 0
  let unitPrice = 34.99
  let totalCost = 0

  const handleCheckout = () => {
    totalCost = quantity * unitPrice
  }
</script>

<section class="ml-6 mr-8">
  <div class="flex flex-row h-full w-full justify-center pt-4">
    <div class="place-self-center pt-4 lg:pt-0">
      <img class="ml-20 w-16 h-16 invisible md:visible" 
        src="Chingu.jpg" alt="Chingu Logo"/>
      <h1 class="pt-4 text-3xl font-bold underline">Chingu Roundtable</h1>
    </div>
  </div>
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
</section>