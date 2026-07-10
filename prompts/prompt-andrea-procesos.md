# Prompt para Andrea — Presentación "Procesos"
### Portal OPEX · GHVeggies

> Instrucciones de uso: copia **todo** este documento (desde el encabezado de abajo
> hasta el final) y pégalo como tu **primer mensaje** en cualquier LLM (ChatGPT,
> Claude, Gemini, etc.). Si la conversación se alarga y notas que el diseño empieza
> a desviarse — colores distintos, tipografía distinta, la navegación deja de
> funcionar — vuelve a pegar la sección "Reglas de diseño (no negociables)" para
> traer al modelo de regreso.

---

## Mensaje para pegar en el LLM

Vas a ayudarme a construir una **presentación de slides en HTML**, para el
portal de excelencia operativa **OPEX** de GHVeggies. Es un archivo único que subiré
directamente a un repositorio de GitHub (GitHub Pages), así que debe funcionar
sin ningún paso de compilación: solo abrir el `.html` en un navegador.

El tema de esta presentación es **Procesos**: la línea que diseña, implementa
y ejecuta la mejora continua y la excelencia operativa en la operación de
GHVeggies.

### Reglas de diseño (no negociables)

Sigue esto exactamente, sin reinterpretar ni "mejorar" por tu cuenta:

- **Stack**: un solo archivo `.html`, autocontenido. Tailwind CSS por CDN,
  Google Fonts por CDN, JavaScript vanilla (sin frameworks, sin build step).
- **Tipografía**: `Playfair Display` (600/700/800) para títulos, `Inter`
  (400/500/600/700) para cuerpo de texto. Cárgalas así:
  ```html
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@600;700;800&family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
  ```
- **Paleta de color exacta** (no inventes tonos nuevos, no uses degradados que
  mezclen colores entre sí):
  - Verde NS: `#307338`
  - Verde NS profundo (texto principal): `#1F4F26`
  - Verde fresco (acento): `#79BF00`
  - Amarillo NS (acento, úsalo lo mínimo posible — un detalle, no un color
    estructural): `#FBE122`
  - Crema (para superficies suaves, no blanco puro): `#FFFCEB`
  - Fondo general: blanco sólido `#FFFFFF`
- **Sin logos.** No coloques el logo de NatureSweet ni de GHVeggies en ningún
  slide. Usa solo íconos de línea simples (SVG inline, `stroke-width: 1.6`,
  trazo en verde o blanco según el fondo).
- **Nada de colores desvanecidos, mezclados o gradientes decorativos** entre
  dos tonos distintos. Los únicos "degradados" permitidos son los que se
  describen abajo (blobs con blur y el contorno de cursor), que son capas de
  un solo color con transparencia — no mezclas de colores.
- **Diseño serio y ejecutivo, pero dinámico** — no estático ni acartonado.

### Base de código (úsala tal cual como punto de partida)

Este es el esqueleto ya probado y aprobado del portal OPEX. Empieza desde aquí
y agrega tus slides dentro de `.slides-track`, no rediseñes el motor:

