# MULENYAS — Web Editorial

> Alta costura de verano en rafia, yute y flecos.

## 🎯 Filosofía de diseño

Esta web **NO es una tienda**. Es una **experiencia editorial** con posibilidad de compra.

### Inspiración
- **Loewe** (craft como lenguaje)
- **Miu Miu** (energía joven, icono raro)
- **Paloma Wool** (editorial, ritmo, silencio)
- **Laagam** (impacto visual fuerte)

### Principios
- Scroll lento
- Mucho espacio en blanco
- Tipografía serif elegante
- Sin menús tradicionales
- Shopify invisible hasta el checkout

---

## 📁 Estructura de archivos

```
mulenyas-web/
├── index.html          # HOME (hero + escenas)
├── styles.css          # Estilos completos
├── 404.html            # Página de error
├── CNAME               # Tu dominio (si lo tienes)
└── assets/
    ├── hero.jpg                 # Imagen hero principal (1920x1200px mín)
    ├── craft-detail.jpg         # Segunda escena
    ├── mule-1.jpg              # Producto 1
    ├── mule-2.jpg              # Producto 2
    ├── mule-3.jpg              # Producto 3
    ├── story-image.jpg         # Imagen story
    ├── hands-weaving.jpg       # Craft 1
    ├── materials.jpg           # Craft 2
    └── workshop.jpg            # Craft 3
```

---

## 🚀 Cómo subir a GitHub Pages

### 1. Crear repositorio nuevo
```bash
# En GitHub, crea un repo nuevo llamado "mulenyas-web"
# Clónalo en tu ordenador:
git clone https://github.com/TU-USUARIO/mulenyas-web.git
cd mulenyas-web
```

### 2. Copiar estos archivos
```bash
# Copia estos 3 archivos al repositorio:
# - index.html
# - styles.css
# - 404.html

# Crea la carpeta assets/
mkdir assets
```

### 3. Añadir tus imágenes
Sube tus fotos a la carpeta `assets/` con estos nombres:
- `hero.jpg` → Imagen principal (grande, vertical mejor)
- `craft-detail.jpg` → Detalle artesanal
- `mule-1.jpg`, `mule-2.jpg`, `mule-3.jpg` → Productos
- `story-image.jpg` → Imagen del manifiesto
- `hands-weaving.jpg`, `materials.jpg`, `workshop.jpg` → Proceso

**Recomendaciones de imagen:**
- Resolución mínima: 1920px de ancho
- Formato: JPG (optimizado para web)
- Peso: máximo 500KB por imagen (usa TinyPNG para comprimir)

### 4. Subir a GitHub
```bash
git add .
git commit -m "Primera versión Mulenyas web"
git push origin main
```

### 5. Activar GitHub Pages
1. Ve a tu repositorio en GitHub
2. Settings → Pages
3. Source: `main` / `root`
4. Save

Tu web estará en: `https://TU-USUARIO.github.io/mulenyas-web/`

---

## 🛍️ Integración con Shopify (sin romper la magia)

### Estrategia: Shopify invisible

**Lo que vamos a hacer:**
- La web vive en GitHub (la experiencia)
- Shopify solo aparece en el checkout (el motor)

### Pasos exactos

#### 1. Crear cuenta Shopify
- Ve a Shopify.com
- Crea una cuenta (14 días gratis)
- Configura tu tienda

#### 2. Añadir productos en Shopify
- Crea tus 3 productos (Rafia Natural, Yute con Flecos, Rafia Oscura)
- Sube imágenes
- Añade descripciones
- Configura precios y stock

#### 3. Generar Buy Buttons
En Shopify Admin:
1. Sales Channels → Add channel → "Buy Button"
2. Create Buy Button → selecciona producto
3. Customize (quita todo lo que puedas: título, descripción visible)
4. Copy code

#### 4. Integrar en el HTML

Reemplaza los botones "Acquire" por el código de Shopify:

