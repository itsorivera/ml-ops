### 1. La semilla fija (`seed=42`): ¿Es un *handicap* o limitación?

**No solo no es un handicap, es la regla de oro del método científico en Machine Learning.**

Imagina que entrenas tu modelo hoy con una semilla aleatoria (`seed` variable) y obtienes un resultado: **Precisión = 85%**.

Luego cambias un hiperparámetro crítico, por ejemplo la tasa de aprendizaje (`learning_rate`), y vuelves a entrenar. Obtienes: **Precisión = 89%**.

Aquí surge la pregunta mortal del Ingeniero de ML:
> *¿El modelo mejoró un 4% porque tu nuevo learning rate es mejor, o simplemente porque el orden aleatorio del dataset le tocó de suerte más fácil en esa corrida?*

Si no fijas la semilla (`seed=42`), **no puedes saberlo**, porque tienes dos variables moviéndose al mismo tiempo (el dataset aleatorio y tu hiperparámetro).

* **En desarrollo e investigación:** Se fija la semilla para aislar las variables. Así sabes que **cualquier cambio en la métrica se debe 100% a tus decisiones de ingeniería**, no al azar.
* **En producción/Validación cruzada (*Cross-Validation*):** Si quieres probar la robustez del modelo frente a la variabilidad del mundo real, no dejas la semilla libre al azar; entrenas formalmente con 3 o 5 semillas distintas conocidas (`seed=42`, `seed=123`, `seed=999`) y promedias los resultados.

---

### 2. Según la industria y referencias reales: ¿Cuál es el mínimo y máximo de datos para Fine-Tuning con LoRA?

En el mundo real de la industria (Meta, OpenAI, Mistral, Databricks), la respuesta depende de **qué le estás enseñando al modelo**:

```
¿Qué le estás enseñando al LLM?
 ├── A) Estilo, Tono, Formato o Clasificación (Instruction Tuning) ──> Pocos datos (100 - 5.000)
 └── B) Nuevo Dominio de Conocimiento / Jerga Técnica ───────────────> Muchos datos (10.000 - 100.000+)
```

#### A. Si solo quieres cambiar el **Estilo, Tono o Formato** (Instrucciones)
* **Mínimo:** **~50 a 100 ejemplos** de altísima calidad humana.
* **Típico en la industria:** **1.000 a 5.000 ejemplos**.
* **Referencia real:** El famoso paper de **Meta AI: *LIMA (Less Is More for Alignment)* (2023)**. 
  Meta demostró que entrenar un modelo LLaMA con solo **1.000 ejemplos curados a mano** superaba a modelos entrenados con 50.000 ejemplos automáticos. Los LLMs ya saben hablar y razonan; solo necesitan unas pocas decenas de ejemplos para entender *cómo* quieres que respondan (conciso, con disclaimer médico, etc.).

#### B. Si quieres enseñarle un **Dominio Específico o Jerga** (Medicina, Legal, Finanzas)
* **Mínimo:** **~5.000 a 10.000 ejemplos**.
* **Típico en la industria:** **20.000 a 100.000 ejemplos**.
* **Referencia real:** Modelos como **MedAlpaca** o **PMC-LLaMA** usaron entre **50.000 y 200.000 pares de preguntas/respuestas médicas** para que el modelo realmente interiorizara correlaciones de síntomas poco comunes y literatura clínica.

#### ¿Existe un "Máximo"? ¿Cuándo hay rendimientos decrecientes?
* En LoRA, **más no siempre es mejor**. Si le metes más de **100.000 o 200.000 ejemplos** usando LoRA con un rango bajo (ej. $$r=8$$ o $$r=16$$), saturas la capacidad matemática del adaptador. El adaptador empieza a olvidar habilidades generales (*catastrophic forgetting*).
* A partir de 100.000 ejemplos en adelante, la industria suele preferir:
  1. O bien aumentar el rango de LoRA a $$r=64$$ o $$r=128$$.
  2. O pasar a un **Full Fine-Tuning** (entrenar todas las capas del modelo, no solo LoRA), si el presupuesto de GPUs lo permite.

---

### Comparativa rápida para tu caso actual (MedQuAD):

| Escenario | Cantidad de datos | Tiempo aprox. en 1 GPU (T4/A10G) | Objetivo en la industria |
| :--- | :--- | :--- | :--- |
| **Tu notebook actual** | **96 ejemplos** | ~1 - 2 minutos | **Prototipado rápido:** Verificar que el código compila, no hay bugs de memoria y el formato encaja. |
| **Prueba de Concepto (PoC)** | **1.000 - 2.000 ejemplos** | ~20 - 40 minutos | Validar si el estilo y la estructura médica son consistentes. |
| **Producción Médica Real** | **Todo el dataset (16.000+)** | ~4 - 8 horas | Maximizar el conocimiento fáctico y la precisión de los diagnósticos. |