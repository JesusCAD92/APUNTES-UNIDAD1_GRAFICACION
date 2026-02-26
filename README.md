# Unidad I. Interfaz gráfica de usuario
## Reporte Técnico: Escenario Procedural con Animación de Cámara
**Proyecto Integrador - Unidad I Materia:** Graficación por Computadora / Tópicos Avanzados

**Software:** Blender 4.x, Python (bpy), Git Bash
## 1. Objetivo del Módulo
Desarrollar un entorno tridimensional generado proceduralmente que integre la creación automática de geometría (paredes y suelos) y una animación de cámara dinámica. Se busca simular el movimiento humano (Head Bobbing) y el balanceo lateral mientras la cámara recorre un camino en zigzag definido matemáticamente mediante funciones senoidales.
## 2. Arquitectura de la Escena y Tiempos
Antes de generar la geometría, el script configura el lienzo tridimensional y el rango de la línea de tiempo. En Blender, la escena controla la duración de la animación.
Instrucciones:

* **Limpieza de Escena:** Eliminar objetos previos para evitar duplicidad.

* **Configuración de Frames:** Definir el inicio y fin de la animación basados en la longitud del pasillo.

---
```python
# Configuración de tiempos dentro de Blender
pasos_por_bloque = 10 
bpy.context.scene.frame_start = 1
# El final depende del largo del pasillo por los pasos definidos
bpy.context.scene.frame_end = largo * pasos_por_bloque
```
## 3. Generación Procedural de Materiales y Geometría
El manejo de componentes en Blender implica crear materiales y mallas dinámicamente.
**Guía de Componentes:**
* **Materiales RGB:** Uso de diffuse_color para asignar colores a las paredes.

* **Cubes (Paredes):** Instanciación de cubos en posiciones calculadas con math.sin.

* **Planes (Suelo):** Escalado de un plano para cubrir la extensión total del recorrido.

---
```python
# Creación de materiales dinámicos
def crear_material(nombre, color_rgb):
    mat = bpy.data.materials.new(name=nombre)
    mat.diffuse_color = (*color_rgb, 1.0) # RGBA
    return mat

# Colocación de cubos en zigzag
offset_x = math.sin(i * frecuencia) * amplitud
bpy.ops.mesh.primitive_cube_add(location=(ancho_pasillo + offset_x, pos_y, 1))
```
## 4. Implementación de Lógica de Animación (Head Bobbing)
Para lograr realismo, no basta con mover la cámara; se aplican funciones trigonométricas para simular el rebote de la caminata.
### ¿Cómo funciona la oscilación?

* **Eje Z (Rebote):** Simula el impacto del paso mediante una onda senoidal de alta frecuencia.

* **Eje Y local (Roll):** Genera un balanceo lateral sutil.

* **Look Ahead:** La cámara calcula la tangente del siguiente punto para rotar y "mirar" hacia la curva.

---
```python
# Oscilación vertical (Z): Simula el rebote de la cabeza
rebote_z = math.sin(i * 0.5) * 0.05 
# Balanceo lateral (Roll): Pequeña rotación en el eje Y
balanceo_roll = math.sin(i * 0.25) * 0.02
```
## 5. Gestión de Keyframes y Feedback Visual
El manejo de eventos en Blender se traduce en la inserción de Keyframes. Cada iteración del ciclo calcula una posición y la "graba" en la línea de tiempo.
### El proceso de Animación:

1) Se calcula la ubicación (location) y rotación (rotation_euler).

2) Se aplica al objeto cámara.

3) Se inserta el fotograma clave para asegurar que Blender interpole el movimiento.

---
```python
# Aplicar posición y rotación a la cámara
cam.location = (offset_x, pos_y, 1.2 + rebote_z)
# Insertar Keyframes en los canales correspondientes
cam.keyframe_insert(data_path="location", frame=frame_actual)
cam.keyframe_insert(data_path="rotation_euler", frame=frame_actual)
```
## 6. Diseño del Escenario en Zigzag
Para que el pasillo no sea recto, se utiliza una función de frecuencia y amplitud que deforma el eje X a medida que avanzamos en el eje Y.
### Instrucciones de Acomodo:

* **Frecuencia:** Controla qué tan cerradas son las curvas.

* **Amplitud:** Define qué tan ancho es el desplazamiento lateral del zigzag.
## 7. Código Completo para Implementación
A continuación, el script íntegro para ejecutar en el panel de Scripting de Blender:

