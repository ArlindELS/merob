<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>McDonald's Style Menu</title>
    <style>
        /* Add your CSS styles here */
        body {
            font-family: 'Arial', sans-serif;
        }

        #menu {
            display: flex;
            flex-wrap: wrap;
        }

        .menu-item {
            border: 1px solid #ddd;
            border-radius: 8px;
            margin: 10px;
            padding: 15px;
            width: 200px;
            text-align: center;
        }

        .menu-item img {
            max-width: 100%;
            border-radius: 5px;
            margin-bottom: 10px;
        }

        button {
            background-color: #4CAF50;
            color: white;
            padding: 10px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }
    </style>
</head>
<body>
    <div id="app">
        <h1>McDonald's Style Menu</h1>
        <div id="menu"></div>
        <div id="cart"></div>
        <button onclick="checkout()">Checkout</button>
    </div>

    <script>
        // Mock data for McDonald's style menu items
        const menuItems = [
            { id: 1, name: 'Big Mac', price: 5.99, image: 'big_mac.jpg' },
            { id: 2, name: 'McChicken', price: 4.49, image: 'mcchicken.jpg' },
            { id: 3, name: 'French Fries', price: 2.29, image: 'fries.jpg' },
            // Add more menu items
        ];

        // User's cart
        let cart = [];

        // Display McDonald's style menu items
        const menuElement = document.getElementById('menu');
        menuItems.forEach(item => {
            const itemElement = document.createElement('div');
            itemElement.classList.add('menu-item');
            itemElement.innerHTML = `
                <img src="${item.image}" alt="${item.name}">
                <p>${item.name}</p>
                <p>$${item.price.toFixed(2)}</p>
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
                cartItemElement.innerHTML = `${item.name} x${item.quantity} - $${(item.price * item.quantity).toFixed(2)}`;
                cartElement.appendChild(cartItemElement);
            });
        }

        // Checkout
        function checkout() {
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
            return cart.map(item => `${item.name} x${item.quantity} - $${(item.price * item.quantity).toFixed(2)}`).join('\n');
        }
    </script>
</body>
</html>
