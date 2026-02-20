CODE:
class ECommerceApp:
    def __init__(self):
        self.users = {}
        self.products = {
            "Laptop": {"price": 50000, "stock": 5},
            "Phone": {"price": 20000, "stock": 10}
        }
        self.cart = []

    def register(self, email, password):
        if email in self.users:
            return "Email already exists"
        if "@" not in email:
            return "Invalid email format"
        self.users[email] = password
        return "Registration successful"

    def login(self, email, password):
        if email in self.users and self.users[email] == password:
            return "Login successful"
        return "Invalid credentials"

    def search_product(self, product_name):
        if product_name in self.products:
            return self.products[product_name]
        return "Product not found"

    def add_to_cart(self, product_name):
        if product_name not in self.products:
            return "Product not found"
        if self.products[product_name]["stock"] <= 0:
            return "Out of stock"
        self.cart.append(product_name)
        self.products[product_name]["stock"] -= 1
        return "Added to cart"

    def checkout(self):
        if not self.cart:
            return "Cart is empty"
        total = sum(self.products[item]["price"] for item in self.cart)
        self.cart.clear()
        return f"Order placed successfully. Total amount: ₹{total}"


import unittest

class TestECommerceApp(unittest.TestCase):

    def setUp(self):
        self.app = ECommerceApp()


    def test_registration_success(self):
        result = self.app.register("user@gmail.com", "Pass123")
        self.assertEqual(result, "Registration successful")

    def test_registration_existing_email(self):
        self.app.register("user@gmail.com", "Pass123")
        result = self.app.register("user@gmail.com", "Pass123")
        self.assertEqual(result, "Email already exists")

    def test_login_success(self):
        self.app.register("user@gmail.com", "Pass123")
        result = self.app.login("user@gmail.com", "Pass123")
        self.assertEqual(result, "Login successful")

    def test_login_invalid(self):
        result = self.app.login("wrong@gmail.com", "123")
        self.assertEqual(result, "Invalid credentials")

    def test_search_product_found(self):
        result = self.app.search_product("Laptop")
        self.assertIsInstance(result, dict)

    def test_search_product_not_found(self):
        result = self.app.search_product("TV")
        self.assertEqual(result, "Product not found")

    def test_add_to_cart_success(self):
        result = self.app.add_to_cart("Phone")
        self.assertEqual(result, "Added to cart")

    def test_checkout_success(self):
        self.app.add_to_cart("Laptop")
        result = self.app.checkout()
        self.assertIn("Order placed successfully", result)
unittest.main(argv=[''], exit=False)

OUTPUT:
<img width="789" height="143" alt="Screenshot 2026-02-20 112850" src="https://github.com/user-attachments/assets/82a50bff-c761-4b90-9eb9-4e62ad7ca681" />
