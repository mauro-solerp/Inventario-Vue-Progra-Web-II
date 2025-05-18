<script setup>
import '../styles/inventory.css';
import { reactive, onMounted } from 'vue';

const inventario = reactive([]);

const URL_DE_GRAPHQL = 'http://localhost:5000/graphql';

async function hacerConsultaGraphQL(consulta, variables = {}) {
  const respuesta = await fetch(URL_DE_GRAPHQL, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ query: consulta, variables: variables }),
  });

  const resultadoJson = await respuesta.json();

  if (resultadoJson.errors) {
    console.error('Errores en la consulta GraphQL:', resultadoJson.errors);
    throw new Error('Ocurrió un error con la consulta GraphQL');
  }

  return resultadoJson;
}

async function cargarListaDeProductos() {
  const consulta = `
    query {
      productos {
        id
        nombre
        precio
        stock
        disponible
      }
    }
  `;

  const datos = await hacerConsultaGraphQL(consulta);

  inventario.splice(0, inventario.length, ...datos.productos);
}

async function cambiarStockDelProducto(idDelProducto, cantidadACambiar) {
  const mutacion = `
    mutation ($id: Int!, $cantidad: Int!) {
      modificarStock(id: $id, cantidad: $cantidad) {
        ok
        mensaje
        producto {
          id
          nombre
          precio
          stock
          disponible
        }
      }
    }
  `;

  const variablesParaMutacion = { id: idDelProducto, cantidad: cantidadACambiar };

  const datosDeMutacion = await hacerConsultaGraphQL(mutacion, variablesParaMutacion);

  if (!datosDeMutacion.modificarStock.ok) {
    alert(datosDeMutacion.modificarStock.mensaje);
    return;
  }

  const posicionDelProducto = inventario.findIndex(function(productoEnInventario) {
    return productoEnInventario.id === idDelProducto;
  });

  if (posicionDelProducto !== -1) {
    inventario[posicionDelProducto] = datosDeMutacion.modificarStock.producto;
  }
}

function disminuirStockDeProducto(producto) {
  if (producto.stock > 0) {
    cambiarStockDelProducto(producto.id, -1);
  }
}

function aumentarStockDeProducto(producto) {
  cambiarStockDelProducto(producto.id, 1);
}

onMounted(function() {
  cargarListaDeProductos();
});
</script>
<template>
  <div class="inventory-container">
    <h2 class="title">Inventario de la Tienda</h2>
    <div class="cards-grid">
      <div
        v-for="producto in inventario"
        :key="producto.id"
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
            @click="disminuirStockDeProducto(producto)"
            :disabled="producto.stock === 0"
          >-</button>
          <button
            class="btn increase"
            @click="aumentarStockDeProducto(producto)"
          >+</button>
        </div>
      </div>
    </div>
  </div>
</template>

