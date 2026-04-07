# 🧾 Prisma Migration & Seeding — Debugging Notes

## ⚙️ Commands Used

```bash
npm run db:migrate   # prisma migrate dev
npm run db:seed      # node prisma/seed.js
```

---

# 🚨 Errors Faced & Learnings

---

## 1️⃣ Database Does Not Exist (`P1003`)

### ❌ Error

```text
Database `devanshi` does not exist on the database server
```

### 🧠 Concept

* Prisma connects using `DATABASE_URL`
* If `.env` is not loaded → Prisma may fallback or misread config

### 💥 Root Cause

* Seed script was **not loading environment variables**
* So it tried connecting to a **wrong DB**

---

### ✅ Fix

```js
import 'dotenv/config'
```

### 🎯 Learning

* Always load `.env` in standalone scripts (like seed)
* Prisma CLI loads env automatically, but **Node scripts do NOT**

---

---

## 2️⃣ ESM vs CommonJS Conflict

### ❌ Error

```text
ReferenceError: require is not defined in ES module scope
```

---

### 🧠 Concept

| System           | Syntax      |
| ---------------- | ----------- |
| CommonJS         | `require()` |
| ES Modules (ESM) | `import`    |

👉 Node.js supports both, but **you cannot mix them**

---

### 💥 Root Cause

* Project was running in **ES Module mode**
* But `seed.js` used:

```js
require('dotenv').config()
```

---

### ✅ Fix

Converted everything to ESM:

```js
import 'dotenv/config'
import { PrismaClient } from '@prisma/client'
import { PrismaPg } from '@prisma/adapter-pg'
import bcrypt from 'bcryptjs'
```

---

### 🎯 Learning

* Modern Node (v20+) prefers **ES Modules**
* Avoid `require()` in new projects

---

---

## 3️⃣ Duplicate Identifier Error

### ❌ Error

```text
SyntaxError: Identifier 'PrismaClient' has already been declared
```

---

### 💥 Root Cause

Both were present:

```js
import { PrismaClient } from '@prisma/client'
const { PrismaClient } = require('@prisma/client')
```

---

### ✅ Fix

* Removed all `require()` imports
* Kept only `import`

---

### 🎯 Learning

* Same variable cannot be declared twice
* Mixing module systems often causes **hidden duplication bugs**

---

---

## 4️⃣ Module Type Warning

### ⚠️ Warning

```text
[MODULE_TYPELESS_PACKAGE_JSON]
```

---

### 🧠 Concept

Node determines module type using:

```json
"type": "module"
```

---

### 💥 Cause

* `package.json` didn’t specify module type
* Node had to **auto-detect** → slower

---

### ✅ Fix (Recommended)

```json
{
  "type": "module"
}
```

---

### 🎯 Learning

* Always define module type explicitly
* Prevents ambiguity + improves performance

---

---

# ✅ Final Working Setup

### `seed.js` (clean version)

```js
import 'dotenv/config'
import { PrismaClient } from '@prisma/client'
import { PrismaPg } from '@prisma/adapter-pg'

const prisma = new PrismaClient({
  adapter: new PrismaPg({
    connectionString: process.env.DATABASE_URL,
  }),
})
```

---

# 🎉 Final Output

```bash
npm run db:migrate
✔ Migration applied

npm run db:seed
Seeding database...
Seed complete!
```

---

# 🧠 Key Takeaways (Important)

* ✅ Prisma CLI auto-loads `.env`, but custom scripts don’t
* ✅ Never mix `require` and `import`
* ✅ Node 22 + Prisma 7 → prefer ESM
* ✅ Always define `"type": "module"`
* ✅ Errors can cascade — fix root cause, not symptoms

---

# 🏁 Outcome

✔ Database schema created
✔ Seed data inserted
✔ Backend ready for development
