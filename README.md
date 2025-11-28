<img width=100% src="https://capsule-render.vercel.app/api?type=waving&color=3DF4AD&height=120&section=header&animation=twinkling"/>

<h1 align="center">Hello World! I'm Kevin Sinatria Budiman</h1>
<h3 align="center">Full Stack Web Dev Enthusiast</h3>

```json
{
  "about_me": {
    "headline": "Web Developer",
    "description": "I am a web developer passionate about building impactful web applications using modern technologies.",
    "interests": [
      "Exploring new things in the world of the web",
      "APIs",
      "UI/UX",
      "Application Performance"
    ],
    "tagline": "Keep committing even if it's only 1 line."
  }
}
```

---
<div align="center">
   
## My Tech Stack:

### Frontend Development

<div>
   <img src="https://skillicons.dev/icons?i=html,css,js,ts,react,next,alpinejs,tailwind,bootstrap,vite" />
</div>

### Backend Development
<div>
  <img src="https://skillicons.dev/icons?i=nodejs,express,prisma,laravel,php" />
</div>

### Database
<div>
<img src="https://skillicons.dev/icons?i=postgresql,mysql,sqlite,supabase" />
</div>

### DevOps & Cloud
<div>
<img src="https://skillicons.dev/icons?i=git,github,docker,vercel,netlify" />
</div>

### Tools & Utilities
<div>
<img src="https://skillicons.dev/icons?i=vscode,postman,figma,notion,discord" />
</div>

</div>

---

<div align="center"><img src="https://user-images.githubusercontent.com/74038190/216122041-518ac897-8d92-4c6b-9b3f-ca01dcaf38ee.png" alt="Fire" width="64" /></div>
<h2 align="center">My Stats</h2>

<div align="center">
<img src="https://streak-stats.demolab.com?user=KevinSinatria&locale=en&mode=daily&theme=dark&hide_border=false&border_radius=5&order=3" height="220" alt="streak graph"  />
</div>

<p align="center">
  <img src="https://github-readme-stats.vercel.app/api?username=KevinSinatria&show_icons=true&theme=radical" height="170"/>
  <img src="https://github-readme-stats.vercel.app/api/top-langs/?username=kevinsinatria&layout=compact&theme=radical" height="170"/>
</p>

---

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/KevinSinatria/KevinSinatria/output/pacman-contribution-graph-dark.svg">
  <source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/KevinSinatria/KevinSinatria/output/pacman-contribution-graph.svg">
  <img alt="pacman contribution graph" src="https://raw.githubusercontent.com/KevinSinatria/KevinSinatria/output/pacman-contribution-graph.svg">
</picture>

---

### 📬 Contact Me

<div align="left">
  <a href="https://www.linkedin.com/in/kevin-sinatria-budiman/">
    <img src="https://raw.githubusercontent.com/maurodesouza/profile-readme-generator/master/src/assets/icons/social/linkedin/default.svg" width="52" height="40" alt="linkedin logo"  />
  </a>
  <a href="https://www.instagram.com/kevinsinatriabudiman/">
    <img src="https://raw.githubusercontent.com/maurodesouza/profile-readme-generator/master/src/assets/icons/social/instagram/default.svg" width="52" height="40"   alt="instagram logo"  />
  </a>
  <a href="">
  <img src="https://raw.githubusercontent.com/maurodesouza/profile-readme-generator/master/src/assets/icons/social/gmail/default.svg" width="52" height="40" alt="gmail logo"  />
  </a>
</div>

###

📩 Open for collaboration, freelancing, or just chatting about the world of technology!

---

<div align="center"><img src="https://user-images.githubusercontent.com/74038190/216122028-c05b52fb-983e-4ee8-8811-6f30cd9ea5d5.png" alt="Comet" width="120" /></div>
<p align="center">
  Keep learning. Keep creating. Keep developing.
</p>
<h2 align="center">
  “Man Jadda Wajada”
</h2>

<img width=100% src="https://capsule-render.vercel.app/api?type=waving&color=3DF4AD&height=80&section=footer&animation=twinkling"/>





// This is your Prisma schema file,
// learn more about it in the docs: https://pris.ly/d/prisma-schema

