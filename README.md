# Auditoría ASG sobre la web de Cosentino

**Autor:** José Luis Contreras  
**Módulo:** Digitalización / GreenOps (DAM-DAW)  
**Fecha:** 13 de mayo de 2026  

Empresa elegida: **Cosentino S.A.**  
Web auditada: **https://www.cosentino.com/es**  
Dirección de referencia: Carretera del Moral 51, 03600 Cantoria (Almería).

El trabajo consiste en pasar de un análisis “a ojo” a algo que se pueda discutir en clase con números: cuánto pesa la home, qué pinta tiene eso en CO₂ aproximado, si el sitio es usable para todo el mundo y si el tema cookies y formularios está razonablemente cuidado. Después propongo cambios concretos de front, sin montar un discurso vacío.

Herramientas que he usado: [Website Carbon Calculator](https://www.websitecarbon.com/), Lighthouse dentro de Chrome, [WAVE](https://wave.webaim.org/) y la pestaña **Red** de las herramientas de desarrollador. Las pruebas las hice en **incógnito** y recargando en frío para que no me inflaran los datos una sesión vieja con todo cacheado.

Las capturas están en la carpeta `evidencias/` y van enlazadas abajo. Los archivos tienen que llamarse `01-website-carbon.png` … `06-cookies-banner.png` (si los tienes en jpg, renómbralos o pásalos a png para que GitHub no rompa el enlace).

---

## 1. Por qué Cosentino

Cosentino vende superficies (Silestone, Dekton, etc.) y la web hace de escaparate global: mucha foto, vídeo y catálogo. Eso encaja con lo que vimos en clase sobre “digitalización aplicada al sistema productivo”: no es una landing mínima, es un producto digital pesado que intenta reproducir texturas y ambientes. Ahí suele estar el conflicto: más calidad visual suele significar más megas y más trabajo en el navegador.

---

## 2. Fase A — Lo ambiental

### 2.1. Website Carbon y Lighthouse

El calculador de carbono me ha dado **0,91 g CO₂e por visita**, nota **F**, y el texto de la propia herramienta dice que la página está por encima del **88 %** del resto de sitios que tienen en su base (o sea, cola mala). También comenta lo del hosting: con energía “normal” sale ese dato; con **hosting verde** bajaría en torno a un **9 %** según su modelo. La proyección anual que enseña la web (10 000 visitas al mes) ronda **109,5 kg CO₂e** y **222 kWh**; lo cito como orden de magnitud, no como medición contable de la empresa.

Captura 1 — Website Carbon.

![Captura 1: resultado en Website Carbon Calculator](evidencias/01-website-carbon.png)

Lighthouse me ha dejado un cuadro bastante claro: **Performance 78**, **Accessibility 79**, **Best Practices 54**, **SEO 85**. Lo que más me ha chirriado no es solo el 78 de rendimiento, sino el **54** de buenas prácticas: ahí suelen colarse cosas de seguridad, APIs viejas, third-parties chapuceros, etc., y conviene abrir el desplegable y leer el detalle en clase.

Además Lighthouse avisó de que la página **iba tan lenta que no terminó dentro del tiempo** del análisis, así que el informe puede quedarse corto. Aun así salieron métricas: **FCP 0,8 s**, **LCP 2,3 s**, **TBT 60 ms**, **CLS 0,001** (eso último está muy bien, la página no “salta”), pero el **Speed Index va a 12,5 s**, que es mucho: significa que tarda un mundo en pintarse del todo aunque el primer texto salga pronto.

Captura 2 — Lighthouse (resumen).

![Captura 2: Lighthouse, categorías y métricas](evidencias/02-lighthouse-performance.png)

### 2.2. Red: qué es lo más gordo

En la pestaña Red, ordenando por tamaño tras un **Ctrl+Mayús+R**, lo que más suele pesar es un bloque grande de **imágenes del hero o carrusel**, detrás el bloque de **Google Tag Manager** con lo que cuelga, y luego **fuentes woff2** repetidas o pesadas. No hace falta ser adivino: en una web de interiores el ojo compra, pero el navegador paga el pato en megas y en batería.

Captura 3 — Network.

![Captura 3: DevTools, pestaña Red, recursos más pesados](evidencias/03-network-top3.png)

### 2.3. ¿Hay inflación de software?

Hay partes que son inevitables (quieres ver el material), pero también hay **margen de recorte** sin perjudicar la imagen de marca: servir fotos más pequeñas de lo que se ve en pantalla, no arrancar todo el marketing tag al milisegundo cero, y no arrastrar fuentes enteras si con dos pesos basta. Eso es lo que en Green Software llaman inflación cuando **podrías contar la misma historia con menos bytes y menos CPU**.

---

## 3. Fase S — Accesibilidad

WAVE me ha salido con **5 errores** y **18 alertas** (no es dramático, pero tampoco es un 10 sobresaliente). Lighthouse en accesibilidad marca **79**, coherente con que la web sea “bonita” pero con fallos típicos de contraste y componentes complejos.

Captura 4 — WAVE.

![Captura 4: informe WAVE](evidencias/04-wave.png)

Captura 5 — Lighthouse, accesibilidad.

![Captura 5: Lighthouse, pestaña Accessibility](evidencias/05-lighthouse-accessibility.png)

Dos problemas que yo destacaría para la memoria:

1. **Contraste** en textos o botones encima de fotos o degradados. Entra en juego el criterio **1.4.3** de WCAG. En la práctica quien lo nota es gente con poca visión o alguien mirando el móvil a la luz del sol.

2. **Carruseles y botones solo con icono**: si el foco casi no se ve o el lector de pantalla no recibe un nombre claro, fallas **2.4.7** y **4.1.2**. No es un capricho técnico, es gente que navega con teclado.

---

## 4. Fase G — Cookies y datos

En el banner (captura 6) se ve si el usuario puede **rechazar lo no esencial** sin hacer un máster. En muchos sitios el “Aceptar todo” gana por goleada visual; aquí toca mirar con calma si “Configurar” o “Rechazar” están al mismo nivel de claridad. Legalmente el marco es el **RGPD** y la **LOPDGDD**; las guías de la AEPD sobre cookies sirven para no liarla al exponer en oral.

Captura 6 — Cookies.

![Captura 6: banner de cookies](evidencias/06-cookies-banner.png)

Sobre formularios de contacto B2B: pedir nombre, mail, teléfono, país, sector y mensaje es habitual. Lo razonable es que no pidan de más y que lo opcional se note; el teléfono como obligatorio solo si de verdad lo necesitan para cerrar leads.

---

## 5. Qué haría yo en una refactorización

**Imágenes:** pasar a **WebP/AVIF** con `picture`, `srcset` y `sizes`, y `loading="lazy"` en lo que no sea el primer pantallazo. El LCP que elijas puede llevar `fetchpriority="high"`, el resto no.

**Vídeo:** sin autoplay con sonido, `preload="none"` y un poster estático.

**Scripts:** GTM y similares **después** del consentimiento o al menos en idle, no a la fuerza en el segundo 0. Revisar duplicados (jQuery + framework moderno pasa más de lo que creemos).

**Código de ejemplo (hero):** de un `img` gigante sin alt decente a algo así:

```html
<picture>
  <source type="image/avif" srcset="/media/hero-1600.avif 1600w" sizes="100vw">
  <source type="image/webp" srcset="/media/hero-1600.webp 1600w" sizes="100vw">
  <img src="/media/hero-1200.jpg" width="1600" height="900" alt="Cocina con encimera Cosentino"
       loading="lazy" decoding="async" fetchpriority="high">
</picture>
```

**Objetivos realistas** para una segunda iteración: bajar peso de home de ~4,8 MB hacia ~3,2 MB, recortar peticiones de ~118 a ~85, subir Performance de **78** hacia **88**, accesibilidad de **79** hacia **95**, buenas prácticas de **54** hacia **90**, y bajar el Speed Index de **12,5 s** hacia algo razonable en móvil (yo pondría como norte **4,5 s**, sabiendo que depende del throttling).

### Paradoja de Jevons

Si la página pesa la mitad pero la gente la usa el doble de veces porque ahora va fina, el ahorro por visita se come en parte con el volumen. Por eso en un proyecto serio no basta con Lighthouse: hace falta **techo de peso** en el repo, revisar terceros cada trimestre y mirar **visitas × gramos** aunque sea de forma aproximada.

---

## 6. Cierre

En resumen: la web cumple como escaparate, pero los números dicen que **paga un precio alto en transferencia y en tiempo de pintura**, y en accesibilidad aún queda tela. La parte de cookies y datos no la voy a moralizar en exceso desde aquí: está en la captura y en cómo de cómodo sea decir que no a lo que no hace falta.

### Referencias

- [WCAG 2.2](https://www.w3.org/TR/WCAG22/)  
- [Green Software Foundation](https://greensoftware.foundation/)  
- [Website Carbon Calculator](https://www.websitecarbon.com/)  
- [AEPD — cookies](https://www.aepd.es/)  
- [MDN — lazy loading](https://developer.mozilla.org/es/docs/Web/Performance/Lazy_loading)

