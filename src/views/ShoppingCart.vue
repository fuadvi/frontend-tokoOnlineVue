<template>
  <div class="shopping">
    <HeaderSayna />
    <!-- Breadcrumb Section Begin -->
    <div class="breacrumb-section text-left">
      <div class="container">
        <div class="row">
          <div class="col-lg-12">
            <div class="breadcrumb-text product-more">
              <router-link to="/"><i class="fa fa-home"></i> Home</router-link>
              <span>Shopping Cart</span>
            </div>
          </div>
        </div>
      </div>
    </div>
    <!-- Breadcrumb Section Begin -->

    <!-- Shopping Cart Section Begin -->
    <section class="shopping-cart spad">
      <div class="container">
        <div class="row">
          <div class="col-lg-8">
            <div class="row">
              <div class="col-lg-12">
                <div class="cart-table">
                  <table>
                    <thead>
                      <tr>
                        <th>Image</th>
                        <th class="p-name text-center">Product Name</th>
                        <th>Price</th>
                        <th>Action</th>
                      </tr>
                    </thead>
                    <tbody v-if="keranjangUser.length > 0">
                      <tr
                        v-for="keranjang in keranjangUser"
                        :key="keranjang.id"
                      >
                        <td class="cart-pic first-row">
                          <img class="card-image" :src="keranjang.photo" />
                        </td>
                        <td class="cart-title first-row text-center">
                          <h5>{{ keranjang.name }}</h5>
                        </td>
                        <td class="p-price first-row">
                          ${{ keranjang.price }}
                        </td>
                        <td class="delete-item">
                          <a @click="removeItem(keranjangUser.index)" href="#"
                            ><i class="material-icons"> close </i></a
                          >
                        </td>
                      </tr>
                    </tbody>
                  </table>
                </div>
              </div>
              <div class="col-lg-8">
                <h4 class="mb-4 text-left">Informasi Pembeli:</h4>
                <div class="user-checkout text-left">
                  <form>
                    <div class="form-group">
                      <label for="namaLengkap">Nama lengkap</label>
                      <input
                        type="text"
                        class="form-control"
                        id="namaLengkap"
                        aria-describedby="namaHelp"
                        placeholder="Masukan Nama"
                        v-model="costumerInfo.name"
                      />
                    </div>
                    <div class="form-group">
                      <label for="namaLengkap">Email Address</label>
                      <input
                        type="email"
                        class="form-control"
                        id="emailAddress"
                        aria-describedby="emailHelp"
                        placeholder="Masukan Email"
                        v-model="costumerInfo.email"
                      />
                    </div>
                    <div class="form-group">
                      <label for="namaLengkap">No. HP</label>
                      <input
                        type="text"
                        class="form-control"
                        id="noHP"
                        aria-describedby="noHPHelp"
                        placeholder="Masukan No. HP"
                        v-model="costumerInfo.number"
                      />
                    </div>
                    <div class="form-group">
                      <label for="alamatLengkap">Alamat Lengkap</label>
                      <textarea
                        class="form-control"
                        id="alamatLengkap"
                        rows="3"
                        v-model="costumerInfo.address"
                      ></textarea>
                    </div>
                  </form>
                </div>
              </div>
            </div>
          </div>
          <div class="col-lg-4">
            <div class="row">
              <div class="col-lg-12">
                <div class="proceed-checkout text-left">
                  <ul>
                    <li class="subtotal">
                      ID Transaction <span>#SH12000</span>
                    </li>
                    <li class="subtotal mt-3">
                      Subtotal <span>${{ totalharga }}.00</span>
                    </li>
                    <li class="subtotal mt-3">
                      Pajak <span>10% (${{ pajak }})</span>
                    </li>
                    <li class="subtotal mt-3">
                      Total Biaya <span>${{ totalBiaya }}.00</span>
                    </li>
                    <li class="subtotal mt-3">
                      Bank Transfer <span>Mandiri</span>
                    </li>
                    <li class="subtotal mt-3">
                      No. Rekening <span>2208 1996 1403</span>
                    </li>
                    <li class="subtotal mt-3">
                      Nama Penerima <span>Shayna</span>
                    </li>
                  </ul>
                  <router-link to="success">
                    <a
                      @click="checkout()"
                      href="success.html"
                      class="proceed-btn"
                    >
                      I ALREADY PAID
                    </a>
                  </router-link>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
    <!-- Shopping Cart Section End -->
  </div>
</template>

<script>
import HeaderSayna from "@/components/HeaderSayna.vue";
import axios from "axios";

export default {
  name: "cart",
  components: {
    HeaderSayna,
  },
  data() {
    return {
      keranjangUser: [],
      costumerInfo: {
        name: "",
        email: "",
        number: "",
        address: "",
      },
    };
  },
  methods: {
    removeItem(index) {
      this.keranjangUser.splice(index, 1);
      const parsed = JSON.stringify(this.keranjangUser);
      localStorage.setItem("keranjangUser", parsed);
    },
    checkout() {
      let productIds = this.keranjangUser.map(function (product) {
        return product.id;
      });

      let checkoutData = {
        name: this.costumerInfo.name,
        email: this.costumerInfo.email,
        number: this.costumerInfo.number,
        address: this.costumerInfo.address,
        transaction_total: this.totalBiaya,
        transaction_status: "PENDING",
        transaction_details: productIds,
      };

      axios
        .post("http://127.0.0.1:8000/api/checkout", checkoutData)
        .then(() => this.$route.push("success"))
        .catch((err) => console.log(err));
    },
  },
  mounted() {
    if (localStorage.getItem("keranjangUser")) {
      try {
        this.keranjangUser = JSON.parse(localStorage.getItem("keranjangUser"));
      } catch (e) {
        localStorage.removeItem("keranjangUser");
      }
    }
    // axios
    //   .get("http://127.0.0.1:8000/api/products", {
    //     params: {
    //       id: this.$route.params.id,
    //     },
    //   })
    //   .then((res) => this.setDataPicture(res.data.data))
    //   .catch((err) => console.log(err));
  },
  computed: {
    totalharga() {
      return this.keranjangUser.reduce(function (items, data) {
        return items + data.price;
      }, 0);
    },
    pajak() {
      return (this.totalharga * 10) / 100;
    },
    totalBiaya() {
      return this.totalharga + this.pajak;
    },
  },
};
</script>

<style scoped>
.card-image {
  width: 150px;
  height: 150px;
}
</style>