// Looking for ways to speed up your queries, or scale easily with your serverless or edge functions?
// Try Prisma Accelerate: https://pris.ly/cli/accelerate-init

generator client {
  provider = "prisma-client"
  output   = "../generated/prisma"
}

datasource db {
  provider = "mysql"
}

model User {
  id         Int      @id @default(autoincrement())
  username   String   @unique @db.VarChar(255)
  password   String   @db.VarChar(255)
  fullname   String   @db.VarChar(255)
  role       Role     @default(CASHIER)
  created_at DateTime @default(now())
  updated_at DateTime @updatedAt

  products        Product[]
  stock_movements StockMovement[]
  transactions    Transaction[]
}

model Product {
  id            Int      @id @default(autoincrement())
  name          String   @unique @db.VarChar(255)
  product_code  String   @unique @db.VarChar(255)
  price         Int
  stock         Int
  image_url     String?   @db.VarChar(255)
  created_by_id Int
  created_at    DateTime @default(now())
  updated_at    DateTime @updatedAt

  created_by          User                @relation(fields: [created_by_id], references: [id], onDelete: Cascade, onUpdate: Cascade)
  discounts           Discount[]
  stock_movements     StockMovement[]
  transaction_details TransactionDetail[]
}

model Member {
  id         Int      @id @default(autoincrement())
  name       String   @db.VarChar(255)
  phone      String   @unique @db.VarChar(255)
  address    String?   @db.Text
  created_at DateTime @default(now())
  updated_at DateTime @updatedAt

  transactions Transaction[]
}

// Discount bisa berupa level transaksi jika:
// - product_id === null
// - is_transaction_level === true
// 
// Dan Sebaliknya bisa berupa level produk jika:
// - product_id !== null
// - product === ada
// - is_transaction_level === false
model Discount {
  id                   Int          @id @default(autoincrement())
  name                 String       @db.VarChar(255)
  description          String?       @db.Text
  type                 DiscountType @default(FIXED)
  value                Int
  min_qty              Int?
  product_id           Int?
  is_active            Boolean      @default(true)
  is_transaction_level Boolean      @default(false)
  is_member_level      Boolean      @default(false)
  start_date           DateTime
  end_date             DateTime
  created_at           DateTime     @default(now())
  updated_at           DateTime     @updatedAt

  product Product? @relation(fields: [product_id], references: [id], onDelete: Cascade, onUpdate: Cascade)
}

model StockMovement {
  id         Int               @id @default(autoincrement())
  type       StockMovementType
  quantity   Int
  note       String            @db.VarChar(255)
  user_id    Int
  product_id Int
  created_at DateTime          @default(now())

  user    User    @relation(fields: [user_id], references: [id], onDelete: Cascade, onUpdate: Cascade)
  product Product @relation(fields: [product_id], references: [id], onDelete: Cascade, onUpdate: Cascade)
}

model Transaction {
  id             Int           @id @default(autoincrement())
  invoice_no     String        @unique @db.VarChar(255)
  subtotal       Int
  discount       Int?
  discount_desc  String?
  discount_total Int?
  total          Int
  payment_method PaymentMethod
  payment_amount Int
  return_amount  Int
  user_id        Int
  member_id      Int?
  created_at     DateTime      @default(now())
  updated_at     DateTime      @updatedAt

  user    User                @relation(fields: [user_id], references: [id], onDelete: Cascade, onUpdate: Cascade)
  member  Member?             @relation(fields: [member_id], references: [id], onDelete: Cascade, onUpdate: Cascade)
  details TransactionDetail[]
}

model TransactionDetail {
  id                Int     @id @default(autoincrement())
  product_id        Int
  price_snapshot    Int
  quantity          Int
  original_subtotal Int
  subtotal          Int
  discount          Int?
  discount_desc     String?
  transaction_id    Int

  transaction Transaction @relation(fields: [transaction_id], references: [id], onDelete: Cascade, onUpdate: Cascade)
  product     Product     @relation(fields: [product_id], references: [id], onDelete: Cascade, onUpdate: Cascade)
}

enum DiscountType {
  FIXED
  PERCENTAGE
}

enum Role {
  ADMIN
  CASHIER
}

enum StockMovementType {
  OUT
  IN
}

enum PaymentMethod {
  CASH
  CREDIT
  DEBIT
  QRIS
}

