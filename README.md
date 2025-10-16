const express = require('express');
const cors = require('cors');
const app = express();

app.use(cors());

// Dummy product data
const products = [
  { id: 1, name: 'Laptop', price: 1200 },
  { id: 2, name: 'Mouse', price: 25 },
  { id: 3, name: 'Keyboard', price: 45 },
];

// API endpoint
app.get('/api/products', (req, res) => {
  res.json(products);
});

const PORT = 5000;
app.listen(PORT, () => console.log(`Server running on http://localhost:${PORT}`));
cd backend
npm init -y
npm install express cors
node server.js
npx create-react-app frontend
cd frontend
npm install axios
import React from 'react';
import ProductList from './ProductList';
import './App.css';

function App() {
  return (
    <div className="App">
      <h1>Product List</h1>
      <ProductList />
    </div>
  );
}

export default App;
import React, { useEffect, useState } from 'react';
import axios from 'axios';
import './App.css';

const ProductList = () => {
  const [products, setProducts] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    axios.get('http://localhost:5000/api/products')
      .then(response => {
        setProducts(response.data);
        setLoading(false);
      })
      .catch(() => {
        setError('Failed to fetch products');
        setLoading(false);
      });
  }, []);

  if (loading) return <p>Loading products...</p>;
  if (error) return <p>{error}</p>;

  return (
    <div className="product-container">
      {products.map(product => (
        <div key={product.id} className="product-card">
          <h2>{product.name}</h2>
          <p>Price: ${product.price}</p>
          <button className="buy-btn">Buy Now</button>
        </div>
      ))}
    </div>
  );
};

export default ProductList;
body {
  background-color: #1f1f1f;
  color: white;
  font-family: Arial, sans-serif;
}

h1 {
  text-align: center;
  margin-top: 20px;
}

.product-container {
  display: flex;
  justify-content: center;
  gap: 20px;
  margin: 40px;
}

.product-card {
  background-color: #2c2c2c;
  border: 1px solid #444;
  border-radius: 10px;
  padding: 20px;
  width: 200px;
  text-align: center;
}

.buy-btn {
  background-color: #007bff;
  border: none;
  padding: 10px;
  color: white;
  font-weight: bold;
  cursor: pointer;
  border-radius: 5px;
  margin-top: 10px;
}

.buy-btn:hover {
  background-color: #0056b3;
}
