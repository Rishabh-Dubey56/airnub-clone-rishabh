<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Airbnb Clone</title>

<style>
* {
  box-sizing: border-box;
  font-family: 'Segoe UI', sans-serif;
}

body {
  margin: 0;
  background: #f8f8f8;
}

.navbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 14px 20px;
  background: rgba(255,255,255,0.95);
  backdrop-filter: blur(10px);
  box-shadow: 0 2px 10px rgba(0,0,0,0.1);
  position: sticky;
  top: 0;
  z-index: 100;
}

.logo {
  color: #ff385c;
  font-size: 26px;
  font-weight: bold;
}

.menu-btn {
  display: none;
  font-size: 22px;
  cursor: pointer;
}

.nav-links {
  display: flex;
  gap: 20px;
  align-items: center;
}

.nav-links a {
  text-decoration: none;
  color: #444;
  font-weight: bold;
  transition: 0.3s;
}

.nav-links a:hover {
  color: #ff385c;
}

.search {
  width: 38%;
  padding: 12px 18px;
  border-radius: 30px;
  border: 1px solid #ddd;
  outline: none;
  transition: 0.3s;
}

.search:focus {
  border-color: #ff385c;
  box-shadow: 0 0 8px rgba(255,56,92,0.4);
}

.profile {
  padding: 8px 16px;
  border-radius: 20px;
  border: none;
  background: #eee;
  cursor: pointer;
  transition: 0.3s;
}

.profile:hover {
  background: #ff385c;
  color: white;
}

.categories {
  display: flex;
  gap: 20px;
  padding: 12px 20px;
  overflow-x: auto;
  background: white;
  border-bottom: 1px solid #ddd;
}

.categories span {
  cursor: pointer;
  color: gray;
  font-weight: bold;
  transition: 0.3s;
}

.categories span:hover,
.categories span.active {
  color: #ff385c;
}

.listings {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(260px, 1fr));
  gap: 22px;
  padding: 25px;
}

.card {
  background: white;
  border-radius: 18px;
  overflow: hidden;
  box-shadow: 0 6px 15px rgba(0,0,0,0.08);
  transition: 0.3s;
  cursor: pointer;
  position: relative;
}

.card:hover {
  transform: translateY(-6px) scale(1.03);
  box-shadow: 0 12px 25px rgba(0,0,0,0.15);
}

.card img {
  width: 100%;
  height: 190px;
  object-fit: cover;
}

.favorite {
  position: absolute;
  top: 10px;
  right: 12px;
  background: white;
  border-radius: 50%;
  padding: 6px;
  cursor: pointer;
  transition: 0.3s;
}

.favorite.active {
  color: red;
}

.card-body {
  padding: 14px;
}

.card-body h3 {
  margin: 0;
  font-size: 18px;
}

.card-body p {
  color: gray;
  margin-top: 6px;
  font-size: 14px;
}

.footer {
  text-align: center;
  padding: 20px;
  background: white;
  margin-top: 30px;
  color: gray;
}

@media(max-width: 768px) {
  .search {
    display: none;
  }
  .menu-btn {
    display: block;
  }
  .nav-links {
    position: absolute;
    top: 60px;
    right: 0;
    background: white;
    flex-direction: column;
    width: 200px;
    padding: 15px;
    display: none;
    box-shadow: 0 8px 15px rgba(0,0,0,0.15);
  }
  .nav-links.show {
    display: flex;
  }
}
</style>
</head>

<body>

<header class="navbar">
  <h1 class="logo">Airbnb</h1>

  <input type="text" placeholder="Search destinations..." class="search" id="searchBox">

  <div class="menu-btn" id="menuBtn">☰</div>

  <div class="nav-links" id="navLinks">
    <a href="#">Explore</a>
    <a href="#">Trips</a>
    <a href="#">Host</a>
    <button class="profile">Profile</button>
  </div>
</header>

<section class="categories" id="categoryBar">
  <span class="active">All</span>
  <span>Beach</span>
  <span>Rooms</span>
  <span>Trending</span>
  <span>Mountains</span>
  <span>Luxury</span>
  <span>City</span>
  <span>Nature</span>
</section>

<section class="listings" id="listings"></section>

<footer class="footer">
  © 2026 Airbnb Clone by Rishabh Dubey
</footer>

<script>
const homes = [
  { category:"Luxury", img:"https://images.unsplash.com/photo-1505691938895-1758d7feb511", title:"Luxury Villa", price:"$120" },
  { category:"City", img:"https://images.unsplash.com/photo-1522708323590-d24dbb6b0267", title:"City Apartment", price:"$85" },
  { category:"Trending", img:"https://images.unsplash.com/photo-1507089947368-19c1da9775ae", title:"Modern Home", price:"$95" },
  { category:"Beach", img:"https://images.unsplash.com/photo-1523217582562-09d0def993a6", title:"Beach House", price:"$180" },
  { category:"Luxury", img:"https://images.unsplash.com/photo-1502673530728-f79b4cab31b1", title:"Resort Stay", price:"$160" },
  { category:"Mountains", img:"https://images.unsplash.com/photo-1568605114967-8130f3a36994", title:"Mountain Cabin", price:"$90" },
  { category:"Nature", img:"https://images.unsplash.com/photo-1484154218962-a197022b5858", title:"Nature Retreat", price:"$110" },
  { category:"Luxury", img:"https://images.unsplash.com/photo-1512917774080-9991f1c4c750", title:"Premium Villa", price:"$210" }
]

const listings = document.getElementById("listings")
const searchBox = document.getElementById("searchBox")
const categories = document.querySelectorAll(".categories span")
const menuBtn = document.getElementById("menuBtn")
const navLinks = document.getElementById("navLinks")

function displayHomes(data) {
  listings.innerHTML = ""
  data.forEach(home => {
    const card = document.createElement("div")
    card.className = "card"
    card.innerHTML = `
      <span class="favorite">❤</span>
      <img src="${home.img}?auto=format&fit=crop&w=800&q=80">
      <div class="card-body">
        <h3>${home.title}</h3>
        <p>${home.price} / night</p>
      </div>
    `
    card.querySelector(".favorite").addEventListener("click", e => {
      e.target.classList.toggle("active")
    })
    listings.appendChild(card)
  })
}

displayHomes(homes)

searchBox.addEventListener("input", () => {
  const value = searchBox.value.toLowerCase()
  const filtered = homes.filter(home =>
    home.title.toLowerCase().includes(value)
  )
  displayHomes(filtered)
})

categories.forEach(cat => {
  cat.addEventListener("click", () => {
    categories.forEach(c => c.classList.remove("active"))
    cat.classList.add("active")

    const category = cat.innerText
    if (category === "All") {
      displayHomes(homes)
    } else {
      const filtered = homes.filter(home => home.category === category)
      displayHomes(filtered)
    }
  })
})

menuBtn.addEventListener("click", () => {
  navLinks.classList.toggle("show")
})
</script>

</body>
</html>
# airnub-clone-rishabh
