# Estándar visual MCCO: Apple + antigravity

## Visión general

Las webs MCCO usan bloques a todo el ancho, títulos grandes y texto breve, con la imagen del activo bajo las acciones. Alternan blanco, parchment y negro. El cambio de superficie separa las secciones y un solo color de acción identifica enlaces, botones y foco.

Esta guía adapta el lenguaje de apple.com descrito en `design-md/apple/DESIGN.md` de VoltAgent/awesome-design-md. Conserva su estructura de referencia y los valores que adopta MCCO, incorpora antigravity y omite las piezas de tienda que no usa el grupo. El aviso MIT está al final. Si la hoja de Apple difiere de la implementación, mandan los tokens y componentes de `@mcco/web-kit`, documentados en `docs/ESTANDAR.md`.

El kit fija la tipografía, las superficies, la composición y el movimiento. Cada sitio aporta su color de acción, sus activos y su contenido. La página `src/pages/demo-estandar.astro` muestra las piezas juntas con el layout `Estandar`.

La plantilla conserva las páginas v1, entre ellas inicio, blog, 404 y gracias, con IBM Plex y los valores actuales de `src/styles/theme.css`. La demo del estándar no importa esa hoja porque sus reglas de `body`, tipografía y ancho de `main` interfieren con el layout nuevo. El bloque comentado de variables en `theme.css` indica dónde poner el color del sitio cuando se migre esa hoja.

## Color

### Acción

Cada web tiene un color de acción con variantes para cada fondo. El azul de Apple es el valor por defecto. El sitio declara sus variantes en el `:root` de `src/styles/theme.css`, sin cambiar los tokens `--std-*` del kit.

| Variable del sitio | Valor por defecto | Uso |
|---|---|---|
| `--accion` | `#0066cc` | Texto y enlaces sobre blanco |
| `--accion-boton` | `#0071e3` | Píldora llena con texto blanco |
| `--accion-foco` | `#0071e3` | Anillo de foco sobre claro |
| `--accion-oscuro` | `#2997ff` | Texto, enlaces y foco sobre negro |
| `--campo-claro` | `var(--accion-oscuro)` | Tono claro del campo |
| `--campo-oscuro` | `var(--accion)` | Tono oscuro del campo |

La hoja del kit usa `:where(:root)` para estos valores. El `:root` del sitio tiene mayor especificidad y gana sin depender del orden de las hojas. Cada variante debe cumplir 4,5:1 en texto y botones, y 3:1 en el anillo de foco. No se agrega un segundo color para otras acciones.

En mcco.cl, el texto usa `#a74716`, el botón `#ae4713`, el foco `#ba521a` y la variante sobre negro `#f08b50`. Sus tonos del campo son `#e0892d` y `#7c2d10`. Son valores del piloto, no los valores iniciales de esta plantilla.

### Superficies y texto

| Token del kit | Valor | Uso |
|---|---|---|
| `--std-blanco` | `#ffffff` | Hero y bloques claros |
| `--std-parchment` | `#f5f5f7` | Alternancia clara y pie |
| `--std-negro` | `#000000` | Barra global e historia |
| `--std-negro-1` | `#272729` | Superficie oscura |
| `--std-negro-2` | `#2a2a2c` | Variación de superficie oscura |
| `--std-negro-3` | `#161617` | Bloque negro suave |
| `--std-tinta` | `#1d1d1f` | Títulos y cuerpo sobre claro |
| `--std-tinta-80` | `#333336` | Bajadas y enlaces de 12 px |
| `--std-gris` | `#6e6e73` | Letra fina sobre claro |
| `--std-gris-claro` | `#a1a1a6` | Letra fina sobre oscuro |
| `--std-linea` | `#d2d2d7` | Líneas finas |

Los bloques oscuros llevan `data-std-oscuro`, que activa las variantes claras de las piezas interiores. Las tarjetas de marca usan `#1d1d1f` sobre negro, una excepción a las seis superficies tomada de la prueba aprobada. El valor `#333336` de las bajadas también viene de esa prueba y reemplaza el `#333333` de la hoja de Apple.

