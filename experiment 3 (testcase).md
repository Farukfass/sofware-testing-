CODE:
class ECommerce:
    def __init__(self):
        self.users = {}
        self.logged_in_user = None
        self.products = {
            "Laptop": {"price": 50000, "stock": 2},
            "Headphones": {"price": 2000, "stock": 0}
        }
        self.cart = []

    def register(self, email, password):
        if email in self.users:
            return "User already exists"
        self.users[email] = password
        return "Registration successful"

    def login(self, email, password):
        if email in self.users and self.users[email] == password:
            self.logged_in_user = email
            return "Login successful"
        return "Invalid credentials"

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
            return "Cart empty"
        total = sum(self.products[item]["price"] for item in self.cart)
        return total


defect_report = []

def log_defect(defect_id, description, expected, actual, severity):
    defect_report.append({
        "Defect ID": defect_id,
        "Description": description,
        "Expected": expected,
        "Actual": actual,
        "Severity": severity
    })


app = ECommerce()

result = app.add_to_cart("Laptop")
if result == "Added to cart":
    log_defect(
        "DEF_001",
        "User can add product without login",
        "System should restrict cart access without login",
        "Product added successfully",
        "Medium"
    )

app.register("user@gmail.com", "123")
app.login("user@gmail.com", "123")
app.add_to_cart("Laptop")
app.products["Laptop"]["price"] = 60000

total = app.checkout()
if total != 50000:
    log_defect(
        "DEF_002",
        "Checkout total changes if product price updated after adding to cart",
        "Total should remain 50000",
        f"Total calculated as {total}",
        "High"
    )

result = app.add_to_cart("Headphones")
if result != "Out of stock":
    log_defect(
        "DEF_003",
        "Out of stock product added to cart",
        "System should block out of stock items",
        result,
        "High"
    )

if defect_report:
    print("DEFECT REPORT")
    print("=" * 50)
    for defect in defect_report:
        print(f"\nDefect ID: {defect['Defect ID']}")
        print(f"Description: {defect['Description']}")
        print(f"Expected: {defect['Expected']}")
        print(f"Actual: {defect['Actual']}")
        print(f"Severity: {defect['Severity']}")
else:
    print("No defects found.")
OTUPUT:
<img width="911" height="341" alt="Screenshot 2026-02-20 113859" src="https://github.com/user-attachments/assets/c4a39424-79fb-4b28-97e3-4f0c2342cf35" />
