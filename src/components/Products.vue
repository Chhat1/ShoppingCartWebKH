<template>
  <header class="position-sticky top-0 z-3">
        <nav class="container d-flex justify-content-between align-items-center bg-primary mt-5 rounded-4 py-3 px-5">
            <h4 class="logo mb-0 text-light">ProductKH</h4>
            <div class="icon-cart position-relative">
                <i class="bi fs-3 text-light bi-bag-check-fill" style="cursor: pointer;" data-bs-toggle="offcanvas"
  data-bs-target="#cartOffcanvas"></i>
                <span class="bg-danger  position-absolute px-2 text-light rounded-5 fw-bold" style="left: 15px;">{{ cartCount }}</span>
            </div>
        </nav>
  </header>

  <!-- Loading -->
  <div class="loading d-flex justify-content-center my-3">
    <img width="100px" v-if="isLoading" src="https://cdn.pixabay.com/animation/2023/10/08/03/19/03-19-26-213_512.gif" alt="loading">
    <div v-else-if="error">{{ error }}</div>
  </div>


  <!-- Product List -->
<div class="container mt-5">
  <div class="row g-4">

    <!-- CARD -->
    <div 
      class="col-12 col-sm-6 col-md-4 col-lg-3"
      v-for="product in products" 
      :key="product.id"
    >
      <div class="card shadow-lg border-0 product-card" style="height: 500px;">

        <!-- IMAGE -->
        <div class="card-header bg-white border-0 p-3 overflow-hidden" style="height: 70%;">
          <img 
            :src="product.image" 
            class="img-fluid product-img" 
            alt="product"
          >
        </div>

        <!-- BODY -->
        <div class="card-body d-flex flex-column" style="height: 30%;">
          <h6 class="fw-bold text-truncate">
            {{ product.title }}
          </h6>

          <p class="text-primary fw-bold fs-5 mb-2">
            ${{ product.price }}
          </p>

          <!-- BUTTON -->
          <button 
            class="btn btn-dark mt-auto w-100 rounded-pill"
            @click="addToCart(product)"
          >
            🛒 Add to Cart
          </button>
        </div>

      </div>
    </div>

  </div>
</div>


<!-- Offcanvas Checkout -->
<div 
  class="offcanvas offcanvas-end" 
  tabindex="-1" 
  id="cartOffcanvas"
>

  <div class="offcanvas-header">
    <h5 class="offcanvas-title fw-bold">🛒 Your Bag</h5>
    <button type="button" class="btn-close" data-bs-dismiss="offcanvas"></button>
  </div>

  <div class="offcanvas-body d-flex flex-column">


    <div 
    v-if="showEmptyAlert" 
    class="alert alert-warning text-center py-2"
  >
    ⚠️ Your cart is empty!
  </div>



    <!-- EMPTY -->
    <div v-if="cart.length === 0" class="text-center my-auto">
      <h6 class="text-muted">Your cart is empty</h6>
    </div>

    <!-- ITEMS -->
    <div v-else class="flex-grow-1 overflow-auto">

      <div 
        v-for="item in cart" 
        :key="item.id"
        class="d-flex justify-content-between align-items-center border-bottom py-3"
      >

        <!-- LEFT -->
        <div class="d-flex gap-3">

          <img 
            :src="item.image"
            style="width:60px; height:60px; object-fit:contain;"
          >

          <div>
            <h6 class="mb-1 text-truncate" style="max-width:160px;">
              {{ item.title }}
            </h6>

            <small class="text-muted">${{ item.price }}</small>

            <!-- QTY -->
            <div class="d-flex align-items-center mt-2">
              <button 
                class="btn btn-sm btn-outline-secondary"
                @click="updateQty(item.id, -1)"
              >-</button>

              <span class="mx-2 fw-bold">{{ item.qty }}</span>

              <button 
                class="btn btn-sm btn-outline-secondary"
                @click="updateQty(item.id, 1)"
              >+</button>
            </div>
          </div>

        </div>

        <!-- RIGHT -->
        <div class="text-end">
          <h6 class="fw-bold">
            ${{ (item.price * item.qty).toFixed(2) }}
          </h6>

          <button 
            class="btn btn-sm btn-outline-danger"
            @click="removeFromCart(item.id)"
          >
            ✕
          </button>
        </div>

      </div>

    </div>

    <!-- SUMMARY (sticky bottom) -->
    <div class="border-top pt-3 mt-3">

      <div class="d-flex justify-content-between mb-2">
        <span>Subtotal</span>
        <span>${{ cartTotal.toFixed(2) }}</span>
      </div>

      <div class="d-flex justify-content-between mb-2">
        <span>Shipping</span>
        <span class="text-success">Free</span>
      </div>

      <hr>

      <div class="d-flex justify-content-between fw-bold mb-3">
        <span>Total</span>
        <span>${{ cartTotal.toFixed(2) }}</span>
      </div>

      <button  @click="handleCheckout" class="btn btn-dark w-100 rounded-pill" :disabled="cart.length === 0">
        ✅ Checkout
      </button>

    </div>

  </div>