### Líneas y degradados

Las líneas de 1 px sirven para separar cifras y contenido denso. Los bloques a todo el ancho se separan por color, sin marcos ni sombras. No se usan degradados decorativos de fondo. La mezcla angular de tonos del campo antigravity pertenece al efecto definido más abajo.

## Tipografía

### Familias

Los titulares usan `-apple-system, BlinkMacSystemFont, "SF Pro Display", "Inter", "Helvetica Neue", Arial, sans-serif`. El cuerpo usa la misma pila con `"SF Pro Text"`. En equipos sin SF Pro se carga Inter variable autoalojada desde `styles/fuentes/` del kit, con su licencia OFL. El layout precarga el subconjunto latino para el H1.

El estándar no carga Google Fonts. El peso del cuerpo es 400, el de los titulares es 600 y la escala admite 300, 400, 600 y 700. No se usa 500.

### Jerarquía

La escala completa es 56, 48, 40, 34, 28, 24, 21, 17, 14 y 12 px. Estas son las clases de la hoja compartida y sus valores de escritorio:

| Clase o uso | Tamaño | Peso | Interlineado | Espaciado entre letras |
|---|---|---|---|---|
| `.std-h1` | 56 px | 600 | 1,07 | −0,02 em |
| `.std-h2` | 48 px | 600 | 1,08 | −0,02 em |
| `.std-h3` | 40 px | 600 | 1,1 | −0,015 em |
| `.std-lead` | 28 px | 400 | 1,14 | 0,007 em |
| `.std-tagline` | 24 px | 400 | 1,25 | 0,009 em |
| Cuerpo `.std` | 17 px | 400 | 1,44 | −0,022 em |
| `.std-fina` | 12 px | 400 | 1,33 | −0,01 em |

Los títulos tienen espaciado negativo y el cuerpo mantiene el intervalo de lectura de 1,44 a 1,47 de la referencia, con 1,44 en la hoja del kit. Las bajadas conservan su espaciado positivo. El rótulo de origen usa 11 px: es la excepción a la escala, no un tamaño para párrafos o información legal.

## Composición

### Espaciado y grilla

La referencia usa una base de 8 px y pasos de 4, 8, 12, 17, 24, 32, 48 y 80 px. En los componentes MCCO, `Bloque` deja 80 px arriba y abajo, y 22 px a cada lado. En ventanas de hasta 640 px pasa a 56 px verticales y 16 px laterales.

Los títulos, bajadas y acciones se centran. El H1 tiene un máximo de 900 px, el H2 de 860 px, la bajada principal de 760 px y `.std-tagline` de 720 px. Las barras, galerías y tarjetas de marca usan contenedores de hasta 1024 px. La grilla de productos ocupa el ancho disponible, con dos columnas iguales y 12 px entre piezas.

La imagen queda debajo del texto y de las acciones. El espacio alrededor permite reconocer el activo sin agregar bordes, rótulos numerados ni adornos. El pie concentra los enlaces en columnas de 12 px sobre parchment, con una línea legal.

## Profundidad

Las superficies son planas. La sub-navegación usa fondo translúcido y desenfoque para mantener la lectura sobre el contenido que pasa por debajo.

La única sombra del sistema es `rgba(0,0,0,.22) 3px 5px 30px 0`, guardada en `--std-sombra`. Se reserva para imágenes de producto apoyadas sobre una superficie. No se aplica a títulos, botones ni tarjetas.

## Formas e imágenes

| Pieza | Radio |
|---|---|
| Bloque a todo el ancho | 0 |
| Tarjeta de galería o marca | 18 px |
| Píldora | 980 px |

Las imágenes tienen `max-width: 100%` y `height: auto` en la base. Cada componente define el recorte que necesita. `Producto` admite `foto.encuadre` como `object-position`, además de `srcset` y `sizes`. Las imágenes de contenido llevan dimensiones y texto alternativo que describa lo que se ve.

