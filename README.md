print("=====================================================================")
print("WELCOME TO JOLLY CANTEEN SA PHINMA!")
print("=====================================================================")

#  REGISTER & LOGIN SYSTEM
users = {
    "charish": "12345"   # Default account user
}

def register():
    print("=====================================================================")
    print("REGISTER")
    print("=====================================================================")
    
    while True:
        new_user = input("Enter new username: ").strip()
        if new_user in users:
            print("=====================================================================")
            print("Username already exists! Please try another.")
            print("=====================================================================")
            continue

        new_pass = input("Enter new password: ").strip()
        confirm_pass = input("Confirm password: ").strip()

        if new_pass != confirm_pass:
            print("=====================================================================")
            print("Passwords do not match! Please try again.")
            print("=====================================================================")
            continue

        users[new_user] = new_pass
        print("=====================================================================")
        print(f"Account for '{new_user}' has been successfully created!")
        print("=====================================================================")
        break


def login():
    while True:
        choice = input("Do you want to (1)login or (2)register? ").strip()

        if choice == '2':
            register()

        elif choice == '1':
            username = input("Enter username: ").strip()
            password = input("Enter password: ").strip()

            if username == "" or password == "":
                print("=====================================================================")
                print("Please Try Again, this cannot be empty")
                print("=====================================================================")
                continue

            if username not in users:
                print("=====================================================================")
                print(f"Username '{username}' does not exist! Please register first.")
                print("=====================================================================")
                continue

            if users[username] == password:
                print("=====================================================================")
                print(f"WELCOME, {username}! You are now logged in.")
                print("=====================================================================")
                return True
            
            print("=====================================================================")
            print("Incorrect password! Please try again.")
            print("=====================================================================")

        else:
            print("=====================================================================")
            print("Only type '1' to login or '2' to register.")
            print("=====================================================================")


menu = {
    "C1": {"name": "AFTERNON", "details": "Chicken, Rice, Coke", "price": 60.00},
    "C2": {"name": "BEST COMBO", "details": "Beef Steak, Rice, Coke", "price": 65.00},
    "C3": {"name": "HAPPY MEAL", "details": "Spaghetti Meatball, French Fries, Coke", "price": 79.00},
    "C4": {"name": "SNACK MEAL", "details": "Yumburger, 2pcs Pizza, Coke", "price": 70.00},
    "C5": {"name": "SULIT 60", "details": "Jumbo Hotdog, Fries, Coke", "price": 60.00},
}

cart = {}  



def show_menu():
    print("Available Products:")
    print("----------------------------------------------------------------------------")
    for code, item in menu.items():
        print(f"{code:<5} {item['name']:<20} ₱{item['price']:>8,.2f} - {item['details']}")
    print("----------------------------------------------------------------------------")


def show_cart():
    if not cart:
        print("Your cart is empty.")
        return 0

    print("----------------------------------------------------------------------------")
    print("Your Cart:")
    total = 0
    for code, qty in cart.items():
        item = menu[code]
        subtotal = item["price"] * qty
        total += subtotal
        print(f"{code:<5} {item['name']:<20} ₱{item['price']:>8,.2f} x {qty:<3} = ₱{subtotal:>8,.2f}")
    print("----------------------------------------------------------------------------")
    print(f"Total: ₱{total:,.2f}")
    print("----------------------------------------------------------------------------")
    return total



def main():
    login()

    while True:
        print("""
----------------------------------------------------------------------------
Type:
(o) to show menu & add order
(v) to view cart
(c) to checkout
(x) to exit
----------------------------------------------------------------------------""")

        action = input("Enter option: ").lower().strip()

        if action == 'x':
            print("=====================================================================")
            print("Thank you for visiting JOLLY CANTEEN PHINMA! Come back again.")
            print("=====================================================================")
            break

        elif action == 'o':
            show_menu()
            while True:
                codes_input = input("Enter item codes (C1 C2 C3) or 'done' to finish: ").upper().strip()

                if codes_input == "DONE":
                    break

                codes = codes_input.split()

                for code in codes:
                    if code not in menu:
                        print("----------------------------------------------------------------------------")
                        print(f"Invalid code '{code}'! Skipping...")
                        print("----------------------------------------------------------------------------")
                        continue

                    try:
                        qty = int(input(f"Enter quantity for {code} - {menu[code]['name']}: "))
                        if qty <= 0:
                            print("----------------------------------------------------------------------------")
                            print("Quantity must be positive!")
                            print("----------------------------------------------------------------------------")
                            continue

                        cart[code] = cart.get(code, 0) + qty
                        print("----------------------------------------------------------------------------")
                        print(f"{qty} x {menu[code]['name']} added to cart.")
                        print("----------------------------------------------------------------------------")
                    except ValueError:
                        print("Please enter a valid number!")

        elif action == 'v':
            show_cart()

        elif action == 'c':
            if not cart:
                print("=====================================================================")
                print("Your cart is empty! Add something first.")
                print("=====================================================================")
                continue

            total = show_cart()
            confirm = input("Proceed to checkout? (yes/no): ").lower().strip()

            if confirm == "yes":
                print("=====================================================================")
                print("PAYMENT")
                print("=====================================================================")

                # PAYMENT LOOP
                while True:
                    try:
                        amount = float(input(f"Enter payment amount (Total = ₱{total:,.2f}): ₱"))
                        if amount < total:
                            print("NOT ENOUGH, please enter enough amount.")
                        else:
                            change = amount - total
                            print("=====================================================================")
                            print("                     PAYMENT SUCCESSFUL!")
                            print("=====================================================================")
                            print(f"Amount Paid: ₱{amount:,.2f}")
                            print(f"Total Cost:  ₱{total:,.2f}")
                            print(f"Change:      ₱{change:,.2f}")
                            print("=====================================================================")
                            print("Thank you for ordering at JOLLY CANTEEN PHINMA! See you next time.")
                            print("=====================================================================")

                            cart.clear()   
                            return

                    except ValueError:
                        print("----------------------------------------------------------------------------")
                        print("Please enter a valid number.")
                        print("----------------------------------------------------------------------------")

            elif confirm == "no":
                print("OKEY!., Returning to menu...")

            continue

        else:
            print("----------------------------------------------------------------------------")
            print("Invalid option. Please choose o, v, c, or x ONLY")
            print("----------------------------------------------------------------------------")


if __name__ == "__main__":
    main()