</div>


<!-- Modal Payment -->
<div class="modal fade" id="paymentModal" tabindex="-1">
  <div class="modal-dialog modal-dialog-centered">
    <div class="modal-content rounded-4 shadow-lg border-0">

      <!-- HEADER -->
      <div class="modal-header border-0">
        <h5 class="modal-title fw-bold">💳 Checkout Payment</h5>
        <button class="btn-close shadow-none" data-bs-dismiss="modal"></button>
      </div>

      <!-- BODY -->
      <div class="modal-body pt-0">

        <!-- ORDER SUMMARY -->
        <div class="bg-light p-3 rounded-4 mb-3">
          <div class="d-flex justify-content-between">
            <span class="text-muted">Subtotal</span>
            <span>${{ cartTotal.toFixed(2) }}</span>
          </div>

          <div class="d-flex justify-content-between">
            <span class="text-muted">Shipping</span>
            <span class="text-success">Free</span>
          </div>

          <hr>

          <div class="d-flex justify-content-between fw-bold fs-5">
            <span>Total</span>
            <span class="text-primary">${{ cartTotal.toFixed(2) }}</span>
          </div>
        </div>

        <!-- PAYMENT METHODS -->
        <h6 class="fw-bold mb-2">Payment Method</h6>

        <div class="list-group mb-3">

          <label class="list-group-item d-flex align-items-center gap-2">
            <input type="radio" value="card" v-model="selectedMethod">
            💳 Credit / Debit Card
          </label>

          <label class="list-group-item d-flex align-items-center gap-2">
            <input type="radio" value="bank" v-model="selectedMethod">
            🏦 Bank Transfer
          </label>

          <label class="list-group-item d-flex align-items-center gap-2">
            <input type="radio" value="aba" v-model="selectedMethod">
            📱 ABA / Wing Pay
          </label>

          <label class="list-group-item d-flex align-items-center gap-2">
            <input type="radio" value="cod" v-model="selectedMethod">
            💵 Cash on Delivery
          </label>
          <label class="list-group-item d-flex align-items-center gap-2">
            <input type="radio" value="qr" v-model="selectedMethod">
            📷 QR Code Payment
        </label>
        </div>

        <!-- ================= FORMS ================= -->

        <!-- CARD -->
        <div v-if="selectedMethod === 'card'" class="mt-3">
          <input v-model="paymentForm.cardNumber"
                 class="form-control mb-2"
                 placeholder="Card Number">

          <div class="d-flex gap-2">
            <input v-model="paymentForm.expiry"
                   class="form-control"
                   placeholder="MM/YY">

            <input v-model="paymentForm.cvv"
                   class="form-control"
                   placeholder="CVV">
          </div>
        </div>

        <!-- BANK -->
        <div v-if="selectedMethod === 'bank'" class="mt-3">
          <input v-model="paymentForm.bankName"
                 class="form-control mb-2"
                 placeholder="Bank Name">

          <input v-model="paymentForm.accountNumber"
                 class="form-control"
                 placeholder="Account Number">
        </div>

        <!-- ABA -->
        <div v-if="selectedMethod === 'aba'" class="mt-3">
          <input v-model="paymentForm.phone"
                 class="form-control"
                 placeholder="Phone Number">
        </div>

        <!-- COD -->
        <div v-if="selectedMethod === 'cod'" class="mt-3">
          <p class="text-muted">
            You will pay when receiving the product.
          </p>
        </div>
        <div v-if="selectedMethod === 'qr'" class="mt-3 text-center">

  <p class="text-muted mb-2">
    Scan this QR code using ABA / Wing
  </p>

  <img 
    src="https://api.qrserver.com/v1/create-qr-code/?size=200x200&data=PAYMENT123"
    class="img-fluid border rounded p-2 mb-3"
  >

  <div class="form-check text-start">
    <input 
      class="form-check-input" 
      type="checkbox" 
      v-model="qrConfirmed"
    >
    <label class="form-check-label">
      I have completed the payment
    </label>
  </div>

