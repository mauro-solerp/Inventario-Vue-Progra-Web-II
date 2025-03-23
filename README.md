## 1. Vue no detecta cambios dentro de objetos reactivos de la forma que esperarías. ¿Cómo podrías observar un cambio en una propiedad anidada?

Para observar cambios en propiedades anidadas dentro de un objeto reactivo en Vue, puedes utilizar la función `watch` con la opción `deep: true`. Esto permite que Vue observe cambios en todos los niveles del objeto, incluyendo propiedades anidadas.

### Ejemplo 
```javascript
import { reactive, watch } from 'vue';

const state = reactive({
  user: {
    name: 'John',
    age: 30
  }
});

watch(
  () => state.user,
  (newValue, oldValue) => {
    console.log('Cambio detectado en user:', newValue);
  },
  { deep: true }
);
```

## 2. watch() permite escuchar cambios en propiedades específicas dentro de reactive(), explica cómo funciona.

La función watch() en Vue permite observar cambios en propiedades específicas de un objeto reactivo. Cuando la propiedad observada cambia, se ejecuta una función de callback que recibe el nuevo valor y el valor anterior.

### Ejemplo

```javascript
import { reactive, watch } from 'vue';

const state = reactive({
  count: 0,
  message: 'Hola'
});

watch(
  () => state.count,
  (newValue, oldValue) => {
    console.log(`El contador cambió de ${oldValue} a ${newValue}`);
  }
);
```
## 3. ¿Cómo harías que un watch() detecte cambios en stock dentro de un array de productos?
Para detectar cambios en la propiedad stock dentro de un array de productos, puedes usar watch con la opción deep: true. Esto permitirá que Vue observe cambios en las propiedades anidadas de los objetos dentro del array.

### Ejemplo

```javascript
import { reactive, watch } from 'vue';

const state = reactive({
  productos: [
    { id: 1, nombre: 'Producto A', stock: 10 },
    { id: 2, nombre: 'Producto B', stock: 5 }
  ]
});

watch(
  () => state.productos,
  (newValue, oldValue) => {
    console.log('Cambio en el stock de los productos:', newValue);
  },
  { deep: true }
);
```