# Django REST E-Commerce Backend

A simple and scalable e-commerce backend API built with **Django** and **Django REST Framework (DRF)**.

This project was developed as part of learning modern backend development with Django and demonstrates how a frontend application (React, Vue, etc.) communicates with a RESTful API.

---

## 🚀 Features

* Custom User Model
* JWT Authentication
* Product Listing API
* Product Detail API
* Automatic Slug Generation
* Shopping Cart System
* Add Products to Cart
* Update Cart Quantity
* Remove Cart Items
* Check if Product Exists in Cart
* Cart Statistics
* User Profile Endpoint
* Media File Handling
* CORS Configuration for Frontend Integration

---

## 🛠 Technologies Used

* Python
* Django
* Django REST Framework
* Simple JWT
* django-cors-headers
* Pillow
* SQLite (Development Database)

---

## 📂 Project Structure

```text
backend/
│
├── core/
│   ├── models.py
│   ├── admin.py
│   └── ...
│
├── shop_app/
│   ├── models.py
│   ├── serializers.py
│   ├── views.py
│   ├── urls.py
│   └── admin.py
│
├── shoppit/
│   ├── settings.py
│   ├── urls.py
│   └── ...
│
├── media/
├── requirements.txt
├── README.md
└── manage.py
```

---

## 🗄 Database

The project currently uses **SQLite**, Django's default development database.

Database tables are managed through Django's migration system:

```bash
python manage.py makemigrations
python manage.py migrate
```

The application can easily be migrated to PostgreSQL or MySQL for production deployment.

---

## 👤 Custom User Model

Instead of Django's default User model, a custom user model was implemented by extending `AbstractUser`.

Additional fields include:

* City
* State
* Address
* Phone Number

```python
class CustomUser(AbstractUser):
    city = models.CharField(...)
    state = models.CharField(...)
    address = models.TextField(...)
    phone = models.CharField(...)
```

---

## 📦 Product Model

Each product contains:

* Name
* Slug
* Image
* Description
* Category
* Price

The slug is automatically generated using Django's `slugify()` utility.

Example:

```
HP Laptop
```

becomes

```
hp-laptop
```

Duplicate product names are handled automatically:

```
hp-laptop
hp-laptop-1
hp-laptop-2
```

---

## 🛒 Shopping Cart Architecture

The cart system consists of two models:

### Cart

Represents a user's shopping session.

| Field       | Description                            |
| ----------- | -------------------------------------- |
| cart_code   | Unique cart identifier                 |
| user        | Optional authenticated user            |
| paid        | Indicates whether checkout is complete |
| created_at  | Cart creation timestamp                |
| modified_at | Last update timestamp                  |

---

### CartItem

Represents an individual product inside a cart.

| Field    | Description      |
| -------- | ---------------- |
| cart     | Related Cart     |
| product  | Selected Product |
| quantity | Number of units  |

Example:

```
Cart
 ├── 2 × Laptop
 ├── 1 × Mouse
 └── 3 × Keyboard
```

Each line above is a CartItem.

---

## 🔄 API Endpoints

### Products

| Method | Endpoint                  | Description             |
| ------ | ------------------------- | ----------------------- |
| GET    | `/products/`              | Retrieve all products   |
| GET    | `/product_detail/<slug>/` | Retrieve single product |

---

### Cart

| Method | Endpoint            | Description             |
| ------ | ------------------- | ----------------------- |
| POST   | `/add_item/`        | Add product to cart     |
| GET    | `/product_in_cart/` | Check if product exists |
| GET    | `/get_cart_stat/`   | Get cart summary        |
| GET    | `/get_cart/`        | Retrieve full cart      |
| PATCH  | `/update_quantity/` | Update cart quantity    |
| DELETE | `/delete_cartitem/` | Remove cart item        |

---

### Authentication

| Method | Endpoint          | Description          |
| ------ | ----------------- | -------------------- |
| POST   | `/token/`         | Obtain JWT tokens    |
| POST   | `/token/refresh/` | Refresh access token |

---

### User

| Method | Endpoint      | Description                       |
| ------ | ------------- | --------------------------------- |
| GET    | `/user_info/` | Retrieve logged-in user's profile |

---

## 🔐 JWT Authentication

The project uses **JSON Web Tokens (JWT)** via `djangorestframework-simplejwt`.

### Login

**POST**

```
/token/
```

Request:

```json
{
    "username": "admin",
    "password": "password"
}
```

Response:

```json
{
    "access": "<access_token>",
    "refresh": "<refresh_token>"
}
```

Protected endpoints require the following header:

```
Authorization: Bearer <access_token>
```

---

## 📡 Example API Response

### GET `/products/`

```json
[
    {
        "id": 1,
        "name": "HP Laptop",
        "slug": "hp-laptop",
        "image": "/img/img/hplaptop.jpg",
        "description": "A powerful laptop.",
        "category": "electronics",
        "price": "500000.00"
    }
]
```

---

### GET `/get_cart/`

```json
{
    "id": 1,
    "cart_code": "ABC123XYZ",
    "items": [
        {
            "id": 1,
            "quantity": 2,
            "product": {
                "id": 1,
                "name": "HP Laptop",
                "price": "500000.00"
            },
            "total": 1000000
        }
    ],
    "sum_total": 1000000,
    "num_of_items": 2
}
```

---

## 🌐 Frontend Integration

The backend was designed to work with modern frontend frameworks such as React.

Typical frontend flow:

```
React
   │
   ├── GET /products/
   │
   ├── GET /product_detail/<slug>/
   │
   ├── POST /add_item/
   │
   ├── GET /get_cart/
   │
   ├── POST /token/
   │
   └── GET /user_info/
```

CORS is configured to allow local frontend development.

---

## 📁 Media Files

Media uploads are configured using:

```python
MEDIA_URL = "img/"
MEDIA_ROOT = BASE_DIR / "media"
```

Uploaded product images are stored inside the `media/` directory.

---

## ▶️ Running the Project

Clone the repository:

```bash
git clone https://github.com/Abdulmumin3/django-ecommerce-backend.git
```

Navigate into the project:

```bash
cd django-ecommerce-backend
```

Create a virtual environment:

```bash
python -m venv venv
```

Activate it:

### Windows

```bash
venv\Scripts\activate
```

Install dependencies:

```bash
pip install -r requirements.txt
```

Run migrations:

```bash
python manage.py makemigrations
python manage.py migrate
```

Create a superuser:

```bash
python manage.py createsuperuser
```

Start the development server:

```bash
python manage.py runserver
```

Visit:

```
http://127.0.0.1:8000/admin/
```

---

## 🔮 Future Improvements

* Payment Integration (Flutterwave / PayPal)
* Order Management System
* Wishlist Feature
* Product Reviews
* UUID Cart Codes
* PostgreSQL Deployment
* Docker Support
* API Documentation with Swagger/OpenAPI
* DRF Generic Views & ViewSets

---

## 📚 Learning Outcomes

This project demonstrates understanding of:

* Django Models
* Django ORM
* Django REST Framework
* Model Serializers
* Nested Serializers
* SerializerMethodField
* JWT Authentication
* API Design
* CRUD Operations
* Shopping Cart Logic
* Custom User Models
* File Upload Handling
* Frontend-Backend Communication

---

## 👨‍💻 Author

**AbdulMumeen Adesoye**

Backend Developer (Django / DRF)

---

## 📄 License

This project was developed for educational and portfolio purposes.
