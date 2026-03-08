# pro
<!DOCTYPE html>
<html>
<head>
<title>Smart Product Recommendation Platform</title>

<style>

body{
font-family:Arial;
margin:0;
background:#eef2f7;
}

header{
background:#2563eb;
color:white;
padding:18px;
text-align:center;
font-size:24px;
}

.container{
padding:25px;
text-align:center;
}

.page{
display:none;
animation:fade 0.7s ease;
}

@keyframes fade{
from{opacity:0;transform:translateY(10px);}
to{opacity:1;transform:translateY(0);}
}

input,select{
padding:10px;
margin:6px;
border-radius:6px;
border:1px solid #ccc;
width:220px;
}

button{
padding:10px 15px;
border:none;
background:#2563eb;
color:white;
border-radius:6px;
cursor:pointer;
margin:6px;
transition:0.3s;
}

button:hover{
background:#1e40af;
transform:scale(1.05);
}

.reset{
background:#ef4444;
}

.products{
display:grid;
grid-template-columns:repeat(auto-fill,minmax(250px,1fr));
gap:20px;
margin-top:25px;
}

.card{
background:white;
border-radius:12px;
padding:15px;
box-shadow:0 4px 15px rgba(0,0,0,0.1);
transition:0.3s;
}

.card:hover{
transform:scale(1.05);
}

.card img{
width:100%;
height:170px;
object-fit:cover;
border-radius:8px;
}

.rating{
color:#f59e0b;
}

.best{
background:#16a34a;
color:white;
padding:4px;
border-radius:5px;
display:inline-block;
margin-top:5px;
}

.pricebox{
background:#eef2ff;
padding:6px;
margin-top:5px;
border-radius:5px;
}