En `Galeria`, el formato `cuadros` recorta a 4:3. El mosaico conserva la proporción, sin recortar ni ampliar una imagen más allá del ancho de su archivo. Una captura pequeña se presenta pequeña.

## Componentes

### Navegación

El layout `@mcco/web-kit/layouts/Estandar.astro` reúne `Base`, `NavGlobal` y `SubNav`, coloca el contenido dentro de `main` y agrega el pie del grupo. Acepta `title`, `description`, `nav` y `subnav`. El slot `head` permite sumar el favicon y el slot `pie` recibe el pie propio del sitio antes del bloque Grupo MCCO.

`NavGlobal` es una barra negra de 44 px con el logo en claro y enlaces de 12 px. En hasta 833 px muestra un botón de menú. La bandeja se puede abrir con teclado y cerrar con Escape, al hacer clic fuera, al salir el foco o al volver al ancho de escritorio. Sin JavaScript, los enlaces siguen disponibles. La página actual se identifica con `aria-current="page"`, sin una marca visual adicional.

`SubNav` mide 52 px, queda fija arriba al bajar y mantiene la acción principal visible. En hasta 833 px oculta los enlaces de sección. En hasta 640 px mide 48 px y usa `textoCorto` para la acción. Cambia al tono oscuro cuando tiene debajo un bloque negro a todo el ancho.

### Acciones

`Acciones` recibe una lista con `href`, `texto` y `variante`. La variante `llena` es la predeterminada, `fantasma` tiene borde y fondo transparente, y `mas` es un enlace de texto con un chevrón decorativo agregado por CSS. No se escriben flechas en el texto.

La píldora normal usa texto de 17 px, relleno de 11 px por 22 px y un área táctil mínima de 44 × 44 px. Al presionarla se reduce a `scale(.95)`. Con `tamano="chica"`, el texto pasa a 12 px y el área táctil conserva 44 px aunque la píldora visible sea más baja. En el hero, la fantasma lleva fondo blanco para que los guiones del campo no dificulten la lectura.

El foco es una línea de 2 px separada 3 px del borde, con esquinas rectas. No adopta el radio de foco de 4 px de la referencia de Apple. Dentro de un bloque oscuro usa `--accion-oscuro`.

### Hero y bloques

`HeroEstandar` contiene el único H1, una bajada y las acciones. Puede recibir `nube` con una imagen de respaldo y `campo` para activar los guiones detrás del texto. El título debe ser legible desde el primer pintado y es el elemento previsto como LCP. El hero incorpora su botón de pausa cuando tiene nube o campo.

`Bloque` ofrece los tonos `claro`, `parchment`, `negro` y `negro-suave`. No tiene esquinas redondeadas. Se le asigna un `id` para los enlaces internos y `aria-labelledby` cuando el título nombra la sección.

`HistoriaNube` recibe de uno a cuatro activos y un paso por activo. El primero debe coincidir con el respaldo del hero para dar continuidad al recorrido. El movimiento y las alternativas estáticas se describen en Antigravity.

### Productos

`GrillaProductos` contiene los componentes `Producto`. Cada producto tiene título, bajada y acciones arriba, y una foto a sangre con rótulo abajo. El título es H3 por defecto y puede ser H2 con `nivel={2}`, según la jerarquía de la página.

La tarjeta tiene un alto mínimo de 640 px y una foto de 320 px. Con `ancho`, ocupa ambas columnas, su alto mínimo pasa a 700 px y la foto a 378 px. El título crece de 40 a 48 px. En hasta 640 px la tarjeta toma el alto de su contenido y la foto queda en 4:3 bajo el texto.

### Galería

`Galeria` acepta imágenes y videos con `origen`, dimensiones y `alt`, más un pie opcional. En `cuadros` muestra tres columnas, dos en hasta 833 px y una en hasta 640 px. En `mosaico` arma filas de unos 240 px, con 320 px como tope de alto y el ancho nativo de cada archivo como límite.

