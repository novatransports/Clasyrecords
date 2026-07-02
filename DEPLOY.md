# Clasy Récords — Deploy Guide

## 🚀 Paso 1: Subir a Netlify (2 min)

1. Descomprime el zip
2. Entra a https://app.netlify.com/drop
3. Arrastra la carpeta completa `clasy-records/`
4. Netlify te da una URL temporal tipo `abc123.netlify.app` — ábrela para verificar

## 🌐 Paso 2: Conectar `clasyrecords.com` (5 min)

**En Netlify:**
1. Site → Domain management → Add custom domain
2. Escribe `clasyrecords.com` → Verify → Yes, add domain
3. También agrega `www.clasyrecords.com` (Netlify lo redirige solo al apex gracias al `netlify.toml`)

**En Cloudflare (donde compraste el dominio):**
1. Entra a `clasyrecords.com` → DNS → Records
2. Elimina cualquier registro A o CNAME existente en el apex y www
3. Añade estos 2 registros:
   - **Type:** `CNAME` — **Name:** `@` — **Target:** `[tu-site].netlify.app` — **Proxy:** ⚠️ **DNS only** (nube gris, NO naranja)
   - **Type:** `CNAME` — **Name:** `www` — **Target:** `[tu-site].netlify.app` — **Proxy:** **DNS only**
4. Guarda

**En 10-30 min:** Netlify detectará el dominio, activará HTTPS automático (Let's Encrypt), y quedará vivo en `https://clasyrecords.com`

> **Importante:** el proxy naranja de Cloudflare NO es compatible con Netlify SSL. Déjalo en gris (DNS only). El SSL lo maneja Netlify.

## 📧 Paso 3: Crear el correo `hola@clasyrecords.com` (3 min, GRATIS)

Cloudflare Email Routing reenvía correos a tu Gmail personal sin costo:

1. En Cloudflare, dominio `clasyrecords.com` → Email → Email Routing → Get started
2. Cloudflare añade automáticamente los MX records
3. Create address → `hola` → forward to → `tu-correo-personal@gmail.com`
4. Verifica desde el email de confirmación de Gmail
5. Listo — cuando alguien mande a `hola@clasyrecords.com`, te llega a tu Gmail

## 📊 Paso 4: Activar Cloudflare Web Analytics (2 min)

1. Entra a Cloudflare Dashboard → Analytics & Logs → Web Analytics
2. Add site → `clasyrecords.com` → Done
3. Cloudflare te da un **token** (un string como `a1b2c3d4e5f6...`)
4. Abre `index.html` en tu editor
5. Busca el comentario `<!-- Cloudflare Web Analytics -->` (al final, antes de `</body>`)
6. Descomenta la línea del script y reemplaza `TU_TOKEN_AQUI` con el token real
7. Sube el archivo actualizado a Netlify (arrastra otra vez la carpeta o `netlify deploy`)

## 🔍 Paso 5: Meter en Google (opcional, 5 min)

1. Entra a https://search.google.com/search-console
2. Add property → `https://clasyrecords.com`
3. Verifica ownership (Netlify + Cloudflare permiten verificación por DNS)
4. Sitemaps → añade `https://clasyrecords.com/sitemap.xml`
5. En 24-72 horas Google empieza a indexar

## ✅ Checklist final

- [ ] Sitio subido a Netlify
- [ ] Dominio `clasyrecords.com` conectado y con HTTPS
- [ ] www redirige al apex
- [ ] Correo `hola@clasyrecords.com` funcionando
- [ ] Analytics activado con token real
- [ ] Sitemap enviado a Google Search Console
- [ ] Probado en móvil y desktop
- [ ] OG card se ve bien al compartir el link en WhatsApp (mándate el link a ti mismo para probar)

## 🆘 Si algo se rompe

- Sitio no carga → chequea DNS con https://dnschecker.org/#CNAME/clasyrecords.com
- HTTPS marca error → espera 15 min más, Netlify tarda en emitir el cert
- OG card no aparece en WhatsApp → limpia caché de WA: https://developers.facebook.com/tools/debug/ pega la URL y "Scrape Again"

## 📁 Archivos en este paquete

```
clasy-records/
├── index.html              — sitio principal
├── netlify.toml            — config Netlify (headers, cache, www redirect)
├── robots.txt              — permite indexado, apunta al sitemap
├── sitemap.xml             — mapa del sitio para Google
├── .htaccess               — config Apache (por si Netlify falla o migran a otro host)
└── assets/img/
    ├── clasy-logo.jpg      — logo brush stroke
    └── og-card.jpg         — imagen para preview en WhatsApp/redes
```
