# 🎨 Graficación 2D - Unidad 2: Geometría y Algoritmos
Bienvenido al repositorio de la Unidad 2 de la materia Graficación. En este módulo, exploramos los fundamentos de la representación visual bidimensional, enfocándonos en cómo los algoritmos matemáticos se transforman en representaciones gráficas mediante el uso de software de alto rendimiento.
## 🏛️ Introducción: Blender como Laboratorio 2D
Aunque Blender es reconocido mundialmente por su motor de renderizado 3D, para un Ingeniero en Sistemas es la herramienta ideal para el estudio de la Graficación 2D por tres razones fundamentales:

* **Grease Pencil:** Un entorno de dibujo vectorial nativo que permite manipular puntos y líneas (primitivas) como objetos matemáticos.

* **Proyecciones Ortogonales:** Al configurar la cámara en modo Orthographic y bloquear el eje Z, Blender se convierte en un plano cartesiano perfecto para estudiar transformaciones.

* **Matemáticas de Transformación:** Blender permite visualizar de forma directa cómo las matrices de traslación, rotación y escalamiento afectan a los vértices en un espacio de dos dimensiones.
## 📑 Tabla de Contenidos
* 2.1 Representación de objetos 2D: Coordenadas y primitivas.

* 2.2 Algoritmos de trazado: DDA y Bresenham para líneas y círculos.

* 2.3 Transformaciones geométricas: Traslación, Rotación y Escalamiento.

* 2.4 Proyecciones y Ventanas: Del espacio de mundo al espacio de pantalla.

* 2.5 Técnicas de Relleno y Recorte: Algoritmos de Clipping.
## 📂 Estructura del Repositorio
Para mantener un flujo de trabajo profesional, los archivos se organizan de la siguiente manera:
```python
Graficacion-Unidad-2/
├── 📁 exercises/             # Prácticas numeradas (2.1 a 2.5)
│   ├── 📝 ex_01_primitivas/
│   └── 📝 ex_02_algoritmos/
├── 📁 models/                # Archivos de Blender (.blend)
│   ├── 🧊 plano_cartesiano.blend
│   └── 🧊 transformaciones_2D.blend
├── 📁 renders/               # Capturas de pantalla y resultados finales
│   └── 🖼️ 2.3_transformacion_final.png
├── 📁 scripts/               # Scripts de Python (bpy) para automatización
│   └── 🐍 draw_line_bresenham.py
├── 📁 docs/                  # Apuntes teóricos y cálculos matemáticos
└── 📄 README.md              # Documentación principal
```
## 2.1 Transformación Bidimensional y 2.2 Representación Matricial

En graficación por computadora, una transformación es una operación que mapea un punto $P(x, y)$ a una nueva ubicación $P'(x', y')$. Para realizar estas operaciones de manera eficiente y combinarlas (composición), utilizamos coordenadas homogéneas, lo que nos permite representar todas las transformaciones, incluida la traslación, como multiplicaciones de matrices de $3 \times 3$.
### 📐 Definiciones y Fórmulas
<img width="915" height="589" alt="image" src="https://github.com/user-attachments/assets/315dfa40-0b43-42b9-a367-2192ba72c2b5" />

### ⌨️ Conexión con Blender: El Teclado como Interfaz Matricial
Blender es una implementación visual de este álgebra lineal. Cuando manipulas un objeto en el Viewport, estás invocando estas matrices mediante atajos de teclado (Shortcuts):
* **'G' (Grab/Translate):** Activa la Matriz de Traslación. Al mover el mouse, Blender actualiza los valores $t_x, t_y$ en tiempo real dentro de la matriz de transformación del objeto.
* **'S' (Scale):** Invoca la Matriz de Escalamiento. El alejamiento o acercamiento del cursor respecto al origen del objeto define los factores $s_x$ y $s_y$.
* **'R' (Rotate):** Ejecuta la Matriz de Rotación. El ángulo $\theta$ se calcula basándose en la posición circular del puntero.
Dato de Ingeniería: Blender utiliza internamente Matrices de Transformación de Mundo (4x4 para 3D), pero al trabajar en vistas ortogonales (Top View, Numpad 7), las filas y columnas del eje Z se mantienen constantes, comportándose matemáticamente como las matrices 2D mostradas arriba.

