<template>
  <footer class="pt-5 footer-main">
    <div class="container-fluid">
      <div class="row">
        <div class="col-md-5 mb-4">
          <h4 class="font-weight-bold">TENTANG</h4>
          <hr class="footer-hr">
          <p> Mi Store Official Terpercaya di Indonesia. Jual Beli Aman &amp; Harga Termurah! Belanja
            sekarang. </p>
        </div>
        <div class="col-md-3 mb-4">
          <h4 class="font-weight-bold">METODE PEMBAYARAN</h4>
          <hr class="footer-hr">
          <div class="row">
            <div class="col-md-4 col-4 mb-3" v-for="(img, idx) in paymentImages" :key="idx">
              <div class="card rounded">
                <div class="card-body p-2 text-center">
                  <img :src="img.src" :alt="img.alt" :style="{width: img.width}" loading="lazy">
                </div>
              </div>
            </div>
          </div>
        </div>
        <div class="col-md-4 mb-4">
          <h4 class="font-weight-bold">JAM OPERASIONAL</h4>
          <hr class="footer-hr">
          <p><i class="fa fa-clock"></i> Toko Buka Setiap Hari : <br><br> <strong>Senin - Jum'at</strong> ( 07.00 s/d
            19.00 ) <br> <strong>Sabtu - Minggu</strong> ( 07.00 s/d 16.00 ) </p>
        </div>
      </div>
      <div class="row text-center mt-3 pb-3">
        <div class="col-md-12">
          <hr> © <strong>MI STORE</strong> 2021 • Hak Cipta Dilindungi </div>
      </div>
    </div>
  </footer>
</template>

<script>
  export default {
    data() {
      return {
        paymentImages: [
          { src: '/images/payment/BCA.png', alt: 'BCA', width: '50px' },
          { src: '/images/payment/BNI.png', alt: 'BNI', width: '45px' },
          { src: '/images/payment/BRI.png', alt: 'BRI', width: '60px' },
          { src: '/images/payment/GOPAY.png', alt: 'GOPAY', width: '60px' },
          { src: '/images/payment/indomaret-logo.png', alt: 'Indomaret', width: '60px' },
          { src: '/images/payment/atm-bersama.jpg', alt: 'ATM Bersama', width: '40px' },
        ]
      }
    }
  }
</script>

<style scoped>
.footer-main {
  background: #fff;
  border-top: 5px solid #e64a1a;
}
.footer-hr {
  border-top: 3px solid #e2e8f0;
  border-radius: 0.5rem;
}
</style>