import React, { useState } from "react";
import * as XLSX from "xlsx";

// Component for Login page
const LoginPage = ({ onLogin }) => {
  const handleLogin = (role) => {
    // Login logic
    onLogin(role);
  };

  return (
    <div>
      <h2>Login</h2>
      <button onClick={() => handleLogin("admin")}>Login as Admin</button>
      <button onClick={() => handleLogin("user")}>Login as User</button>
    </div>
  );
};

// Component for displaying products and shopping cart
const OrderPage = () => {
  const [products, setProducts] = useState([]);
  const [cart, setCart] = useState([]);
  const [userData, setUserData] = useState({ name: "", phone: "" });

  // Handle file upload
  const handleFileUpload = (e) => {
    const file = e.target.files[0];
    const reader = new FileReader();
    reader.onload = (e) => {
      const data = e.target.result;
      const workbook = XLSX.read(data, { type: "binary" });
      const sheet = workbook.Sheets[workbook.SheetNames[0]];
      const jsonData = XLSX.utils.sheet_to_json(sheet);
      setProducts(jsonData); // Store product data
    };
    reader.readAsBinaryString(file);
  };

  // Add to cart
  const addToCart = (product) => {
    setCart((prevCart) => [...prevCart, product]);
  };

  // Remove from cart
  const removeFromCart = (productIndex) => {
    setCart((prevCart) => prevCart.filter((_, index) => index !== productIndex));
  };

  // Update quantity in cart
  const updateQuantity = (productIndex, quantity) => {
    const newCart = [...cart];
    newCart[productIndex].quantity = quantity;
    setCart(newCart);
  };

  // Calculate total price
  const calculateTotal = () => {
    return cart.reduce((total, product) => total + (product.price * product.quantity), 0);
  };

  // Handle form submit for user data
  const handleSubmit = (e) => {
    e.preventDefault();
    console.log("User Data: ", userData);
    // You can implement further actions like sending data to a backend
  };

  // Generate Excel file of cart
  const exportCartToExcel = () => {
    const ws = XLSX.utils.json_to_sheet(cart);
    const wb = XLSX.utils.book_new();
    XLSX.utils.book_append_sheet(wb, ws, "Cart");
    XLSX.writeFile(wb, "cart.xlsx");
  };

  // Generate a WhatsApp or Telegram link to send cart
  const sendToWhatsApp = () => {
    const cartDetails = cart.map(
      (item) => `${item.name} - ${item.quantity} x ${item.price} = ${item.quantity * item.price}`
    ).join("\n");
    const message = encodeURIComponent(`Your Cart:\n${cartDetails}\nTotal: ${calculateTotal()} IRR`);
    window.open(`https://wa.me/1234567890?text=${message}`, "_blank");
  };

  const sendToTelegram = () => {
    const cartDetails = cart.map(
      (item) => `${item.name} - ${item.quantity} x ${item.price} = ${item.quantity * item.price}`
    ).join("\n");
    const message = encodeURIComponent(`Your Cart:\n${cartDetails}\nTotal: ${calculateTotal()} IRR`);
    window.open(`https://t.me/yourtelegramusername?text=${message}`, "_blank");
  };

  return (
    <div>
      <h2>Order Page</h2>
      <input type="file" accept=".xlsx" onChange={handleFileUpload} />
      <div>
        {products.map((product, index) => (
          <div key={index}>
            <h3>{product.name}</h3>
            <p>Price: {product.price} IRR</p>
            <button onClick={() => addToCart({ ...product, quantity: 1 })}>Add to Cart</button>
          </div>
        ))}
      </div>

      <h3>Shopping Cart</h3>
      <div>
        {cart.map((product, index) => (
          <div key={index}>
            <p>{product.name} - {product.quantity} x {product.price} = {product.quantity * product.price} IRR</p>
            <button onClick={() => removeFromCart(index)}>Remove</button>
            <button onClick={() => updateQuantity(index, product.quantity + 1)}>Increase Quantity</button>
            <button onClick={() => updateQuantity(index, product.quantity - 1)}>Decrease Quantity</button>
          </div>
        ))}
      </div>

      <div>
        <p>Total: {calculateTotal()} IRR</p>
      </div>

      <form onSubmit={handleSubmit}>
        <input
          type="text"
          placeholder="Name"
          value={userData.name}
          onChange={(e) => setUserData({ ...userData, name: e.target.value })}
        />
        <input
          type="text"
          placeholder="Phone Number"
          value={userData.phone}
          onChange={(e) => setUserData({ ...userData, phone: e.target.value })}
        />
        <button type="submit">Submit</button>
      </form>

      <button onClick={exportCartToExcel}>Export Cart to Excel</button>
      <button onClick={sendToWhatsApp}>Send to WhatsApp</button>
      <button onClick={sendToTelegram}>Send to Telegram</button>
    </div>
  );
};

// Main App component
const App = () => {
  const [role, setRole] = useState(null);

  const handleLogin = (role) => {
    setRole(role);
  };

  return (
    <div>
      {!role ? (
        <LoginPage onLogin={handleLogin} />
      ) : (
        <OrderPage />
      )}
    </div>
  );
};

export default App;