---
```python
import bpy
import math

def crear_material(nombre, color_rgb):
    mat = bpy.data.materials.new(name=nombre)
    mat.diffuse_color = (*color_rgb, 1.0)
    return mat

def animar_camara(largo, separacion_y, amplitud, frecuencia):
    if "CamaraPasillo" in bpy.data.objects:
        cam = bpy.data.objects["CamaraPasillo"]
    else:
        bpy.ops.object.camera_add()
        cam = bpy.context.active_object
        cam.name = "CamaraPasillo"

    pasos_por_bloque = 10 
    bpy.context.scene.frame_start = 1
    bpy.context.scene.frame_end = largo * pasos_por_bloque
    
    for i in range((largo * pasos_por_bloque) + 1):
        frame_actual = i
        progreso = i / pasos_por_bloque
        
        pos_y = progreso * separacion_y
        offset_x = math.sin(progreso * frecuencia) * amplitud
        
        rebote_z = math.sin(i * 0.5) * 0.05 
        balanceo_roll = math.sin(i * 0.25) * 0.02 
        
        cam.location = (offset_x, pos_y, 1.2 + rebote_z)
        
        siguiente_x = math.sin((progreso + 0.1) * frecuencia) * amplitud
        tangente = (siguiente_x - offset_x) / 0.1
        angulo_z = -math.atan(tangente)
        
        cam.rotation_euler = (math.radians(90), balanceo_roll, angulo_z)
        cam.keyframe_insert(data_path="location", frame=frame_actual)
        cam.keyframe_insert(data_path="rotation_euler", frame=frame_actual)

def generar_escenario_zigzag():
    bpy.ops.object.select_all(action='SELECT')
    bpy.ops.object.delete()

    mat_pared_a = crear_material("ParedOscura", (0.05, 0.05, 0.05))
    mat_pared_b = crear_material("ParedDetalle", (0.8, 0.2, 0.0))

    largo_pasillo = 30
    ancho_pasillo = 3
    amplitud = 2.5
    frecuencia = 0.4
    separacion_y = 2

    for i in range(largo_pasillo):
        offset_x = math.sin(i * frecuencia) * amplitud
        pos_y = i * separacion_y
        
        bpy.ops.mesh.primitive_cube_add(location=(-ancho_pasillo + offset_x, pos_y, 1))
        p_izq = bpy.context.active_object
        p_izq.data.materials.append(mat_pared_a if i % 2 == 0 else mat_pared_b)
        
        bpy.ops.mesh.primitive_cube_add(location=(ancho_pasillo + offset_x, pos_y, 1))
        p_der = bpy.context.active_object
        p_der.data.materials.append(mat_pared_a)

    bpy.ops.mesh.primitive_plane_add(size=1, location=(0, (largo_pasillo * separacion_y) / 2, 0))
    suelo = bpy.context.active_object
    suelo.scale.x = 20
    suelo.scale.y = largo_pasillo * separacion_y

    animar_camara(largo_pasillo, separacion_y, amplitud, frecuencia)

generar_escenario_zigzag()
```
### Resultado grafico:
<img width="1153" height="842" alt="image" src="https://github.com/user-attachments/assets/4c31501c-e4f5-447e-801b-1b3aed6f5b70" />


https://github.com/user-attachments/assets/9d87853a-ca44-4d07-aeb7-830045265ec8



## 8. Resolución de Problemas Comunes (Troubleshooting)
**❌ Problema 1: La cámara no se mueve o no tiene keyframes**

* **Causa:** No seleccionar la cámara antes de ejecutar o error en el nombre del objeto.

* **Solución:** Verificar que el data_path en keyframe_insert sea exactamente "location" o "rotation_euler".
**❌ Problema 2: El zigzag es demasiado brusco**

* **Causa:** Valores de frecuencia muy altos.

* **Solución:** Reducir el valor de la variable frecuencia (ej. 0.2 en lugar de 0.8).

**❌ Problema 3: Blender se congela al ejecutar**

* **Causa:** Demasiados pasos de animación o largo de pasillo excesivo.

* **Solución:** Reducir largo_pasillo para pruebas rápidas.
## 9. Glosario de Términos Técnicos Aplicados
* **Head Bobbing:** Técnica de animación que simula el movimiento de la cabeza al caminar.

* **Keyframe:** Fotograma clave que define el estado de una propiedad en un tiempo específico.

* **Tangente:** Línea que toca una curva; usada aquí para orientar la rotación de la cámara hacia adelante.

* **bpy (Blender Python):** API oficial para controlar Blender mediante scripts.

* **Euler Rotation:** Sistema de rotación basado en tres ángulos (X, Y, Z).
## Conclusión del Proyecto Integrador
La realización de este escenario procedural representa la integración definitiva de los conceptos de geometría computacional y animación paramétrica estudiados en esta unidad. Como estudiante de sistemas, este proyecto me permitió comprender que el entorno de Blender no es solo una herramienta de diseño manual, sino un motor potente que puede ser controlado mediante scripts de Python para generar mundos complejos de forma eficiente y reproducible.

Uno de los mayores aprendizajes fue la implementación técnica del Head Bobbing y el balanceo de cámara. A través de funciones trigonométricas ($\sin$ y $\cos$), logramos transformar un movimiento lineal rígido en una simulación orgánica que emula la percepción humana al caminar. Asimismo, el cálculo de la tangente para orientar la rotación de la cámara conforme avanza el zigzag demuestra cómo el cálculo matemático es el núcleo que da coherencia visual a cualquier animación procedural.

Finalmente, este proyecto refuerza la importancia del Manejo de Componentes y Estados en el desarrollo de software gráfico. Desde la creación automatizada de materiales hasta la gestión precisa de Keyframes en la línea de tiempo, queda claro que la programación es el puente que permite automatizar tareas creativas, reduciendo el margen de error y permitiendo la creación de escenarios que serían sumamente tediosos de modelar y animar cuadro por cuadro. Esta base técnica es fundamental para abordar retos futuros en áreas como la simulación, la realidad virtual y el desarrollo de motores de juegos.

