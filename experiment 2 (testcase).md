CODE:
class ECommerce:
    def __init__(self):
        self.users = {}
        self.logged_in_user = None
        self.products = {
            "Laptop": {"price": 50000, "stock": 3},
            "Headphones": {"price": 2000, "stock": 0}
        }
        self.cart = []

    def register(self, email, password):
        if not email or not password:
            return "Fields cannot be empty"
        if "@" not in email:
            return "Invalid email"
        if email in self.users:
            return "User already exists"
        self.users[email] = password
        return "Registration successful"

    def login(self, email, password):
        if email in self.users and self.users[email] == password:
            self.logged_in_user = email
            return "Login successful"
        return "Invalid credentials"

    def search(self, product):
        return self.products.get(product, "Product not found")

    def add_to_cart(self, product):
        if product not in self.products:
            return "Product not found"
        if self.products[product]["stock"] <= 0:
            return "Out of stock"
        self.cart.append(product)
        self.products[product]["stock"] -= 1
        return "Added to cart"

    def checkout(self):
        if not self.logged_in_user:
            return "User not logged in"
        if not self.cart:
            return "Cart is empty"
        total = sum(self.products[item]["price"] for item in self.cart)
        self.cart.clear()
        return f"Order successful. Total: ₹{total}"


import unittest

class TestECommerce(unittest.TestCase):

    def setUp(self):
        self.app = ECommerce()

    def test_register_valid(self):
        self.assertEqual(self.app.register("test@gmail.com", "Pass123"),
                         "Registration successful")

    def test_register_empty(self):
        self.assertEqual(self.app.register("", ""),
                         "Fields cannot be empty")

    def test_register_invalid_email(self):
        self.assertEqual(self.app.register("invalidemail", "123"),
                         "Invalid email")

    def test_register_existing(self):
        self.app.register("test@gmail.com", "123")
        self.assertEqual(self.app.register("test@gmail.com", "123"),
                         "User already exists")

    def test_login_success(self):
        self.app.register("user@gmail.com", "123")
        self.assertEqual(self.app.login("user@gmail.com", "123"),
                         "Login successful")

    def test_login_fail(self):
        self.assertEqual(self.app.login("wrong@gmail.com", "123"),
                         "Invalid credentials")

    def test_search_found(self):
        self.assertIsInstance(self.app.search("Laptop"), dict)

    def test_search_not_found(self):
        self.assertEqual(self.app.search("TV"),
                         "Product not found")

    def test_add_to_cart_success(self):
        self.assertEqual(self.app.add_to_cart("Laptop"),
                         "Added to cart")

    def test_add_to_cart_out_of_stock(self):
        self.assertEqual(self.app.add_to_cart("Headphones"),
                         "Out of stock")

    def test_checkout_without_login(self):
        self.app.add_to_cart("Laptop")
        self.assertEqual(self.app.checkout(),
                         "User not logged in")

    def test_checkout_success(self):
        self.app.register("user@gmail.com", "123")
        self.app.login("user@gmail.com", "123")
        self.app.add_to_cart("Laptop")
        result = self.app.checkout()
        self.assertIn("Order successful", result)
unittest.main(argv=[''], exit=False)

OTUPUT:
<img width="770" height="148" alt="Screenshot 2026-02-20 113422" src="https://github.com/user-attachments/assets/ff7849cf-e238-4c31-ad65-8df54c5ea0e3" />