El video requiere poster WebP, va en H.264 con `faststart` y se aloja en R2. Usa `muted`, `playsinline`, `loop` y `preload="none"`. El kit lo descarga y reproduce cuando está a la vista. Con movimiento reducido o ahorro de datos (`Save-Data`) queda el poster con un botón para reproducirlo.

### Cifras y marcas

`Cifras` muestra valores de 56 px en el color de acción, con etiqueta debajo, líneas finas y una nota opcional. En hasta 640 px baja a 48 px. El contenedor decide la alineación. Los números se agrupan como en Chile y se redondean a dos decimales. Un año o un código se pasa como texto: `valor: '2011'` evita que aparezca como `2.011`.

`TarjetasMarca` presenta un logo, una frase y un dominio, o el aviso «Sitio en preparación». La tarjeta completa es enlace cuando tiene `href`. El logo usa una máscara CSS de un SVG o PNG transparente y un tono claro. Con colores forzados adopta el color del texto del sistema. La grilla tiene tres columnas, dos en hasta 900 px y una en hasta 640 px.

### Pie y formularios

El pie propio va en el slot `pie` del layout. Los datos de contacto y de entidad salen de `site.yaml` y del kit. El bloque Grupo MCCO se agrega después y puede desactivarse con `footer={false}` cuando corresponda.

Esta guía no define un nuevo formulario ni estados de validación. Se conserva el contrato del formulario de cada sitio hasta su migración, sin copiar las piezas de búsqueda, compra o configuración de la tienda de Apple.

## Uso del estándar

Se reutilizan los componentes del kit y sus clases `std-*`. Un ajuste de geometría, navegación o comportamiento compartido se resuelve en el kit y llega al sitio mediante una versión fijada. Los colores de acción y activos propios se cambian en el sitio.

Los bloques alternan superficies, los titulares mantienen el peso 600 y el cuerpo usa 17 px. Se evita añadir sombras, un segundo color de acción o radios intermedios para diferenciar secciones. Los textos deben poder leerse sin que termine una animación.

## Adaptación a la pantalla

| Condición | Cambio |
|---|---|
| Hasta 833 px de ancho | Menú compacto, sub-navegación sin enlaces de sección, productos en una columna y galería en dos |
| Hasta 640 px de ancho | Sub-navegación de 48 px, bloques con 56 px de aire vertical, galería y marcas en una columna |
| Hasta 640 px de ancho | H1 de 40 px, H2 de 34 px, H3 de 28 px y bajadas de 21 px |
| Hasta 419 px de ancho | H1 de 34 px |
| Hasta 900 px de ancho | Marcas en dos columnas y pasos de historia en dos columnas |
| Desde 901 px de ancho y 700 px de alto | Historia fija durante su recorrido y presupuesto visual de computador |

Los cortes de composición no sustituyen la condición del sistema visual: `(min-width: 901px) and (min-height: 700px)`. Un celular girado o una ventana de computador baja usa el presupuesto móvil si no cumple ambas dimensiones. Al redimensionar, los lienzos ajustan su búfer, la nube recalcula las anclas y la historia su altura. El campo reconstruye los anillos al cruzar ese corte.

Los controles conservan un área táctil mínima de 44 × 44 px. Los enlaces a anclas llegan a su posición sin desplazamiento suave y dejan espacio para la sub-navegación.

## Antigravity

### Nube de puntos

El sitio entrega de uno a cuatro activos en nube de puntos sobre negro. La entrada actual de `HistoriaNube` usa ilustraciones de 960 × 540 px. Todos los activos se muestrean con igual cantidad de puntos y un orden espacial para pasar de una forma a otra. La imagen del primer activo también se entrega al hero como respaldo con dimensiones y descripción.

Después del primer pintado, los puntos suben desde abajo y arman ese activo en unos 2,6 s. La nube acompaña el scroll hasta la historia negra, con media opacidad durante el trayecto para dejar leer lo que cruza. Allí cambia de activo y enciende el paso que corresponde al progreso. El cursor dispersa puntos y un barrido de luz recorre la forma.

