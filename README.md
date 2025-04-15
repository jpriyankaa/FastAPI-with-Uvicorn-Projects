# 📦 FastAPI CRUD Application with Swagger UI

This project demonstrates how to build a simple CRUD (Create, Read, Update, Delete) API using **FastAPI** and **Uvicorn**, tested via **Swagger UI**.

---

## 📁 Project Structure

```
fastapi_CRUD_app/
│
├── main.py
└── README.md
```

---

## 🛠️ Step-by-Step Guide

### ✅ Step 1: Create Project Folder

```bash
fastapi_CRUD_app
```

### ✅ Step 2: Create `main.py` File

```bash
code main.py
```

### ✅ Step 3: Install Required Packages in VS Code Terminal for the 1st time

```bash
pip install fastapi uvicorn
```

---

## ✍️ main.py – Full CRUD Implementation

```python
from fastapi import FastAPI, HTTPException
from pydantic import BaseModel
from typing import List, Optional

app = FastAPI()

# In-memory data store
items_db = []

# Pydantic model
class Item(BaseModel):
    id: int
    name: str
    price: float
    description: Optional[str] = None

# Create
@app.post("/items/", response_model=Item)
def create_item(item: Item):
    for existing_item in items_db:
        if existing_item["id"] == item.id:
            raise HTTPException(status_code=400, detail="Item with this ID already exists")
    items_db.append(item.dict())
    return item

# Read All
@app.get("/items/", response_model=List[Item])
def get_all_items():
    return items_db

# Read One
@app.get("/items/{item_id}", response_model=Item)
def get_item(item_id: int):
    for item in items_db:
        if item["id"] == item_id:
            return item
    raise HTTPException(status_code=404, detail="Item not found")

# Update
@app.put("/items/{item_id}", response_model=Item)
def update_item(item_id: int, updated_item: Item):
    for idx, item in enumerate(items_db):
        if item["id"] == item_id:
            items_db[idx] = updated_item.dict()
            return updated_item
    raise HTTPException(status_code=404, detail="Item not found")

# Delete
@app.delete("/items/{item_id}")
def delete_item(item_id: int):
    for idx, item in enumerate(items_db):
        if item["id"] == item_id:
            del items_db[idx]
            return {"message": "Item deleted successfully"}
    raise HTTPException(status_code=404, detail="Item not found")
```

---

## 🚀 Step 4: Run the Server

```bash
python -m uvicorn main:app --reload
```

### Output

```
Uvicorn running on http://127.0.0.1:8000
```

---

## 🔍 Step 5: Test Using Swagger UI

