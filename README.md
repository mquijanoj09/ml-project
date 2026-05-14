# Segmentación Semántica de Imágenes de Mascotas

**Curso:** Aprendizaje de Máquina Aplicado · EAFIT
**Profesor:** Marco Terán
**Autores:** Jose Blanco · Miguel Quijano
**Estado:** Entrega 2 completada

---

## Dataset elegido

**Oxford-IIIT Pet Dataset**, desarrollado por el Visual Geometry Group de la Universidad de Oxford. Es un benchmark estándar en visión por computador que contiene aproximadamente 7,349 imágenes de mascotas con anotaciones a nivel de píxel mediante _trimaps_.

| Campo                 | Valor                                       |
| --------------------- | ------------------------------------------- |
| **Fuente**            | Visual Geometry Group, University of Oxford |
| **URL de descarga**   | https://www.robots.ox.ac.uk/~vgg/data/pets/ |
| **Tamaño**            | ~7,349 imágenes con trimap                  |
| **Razas**             | 37 (25 perros, 12 gatos)                    |
| **Tarea**             | Segmentación semántica binaria              |
| **Conversión binaria**| 1 (mascota) → 1; {2,3} (fondo, borde) → 0   |

---

## Entrega 2 — Resultados

### Comparación de las 3 familias de modelos

| Modelo | Params | Val IoU | Test IoU | Test Dice |
|---|---|---|---|---|
| SimpleCNN (E1) | 0.33M | 0.656 | 0.654 | 0.777 |
| U-Net scratch | 7.76M | 0.766 | 0.756 | 0.850 |
| **U-Net + ResNet18 preentrenado** ⭐ | **14.33M** | **0.818** | **0.811** | **0.887** |

### Decisión provisional

**Modelo elegido:** U-Net con encoder ResNet18 preentrenado en ImageNet
**Threshold óptimo:** 0.55
**Mejora vs Entrega 1:** +0.156 IoU absoluto (+24% relativo)

### Análisis adicionales realizados

- **Threshold sweep** sobre validación (rango 0.3-0.7)
- **BCE vs Dice Loss** sobre U-Net from scratch (BCE ganó por 1.6 puntos)
- **IoU por raza**: 37 razas analizadas; peores: miniature pinscher, shiba inu, american bulldog
- **5 peores casos cualitativos** analizados

### Protocolo de validación

- Split estratificado por raza (80% train / 20% val) sobre trainval oficial
- Test set oficial reservado, evaluado solo una vez al final
- Stats de normalización calculadas solo sobre train
- Semilla fija (42) y mismo split para los 3 modelos → comparación justa

---

## Estructura del repositorio

```
project/
├── README.md
├── requirements.txt
├── .gitignore
├── data/
├── figures/
├── notebooks/
│   ├── 01_eda_baseline.ipynb
│   └── 02_modelos.ipynb
├── report/
│   ├── reporte_entrega1.pdf
│   └── reporte_entrega2.pdf
└── poster/
```

---

## Cómo reproducir

### Google Colab (recomendado)

1. Abrir `notebooks/02_modelos.ipynb` en Colab.
2. **Runtime → Change runtime type → T4 GPU**.
3. **Runtime → Run all**.

Tiempo: ~40 min en T4.

### Local

```bash
pip install -r requirements.txt
mkdir -p data/raw && cd data/raw
wget https://www.robots.ox.ac.uk/~vgg/data/pets/data/images.tar.gz
wget https://www.robots.ox.ac.uk/~vgg/data/pets/data/annotations.tar.gz
tar -xzf images.tar.gz && tar -xzf annotations.tar.gz
cd ../..
jupyter notebook notebooks/02_modelos.ipynb
```
