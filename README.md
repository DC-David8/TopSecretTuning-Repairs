# Caja Top Secret: Tuning & Repairs · Taller mecánico

Herramienta web para el rol del servidor: los mecánicos del taller **Top Secret: Tuning & Repairs** cobran los trabajos desde la Caja, el encargado calcula lo que hay que pagar a cada empleado según su rango y lleva el control del almacén a partir de los logs de Discord.

**Web:** https://dc-david8.github.io/TopSecretTuning-Repairs/

> Basada en la Caja de [Aguila Motors](https://github.com/DC-David8/AguilaMotors) (versión 17), con el nombre, el logo y la paleta de Top Secret, y sus propios datos guardados aparte.

La versión que estás usando aparece al final de la página. Si no ves los últimos cambios, pulsa **Ctrl+F5** o ábrela en una ventana de incógnito.

## Qué hace

1. **Módulo de Caja** · Catálogo de 131 productos y servicios por pestañas. Se pulsa un producto para sumarlo al ticket (clic derecho para restar), se ajustan las cantidades y se copia el **total** o el **concepto** para pasarlo por el datáfono (máximo 40 caracteres, ver *Concepto para el datáfono*).
2. **Módulo de Pagos** · Se pega el texto de Discord con las facturas (datáfono TPV y el sistema de facturas antiguo) y las ventas de kits. Calcula el total cobrado por cada empleado, su comisión según el rango y el **Total a Pagar** a toda la plantilla. Se puede exportar a CSV.
3. **Módulo de Almacén** · Se pega el log de inventario de Discord (METER / SACAR) y muestra las existencias, quién mete y quién saca cada artículo (con su Discord) y el historial completo. Avisa cuando un artículo baja del mínimo.

## Catálogo de la Caja

| Pestaña | Contenido |
|---|---|
| Motos, Gama Baja, Gama Media, Gama Alta, Gama VIP, Camiones, Aéreos | 14 piezas de tuning por gama: pintura, neones, luces, ruedas, humo, carrocería, claxon, tintado, turbo, motor, transmisión, blindaje, suspensión y frenos |
| Full Tuning | El Full Tuning de cada gama, de $10.000 a $50.000 |

Los **Camiones** tienen los mismos precios que la Gama Alta, porque su precio de compra es parecido.
| Servicios (Taller) / Servicios (Grúa) | Reparaciones, limpiezas y repostajes, dentro y fuera del taller. Los kits de reparación no están: se venden desde el menú de kits del juego y se cuentan en Pagos → Venta de Kits |
| Tienda | La tienda junto al taller: Kit de Desvuelco ($1.500), Rueda ($500), Mando de Neones RGB ($2.000) y Bayetas ($400). Se compran a precio de taller y se revenden aquí |
| Camaleónica | Pintura camaleónica y cambios de color |
| Nitro | Instalación y botella. Venta restringida: la Caja muestra un aviso |
| Neones RGB | Instalación (el mando está en Tienda) |
| Compraventa | Cambio de papeles por gama |

Desde **Gestión de Productos** (con contraseña) se pueden agregar productos nuevos con su icono, cambiar precios e imágenes y ocultar o restaurar los fijos.

## Concepto para el datáfono

El datáfono solo acepta **40 caracteres** de concepto. Debajo del total del ticket se ve cuántos ocupa, y **Copiar concepto** elige solo:

1. **Completo**, si cabe: `x1 Turbo (Gama Alta)`.
2. **Abreviado**, si el completo no cabe. La gama o el lugar se escribe una vez al final: `Neon+Turbo+FT GA`, `4xRued+Turbo GB, Rep T`.
3. **En varios cobros**, si ni abreviado cabe: se abre una ventana con cada cobro (total y concepto) para pasarlos por el datáfono uno tras otro.

| Abreviatura | Significa |
|---|---|
| MT, GB, GM, GA, VIP, CAM, AE | Motos, Gama Baja, Media, Alta, VIP, Camiones, Aéreos |
| T / G | Taller / Grúa |
| Pint, Neon, Luces, Rued, Humo, Carr, Clax, Tint | Pintura, Neones, Cambio de Luces, Ruedas, Humo de Ruedas, Carrocería, Claxon, Tintado de Lunas |
| Turbo, Motor, Trans, Blind, Susp, Fren, FT | Turbo, Motor, Transmisión, Blindaje, Suspensión, Frenos, Full Tuning |
| Rep, Limp, Gas, Grua | Reparación, Limpieza, Repostaje, Servicio de Grúa |
| Desvuelco, Rueda, Mando RGB, Bayeta | Productos de la Tienda |
| Camaleon, Color cam | Pintura Camaleónica, Cambio de Color Camaleónico |
| Nitro, Bot nitro, Neon RGB, Compra | Instalación de Nitro, Botella de Nitro, Instalación de Neones RGB, Compraventa |
| `2x` delante | Cantidad (si es más de 1) |

Los productos agregados desde Gestión de Productos usan su nombre completo. Las abreviaturas se cambian en `app.js` → `PIECE_SHORT`, `GAMA_SHORT` y `SERVICE_SHORT`.

## Comisiones por rango

| Rango | Comisión |
|---|---|
| Recluta | 40 % |
| Mecánico | 45 % |
| Experimentado | 50 % |
| Subjefe | 50 % |
| Jefe | 50 % |

Se aplican igual a las facturas y a las ventas de kits. El rango de cada empleado se cambia en **Gestión de empleados**. Para que se reconozca a un empleado, su nombre y apellido tienen que ser iguales a los que salen en Discord como *Cobrador* o *Vendedor*.

Los porcentajes se cambian en `app.js` → `RANKS`.

### Etiquetar a cada empleado en Discord

En **Gestión de empleados** cada empleado tiene una casilla **Discord** para su ID de usuario (modo desarrollador de Discord → clic derecho sobre la persona → *Copiar ID de usuario*; también vale pegarlo como `<@ID>`). Al copiar los totales, a quien lo tenga se le etiqueta:

```
* Damián Alberto (Jefe) <@ID>: 5000 <@&ID-del-rol>
```

Los IDs se guardan en el navegador de cada PC, como el resto de datos.

### IBAN para la nómina

Cada empleado tiene también una casilla **IBAN** con la cuenta de su personaje en el banco del servidor (una letra y 6 números, como `J739745`; sale en el banco, en *Informaciones*). Al copiar los totales aparece junto a lo que hay que pagarle; si falta, sale **IBAN pendiente** en negrita:

```
* Esteban Dido (Jefe) <@ID>: 2000 · IBAN J739745
* Gxbo Dead (Mecánico) <@ID>: 4500 · **IBAN pendiente** <@&ID-del-rol>
```

Los IBAN se publican en `datos.js` junto con el rango y el Discord.

### Empleados que no cobran

En **Gestión de empleados**, la casilla **No cobra** marca a quien no recibe nómina. Sigue apareciendo con lo que factura, pero:

- En Pagos y en Totales empleados, su pago sale como **No cobra** y no suma al **Total a Pagar**.
- Al copiar sale `* Nombre (Rango) <@ID>: No cobra`, sin IBAN ni aviso de *IBAN pendiente*.
- Puede seguir siendo el empleado de la semana si es quien más factura.

Se marca y desmarca cuando haga falta (por ejemplo, si alguien deja de cobrar más adelante) y se publica en `datos.js`.

### Kits vendidos a sí mismo

En el log de **Venta de kits**, cuando *Vendedor* y *Comprador* son la misma persona, el kit se cuenta como vendido **a sí mismo**. Se siguen pagando como cualquier kit, pero se señalan:

- **Venta de Kits** (resumen): «A sí mismos: 20 de 35 (57%)».
- **Kits por empleado**: columna **A sí mismo** con cuántos y qué porcentaje de sus kits.
- **Totales empleados**: debajo del nombre, «Kits a sí mismo: 9 de 12».

Sale resaltado cuando es la mitad o más. No aparece en el texto que se copia a Discord.

### Empleado de la semana

En **Totales empleados**, quien más cobra lleva la etiqueta del rol de Discord *Empleado de la semana* (`<@&1260611794049961995>`) al copiar, tanto en **Copiar seleccionado** como en **Copiar todo**. Si hay empate, la llevan todos los empatados. El texto solo menciona el rol: asignarlo se sigue haciendo en Discord.

Para cambiar de rol, pon su ID en `app.js` → `EMPLOYEE_OF_WEEK_ROLE_ID` (ID del rol: con el modo desarrollador de Discord, clic derecho sobre el rol → *Copiar ID del rol*). Si se deja vacío, vuelve a salir el 🏆.

## Almacén

Artículos con control de existencias y aviso de mínimo:

| Artículo | Avisar en |
|---|---|
| Bayetas | 100 |
| Mandos de Neón | 10 |
| Kits de Desvuelco | 10 |
| Ruedas | 8 |

El mínimo se puede cambiar desde la propia tarjeta del artículo, y con **Conteo real** se corrigen las existencias si no cuadran con el juego. El resto de artículos (tickets del datáfono, megáfono…) aparecen en *Otros artículos*. Por ahora se cuentan los movimientos de **todos** los almacenes que salgan en el log. Cuando se sepa cómo se llama el almacén del taller en el log (`Inventory="..."`), se pone en `app.js` → `ALM_INVENTORY` para contar solo ese.

Las existencias salen solo del log del almacén: la Caja no las descuenta al vender, porque el mecánico ya saca el artículo del almacén antes de usarlo.

## Acceso con contraseña

**Módulo de Pagos**, **Módulo de Almacén**, **Gestión de Productos** y la gestión de empleados piden la misma contraseña de acceso. Es distinta a la de Aguila Motors.

En el código no está la contraseña, solo su huella SHA-256 (`app.js` → `ACCESS_PASSWORD_SHA256`), igual que la del encargado. Para cambiarla, calcula la huella de la nueva (`echo -n "NuevaContraseña" | sha256sum`) y sustitúyela. Las pestañas que la piden están en `PROTECTED_TABS`.

> Es una web estática: la huella impide que alguien lea la contraseña en el código, pero no es seguridad real. Una contraseña corta o fácil se podría adivinar probando, y quien sepa programar puede saltarse la comprobación en su propio navegador.

## Datos compartidos (empleados y productos)

El encargado publica los empleados (con rango, Discord e IBAN) y los productos para que todos vean lo mismo. Los jefes de taller solo los ven: no pueden cambiarlos.

**Cómo funciona**

- Si en el repositorio hay un archivo `datos.js`, la página lo carga sola al abrirse (siempre la última versión). Gestión de empleados queda en solo lectura y Gestión de Productos no se abre.
- Si no hay `datos.js`, cada PC usa sus propios datos como antes.
- El programa de escritorio carga `datos.js` de la web publicada; si no hay internet, usa el de su carpeta.

**Publicar cambios (encargado)**

1. En **Módulo de Pagos → Datos compartidos**, pulsa **Soy el encargado** y escribe la contraseña del encargado. Esa PC queda en modo encargado.
2. Edita empleados, rangos, Discord y productos como siempre (son los datos de tu PC).
3. Pulsa **Exportar datos (datos.js)**.
4. Sube el archivo al repositorio con **Add file → Upload files**, con el nombre exacto `datos.js` (reemplaza al anterior). En uno o dos minutos todos lo ven.

En una PC nueva, después de entrar en modo encargado, **Cargar los publicados en esta PC** copia lo publicado para seguir editando desde ahí.

**Contraseña del encargado:** en el código solo está su huella SHA-256 (`app.js` → `EDITOR_PASSWORD_SHA256`). Para cambiarla, calcula la huella de la nueva (`echo -n "NuevaContraseña" | sha256sum`) y sustitúyela.

> El repositorio es público: los nombres de los empleados, sus IDs de Discord y sus IBAN de `datos.js` los puede ver cualquiera que lo busque.

## Dónde se guardan los datos

Los empleados, los productos agregados o editados y el almacén se guardan **en el navegador de cada PC**, no en este repositorio. Por eso:

- Subir archivos nuevos a GitHub **no borra** los datos.
- En incógnito la página sale vacía (y se borra al cerrar).
- Cada PC y cada navegador tiene sus propios datos. El almacén se puede pasar de uno a otro con **Copiar respaldo** / **Restaurar respaldo**.

## Vista para móvil

En pantallas pequeñas (móvil o app de la pantalla de Inicio) la página cambia sola; en PC se ve igual que siempre:

- Cabecera compacta con un botón **☰** que abre un menú lateral con las secciones (Caja, Pagos, Almacén) y las categorías de productos.
- El botón **Categoría ▾** encima de la lista abre ese mismo menú en las categorías.
- Los productos se ven en **lista**: icono pequeño, nombre y precio en una fila, y un contador (×2) en los que ya están en el ticket.
- Una barra abajo muestra cuántos productos y el total del ticket; al tocarla baja hasta el ticket.
- En el ticket del móvil no sale la columna Precio (el subtotal ya lo incluye) ni el logo grande.

## Como app en el iPhone

En Safari, abre la web → botón **Compartir** → **Añadir a pantalla de inicio**. Queda con el icono de Top Secret y, al abrirla, muestra el logo mientras carga y se abre a pantalla completa, sin la barra de Safari. En Android (Chrome) es **Menú → Añadir a pantalla de inicio**.

> La app de la pantalla de Inicio guarda sus datos aparte de Safari: los empleados y el almacén que tengas en Safari no aparecen en ella, y al borrarla del Inicio se borran los suyos.

## Programa de escritorio

La misma página puede funcionar dentro de un programa de escritorio (pywebview, `.exe`) como el de Aguila Motors. Para actualizarlo, copia `index.html`, `app.js` y `styles.css` encima de los anteriores **sin borrar la carpeta**. En el programa, los empleados los guarda el propio `.exe`.

## Estructura

```
├── index.html               Página: pestañas de Caja, Pagos y Almacén, y ventanas
├── app.js                   Catálogo, ticket, lectura de Discord, comisiones y almacén
├── styles.css               Tema negro y plateado
├── topsecret_logo.png       Logo del taller
├── favicon.ico              Icono de la pestaña
├── datos.js                 Empleados y productos publicados por el encargado (se genera desde la página)
├── manifest.webmanifest     Nombre e iconos de la app en la pantalla de Inicio
└── icons/                   Icono del iPhone/Android y pantallas de arranque del iPhone
```

No necesita servidor ni dependencias: es HTML, CSS y JavaScript puros.

## Personalizar

- **Precios del tuning**: `app.js` → `TUNING_PRICES` (una fila por gama, en el orden de `TUNING_PIECES`).
- **Servicios, camaleónica, nitro, neones y compraventa**: `app.js` → `SERVICE_PRODUCTS`.
- **Pestañas de la Caja**: `app.js` → `PRODUCT_CATEGORIES`.
- **Artículos del almacén y sus mínimos**: `app.js` → `ALM_TRACKED`.
- **Precio por kit** en Pagos: se cambia en la propia página (por defecto $800).
- **Logo**: reemplaza `topsecret_logo.png` manteniendo el nombre. Los iconos y pantallas de arranque de `icons/` se generan a partir de él, así que si cambia el logo hay que generarlos de nuevo.

Al publicar un cambio, sube el número de versión en `index.html`: el pie de página y el `?v=` de `styles.css` y `app.js`. Así los navegadores cargan la versión nueva sin Ctrl+F5.

## Bitácora de cambios

### Versión 4 · 12 de octubre de 2026
- El rango **Chalán** pasa a llamarse **Recluta** (sigue con el 40 %). Los empleados que ya estaban guardados como Chalán aparecen solos como Recluta.

### Versión 3 · 12 de octubre de 2026
- Contraseñas nuevas, propias de Top Secret (ya no son las de Aguila Motors): una de acceso (Pagos, Almacén, Productos y empleados) y otra del encargado.
- La contraseña de acceso ya no está escrita en el código: solo se guarda su huella SHA-256, como la del encargado.

### Versión 2 · 12 de octubre de 2026
- Empleado de la semana: ahora se etiqueta el rol de Discord (`<@&1260611794049961995>`) en vez del 🏆.
- Datáfono: comprobado con recibos reales (Comercio «Top Secret: Tunings & Repairs», importes en €). El cobrador y el cliente se guardan sin espacios dobles («antonio  pulido» → «antonio pulido»).
- Almacén: sigue contando todos los almacenes del log (el inventario está a cero por ahora).

### Versión 1 · 12 de octubre de 2026
- Copia de la Caja de Aguila Motors (versión 17) para **Top Secret: Tuning & Repairs**: Caja con 131 productos, Pagos con comisiones, IBAN, No cobra y kits a sí mismo, Almacén, datos compartidos y vista para móvil.
- Nombre, logo, iconos del iPhone y paleta **negra y plateada** de Top Secret.
- Datos guardados aparte de Aguila Motors (empleados, productos, almacén…), aunque se abran en el mismo navegador.
- Sin empleados publicados todavía (`datos.js`).
- Empleado de la semana con 🏆 hasta configurar un rol de Discord.
- Almacén: cuenta todos los almacenes del log hasta saber el nombre del de Top Secret.
- Datáfono: cuenta los recibos cuyo *Comercio* contiene «Top Secret».

---

Proyecto de rol para FiveM. Top Secret: Tuning & Repairs es un negocio del servidor; no representa a ninguna empresa real.
