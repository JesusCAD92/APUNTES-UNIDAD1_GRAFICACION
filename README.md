# APUNTES-UNIDAD1_GRAFICACION
# Unidad I. Introducción a la Graficación por Computadora
Esta unidad establece las bases teóricas, matemáticas y técnicas para comprender cómo una computadora genera imágenes, desde su evolución histórica hasta la manipulación algorítmica de geometría en entornos 3D profesionales.
## 1.1 Historia y evolución de la graficación
La graficación ha pasado de representar simples puntos en un osciloscopio a simular la realidad mediante técnicas avanzadas como el Ray Tracing (Trazado de rayos). Esta evolución se divide en hitos clave:

* Década de los 50: Surgimiento de sistemas como el SAGE (Semi-Automatic Ground Environment), diseñado para defensa aérea, el cual fue pionero en el uso de monitores de vectores para representar datos de radar.
<img width="250" height="182" alt="image" src="https://github.com/user-attachments/assets/d033accd-b534-442f-98f0-668f0aa0906f" />

---

* Sutherland y el Sketchpad (1963): Ivan Sutherland introdujo el primer sistema que permitía la manipulación de objetos mediante un lápiz óptico, estableciendo las bases de las estructuras de datos gráficas y la interactividad moderna.
<img width="976" height="549" alt="image" src="https://github.com/user-attachments/assets/918ab39c-be19-4baa-96f8-544a0aa4aaee" />

---

* Normalización y GPU: Con el tiempo surgieron estándares como OpenGL y DirectX, lo que permitió que el hardware especializado (GPUs) se encargara de los cálculos matemáticos pesados, liberando a la CPU de estas tareas.
<img width="690" height="345" alt="image" src="https://github.com/user-attachments/assets/3d66ba74-c934-4e42-90d0-050a67fd601c" />

---
## 1.2 Áreas de aplicación
Como ingenieros, aplicamos estos fundamentos en campos diversos:

* **CAD/CAM:** El diseño y manufactura asistida por computadora es la base de herramientas como Blender, utilizadas para crear prototipos industriales precisos.

* **Entretenimiento:** Renderizado de efectos visuales (VFX) y desarrollo de videojuegos de alto rendimiento.

* **Medicina:** Reconstrucción de imágenes mediante tomografía (estándar DICOM) para diagnósticos precisos.

* **GIS:** Sistemas de información geográfica que utilizan mapas 3D para análisis territorial.
## 1.3 Aspectos matemáticos de la graficación
Para que Blender logre posicionar y transformar los objetos mostrados en las prácticas, utiliza una base sólida de Álgebra Lineal y Trigonometría:
* **Espacio 3D:** Los objetos viven en un sistema de coordenadas basado en vectores V = (x, y, z).
* **Transformaciones:** Cada traslación, rotación o escala de un cubo o cono aplica internamente Matrices de Transformación sobre sus vértices.
* **Trigonometría Circular:** Para el dibujo algorítmico de polígonos y flores, se depende de las funciones sin (seno) y cos (coseno) para convertir ángulos polares en coordenadas cartesianas espaciales.
## 1.4 Modelos del color: RGB, CMY, HSV y HSL
El color se procesa digitalmente mediante modelos matemáticos que interpretan la luz física:
* **RGB (Red, Green, Blue):** Modelo aditivo estándar para monitores, donde cada canal varía de 0 a 1 (en la API de Blender) o de 0 a 255.
* **CMY (Cyan, Magenta, Yellow):** Modelo sustractivo utilizado principalmente en la industria de la impresión.
* **HSV (Hue, Saturation, Value):** Un modelo más intuitivo para el diseño, donde el Hue define el color en un círculo cromático de 0° a 360°.
## Proyectos Integradores: De la Teoría a la Práctica en Blender
A continuación, se detalla cómo los conceptos anteriores se materializan a través de código Python utilizando la librería bpy.
### Proyecto 1: Creación Manual de Primitivos
En esta práctica inicial se exploró la interfaz de Blender para comprender la estructura de los objetos.

* Concepto: Se aprendió el uso del 3D Cursor como punto de anclaje para la instanciación de nuevos objetos en el espacio tridimensional.

* Visualización: En el modo edición, se identificó que todo objeto se compone de Vértices (puntos), Aristas (líneas) y Caras (superficies).
### Proyecto 2: Generación Paramétrica de Polígonos 2D
Este proyecto automatiza la creación de una malla (Mesh) desde cero aplicando la trigonometría del punto 1.3.

---
```python
import bpy
import math

def crear_poligono_2d(nombre, lados, radio):
    # 1. Creación de la estructura de datos
    malla = bpy.data.meshes.new(nombre) 
    objeto = bpy.data.objects.new(nombre, malla) 
    bpy.context.collection.objects.link(objeto) 
    
    vertices = []
    aristas = []
    
    # 2. Lógica Trigonométrica: Conversión de ángulos a coordenadas (x, y)
    for i in range(lados):
        angulo = 2 * math.pi * i / lados
        x = radio * math.cos(angulo) 
        y = radio * math.sin(angulo) 
        vertices.append((x, y, 0)) 
        
    # 3. Definición de Aristas: Conexión secuencial de los puntos calculados
    for i in range(lados):
        aristas.append((i, (i + 1) % lados))
        
    malla.from_pydata(vertices, aristas, []) 
    malla.update()

crear_poligono_2d("Hexagono_TAP", lados=6, radio=5)
```
<img width="1582" height="1020" alt="image" src="https://github.com/user-attachments/assets/d7ce8829-da3d-4f83-8e35-c0f44018ee11" />

