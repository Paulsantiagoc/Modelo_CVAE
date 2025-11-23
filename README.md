# 🧠 Monthly-Conditioned CVAE for Synthetic Energy & Weather Scenario Generation

Este repositorio contiene la implementación de un **Conditional Variational Autoencoder (CVAE) condicionado por el mes del año**, diseñado para generar **escenarios sintéticos realistas** de variables energéticas y meteorológicas.  
El modelo aprende distribuciones multivariadas complejas a partir de datos históricos y permite simular perfiles completos diarios de 24 horas con coherencia física y temporal.

---

## 📌 Características principales

- ✔️ Generación **simultánea** de múltiples variables: clima, producción solar, producción eólica, demanda, radiación, velocidad del viento y más.  
- ✔️ Modelo **probabilístico** basado en CVAE utilizando variables latentes gaussianas.  
- ✔️ Condicionamiento por **mes del año** mediante *one-hot encoding*.  
- ✔️ Arquitectura Encoder–Latent Space–Decoder optimizada con ReLU + Adam.  
- ✔️ Métricas implementadas: MSE, MAE, PSNR, Z-Norm, Z-Var.  
- ✔️ Evaluación con **Time Series Clustering** para comparar centroides reales vs sintéticos.  
- ✔️ Reproducción fiel de patrones determinísticos (p. ej., generación solar).  
- ✔️ Buena representación de variables estocásticas (p. ej., viento).

---

## 📂 Estructura del repositorio

```
📁 data/
    ├── daily_matrices/
    └── labels.csv
📁 src/
    ├── encoder.py
    ├── decoder.py
    ├── cvae_model.py
    ├── train.py
    └── utils.py
📁 results/
    ├── clusters/
    └── metrics/
📄 README.md
📄 requirements.txt
```

---

## 📊 Datos utilizados

Los datos consisten en una serie temporal multivariable con:

- **9000+ registros**
- **44 variables climáticas y energéticas**
- Resolución: **1 hora**
- Agrupadas en matrices diarias de **24 × 44**

---

## 🏗️ Arquitectura del CVAE

### 🔸 Encoder
- Entrada: matriz diaria aplanada + vector condicional (mes)
- Capas densas con activación ReLU
- Salida:  
  - μ (media)  
  - σ (desviación estándar)  

### 🔸 Decoder
- Entrada: z + condición mensual
- Reconstruye matriz de 24×44

### 🔸 Función de pérdida (ELBO)
Loss = BCE + KL

---

## 🚀 Entrenamiento

**Parámetros principales:**

- Latent dimension: **20**
- Épocas: **200**
- Optimizador: **Adam**
- Condición: **One-Hot Encoding de 13 meses**
- Batch size sugerido: 64

---

## 📈 Resultados

Evaluación por **Time Series Clustering**, comparando centroides reales vs generados.

### 🔹 Producción Fotovoltaica
- MSE ≈ 1.74×10⁻⁵  
- Reproducción casi perfecta del perfil solar

### 🔹 Producción Eólica
- MSE ≈ 1.39×10⁻⁵  
- Buena aproximación pese a su naturaleza estocástica

---

## 🧪 Ejemplo de uso

```python
from src.cvae_model import CVAE
import torch

model = CVAE(input_dim=1056, latent_dim=20, cond_dim=13)
model.load_state_dict(torch.load("cvae_weights.pth"))
model.eval()

condition = one_hot_encode(1, num_classes=13)
z = torch.randn(1, 20)

generated = model.decode(z, condition)
print(generated.shape)
```

---

## 🧩 Instalación

```
git clone https://github.com/usuario/Modelo_CVAE
cd Modelo_CVAE
pip install -r requirements.txt
```

---

## 🤝 Contribuciones

Contribuciones abiertas vía **Issues** o **Pull Requests**.

---

## 📜 Cita

> “Monthly-Conditioned CVAE for Realistic-Synthetic Energy and Weather Scenarios in Power Systems”,  
> Corella, Grijalva, Acuña, USFQ – 2025.

---

## 📄 Licencia

MIT License
