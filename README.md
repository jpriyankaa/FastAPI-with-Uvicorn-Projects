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

