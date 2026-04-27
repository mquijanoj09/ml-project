# Segmentación Semántica de Imágenes de Mascotas

**Curso:** Aprendizaje de Máquina Aplicado · EAFIT
**Profesor:** Marco Terán
**Autores:** Jose Blanco · Miguel Quijano
**Entrega:** 1

---

## Dataset elegido

**Oxford-IIIT Pet Dataset**, desarrollado por el Visual Geometry Group de la Universidad de Oxford. Es un benchmark estándar en visión por computador que contiene aproximadamente 7,349 imágenes de mascotas con anotaciones a nivel de píxel mediante _trimaps_.

| Campo                 | Valor                                       |
| --------------------- | ------------------------------------------- |
| **Fuente**            | Visual Geometry Group, University of Oxford |
| **URL de descarga**   | https://www.robots.ox.ac.uk/~vgg/data/pets/ |
| **Tamaño**            | ~7,349 imágenes con trimap                  |
| **Razas**             | 37 (25 perros, 12 gatos)                    |
| **Resolución**        | Variable (mediana 500 × 375 px)             |
| **Modalidad**         | Imágenes RGB + trimap de segmentación       |
| **Clases del trimap** | 1 = mascota, 2 = fondo, 3 = borde           |
| **Tarea**             | Segmentación semántica binaria              |
| **Licencia**          | CC BY-SA 4.0                                |

### ¿Por qué este dataset?

1. **Anotaciones pixel-nivel** necesarias para entrenar y evaluar segmentación.
2. **Tamaño manejable** (~7,300 imágenes) para el alcance del curso.
3. **Variabilidad suficiente** (37 razas, fondos y poses diversas) para que el problema no sea trivial.
4. **Benchmark reconocido** en visión por computador, lo que permite comparar con la literatura.
5. **Justifica plenamente el uso de CNNs** por ser datos de imagen con estructura espacial.

### Cómo descargar el dataset

Desde la URL oficial https://www.robots.ox.ac.uk/~vgg/data/pets/ se deben descargar dos archivos:

```bash
# Imágenes
wget https://www.robots.ox.ac.uk/~vgg/data/pets/data/images.tar.gz

# Anotaciones (trimaps)
wget https://www.robots.ox.ac.uk/~vgg/data/pets/data/annotations.tar.gz

# Extraer en data/raw/
tar -xzf images.tar.gz -C data/raw/
tar -xzf annotations.tar.gz -C data/raw/
```

Estructura esperada tras la descarga:

```
data/raw/
├── images/           # imágenes .jpg
└── annotations/
    ├── trimaps/      # máscaras .png
    └── list.txt      # split oficial trainval/test
```

### Conversión a tarea binaria

El trimap original tiene 3 clases que se colapsan a binario:

- `1` (foreground / mascota) → `1`
- `{2, 3}` (background + boundary) → `0`