1. Open your browser and go to:  
   **[http://127.0.0.1:8000/docs](http://127.0.0.1:8000/docs)**

2. Try these endpoints:

- `POST /items/` → Add a new item
- `GET /items/` → Fetch all items
- `GET /items/{item_id}` → Fetch one item
- `PUT /items/{item_id}` → Update item
- `DELETE /items/{item_id}` → Delete item

---

## 🧪 Sample Payload for POST and PUT

```json
{
  "id": 1,
  "name": "Laptop",
  "price": 1500.99,
  "description": "High-end gaming laptop"
}
```

---

## 📚 Summary

| Method | Endpoint            | Description        |
|--------|---------------------|--------------------|
| POST   | `/items/`           | Create new item    |
| GET    | `/items/`           | Get all items      |
| GET    | `/items/{item_id}`  | Get single item    |
| PUT    | `/items/{item_id}`  | Update item        |
| DELETE | `/items/{item_id}`  | Delete item        |


## 🌐 What’s Happening Behind the Scenes?

When you write a **FastAPI CRUD app**, you're building a **web service** — a kind of *digital waiter* that takes your order (called a **request**) and brings back the result (called a **response**).

---

## ⚙️ What is CRUD?

It means you're giving instructions to do:

- **C**reate (POST)
- **R**ead (GET)
- **U**pdate (PUT)
- **D**elete (DELETE)

---

## 🧪 Let’s Focus on POST (Create) Example in Swagger UI

### ✅ Step-by-step What Happens:

### 1. You open Swagger UI (`http://127.0.0.1:8000/docs`)
It shows all your API endpoints — like a menu in a restaurant.

### 2. You click on `POST /items/` → "Try it out"
You're telling the API: "Hey, I want to **add** a new item."

### 3. You enter this JSON:
```json
{
  "id": 1,
  "name": "Laptop",
  "price": 1500.99,
  "description": "High-end laptop"
}
```

This is your **request body** (like placing an order: "I want a Laptop with this info").

### 4. FastAPI gets this request:
```python
@app.post("/items/")
def create_item(item: Item):
    ...
```
- It reads the data.
- Validates it using the `Item` model.
- Adds it to `items_db`.
- Sends a **response** with the same data.

### 5. You see this as the **response**:
```json
{
  "id": 1,
  "name": "Laptop",
  "price": 1500.99,
  "description": "High-end laptop"
}
```

---

## 🧠 So, What Is a Request and Response?

### 📤 Request
You (the client) **send** some data to the API to do something.
- Example: "Please add this new item."

### 📥 Response
FastAPI (the server) **replies** with a result.
- Example: "Item added successfully. Here it is!"

---

## 💡 Real-Life Analogy

Think of Swagger UI as a food delivery app:

| Part                | Real Life (Zomato/Swiggy)      | API Example                      |
|---------------------|--------------------------------|----------------------------------|
| You                 | Customer                       | API Client                       |
| App                 | Zomato/Swiggy UI               | Swagger UI                       |
| Choose an item      | Add food to cart               | POST `/items/`                   |
| View orders         | Look at your orders            | GET `/items/`                    |
| Change item         | Modify your food choice        | PUT `/items/{item_id}`           |
| Cancel item         | Remove order                   | DELETE `/items/{item_id}`        |

---

## 💬 Final Summary

- You’re **testing your code using Swagger UI**.
- Every action like POST/GET/PUT/DELETE is a **request** you send.
- FastAPI handles it and gives you a **response**.
- Right now, the data is stored in **RAM (memory)** using a Python list (`items_db`).
- Once you close the app, the data is gone. (We can connect a database later to fix this.)


## ✅ Run Your App

First, make sure your FastAPI app is running:

```bash
python -m uvicorn main:app --reload
```

Then open your browser and go to:

```
http://127.0.0.1:8000/docs
```

---

## 🧪 1. **CREATE** an item (POST `/items/`)

### ➡ Click `POST /items/` > "Try it out"

### 🔽 Sample JSON input:

```json
{
  "id": 1,
  "name": "Laptop",
  "price": 1500.99,
  "description": "Powerful gaming laptop"
}
```

### 🟢 Click **Execute**  
You should get a response like:
```json
{
  "id": 1,
  "name": "Laptop",
  "price": 1500.99,
  "description": "Powerful gaming laptop"
}
```

✅ Add more items with different IDs if you want.

---

## 🧪 2. **READ ALL** items (GET `/items/`)

### ➡ Click `GET /items/` > "Try it out" > **Execute**

🟢 Response will be a list of all items you added:
```json
[
  {
    "id": 1,
    "name": "Laptop",
    "price": 1500.99,
    "description": "Powerful gaming laptop"
  }
]
```

---

## 🧪 3. **READ ONE** item (GET `/items/{item_id}`)

### ➡ Click `GET /items/{item_id}` > "Try it out"

### 🔽 Example Input:
- `item_id`: `1`

🟢 Click Execute

If found:
```json
{
  "id": 1,
  "name": "Laptop",
  "price": 1500.99,
  "description": "Powerful gaming laptop"
}
```

If not found:
```json
{
  "detail": "Item not found"
}
```

---

## 🧪 4. **UPDATE** an item (PUT `/items/{item_id}`)

### ➡ Click `PUT /items/{item_id}` > "Try it out"

### 🔽 Example Input:
- `item_id`: `1`

#### Body:
```json
{
  "id": 1,
  "name": "Laptop Pro",
  "price": 2000.00,
  "description": "Upgraded model"
}
```

🟢 Click Execute

Response:
```json
{
  "id": 1,
  "name": "Laptop Pro",
  "price": 2000.0,
  "description": "Upgraded model"
}
```

---

## 🧪 5. **DELETE** an item (DELETE `/items/{item_id}`)

### ➡ Click `DELETE /items/{item_id}` > "Try it out"

### 🔽 Example Input:
- `item_id`: `1`

🟢 Click Execute

Response:
```json
{
  "message": "Item deleted successfully"
}
```

Try `GET /items/` again to confirm it’s removed.

---
Excellent! Let's test **invalid inputs** to check how well API handles errors — a very important step for robust APIs. 🚨

You’ll test these using **Swagger UI (`/docs`)** just like before.

---

## 🚫 1. **Duplicate ID (POST)**

### ➡ Try to create this (again, with `id = 1` that already exists):

```json
{
  "id": 1,
  "name": "Tablet",
  "price": 300.00,
  "description": "Android tablet"
}
```

### ❌ Expected Output:

```json
{
  "detail": "Item with this ID already exists"
}
```

✅ This shows the API correctly blocks duplicate entries.

---

## 🚫 2. **Missing Required Field (POST)**

### ➡ Remove `price` field and try:

```json
{
  "id": 4,
  "name": "Monitor",
  "description": "4K Display"
}
```

### ❌ Expected Output:

```json
{
  "detail": [
    {
      "loc": ["body", "price"],
      "msg": "field required",
      "type": "value_error.missing"
    }
  ]
}
```

✅ FastAPI (via Pydantic) automatically checks required fields.

---

## 🚫 3. **Wrong Data Type (POST)**

### ➡ Set `price` to a string (should be a number):

```json
{
  "id": 5,
  "name": "Keyboard",
  "price": "cheap",
  "description": "Mechanical"
}
```

### ❌ Expected Output:

```json
{
  "detail": [
    {
      "loc": ["body", "price"],
      "msg": "value is not a valid float",
      "type": "type_error.float"
    }
  ]
}
```

✅ Pydantic ensures data types are validated.

---

## 🚫 4. **Non-existent ID (GET/PUT/DELETE)**

### Try any of these with `item_id = 999` (which doesn’t exist):

- `GET /items/999`
- `PUT /items/999` with valid data
- `DELETE /items/999`

### ❌ Expected Output:

```json
{
  "detail": "Item not found"
}
```

---

## 💡 Summary of Error Handling Tests:

| Test                          | Triggered? | Correct Response? |
|------------------------------|------------|--------------------|
| Duplicate ID                 | ✅         | `400` Bad Request  |
| Missing Field                | ✅         | `422` Unprocessable Entity |
| Wrong Data Type              | ✅         | `422` Type Error   |
| GET/PUT/DELETE Non-existent  | ✅         | `404` Not Found    |

