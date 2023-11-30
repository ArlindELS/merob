# Interactive Restaurant App

Welcome to the Interactive Restaurant App! This web application allows users to view a restaurant menu, add items to their cart, and proceed with the checkout. The project includes both frontend (HTML, CSS, JavaScript) and backend (Python with Flask) components.

## Features

- **Menu Display:** Browse through a list of menu items with details like name and price.
- **Ordering System:** Add items to the cart and proceed with the checkout.
- **Cart Management:** View and manage items in the cart.
- **Interactive Elements:** Users can customize the quantity of items in their order.

## Usage

### Frontend (HTML, CSS, JavaScript)

#### HTML (`index.html`)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Interactive Restaurant App</title>
    <style>
        /* Add your CSS styles here */
    </style>
</head>
<body>
    <div id="app">
        <h1>Interactive Restaurant Menu</h1>
        <div id="menu">
            <!-- Display menu items here -->
        </div>
        <div id="cart">
            <!-- Display user's cart and order summary here -->
        </div>
        <button onclick="checkout()">Checkout</button>
    </div>

    <script>
        // Mock data for menu items
        const menuItems = [
            { id: 1, name: 'Dish 1', price: 10.99 },
            { id: 2, name: 'Dish 2', price: 8.99 },
            // Add more menu items
        ];

        // User's cart
        let cart = [];

        // Display menu items
        const menuElement = document.getElementById('menu');
        menuItems.forEach(item => {
            const itemElement = document.createElement('div');
            itemElement.innerHTML = `
                <p>${item.name} - $${item.price}</p>
                <label for="quantity-${item.id}">Quantity:</label>
                <input type="number" id="quantity-${item.id}" value="1" min="1">
                <button onclick="addToCart(${item.id})">Add to Cart</button>`;
            menuElement.appendChild(itemElement);
        });

        // Add item to the cart
        function addToCart(itemId) {
            const selectedItem = menuItems.find(item => item.id === itemId);
            const quantity = document.getElementById(`quantity-${itemId}`).value;
            selectedItem.quantity = parseInt(quantity, 10);
            cart.push(selectedItem);
            updateCart();
        }

        // Update cart display
        function updateCart() {
            const cartElement = document.getElementById('cart');
            cartElement.innerHTML = '<h2>Cart</h2>';
            cart.forEach(item => {
                const cartItemElement = document.createElement('div');
                cartItemElement.innerHTML = `${item.name} x${item.quantity} - $${item.price * item.quantity}`;
                cartElement.appendChild(cartItemElement);
            });
        }

        // Checkout
        async function checkout() {
        if (cart.length === 0) {
            alert('Your cart is empty. Please add items before checking out.');
            return;
        }

        // Calculate total amount
        const totalAmount = cart.reduce((total, item) => total + item.price * item.quantity, 0);

        // Display order summary
        const confirmation = confirm(`Order Summary:\n\n${getOrderSummary()}\nTotal Amount: $${totalAmount.toFixed(2)}\n\nProceed with the checkout?`);

        if (confirmation) {
            // Simulate payment (you would integrate a payment gateway in a real scenario)
            alert('Payment successful! Your order has been confirmed.');

            // Clear the cart after successful checkout
            cart = [];
            updateCart();
        } else {
            alert('Checkout canceled.');
        }
    }

    // Helper function to get order summary
    function getOrderSummary() {
        return cart.map(item => `${item.name} x${item.quantity} - $${item.price * item.quantity}`).join('\n');
    }
    </script>
</body>
</html>
