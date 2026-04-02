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