</div>

        <!-- NOTE -->
        <small class="text-muted d-block mb-3 mt-3">
          🔒 Your payment information is secure and encrypted
        </small>

        <!-- BUTTON -->
        <button 
          class="btn btn-dark w-100 py-2 rounded-pill fw-bold"
          :disabled="isPaying || !isFormValid"
          @click="handlePayment"
        >
          <span v-if="!isPaying">
            Confirm & Pay ${{ cartTotal.toFixed(2) }}
          </span>

          <span v-else>
            <img width="30px" src="https://cdn.pixabay.com/animation/2022/07/29/03/42/03-42-11-849_512.gif">
          </span>
        </button>

      </div>

    </div>
  </div>
</div>


<!-- //SuccessModal -->

<div class="modal fade" id="successModal" tabindex="-1">
  <div class="modal-dialog modal-dialog-centered">
    <div class="modal-content text-center p-4 rounded-4 shadow-lg">

      <div class="modal-body">

        <div class="display-4 text-success mb-3">
          <i class="bi bi-check-circle-fill text-success display-1"></i>
        </div>

        <h4 class="fw-bold">Payment Successful</h4>

        <p class="text-muted">
          Your order has been placed successfully
        </p>

        <button 
          class="btn btn-success w-100 rounded-pill mt-3"
          data-bs-dismiss="modal"
        >
          OK
        </button>

      </div>

    </div>
  </div>
</div>

</template>

<script setup>
import { computed, onMounted, ref,watch } from 'vue';

    //State Management
    const products = ref([])
    const cart = ref(JSON.parse(localStorage.getItem('my_cart')) || []);
    const isLoading = ref(false)
    const error = ref(null)
    const showEmptyAlert = ref(false)
    const isPaying = ref(false)

        const selectedMethod = ref('card');

        const paymentForm = ref({
        cardNumber: '',
        expiry: '',
        cvv: '',
        bankName: '',
        accountNumber: '',
        phone: ''
        });


    const qrConfirmed = ref(false);


    //Watch
    watch(cart, (newCart) => {
        localStorage.setItem('my_cart', JSON.stringify(newCart));
    }, { deep: true });
    

    //Fetch Date From API
    const fetchProducts = async () => {
        try{
            isLoading.value = true;
            const res = await fetch("https://fakestoreapi.com/products");
            if(!res.ok) throw new Error('Failed request data!');
            products.value = await res.json();
        }catch(err){
            error.value = err.message;
        }finally{
            isLoading.value = false;
        }
    };

    //Cart Logic
    const addToCart = (product) =>{
        const item = cart.value.find((i) => i.id === product.id);
        if(item){
            item.qty++;
        }else{
            cart.value.push({...product,qty:1})
        }
    };


    //Remove From Cart
    const removeFromCart = (id) =>{
        cart.value = cart.value.filter((item)=>item.id !== id);
    };

    //Update Qty
    const updateQty = (id,amount) =>{
        const item = cart.value.find((i)=>i.id === id);
        if(item){
            item.qty +=amount;
            if(item.qty <= 0) removeFromCart(id);
        }
    };


    //Computed Properties
    const cartTotal = computed(()=>{
        return cart.value.reduce((total,item)=> total + item.price * item.qty,0);
    });

    //CartCount
    const cartCount = computed(()=>{
        return cart.value.reduce((count,item)=>count+item.qty,0)
    })

    onMounted(fetchProducts);




    const handleCheckout = () => {
            if (!cart.value.length) {
                showEmptyAlert.value = true;

                setTimeout(() => {
                showEmptyAlert.value = false;
                }, 2000);

                return;
            }

            const modal = new bootstrap.Modal(
                document.getElementById('paymentModal')
            );
            modal.show();
    };


    const handlePayment = () => {
    isPaying.value = true;

    setTimeout(() => {
        isPaying.value = false;

        const paymentModal = bootstrap.Modal.getInstance(
        document.getElementById('paymentModal')
        );
        paymentModal.hide();

        const successModal = new bootstrap.Modal(
        document.getElementById('successModal')
        );
        successModal.show();

        cart.value = [];
    }, 5000);
    };



const isFormValid = computed(() => {

  if (selectedMethod.value === 'card') {
    return paymentForm.value.cardNumber &&
           paymentForm.value.expiry &&
           paymentForm.value.cvv;
  }

  if (selectedMethod.value === 'bank') {
    return paymentForm.value.bankName &&
           paymentForm.value.accountNumber;
  }

  if (selectedMethod.value === 'aba') {
    return paymentForm.value.phone;
  }

  if (selectedMethod.value === 'qr') {
    return qrConfirmed.value;
  }

  if (selectedMethod.value === 'cod') {
    return true;
  }

  

  return false;
});


</script>

<style scoped>

</style>