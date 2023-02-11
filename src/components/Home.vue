<template>
  <div>
    <div class="m-5">
      <input type="file" @change="onFileChange" />
      <button class="btn btn-success" @click="updateProductMasterList">
        upload
      </button>
    </div>
    <br />
    <hr />
    <br />
    <div class="m-5">
      <h5>Product Master List</h5>
      <table class="table table-bordered table-striped">
        <thead>
          <tr class="bg-info">
            <th>No</th>
            <th>Product ID</th>
            <th>Type</th>
            <th>Brand</th>
            <th>Model</th>
            <th>Capacity</th>
            <th>Quantity</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(product, index) in products" :key="product.product_id">
            <td>{{ index + 1 }}</td>
            <td>{{ product.product_id }}</td>
            <td>{{ product.type }}</td>
            <td>{{ product.brand }}</td>
            <td>{{ product.model }}</td>
            <td>{{ product.capacity }}</td>
            <td>{{ product.quantity }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      products: [],
      file: null,
    };
  },
  created() {
    this.fetchProducts();
  },
  methods: {
    async fetchProducts() {
      try {
        const res = await axios.get("http://127.0.0.1:8000/api/products");
        this.products = res.data.data;
      } catch (e) {
        console.error(e);
      }
    },
    async updateProductMasterList() {
      console.log(this.file);
      try {
        let data = {
          file: this.file,
        };
        let options = {
          headers: {
            "Content-Type": "multipart/form-data",
          },
        };
        const res = await axios.post(
          "http://127.0.0.1:8000/api/products",data , options);

        this.fetchProducts();
        alert("Product successfully updated");
      } catch (e) {
        console.error(e);
        alert(e.response.data.message);
      }
    },
    onFileChange(e) {
      var files = e.target.files || e.dataTransfer.files;
      this.file = files[0];
    },
  },
};
</script>

<style>
</style>