----
## Proyecto 3: La Flor de la Vida (Patrones Iterativos)
Demuestra el uso de ciclos while para generar arte generativo variando el paso_angular.

---
```python
import bpy
import math

# Parámetros esenciales
radio = 3
angulo_actual = 0
paso_angular = 60 

# 1. Círculo Base central
bpy.ops.mesh.primitive_circle_add(radius=radio, location=(0,0,0), vertices=64)

# 2. Ciclo de Generación Perimetral: Distribución de círculos en órbita
while angulo_actual < 360:
    x = radio * math.cos(math.radians(angulo_actual))
    y = radio * math.sin(math.radians(angulo_actual))
    
    # Operador para añadir geometría instantánea en las coordenadas calculadas
    bpy.ops.mesh.primitive_circle_add(radius=radio, location=(x, y, 0), vertices=64)
    angulo_actual += paso_angular
```
### Resultado:
<img width="1590" height="1083" alt="Captura de pantalla 2026-02-11 205513" src="https://github.com/user-attachments/assets/8bada1c9-a767-4c12-a992-e6c19c8ff35a" />

---

**Nota Técnica:** El comando math.radians() es indispensable porque las funciones trigonométricas de Python requieren radianes, mientras que la lógica humana suele trabajar en grados sexagesimales.
## 1.5 Representación de líneas y polígonos
Como se observó en los scripts, un modelo 3D es una estructura llamada Mesh:

* Vertices: La unidad mínima de posición.

* Edges (Aristas): Conexión entre dos vértices.

* Faces (Caras): Polígonos cerrados por aristas.

### 1.5.1 Formatos de imagen
* Raster: Matrices de píxeles ideales para texturas complejas.

* Vectores: Instrucciones matemáticas que generan geometría perfecta, como el código desarrollado en los proyectos.
## 1.6 Procesamiento de mapas de bits y Post-procesamiento
Una vez que la geometría (vectorial) es procesada por el motor de renderizado de Blender (Eeevee o Cycles), se convierte en un mapa de bits (ráster). En esta etapa, el trabajo del ingeniero no termina con el modelo 3D, sino con la manipulación de la imagen final.

Blender integra un potente Compositor de Nodos, que permite aplicar técnicas de Procesamiento Digital de Imágenes (PDI) sobre el renderizado:
* **Filtros de Convolución:** Aplicación de desenfoques (Bloom/Blur) para simular efectos ópticos de lentes reales.

* **Corrección de Color:** Manipulación de los canales RGB y los modelos HSV para ajustar el contraste, la saturación y el balance de blancos de la imagen final.

* **Capas de Render (Render Layers):** Permite separar elementos (sombras, reflejos, objetos) para procesarlos de forma independiente antes de unirlos en la imagen definitiva.

* **Efectos de Lente:** Introducción de aberración cromática, viñeteado o distorsión para aumentar el fotorrealismo de la composición.
Este procesamiento transforma datos geométricos matemáticamente exactos en una representación visual con valor estético y técnico.
## Conclusión 
La finalización de esta unidad nos permite comprender que la Graficación por Computadora no es simplemente el acto de "dibujar" en una pantalla, sino un campo complejo de la ingeniería que integra matemáticas avanzadas, algoritmos de optimización y modelos físicos de la luz. A través del estudio de la historia y evolución de esta disciplina, queda claro cómo el desarrollo de estándares como OpenGL y el poder de procesamiento de las GPUs han democratizado la creación de entornos fotorrealistas que hoy aplicamos en medicina, ingeniería y entretenimiento.<br>
<br>Uno de los aprendizajes más significativos fue la conexión entre la teoría matemática y la programación. Entender que la posición de cada vértice en nuestros proyectos de Blender depende de matrices de transformación y funciones trigonométricas (sin y cos) nos otorga un control total sobre la geometría. El uso del lenguaje Python y la API bpy demostró ser una herramienta poderosa para automatizar la creación de formas paramétricas, como el polígono 2D y la Flor de la Vida, donde la precisión algorítmica supera las capacidades del modelado manual.<br>
<br>Finalmente, el estudio de los modelos de color (RGB, HSV) y el procesamiento de mapas de bits nos enseñó que el trabajo del ingeniero no termina con la generación de la malla, sino con el post-procesamiento de la imagen final. La capacidad de manipular una imagen mediante el compositor de nodos de Blender para aplicar filtros y correcciones de color cierra el ciclo de producción gráfica, transformando datos matemáticos en una representación visual profesional. En conjunto, esta unidad establece las bases técnicas necesarias para abordar retos más complejos en la simulación y la visualización digital avanzada.
## Bibliografía (APA)
* Hearn, D., & Baker, M. P. (2006). Computer Graphics with OpenGL. Pearson Education.

* Blender Foundation. (2026). Blender 4.3 Reference Manual: Scripting & Python. https://docs.blender.org/

* Pérez, J. (2024). Matemáticas para graficación 3D. Scribd.

* YouTube. (s.f.). Blender Python Scripting Tutorial.