Sobre blanco usa tinta y realces de `--accion-boton`, con chispas de `--accion-foco`. Sobre negro usa crema y `--accion-oscuro` con brillo. Un solo lienzo fijo recorre las anclas de la página.

La historia queda fija solo con la condición de 901 × 700 px. En las demás ventanas, cambia mientras cruza la pantalla. Sin JavaScript, sin WebGL2, con movimiento reducido o si falla un activo, se muestran las ilustraciones fijas con sus rótulos y todos los pasos encendidos.

### Campo de guiones

El campo vive en el hero, detrás del texto. Los guiones pasan entre las letras sin recuadros blancos alrededor de los títulos. La píldora fantasma conserva su propio relleno para ser legible.

| Parámetro | Computador | Presupuesto móvil |
|---|---|---|
| Separación entre anillos | 28 px | 24 px |
| Separación entre guiones | 30 px | 26 px |
| Radio interior | 46 px | 46 px |
| Radio máximo | 1180 px | 760 px |
| Banda: aparece, plena hasta, se apaga en | 150, 400, 680 px | 110, 280, 500 px |
| Largo del guion | 2 a 7 px | 2 a 7 px |
| Ancho del guion | 2 a 2,4 px | 2 a 2,4 px |
| Opacidad lejana y en la banda | 0,13 y 0,85 | 0,13 y 0,85 |
| Cantidad de guiones | 5184 | 2841 |

Los guiones apuntan hacia afuera. El centro sigue a una guía que sigue al mouse con dos suavizados de 0,7 s, unos 1,4 s de retraso total. Sin cursor pasea despacio. Los anillos respiran y giran en sentidos alternos a 0,012 rad/s. El scroll no modifica ese giro: el campo sube con el hero.

El color cambia según el ángulo y mezcla `--accion-boton`, `--campo-claro`, `--campo-oscuro` y el neutro `#48484a`. Sobre la nube, la opacidad baja a un tercio con un borde elíptico difuso. El borde inferior del hero también se difumina. Con movimiento reducido o sin WebGL2 se omite el campo porque es decorativo.

### Pausa y carga

Los botones del hero y de la historia comparten una sola pausa para nube y campo. Cambian entre «Pausar movimiento» y «Reanudar movimiento», sin `aria-pressed`, porque el texto ya describe la acción disponible. Los botones de video también cambian su texto y usan `data-accion` para el ícono.

Los efectos arrancan después de la carga, en tiempo ocioso y cerca de la vista. Se detienen con la pestaña oculta y fuera de pantalla. Los lienzos decorativos llevan `aria-hidden="true"`. La preferencia de movimiento reducido deja el contenido completo y quieto.

## Rendimiento y medios

El estándar usa WebGL2 propio, IntersectionObserver y CSS. No incorpora Three.js, GSAP ni CDN en producción. Los módulos viven en `lib/estandar/`, los componentes en `components/estandar/` y la hoja en `styles/estandar.css` del kit.

| Medida | Presupuesto |
|---|---|
| JavaScript de la nube, gzip | Hasta 12 KB |
| JavaScript del campo, gzip | Hasta 5 KB |
| Nube en computador | 26 000 puntos, DPR máximo 1,65 |
| Nube en presupuesto móvil | 11 000 puntos, DPR máximo 1,35 |
| Campo | DPR máximo 1,65 en computador y 1,35 en móvil |
| Cuadros por segundo en móvil | Hasta 30 en nube y campo |
| Lighthouse móvil | Rendimiento de al menos 85 y accesibilidad de al menos 95 |
| LCP previsto | H1 |
| CLS | Menor que 0,1 |
| Imágenes | Menos de 400 KB, según R12 |
| Activos de la nube | De 100 a 200 KB |
| Videos | Hasta 8 MB, en R2 con poster WebP |

La cantidad de puntos se fija al arrancar. El campo cambia sus anillos al cruzar la condición de pantalla. Los límites de Lighthouse son objetivos que se miden en el sitio final, no resultados garantizados por usar la plantilla.

