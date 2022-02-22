<template>
  <div>
    <h3>My Product Page</h3><br>
    <a href="/"><button>Logout </button></a>
    <ul>
      <li v-for="product in products" :key="product">{{product.ProductId}}<br>
        <div class= "card">
          <p>Product Name : {{product.name}}</p>
          <p>Product Description : {{product.description}}</p>
          <p>Product Price : {{product.price}}</p>
          <button v-on:click="addProduct(product.ProductId)"> Add Product </button>
          
          <br>
        </div>
        <br>
      </li>
    </ul>

    <br><br>
    <a href="/mycart"> <button> View My Cart </button></a>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  data(){
    return {
      products:''
    };
  },
  methods:{

    async addProduct(id){
      console.log(id);
      const data={"CustomerId": localStorage.getItem("CustomerId"),"ProductId": id}
      await axios.post("http://localhost:3000/cart/products",data)
      .then((response)=>{
        console.log(response.data);
        alert("Product added successfully to cart");
      })
      .catch(err =>{
        console.error("Error "+ err);
      })

    },
    async getProducts(){
      await axios.get("http://localhost:3000/product/allProducts")
      .then((response) => {
        console.log(response.data);
        this.products = response.data
      })
      .catch(err =>{
        console.error("Error "+ err);
      })
    },
  },
  beforeMount(){
    this.getProducts()
  },
 
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.card{
  border: 1px solid black;
}
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