```html
<!-- Antes (botón genérico) -->
<button class="btn-acquire">Acquire</button>

<!-- Después (con Shopify) -->
<div id="product-component-1"></div>
<script type="text/javascript">
  (function () {
    var scriptURL = 'https://sdks.shopifycdn.com/buy-button/latest/buy-button-storefront.min.js';
    if (window.ShopifyBuy) {
      if (window.ShopifyBuy.UI) {
        ShopifyBuyInit();
      } else {
        loadScript();
      }
    } else {
      loadScript();
    }
    function loadScript() {
      var script = document.createElement('script');
      script.async = true;
      script.src = scriptURL;
      (document.getElementsByTagName('head')[0] || document.getElementsByTagName('body')[0]).appendChild(script);
      script.onload = ShopifyBuyInit;
    }
    function ShopifyBuyInit() {
      var client = ShopifyBuy.buildClient({
        domain: 'TU-TIENDA.myshopify.com',
        storefrontAccessToken: 'TU-TOKEN-AQUI',
      });
      ShopifyBuy.UI.onReady(client).then(function (ui) {
        ui.createComponent('product', {
          id: 'TU-PRODUCTO-ID',
          node: document.getElementById('product-component-1'),
          options: {
            product: {
              styles: {
                button: {
                  'font-family': 'Inter, sans-serif',
                  'font-size': '12px',
                  'padding': '14px 32px',
                  'background-color': '#2A2823',
                  'color': '#FBF9F4',
                  ':hover': {
                    'background-color': '#B8644D'
                  }
                }
              },
              text: {
                button: 'Acquire'
              }
            }
          }
        });
      });
    }
  })();
</script>
```

#### 5. Personalizar el checkout
En Shopify Admin:
- Settings → Checkout
- Añade tu logo
- Personaliza colores (marfil, negro lavado, terracota)
- Activa el dominio personalizado si lo tienes

---

## 🎨 Personalización

### Cambiar colores
Edita `styles.css` en la sección de variables:

```css
:root {
  --marfil: #FBF9F4;        /* Fondo principal */
  --arena: #E8DFD0;          /* Fondo secundario */
  --negro-lavado: #2A2823;   /* Texto principal */
  --terracota: #B8644D;      /* Acento (botones hover) */
}
```

### Cambiar textos
Edita `index.html` directamente. Los textos principales están en:
- `.hero-text h1` → Frase hero
- `.scene-text .lead` → Segunda escena
- `.story-block p` → Bloques del manifiesto

### Añadir más productos
Duplica el bloque `.product` en el HTML:

```html
<article class="product">
  <div class="product-image">
    <img src="assets/mule-4.jpg" alt="Nuevo modelo" />
  </div>
  <div class="product-info">
    <h3>Nombre del Modelo</h3>
    <p class="product-desc">Descripción breve.<br>Máximo dos líneas.</p>
    <button class="btn-acquire">Acquire</button>
  </div>
</article>
```

---

## ✅ Checklist de lanzamiento

- [ ] Todas las imágenes subidas y optimizadas
- [ ] Textos revisados (sin errores)
- [ ] Email de contacto actualizado
- [ ] Link de Instagram correcto
- [ ] Shopify configurado y testeado
- [ ] Dominio personalizado conectado (opcional)
- [ ] Probado en móvil y desktop
- [ ] Meta tags para SEO añadidos
- [ ] Google Analytics configurado (opcional)

---

## 📱 Responsive

La web está completamente optimizada para móvil:
- Hero vertical
- Grid de productos en columna única
- Navegación adaptada
- Imágenes responsivas

---

## 🎯 Próximos pasos

1. **Journal** (blog editorial)
2. **Lookbook** (galería de fotos)
3. **Animaciones scroll** (más sofisticadas)
4. **Newsletter** (MailChimp o similar)

---

## 💛 Notas finales

Esta web está diseñada para **sentirse como un editorial de moda con posibilidad de compra**, no como una tienda con fotos bonitas.

Cada elemento respira. Cada espacio importa. Cada palabra pesa.

**No es sobre vender. Es sobre crear deseo.**

---

*Hecho con calma para Mulenyas*