### 🎮 Ejercicio de Control: Scripting de Movimiento en Blender
Para controlar un objeto mediante las teclas de dirección (flechas), utilizaremos la API de Python de Blender (bpy). Este ejercicio demuestra cómo modificar las coordenadas de traslación mediante programación.
### Guía Técnica: Script de Control Básico
* Abre Blender y cambia al espacio de trabajo Text Editor.
* Crea un nuevo archivo llamado control_teclado.py.
* Copia y ejecuta el siguiente script (este es un ejemplo simplificado para entender la lógica de incremento en las coordenadas $x, y$):

---
```python
import bpy

# Seleccionamos el objeto (por ejemplo, un cuadrado o círculo 2D)
obj = bpy.context.active_object

def mover_objeto(direccion, incremento=1.0):
    """
    Simulación de la lógica de traslación matricial.
    Suma un valor a la posición actual del objeto.
    """
    if direccion == 'ARRIBA':
        obj.location.y += incremento
    elif direccion == 'ABAJO':
        obj.location.y -= incremento
    elif direccion == 'IZQUIERDA':
        obj.location.x -= incremento
    elif direccion == 'DERECHA':
        obj.location.x += incremento

# Ejemplo de ejecución manual:
# Mueve el objeto 2 unidades a la derecha (Internamente aplica Matriz T(2, 0))
mover_objeto('DERECHA', 2.0)
```
### Implementación con Drivers (Sin Código)
Si prefieres un enfoque visual dentro de Blender:

* En el panel de propiedades (N), haz clic derecho sobre Location X.

* Selecciona Add Driver.

* En la configuración del Driver, puedes vincular el valor de la posición a la rotación de otro objeto o a una variable de entrada, emulando un sistema de control paramétrico.
## 2.3 Trazo de líneas curvas: Bézier y B-splines
La graficación de curvas se basa en la interpolación y aproximación de puntos. En lugar de definir cada píxel, definimos puntos de control que dictan la forma de la curva.
<img width="912" height="514" alt="image" src="https://github.com/user-attachments/assets/1318285b-bacc-4b91-a8eb-a3b9f9965925" />

### 🎨 Práctica en Blender: El Arte de los Handles (Manejadores)
En Blender, al trabajar con una Bézier Curve, entramos en Edit Mode (Tab) para manipular su anatomía. Cada vértice tiene dos manejadores que definen la dirección y la fuerza de la curvatura.
#### Tipos de Handles en Blender (Atajo: V):
* Vector: Crea esquinas afiladas; los manejadores apuntan directamente al siguiente punto.
* Aligned: Los dos manejadores están en línea recta. Al mover uno, el otro se ajusta para mantener la suavidad (continuidad $C^1$).
* Free: Permite mover cada manejador de forma independiente, ideal para crear ángulos "quebrados" en una curva.
* Automatic: Blender calcula la curvatura más suave posible de forma matemática.
**Acción Técnica:** Selecciona un manejador y usa 'G' para desplazarlo, 'R' para rotar el ángulo de entrada de la curva y 'S' para estirarlo (a mayor longitud, mayor es la influencia o "tensión" sobre la curva).

## 2.4 Fractales: El Caos Organizado en la Graficación
Un fractal es un objeto geométrico cuya estructura básica, fragmentada o irregular, se repite a diferentes escalas. El concepto fundamental aquí es la autosimilitud: si hacemos "zoom" en una parte del objeto, encontraremos una figura similar a la totalidad.
### 🌀 Generación Procedimental en Blender
Como ingenieros, no dibujamos fractales a mano; programamos o configuramos reglas para que se generen solos. En Blender, esto se logra de dos formas principales:
1) Mediante Modificadores (Geometría):
* Modificador 'Array': Al usar un objeto vacío (Empty) como "Object Offset" y escalar dicho Empty ligeramente hacia abajo, podemos crear estructuras recursivas que imitan fractales clásicos como el Triángulo de Sierpinski.
* Geometría Recursiva: Cada iteración aplica una transformación matricial $M$ sobre la anterior, donde $P_{n+1} = M \cdot P_n$.
2) Mediante Texturas Procedimentales (Sombreado):

* Blender incluye motores de ruido fractal como Musgrave y Voronoi. Estas texturas utilizan algoritmos matemáticos para calcular valores de color basados en funciones de ruido que se superponen a diferentes frecuencias (octavas).

* Uso Técnico: Son ideales para generar terrenos, nubes o texturas orgánicas que requieren un nivel de detalle infinito.
## 2.5 Uso y creación de fuentes de texto
En graficación, el texto no es solo "letras", es un conjunto de curvas matemáticas (generalmente Bézier) que definen contornos rellenables.
### ✍️ El Objeto 'Text' en Blender
Al agregar un objeto de texto (Shift + A > Text), Blender genera una curva especial que interpreta caracteres tipográficos.