.mapbtn{background:#10b981;}
.buybtn{background:#f59e0b;}

.adminpanel{
background:white;
padding:20px;
border-radius:10px;
box-shadow:0 3px 10px rgba(0,0,0,0.1);
display:inline-block;
}

</style>
</head>

<body>

<header>
Smart Product Recommendation System
</header>

<!-- ROLE SELECT PAGE -->

<div id="rolePage" class="container page" style="display:block">

<h2>Select Login Type</h2>

<button onclick="openUser()">User</button>
<button onclick="openAdmin()">Admin</button>

</div>

<!-- USER LOGIN -->

<div id="userLoginPage" class="container page">

<h2>User Login</h2>

<input id="mobile" placeholder="Mobile Number"><br>

<select id="location">
<option value="">Select Location</option>
<option>Chennai</option>
<option>Madurai</option>
<option>Trichy</option>
<option>Coimbatore</option>
</select><br>

<button onclick="login()">Enter Website</button>

</div>

<!-- ADMIN LOGIN -->

<div id="adminLoginPage" class="container page">

<h2>Admin Login</h2>

<input id="adminUser" placeholder="Admin Username"><br>
<input id="adminPass" type="password" placeholder="Password"><br>

<button onclick="adminLogin()">Login</button>

</div>

<!-- MAIN USER PAGE -->

<div id="mainPage" class="container page">

<h3>Search Product</h3>

<input id="searchInput" placeholder="Search phone, furniture, skin care">
<button onclick="searchProduct()">Search</button>
<button class="reset" onclick="resetSearch()">Reset</button>

<div class="products" id="productList"></div>

</div>

<!-- ADMIN DASHBOARD -->

<div id="adminPage" class="container page">

<div class="adminpanel">

<h3>Admin Product Management</h3>

<input id="pname" placeholder="Product Name"><br>
<input id="ptype" placeholder="Product Type"><br>
<input id="pimage" placeholder="Image URL"><br>

<button onclick="addProduct()">Add Product</button>

</div>

</div>

<script>

let products=[

{
name:"iPhone 14",
type:"mobile",
image:"https://images.unsplash.com/photo-1511707171634",
rating:"4.8",
review:"Best camera phone",
amazon:"https://amazon.in",
flipkart:"https://flipkart.com",
online:{amazon:78999,flipkart:78499},
shops:[{name:"Reliance Digital",price:79000,map:"https://maps.google.com"}]
},

{
name:"Samsung Galaxy S23",
type:"mobile",
image:"https://images.unsplash.com/photo-1580910051074",
rating:"4.7",
review:"Flagship performance",
amazon:"https://amazon.in",
flipkart:"https://flipkart.com",
online:{amazon:70999,flipkart:69999},
shops:[{name:"Mobile World",price:70500,map:"https://maps.google.com"}]
},

{
name:"Nivea Face Wash",
type:"skin",
image:"https://images.unsplash.com/photo-1596755389378",
rating:"4.4",
review:"Good for oily skin",
amazon:"https://amazon.in",
flipkart:"https://flipkart.com",
online:{amazon:249,flipkart:239},
shops:[{name:"Apollo Pharmacy",price:240,map:"https://maps.google.com"}]
},

{
name:"Wooden Sofa",
type:"furniture",
image:"https://images.unsplash.com/photo-1505693416388",
rating:"4.6",
review:"Comfortable sofa",
amazon:"https://amazon.in",
flipkart:"https://flipkart.com",
online:{amazon:25000,flipkart:24000},
shops:[{name:"Furniture World",price:23500,map:"https://maps.google.com"}]
},

{
name:"LG Washing Machine",
type:"appliance",
image:"https://images.unsplash.com/photo-1626806787461",
rating:"4.5",
review:"Energy efficient",
amazon:"https://amazon.in",
flipkart:"https://flipkart.com",
online:{amazon:18999,flipkart:17999},
shops:[{name:"Reliance Digital",price:17500,map:"https://maps.google.com"}]
},

{
name:"Sony Headphones",
type:"electronics",
image:"https://images.unsplash.com/photo-1518441902110",
rating:"4.6",
review:"Best sound quality",
amazon:"https://amazon.in",
flipkart:"https://flipkart.com",
online:{amazon:4999,flipkart:4599},
shops:[{name:"Electronics Hub",price:4500,map:"https://maps.google.com"}]
}

]

function openUser(){
document.getElementById("rolePage").style.display="none"
document.getElementById("userLoginPage").style.display="block"
}

function openAdmin(){
document.getElementById("rolePage").style.display="none"
document.getElementById("adminLoginPage").style.display="block"
}

function login(){

let mobile=document.getElementById("mobile").value
let location=document.getElementById("location").value

if(mobile.length>=10 && location!=""){

document.getElementById("userLoginPage").style.display="none"
document.getElementById("mainPage").style.display="block"

displayProducts(products)

}else{
alert("Enter valid details")
}

}

function adminLogin(){

let user=document.getElementById("adminUser").value
let pass=document.getElementById("adminPass").value

if(user=="admin" && pass=="1234"){

document.getElementById("adminLoginPage").style.display="none"
document.getElementById("adminPage").style.display="block"

}else{
alert("Invalid login")
}

}

function displayProducts(list){

let html=""

list.forEach(p=>{

let shopHTML=""
let best=999999
let bestShop=""

p.shops.forEach(s=>{

shopHTML+=`
<div class="pricebox">
🏪 ${s.name} ₹${s.price}
<br>
<a href="${s.map}" target="_blank">
<button class="mapbtn">View Map</button>
</a>
</div>
`

if(s.price<best){
best=s.price
bestShop=s.name
}

})

html+=`

<div class="card">

<img src="${p.image}">

<h4>${p.name}</h4>

<p class="rating">⭐ ${p.rating}</p>

<p>${p.review}</p>

<div class="best">
Best Price ₹${best} at ${bestShop}
</div>

<div class="pricebox">
Amazon ₹${p.online.amazon}
<br>
<a href="${p.amazon}" target="_blank">
<button class="buybtn">Buy Amazon</button>
</a>
</div>

<div class="pricebox">
Flipkart ₹${p.online.flipkart}
<br>
<a href="${p.flipkart}" target="_blank">
<button class="buybtn">Buy Flipkart</button>
</a>
</div>

${shopHTML}

</div>
`

})

document.getElementById("productList").innerHTML=html

}

function searchProduct(){

let q=document.getElementById("searchInput").value.toLowerCase()

let result=products.filter(p=>JSON.stringify(p).toLowerCase().includes(q))

displayProducts(result)

}

function resetSearch(){
document.getElementById("searchInput").value=""
displayProducts(products)
}

function addProduct(){

let name=document.getElementById("pname").value
let type=document.getElementById("ptype").value
let image=document.getElementById("pimage").value

products.push({
name:name,
type:type,
image:image,
rating:"4",
review:"New product",
amazon:"https://amazon.in",
flipkart:"https://flipkart.com",
online:{amazon:100,flipkart:95},
shops:[{name:"Local Shop",price:90,map:"https://maps.google.com"}]
})

alert("Product Added")

}

</script>

</body>
</html>