```html
<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Procesos · OPEX · GHVeggies</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@600;700;800&family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
<script src="https://cdn.tailwindcss.com"></script>
<script>
  tailwind.config = {
    theme: { extend: {
      fontFamily: { display: ['"Playfair Display"','serif'], body: ['Inter','sans-serif'] },
      colors: { 'ns-green':'#307338','ns-green-deep':'#1F4F26','fresh-green':'#79BF00','ns-yellow':'#FBE122','cream':'#FFFCEB' },
      borderRadius: { '2xl':'1.25rem' }
    }}
  };
</script>
<style>
  :root{ --ns-green:#307338; --ns-green-deep:#1F4F26; --fresh-green:#79BF00; --ns-yellow:#FBE122; --cream:#FFFCEB; }
  html,body{ height:100%; margin:0; }
  body{ font-family:'Inter',sans-serif; background:#fff; color:var(--ns-green-deep); overflow:hidden; }
  h1,h2,h3,.font-display{ font-family:'Playfair Display',serif; }

  /* Fondo ambiental: blobs que se mueven solos, no dependen del cursor */
  .blob-field{ position:fixed; inset:0; z-index:0; overflow:hidden; pointer-events:none; }
  .blob{ position:absolute; filter:blur(60px); opacity:.2; border-radius:50%; will-change:transform; }
  .blob-1{ width:520px; height:520px; background:var(--fresh-green); top:-160px; left:-140px; animation:driftA 34s ease-in-out infinite alternate; }
  .blob-2{ width:460px; height:460px; background:var(--ns-green); top:30%; right:-180px; opacity:.14; animation:driftB 40s ease-in-out infinite alternate; }
  .blob-3{ width:380px; height:380px; background:var(--ns-green); bottom:-160px; left:22%; opacity:.16; animation:driftC 46s ease-in-out infinite alternate; }
  @keyframes driftA{ 0%{transform:translate(0,0) scale(1);} 50%{transform:translate(60px,90px) scale(1.08);} 100%{transform:translate(-40px,40px) scale(.96);} }
  @keyframes driftB{ 0%{transform:translate(0,0) scale(1);} 50%{transform:translate(-70px,60px) scale(1.1);} 100%{transform:translate(-20px,-60px) scale(.94);} }
  @keyframes driftC{ 0%{transform:translate(0,0) scale(1);} 50%{transform:translate(50px,-40px) scale(1.06);} 100%{transform:translate(-60px,20px) scale(.98);} }
  @media (prefers-reduced-motion:reduce){ .blob{ animation:none; } }

  /* Carril de slides: avanzan hacia la derecha */
  .slides-track{ display:flex; height:100vh; transition:transform .6s cubic-bezier(.22,1,.36,1); will-change:transform; }
  .slide{ min-width:100vw; height:100vh; display:flex; flex-direction:column; justify-content:center; position:relative; z-index:1; padding:6vw 8vw; }
  @media (prefers-reduced-motion:reduce){ .slides-track{ transition:none; } }

  .slide-in{ opacity:0; transform:translateY(24px); transition:opacity .6s ease, transform .6s ease; }
  .slide.is-active .slide-in{ opacity:1; transform:translateY(0); }

  /* Flechas de navegación con contorno iluminado por cursor — elemento distintivo de OPEX, no lo cambies */
  .nav-arrow{ position:fixed; top:50%; transform:translateY(-50%); z-index:20; width:52px; height:52px; border-radius:9999px; background:#fff; border:1px solid rgba(31,79,38,.12); box-shadow:0 8px 24px -12px rgba(31,79,38,.25); display:flex; align-items:center; justify-content:center; cursor:pointer; overflow:hidden; }
  .nav-arrow.prev{ left:24px; }
  .nav-arrow.next{ right:24px; }
  .nav-arrow svg{ position:relative; z-index:1; }
  .nav-glow{ position:absolute; inset:0; z-index:0; border-radius:inherit; padding:2px; pointer-events:none; opacity:0; transition:opacity .3s ease;
    background:radial-gradient(90px circle at var(--mx,50%) var(--my,50%), var(--fresh-green) 0%, transparent 72%);
    -webkit-mask:linear-gradient(#000 0 0) content-box, linear-gradient(#000 0 0);
    -webkit-mask-composite:xor; mask-composite:exclude; }
  .nav-arrow:hover .nav-glow{ opacity:1; }

  /* Numeración real de slide (aquí sí aplica: es una secuencia genuina) */
  .slide-counter{ position:fixed; bottom:28px; left:50%; transform:translateX(-50%); z-index:20; font-size:.75rem; letter-spacing:.12em; color:var(--ns-green); font-weight:600; }

  .eyebrow{ letter-spacing:.18em; text-transform:uppercase; font-size:.78rem; font-weight:600; }
</style>
</head>
<body>
  <div class="blob-field" aria-hidden="true"><div class="blob blob-1"></div><div class="blob blob-2"></div><div class="blob blob-3"></div></div>

  <div class="slides-track" id="track">

    <!-- SLIDE 1 — Portada -->
    <section class="slide" data-slide>
      <div class="slide-in">
        <p class="eyebrow text-fresh-green">Mejora Continua</p>
        <h1 class="font-display text-5xl md:text-6xl font-extrabold mt-4">Procesos</h1>
      </div>
    </section>

    <!-- Agrega aquí el resto de tus <section class="slide" data-slide> ... </section> -->

  </div>

  <button class="nav-arrow prev" id="prevBtn" aria-label="Anterior">
    <div class="nav-glow"></div>
    <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="#1F4F26" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 6l-6 6 6 6"/></svg>
  </button>
  <button class="nav-arrow next" id="nextBtn" aria-label="Siguiente">
    <div class="nav-glow"></div>
    <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="#1F4F26" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M9 6l6 6-6 6"/></svg>
  </button>
  <div class="slide-counter" id="counter">01 / 0N</div>

  <script>
    const track = document.getElementById('track');
    const slides = [...document.querySelectorAll('.slide')];
    const counter = document.getElementById('counter');
    let i = 0;
    function render(){
      track.style.transform = `translateX(-${i * 100}vw)`;
      slides.forEach((s, idx) => s.classList.toggle('is-active', idx === i));
      counter.textContent = `${String(i+1).padStart(2,'0')} / ${String(slides.length).padStart(2,'0')}`;
    }
    function go(delta){ i = Math.min(Math.max(i + delta, 0), slides.length - 1); render(); }
    document.getElementById('nextBtn').addEventListener('click', () => go(1));
    document.getElementById('prevBtn').addEventListener('click', () => go(-1));
    document.addEventListener('keydown', (e) => { if (e.key === 'ArrowRight') go(1); if (e.key === 'ArrowLeft') go(-1); });
    document.addEventListener('click', (e) => { if (e.target.closest('.nav-arrow')) return; go(1); });
    document.querySelectorAll('.nav-arrow').forEach((btn) => {
      btn.addEventListener('mousemove', (e) => {
        const r = btn.getBoundingClientRect();
        btn.style.setProperty('--mx', `${e.clientX - r.left}px`);
        btn.style.setProperty('--my', `${e.clientY - r.top}px`);
      });
    });
    render();
  </script>
</body>
</html>
```

