# jubilant-rotary-phone
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Modern Shop</title>
<style>
*{
  margin:0;
  padding:0;
  box-sizing:border-box;
  font-family:'Segoe UI',sans-serif;
}

body{
  background:linear-gradient(135deg,#667eea,#764ba2);
  min-height:100vh;
  transition:0.4s;
}

body.dark{
  background:#111;
  color:white;
}

nav{
  display:flex;
  justify-content:space-between;
  align-items:center;
  padding:20px 40px;
  background:rgba(255,255,255,0.2);
  backdrop-filter:blur(10px);
  color:white;
}

nav input{
  padding:8px;
  border:none;
  border-radius:5px;
}

button{
  padding:8px 12px;
  border:none;
  border-radius:5px;
  cursor:pointer;
  background:#ff4d6d;
  color:white;
  transition:0.3s;
}

button:hover{transform:scale(1.05);}

.products{
  display:grid;
  grid-template-columns:repeat(auto-fit,minmax(250px,1fr));
  gap:25px;
  padding:40px;
}

.card{
  background:rgba(255,255,255,0.2);
  backdrop-filter:blur(10px);
  padding:15px;
  border-radius:15px;
  transition:0.3s;
  color:white;
}

.card:hover{
  transform:translateY(-10px);
}

.card img{
  width:100%;
  height:180px;
  object-fit:cover;
  border-radius:10px;
}

.price{
  font-weight:bold;
  margin:10px 0;
}

/* Wishlist */
.wishlist{
  position:absolute;
  right:15px;
  top:15px;
  cursor:pointer;
}

/* Payment Modal */
.modal{
  position:fixed;
  inset:0;
  background:rgba(0,0,0,0.6);
  display:none;
  justify-content:center;
  align-items:center;
}

.modal-content{
  background:white;
  padding:30px;
  border-radius:15px;
  width:300px;
  animation:pop 0.4s ease;
}

@keyframes pop{
  from{transform:scale(0);}
  to{transform:scale(1);}
}

/* Admin Panel */
.admin{
  padding:20px;
  background:rgba(0,0,0,0.4);
  color:white;
}

.admin input{
  margin:5px 0;
  padding:5px;
  width:100%;
}
</style>
</head>
<body>

<nav>
  <h2>ModernShop</h2>
  <input type="text" id="search" placeholder="Search products...">
  <div>
    <button onclick="toggleDark()">🌙</button>
    <button onclick="openPayment()">💳 Checkout</button>
  </div>
</nav>

<section class="products" id="products"></section>

<!-- Payment Modal -->
<div class="modal" id="paymentModal">
  <div class="modal-content">
    <h3>Payment</h3>
    <input placeholder="Card Number"><br><br>
    <input placeholder="Expiry Date"><br><br>
    <input placeholder="CVV"><br><br>
    <button onclick="payNow()">Pay Now</button>
  </div>
</div>

<!-- Admin Panel -->
<div class="admin">
  <h3>Admin Dashboard</h3>
  <input id="adminName" placeholder="Product Name">
  <input id="adminPrice" placeholder="Price">
  <input id="adminImg" placeholder="Image URL">
  <button onclick="addProduct()">Add Product</button>
</div>

<script>
let products = [
  {id:1,name:"Smart Watch",price:120,img:"https://images.unsplash.com/photo-1523275335684-37898b6baf30"},
  {id:2,name:"Shoes",price:150,img:"https://images.unsplash.com/photo-1542291026-7eec264c27ff"},
  {id:3,name:"Headphones",price:90,img:"https://images.unsplash.com/photo-1518441902110-86e4bfc28f7c"}
];

let wishlist = [];

const productContainer = document.getElementById("products");
const searchInput = document.getElementById("search");

function displayProducts(filter=""){
  productContainer.innerHTML="";
  products
    .filter(p=>p.name.toLowerCase().includes(filter.toLowerCase()))
    .forEach(product=>{
      productContainer.innerHTML += `
      <div class="card">
        <div class="wishlist" onclick="toggleWishlist(${product.id})">
          ${wishlist.includes(product.id) ? "❤️" : "🤍"}
        </div>
        <img src="${product.img}">
        <h3>${product.name}</h3>
        <div class="price">$${product.price}</div>
        <button onclick="addToCart()">Add to Cart</button>
      </div>`;
    });
}

searchInput.addEventListener("input",e=>{
  displayProducts(e.target.value);
});

function toggleWishlist(id){
  if(wishlist.includes(id)){
    wishlist = wishlist.filter(item=>item!==id);
  }else{
    wishlist.push(id);
  }
  displayProducts(searchInput.value);
}

function toggleDark(){
  document.body.classList.toggle("dark");
}

function openPayment(){
  document.getElementById("paymentModal").style.display="flex";
}

function payNow(){
  alert("Payment Successful 🎉");
  document.getElementById("paymentModal").style.display="none";
}

function addProduct(){
  const name=document.getElementById("adminName").value;
  const price=document.getElementById("adminPrice").value;
  const img=document.getElementById("adminImg").value;
  if(name && price && img){
    products.push({id:Date.now(),name,price,img});
    displayProducts();
  }
}

function addToCart(){
  alert("Added to cart 🛒");
}

displayProducts();
</script>

</body>
</html>
