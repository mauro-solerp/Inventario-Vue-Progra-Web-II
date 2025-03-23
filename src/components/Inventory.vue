<script setup>
import { reactive, watch } from 'vue';

const inventory = reactive([
  { nombre: 'Producto (1)', precio: 100, stock: 5, disponible: true },
  { nombre: 'Producto (2)', precio: 200, stock: 0, disponible: false },
  { nombre: 'Producto (3)', precio: 150, stock: 10, disponible: true },

]);

watch(
  inventory,
  (newVal) => {
    newVal.forEach((product) => {
      product.disponible = product.stock > 0;
    });
  },
  { deep: true }
);

const reducirStock = (producto) => {
  if (producto.stock > 0) {
    producto.stock--;
  }
};

const aumentarStock = (producto) => {
  producto.stock++;
};
</script>

<template>
  <div class="inventory-container">
    <h2 class="title">Inventario de la Tienda</h2>
    <div class="cards-grid">
      <div 
        v-for="producto in inventory" 
        :key="producto.nombre" 
        class="card"
      >
        <div class="card-header">
          <strong class="product-name">{{ producto.nombre }}</strong>
          <span 
            class="status-badge" 
            :class="{ 'available': producto.disponible, 'unavailable': !producto.disponible }"
          >
            {{ producto.disponible ? 'Disponible' : 'Agotado' }}
          </span>
        </div>
        <div class="card-body">
          <p class="price">Precio: <span>${{ producto.precio }}</span></p>
          <p class="stock">Stock: <span>{{ producto.stock }}</span></p>
        </div>
        <div class="card-footer">
          <button 
            class="btn decrease" 
            @click="reducirStock(producto)"
          >-</button>
          <button 
            class="btn increase" 
            @click="aumentarStock(producto)"
          >+</button>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.inventory-container {
  padding: 20px;
  max-width: 1200px;
  justify-content: center;
  margin: 0 auto;
}

.title {
  text-align: center;
  color: #333;
  margin-bottom: 30px;
  font-size: 2rem;
}

.cards-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
  gap: 20px;
}

.card {
  background: white;
  border-radius: 10px;
  padding: 30px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  transition: transform 0.2s;
}

.card:hover {
  transform: translateY(-5px);
}

.card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 10px;
}

.product-name {
  font-size: 1.2rem;
  color: #2c3e50;
}

.status-badge {
  padding: 4px 8px;
  border-radius: 12px;
  font-size: 0.8rem;
  font-weight: 500;
}

.available {
  background: #e6ffe6;
  color: #27ae60;
}

.unavailable {
  background: #ffe6e6;
  color: #c0392b;
}

.card-body {
  margin-bottom: 15px;
}

.price, .stock {
  margin: 5px 0;
  color: #666;
}

.price span {
  color: #2980b9;
  font-weight: 600;
}

.stock span {
  color: #8e44ad;
  font-weight: 600;
}

.card-footer {
  display: flex;
  justify-content: center;
  gap: 10px;
}

.btn {
  padding: 8px 15px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-weight: 600;
  transition: background-color 0.2s;
}

.decrease {
  background: #e74c3c;
  color: white;
}

.decrease:hover {
  background: #c0392b;
}

.increase {
  background: #2ecc71;
  color: white;
}

.increase:hover {
  background: #27ae60;
}
</style>
