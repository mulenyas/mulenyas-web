# 🛍️ GUÍA COMPLETA: Integración Shopify + GitHub

## Objetivo

Crear una experiencia donde:
- ✅ La web vive en GitHub (control total del diseño)
- ✅ Shopify solo aparece en el checkout (invisible hasta el último momento)
- ✅ El usuario no siente que está en una "tienda Shopify"

---

## PARTE 1: Configurar Shopify

### Paso 1: Crear cuenta Shopify

1. Ve a [shopify.com](https://www.shopify.com)
2. "Iniciar prueba gratuita" (14 días)
3. Crea tu tienda: `mulenyas.myshopify.com`

**Configuración inicial:**
- Nombre de la tienda: "Mulenyas"
- Moneda: EUR (€)
- País: España
- Idioma: Español

### Paso 2: Añadir productos

En Shopify Admin → Products → Add product

**Producto 1: Rafia Natural**
- Title: `Rafia Natural`
- Description: 
  ```
  Tejido a mano. Suela de esparto.
  El gesto más simple.
  
  Materiales: Rafia natural, esparto
  Hecho a mano en España
  Producción limitada
  ```
- Price: `89.00 EUR`
- Compare at price: (dejar vacío)
- SKU: `MUL-RF-NAT-001`
- Inventory: Gestionar stock (activar)
- Quantity: según tu stock

**Producto 2: Yute con Flecos**
- Title: `Yute con Flecos`
- Description:
  ```
  Movimiento suave. Textura viva.
  Para caminar despacio.
  
  Materiales: Yute, flecos naturales, esparto
  Hecho a mano en España
  Producción limitada
  ```
- Price: `95.00 EUR`
- SKU: `MUL-YT-FLC-001`

**Producto 3: Rafia Oscura**
- Title: `Rafia Oscura`
- Description:
  ```
  La misma mulanya,
  otro momento del día.
  
  Materiales: Rafia tintada, esparto
  Hecho a mano en España
  Producción limitada
  ```
- Price: `89.00 EUR`
- SKU: `MUL-RF-OSC-001`

**IMPORTANTE:** Sube las mismas imágenes que usas en tu web GitHub.

### Paso 3: Configurar envíos

Settings → Shipping and delivery

**España (Península):**
- Tarifa plana: 5.00 EUR
- Envío gratis: pedidos > 150 EUR

**Islas Baleares / Canarias:**
- Tarifa: 10.00 EUR

**Internacional (Europa):**
- Tarifa: 15.00 EUR
- Solo si quieres vender fuera

### Paso 4: Configurar métodos de pago

Settings → Payments

**Activar:**
- ✅ Shopify Payments (tarjetas)
- ✅ PayPal (opcional pero recomendado)

**Configurar Shopify Payments:**
- Añade tus datos bancarios
- Activa 3D Secure (seguridad)

---

## PARTE 2: Crear Buy Buttons

### Paso 1: Activar canal Buy Button

1. Shopify Admin → Sales channels
2. Click en "+" (Add channel)
3. Busca "Buy Button"
4. "Add channel"

### Paso 2: Crear botón para cada producto

Para **Producto 1 (Rafia Natural):**

1. Buy Button → Create Buy Button
2. Select product → "Rafia Natural"
3. Customize appearance:
   - **Layout:** Standalone button
   - **Button text:** "Acquire"
   - **Show product title:** NO
   - **Show product description:** NO
   - **Show product image:** NO
   - **Show variant selector:** SI (si tienes tallas)

4. **Customize styles:**
   ```css
   Button background: #2A2823
   Button text: #FBF9F4
   Button font: Inter
   Button size: 14px
   Button padding: 14px 32px
   ```

5. Click "Generate code"

6. **Copiar el código completo** (lo usaremos luego)

Repite esto para los otros 2 productos.

---

## PARTE 3: Integrar en tu web GitHub

### Opción A: Buy Button simple (recomendado para empezar)

#### 1. Añadir el SDK de Shopify

En `index.html`, justo antes de `</body>`:

```html
<!-- Shopify Buy Button SDK -->
<script src="https://sdks.shopifycdn.com/buy-button/latest/buy-button-storefront.min.js"></script>

</body>
</html>
```

#### 2. Reemplazar botones por divs

En cada producto, cambia:

```html
<!-- ANTES -->
<button class="btn-acquire">Acquire</button>

<!-- DESPUÉS -->
<div id="product-rafia-natural"></div>
```

Para los 3 productos:
- Producto 1: `id="product-rafia-natural"`
- Producto 2: `id="product-yute-flecos"`
- Producto 3: `id="product-rafia-oscura"`

#### 3. Añadir script de inicialización

En `index.html`, después del SDK (antes de `</body>`):

```html
<script>
(function() {
  var client = ShopifyBuy.buildClient({
    domain: 'TU-TIENDA.myshopify.com',
    storefrontAccessToken: 'TU-TOKEN-AQUI'
  });

  ShopifyBuy.UI.onReady(client).then(function(ui) {
    
    // PRODUCTO 1: Rafia Natural
    ui.createComponent('product', {
      id: 'PRODUCT-ID-1',
      node: document.getElementById('product-rafia-natural'),
      options: {
        product: {
          styles: {
            button: {
              'font-family': 'Inter, sans-serif',
              'font-size': '12px',
              'font-weight': '400',
              'padding': '14px 32px',
              'text-transform': 'uppercase',
              'letter-spacing': '0.15em',
              'background-color': '#2A2823',
              'color': '#FBF9F4',
              'border-radius': '0',
              ':hover': {
                'background-color': '#B8644D'
              }
            }
          },
          text: {
            button: 'Acquire'
          },
          contents: {
            img: false,
            title: false,
            price: false
          }
        },
        cart: {
          styles: {
            button: {
              'background-color': '#2A2823',
              ':hover': {
                'background-color': '#B8644D'
              }
            }
          }
        }
      }
    });

    // PRODUCTO 2: Yute con Flecos
    ui.createComponent('product', {
      id: 'PRODUCT-ID-2',
      node: document.getElementById('product-yute-flecos'),
      options: {
        // ... mismas opciones
      }
    });

    // PRODUCTO 3: Rafia Oscura
    ui.createComponent('product', {
      id: 'PRODUCT-ID-3',
      node: document.getElementById('product-rafia-oscura'),
      options: {
        // ... mismas opciones
      }
    });

  });
})();
</script>
```

#### 4. Obtener los valores correctos

**Domain:** Tu tienda Shopify
- Formato: `mulenyas.myshopify.com`
- Encuéntralo en: Settings → Domains

**Storefront Access Token:**
1. Shopify Admin → Apps
2. "Develop apps" (arriba a la derecha)
3. "Create an app"
4. Nombre: "GitHub Website"
5. Configure → Storefront API
6. Marca: `unauthenticated_read_product_listings`
7. Save
8. Copia el token que aparece

**Product IDs:**
1. En Buy Button, cuando generas el código
2. Busca la línea: `id: '1234567890123'`
3. Ese es tu Product ID

---

## PARTE 4: Personalizar el Checkout

### Diseño del Checkout

Settings → Checkout

**Logo:**
- Sube tu logo (PNG transparente, 200x80px)

**Colors:**
- Primary: `#2A2823` (negro lavado)
- Secondary: `#B8644D` (terracota)
- Background: `#FBF9F4` (marfil)

**Typography:**
- Headings: "Georgia" (lo más cercano a Cormorant)
- Body: "Helvetica"

**Custom CSS** (si tienes plan Shopify Plus):
```css
.main__content {
  font-family: Georgia, serif;
}

.btn {
  font-family: 'Helvetica Neue', sans-serif;
  text-transform: uppercase;
  letter-spacing: 0.15em;
  font-size: 12px;
}
```

### Email de confirmación

Settings → Notifications → Order confirmation

Personaliza:
- Asunto: "Tu pedido Mulenyas — Confirmación"
- Añade tu tono de voz (cálido, artesanal)

---

## PARTE 5: Conectar dominio personalizado

### Si tienes dominio propio (mulenyas.com)

#### En tu proveedor de dominio (ej: GoDaddy, Namecheap):

1. Panel de control → DNS Settings
2. Añade estos registros:

**Para GitHub Pages:**
```
Type: A
Name: @
Value: 185.199.108.153

Type: A
Name: @
Value: 185.199.109.153

Type: A
Name: @
Value: 185.199.110.153

Type: A
Name: @
Value: 185.199.111.153

Type: CNAME
Name: www
Value: TU-USUARIO.github.io
```

**Para Shopify (checkout):**
```
Type: CNAME
Name: shop
Value: shops.myshopify.com
```

#### En Shopify:

Settings → Domains → Connect existing domain
- Domain: `shop.mulenyas.com`

Ahora:
- `mulenyas.com` → Tu web en GitHub
- `shop.mulenyas.com/checkout` → Checkout de Shopify

---

## PARTE 6: Testing completo

### Checklist antes de lanzar:

- [ ] Productos visibles en la web
- [ ] Botón "Acquire" funciona (abre carrito)
- [ ] Carrito se muestra correctamente
- [ ] Checkout tiene tu branding
- [ ] Métodos de pago funcionan
- [ ] Envíos configurados correctamente
- [ ] Email de confirmación llega
- [ ] Probado en móvil y desktop
- [ ] Probado en Chrome, Safari, Firefox

### Test de compra:

1. Usa el modo "Test" de Shopify Payments
2. Tarjeta de prueba: `4242 4242 4242 4242`
3. Fecha: cualquier futura
4. CVV: cualquier 3 dígitos
5. Completa el pedido
6. Verifica que recibes el email

---

## PARTE 7: Mantenimiento

### Actualizar stock

Shopify Admin → Products → [Producto] → Variants → Update quantity

### Gestionar pedidos

Shopify Admin → Orders
- Ver pedidos nuevos
- Marcar como "Fulfilled" cuando envíes
- Añadir número de seguimiento

### Añadir nuevos productos

1. Crear producto en Shopify
2. Generar nuevo Buy Button
3. Añadir nuevo `<div id="product-nuevo"></div>` en HTML
4. Añadir script de inicialización
5. Subir a GitHub

---

## Resumen de URLs

Después de configurar todo:

| Qué                | URL                              |
|--------------------|----------------------------------|
| Web principal      | `https://mulenyas.com`           |
| GitHub Pages       | `https://tu-usuario.github.io/mulenyas-web/` |
| Shopify Admin      | `https://mulenyas.myshopify.com/admin` |
| Checkout (si dominio propio) | `https://shop.mulenyas.com/checkout` |

---

## Troubleshooting

### Los botones no aparecen

1. Verifica que el token sea correcto
2. Abre la consola del navegador (F12)
3. Busca errores de JavaScript
4. Verifica que los Product IDs sean correctos

### El carrito se ve feo

Ajusta los estilos en el script:
```javascript
cart: {
  styles: {
    button: {
      'background-color': '#2A2823'
    },
    cart: {
      'background-color': '#FBF9F4'
    }
  }
}
```

### Checkout en inglés

Settings → Languages → Change theme language → Spanish

---

## Próximos pasos avanzados

1. **Añadir carrito flotante** (mini cart)
2. **Wishlist** (productos favoritos)
3. **Quick view** (ver producto sin salir)
4. **Variantes** (tallas, colores) si produces varios
5. **Descuentos** (códigos promocionales)

---

*Esta guía está diseñada para mantener la experiencia Mulenyas intacta mientras usas Shopify como motor invisible.*