* Importación de fuentes (.ttf / .otf): Por defecto, Blender usa una fuente básica. Para usar tipografías externas, debemos ir al panel de Data (icono de la 'a') > Font y cargar el archivo de fuente desde el sistema (ej. C:\Windows\Fonts).

* Modo Edición: A diferencia de otros objetos, al presionar Tab en un texto, este funciona como un procesador de palabras donde puedes escribir y borrar.
### 🛠️ Conversión y Manipulación
Para un flujo de trabajo de ingeniería o diseño avanzado, a menudo necesitamos que el texto deje de ser "letras" y pase a ser geometría:

1) **Convertir a Curva:** Right Click > Convert to > Curve. Esto permite manipular los manejadores (handles) de cada letra individualmente como vimos en el subtema 2.3.

2) **Convertir a Malla (Mesh):** Right Click > Convert to > Mesh. El texto se convierte en una red de vértices y caras. Es destructivo (ya no puedes cambiar el texto escrito), pero permite aplicar modificadores como Booleanos o Explosiones.
### 🧠 Reflexión: La Tipografía en la Interfaz de Usuario (UI)
Desde la perspectiva de la Ingeniería en Sistemas, la tipografía no es un elemento estético, es una herramienta de comunicación técnica.

* **Legibilidad vs. Readability:** En una UI, la elección de una fuente Sans Serif (sin remates) suele mejorar la lectura en pantallas de baja resolución.

* **Jerarquía Visual:** El uso de diferentes pesos (Bold, Regular, Light) permite guiar el ojo del usuario hacia las acciones más importantes de la aplicación.

* **Consistencia:** Una tipografía bien implementada reduce la carga cognitiva del usuario, haciendo que el software se sienta más intuitivo y profesional.

## 🎓 Conclusión de la Unidad: La Matemática detrás del Píxel
Finalizar esta unidad sobre Graficación 2D me ha permitido entender que, en la Ingeniería en Sistemas, no existe la "magia visual"; lo que existe es un despliegue masivo y elegante de Álgebra Lineal y Cálculo Vectorial.Como estudiante del ITC, el paso de la teoría a la práctica en software como Blender ha sido revelador. Comprender que cada vez que movemos un personaje en un videojuego o escalamos una ventana en un sistema operativo, estamos ejecutando multiplicaciones de matrices de $3 \times 3$ en milisegundos, cambia por completo mi perspectiva sobre el desarrollo de software.
### 🔑 Impacto en el Desarrollo Profesional
Optimización en Videojuegos: Dominar las transformaciones matriciales es el primer paso para escribir motores gráficos eficientes. No se trata solo de mover objetos, sino de entender cómo manipular el espacio de mundo y el espacio de cámara para ahorrar recursos de procesamiento.

Fluidez y Realismo: El estudio de las curvas de Bézier y B-splines es lo que separa a una aplicación rudimentaria de una profesional. La suavidad en las trayectorias de cámara y el diseño de interfaces orgánicas dependen directamente de nuestra capacidad para programar estas funciones matemáticas.

Versatilidad Técnica: El uso de Blender como laboratorio nos prepara para un mercado laboral donde el scripting (Python) y el diseño 3D convergen. La capacidad de automatizar procesos gráficos mediante código nos da una ventaja competitiva como ingenieros modernos.

"La graficación por computadora es el punto exacto donde la rigidez de las matemáticas se encuentra con la libertad de la imaginación."
## Referencias Bibliográficas (Formato APA)
* Blender Foundation. (2026). Blender 4.2 Reference Manual: Modeling, Curves and Text Editing. Recuperado de https://docs.blender.org/manual/en/latest/

* Hearn, D., Baker, M. P., & Carithers, W. (2014). Computer Graphics with OpenGL (4.ª ed.). Pearson.

* Hughes, J. F., Van Dam, A., & Foley, J. D. (2013). Computer Graphics: Principles and Practice (3.ª ed.). Addison-Wesley Professional.

* Mandelbrot, B. B. (1982). The Fractal Geometry of Nature. W. H. Freeman and Company.

* Shirley, P., & Marschner, S. (2021). Fundamentals of Computer Graphics (5.ª ed.). A K Peters/CRC Press.
​​
