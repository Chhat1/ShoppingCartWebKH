<template>
  <footer class="footer mt-5">

    <div class="container py-5">

      <div class="row g-4">

        <!-- Brand -->
        <div class="col-12 col-md-4">
          <h4 class="footer-logo">🛍️ ProductKH</h4>
          <p class="footer-text">
            Your trusted online shop for modern products. 
            Fast delivery, secure payment, and best quality.
          </p>
        </div>

        <!-- Links -->
        <div class="col-6 col-md-2">
          <h6 class="footer-title">Shop</h6>
          <ul class="footer-links">
            <li><a href="#">All Products</a></li>
            <li><a href="#">New Arrivals</a></li>
            <li><a href="#">Best Sellers</a></li>
            <li><a href="#">Discount</a></li>
          </ul>
        </div>

        <!-- Support -->
        <div class="col-6 col-md-3">
          <h6 class="footer-title">Support</h6>
          <ul class="footer-links">
            <li><a href="#">Help Center</a></li>
            <li><a href="#">Order Tracking</a></li>
            <li><a href="#">Return Policy</a></li>
            <li><a href="#">Contact Us</a></li>
          </ul>
        </div>

        <!-- Contact -->
        <div class="col-12 col-md-3">
          <h6 class="footer-title">Contact</h6>

          <p class="footer-text mb-2">📍 Phnom Penh, Cambodia</p>
          <p class="footer-text mb-2">📞 +855 12 345 678</p>
          <p class="footer-text mb-3">✉️ support@productkh.com</p>

          <!-- Social -->
          <div class="social-icons">
            <i class="bi bi-facebook"></i>
            <i class="bi bi-instagram"></i>
            <i class="bi bi-telegram"></i>
            <i class="bi bi-tiktok"></i>
          </div>
        </div>

      </div>

      <!-- Divider -->
      <hr class="my-4">

      <!-- Bottom -->
      <div class="d-flex flex-column flex-md-row justify-content-between align-items-center">

        <p class="mb-2 mb-md-0 footer-copy">
          © 2026 ProductKH. All rights reserved.
        </p>

        <div class="footer-payments">
          💳 Visa • MasterCard • ABA • Wing
        </div>

      </div>

    </div>

  </footer>
</template>

<script setup>
// no logic needed for now
</script>

<style scoped>

/* Background */
.footer {
  background: #0f172a;
  color: #cbd5e1;
  border-top-left-radius: 30px;
  border-top-right-radius: 30px;
}

/* Logo */
.footer-logo {
  color: white;
  font-weight: bold;
}

/* Text */
.footer-text {
  font-size: 14px;
  color: #94a3b8;
}

/* Titles */
.footer-title {
  color: white;
  margin-bottom: 10px;
}

/* Links */
.footer-links {
  list-style: none;
  padding: 0;
}

.footer-links li {
  margin-bottom: 6px;
}

.footer-links a {
  text-decoration: none;
  color: #94a3b8;
  font-size: 14px;
  transition: 0.3s;
}

.footer-links a:hover {
  color: white;
  padding-left: 5px;
}

/* Social */
.social-icons i {
  font-size: 18px;
  margin-right: 10px;
  cursor: pointer;
  transition: 0.3s;
}

.social-icons i:hover {
  color: white;
  transform: translateY(-3px);
}

/* Bottom */
.footer-copy {
  font-size: 13px;
  color: #64748b;
}

.footer-payments {
  font-size: 13px;
  color: #94a3b8;
}

</style>