Los videos no se agregan al repositorio. Su origen en R2 se declara en `site.yaml`, dentro de `csp.media_src`. Cualquier origen externo de scripts, fuentes o imágenes también debe estar declarado para que la CSP lo permita. El Worker de la nube requiere el mismo origen en `worker-src` o, si esa directiva no existe, en `script-src`.

## Imágenes, rótulos y textos MCCO

Se usa primero material real propio de faena y después renders propios de nubes de puntos, modelos o mapas de desviaciones. La IA se reserva para atmósfera o para ilustrar cómo se trabaja. Cada imagen generada requiere un prompt revisado, geometría creíble y referencias, y se revisa por separado. No puede contener escalas, cifras, textos, logos ni marcas inventadas. Tampoco se usan avatares o personas generadas como testimonios.

Una simulación presentada como trabajo de MCCO debe ser una captura real. Antes de publicarla se retiran escalas, valores, nombres de archivo y cualquier dato que identifique al cliente, la faena, la planta o el folio. También se retiran logos de software si permiten identificar al cliente. La revisión incluye el `forbidden` de `site.yaml` y la blacklist del kit.

| `origen` | Rótulo visible |
|---|---|
| `real` | Foto de faena, o Video de faena en video |
| `render` | Render 3D |
| `ilustracion` | Ilustración |
| `esquema` | Esquema |
| `captura` | Captura de simulación |

`Producto` y `Galeria` requieren el origen y escriben el chip translúcido de 11 px. La nube lleva su rótulo de ilustración. R11 comprueba que las ilustraciones estén rotuladas a la vista. El rótulo no convierte una imagen en evidencia: los datos deben venir de mediciones y fuentes verificables.

Se escribe en español de Chile o neutro, sin voseo, emojis, flechas, rayas como conectores ni exclamaciones. Los títulos dicen qué se hace y las bajadas aportan lo necesario para elegir la siguiente acción. No se inventan cifras, clientes, certificaciones ni testimonios. Los clientes nombrables salen de `site.yaml`, campo `clients_allowed`, y toda métrica publicada necesita una fuente. Los textos nuevos pasan por `work-humanizer` y por el repaso de texto del proyecto.

## Guía de iteración

Antes de editar una página, se identifica el componente que corresponde y se revisa su contrato en `docs/ESTANDAR.md` del kit. La demo permite comprobar tonos, tamaños, galerías y movimiento con los mismos componentes que usa el sitio. No se copian sus implementaciones al repositorio para ajustarlas.

Una web nueva configura `site.yaml`, su color de acción con contraste AA, un logo legible sobre negro y sus imágenes. En una web migrada se conservan las URLs, metadatos y funciones existentes, y se reemplazan las reglas v1 de la hoja del sitio antes de importarla desde `Estandar`.

Cada cambio pasa por `npm run build` y `npm run check`. La revisión de pantalla incluye teclado, pausa, movimiento reducido, ausencia de WebGL, cambio de orientación y saltos de scroll. El manifiesto sigue siendo la fuente de verdad para contacto, CSP y contenido del sitio. El paquete y los workflows se fijan a la misma versión del kit, con lockfile generado según `docs/LOCKFILE.md`.

## Alcance y pendientes

El estándar no incorpora los configuradores, carrito, buscador de tienda ni barras de compra de la referencia de Apple. Tampoco define un modo oscuro automático: cada bloque declara su tono. Los formularios y sus estados se resuelven en su tarea de migración.

La adopción en producción requiere el kit `v1.2.0` final y el visto bueno de Mario sobre la vista previa.

## Fuente y licencia

Adaptación de [Apple DESIGN.md, VoltAgent/awesome-design-md](https://github.com/VoltAgent/awesome-design-md/blob/main/design-md/apple/DESIGN.md), a partir de la copia de lectura obtenida el 16 de octubre de 2026. El [aviso MIT del repositorio de origen](https://github.com/VoltAgent/awesome-design-md/blob/main/LICENSE) se conserva a continuación.

MIT License

Copyright (c) 2026 VoltAgent

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