### Reglas de navegación (no cambies el mecanismo)

- Los slides se recorren **hacia la derecha**, uno a la vez.
- Se avanza con: la flecha derecha del teclado, la flecha visual derecha en
  pantalla, o **haciendo clic en cualquier parte de la pantalla** que no sea
  una flecha.
- Se retrocede con la flecha izquierda del teclado o la flecha visual
  izquierda.
- El contador (`01 / 0N`) siempre refleja el slide actual — sí lleva número
  porque aquí es una secuencia real, a diferencia de otras partes del portal.
- Respeta `prefers-reduced-motion`: ya está resuelto en el código base, no lo
  quites.

### Cómo manejar imágenes

Vas a subir este archivo directo a un repositorio de GitHub, junto con las
imágenes que descargues para tu presentación. Por eso:

- Crea (o pide que te ayude a estructurar) una carpeta `images/` al mismo
  nivel que el archivo `.html`.
- **Nombra las imágenes con un consecutivo**: `procesos1.jpg`, `procesos2.jpg`,
  `procesos3.jpg`... en el orden en que aparecen en la presentación. Dile
  esto al LLM desde el inicio para que las referencie con el nombre correcto
  y no invente otros nombres.
- Referencia siempre las imágenes con **ruta relativa**, así:
  `<img src="./images/procesos1.jpg" alt="...">`.
- No uses imágenes en base64 ni ligadas a URLs externas — deben vivir dentro
  del repositorio.
- Si quieres una imagen de fondo en un slide, dile al LLM que la coloque como
  `background-image` con `background-size: cover` sobre un contenedor, nunca
  como blob decorativo (los blobs son solo de color sólido).

### Estructura de contenido sugerida (ajústala a tu información real)

1. **Portada** — "Procesos" / kicker "Mejora Continua"
2. **Qué es la línea de Procesos** dentro de OPEX y por qué existe
3. **Metodología** — cómo se diseña, implementa y ejecuta una mejora
4. **Proyectos activos** (los que tengas en marcha ahora mismo)
5. **Resultados / impacto** medido de las mejoras ya implementadas
6. **Próximos pasos**
7. **Cierre**

Ajusta el número de slides y el orden a lo que realmente quieras contar — esto
es solo un punto de partida, no una camisa de fuerza.

### Formato de entrega

- Un solo archivo `.html` completo y ejecutable — nunca fragmentos de código.
- Todo el texto en español, redacción profesional, sin placeholders tipo
  "Lorem ipsum" en la versión final.
- Antes de darlo por terminado, revisa visualmente cada slide: que el texto
  no se corte, que los contrastes se lean bien sobre fondo blanco, y que la
  navegación (flechas, teclado, clic) funcione de principio a fin.
