Esta captura muestra exactamente la "trastienda" de cómo funciona Hugging Face por dentro. Vamos a desgranar cada una de tus dudas con metáforas sencillas.

[](/fine-tuning/qwen3-4b-qlora/assets/llm-downloaded.png)

---

### 1. ¿Se descarga el modelo grande en el servidor remoto?

**Sí, pero en el DISCO DURO (almacenamiento), NO en la memoria GPU (VRAM).**

Hay que diferenciar tres lugares físicos:
1. **Los servidores de Hugging Face:** Donde el creador (Alibaba/Qwen) subió el modelo originalmente.
2. **El disco duro (SSD/Storage) de tu entorno:** Al ejecutar `snapshot_download(...)`, descargaste los archivos originales desde internet y los guardaste en la carpeta `/tmp/tensortonic/...`.
3. **La VRAM (memoria de la GPU):** El modelo todavía **no está cargado en la tarjeta gráfica**. Sigue siendo un montón de archivos "dormidos" en el disco duro.

---

### 2. ¿Qué son esos `blobs` y qué significan esos nombres raros con números y letras?

La palabra **BLOB** viene de la computación y significa **B**inary **L**arge **OB**ject (Objeto Binario Grande).

Hugging Face organiza su sistema de descargas de forma idéntica a cómo funciona **Git** por dentro:
* Cada archivo que descargas (pesos, configuraciones, tokenizadores) se guarda como un bloque de datos crudos.
* El nombre raro (`20a8a9156fc8c3f25...`, `378a91d3172359...`) es el **hash criptográfico (SHA256)** del contenido de ese archivo. 
* ¿Por qué lo hacen así? Para **evitar duplicados**. Si descargas dos modelos que comparten los mismos archivos de configuración, Hugging Face solo guarda un blob y crea accesos directos (*symlinks* o enlaces simbólicos) hacia la carpeta `snapshots/`. Así ahorran gigabytes de disco.

---

### 3. ¿Cada archivo dentro de `blobs/` es una parte de la red neuronal?

**Fíjate en los tamaños de los archivos en tu captura:**

* Tienes archivos diminutos: `239 B`, `16.5 KB`, `1.6 MB`.
  * Estos **no** son la red neuronal; son archivos de texto JSON con la arquitectura (`config.json`), el vocabulario del tokenizador (`tokenizer.json`), licencias, etc.
* Tienes dos "monstruos" gigantes:
  * Uno de **3.774,5 MB (~3.7 GB)**
  * Otro de **3.802,7 MB (~3.8 GB)**
  * Sumados dan unos **7.5 GB**, que es exactamente lo que pesa un modelo de 4 mil millones de parámetros (`Qwen-4B`) en formato original `bfloat16` ($$4\text{ mil millones} \times 2\text{ bytes} \approx 8\text{ GB}$$).

**Sí, esos dos archivos gigantes son trozos del modelo partido en pedazos (*shards*):**
* Un LLM es una matriz gigantesca de capas apiladas (Capa 0, Capa 1, ..., Capa 32).
* Hugging Face no guarda los 8 GB en un único archivo gigante porque si se corta internet al 90%, tendrías que volver a empezar desde cero.
* Lo parte en trozos de ~4 GB (usualmente llamados `model-00001-of-00002.safetensors` y `model-00002-of-00002.safetensors`). Esos son los dos archivos enormes que ves dentro de la carpeta `blobs/`.

---

### 4. Y después de cuantizar... ¿dónde se guardan los valores cuantizados?

Aquí es donde entra la celda que estás a punto de ejecutar (`BitsAndBytesConfig` + `AutoModelForCausalLM.from_pretrained(...)`):

#### El viaje paso a paso (Metáfora del camión de mudanzas):

```
[ Disco Duro (SSD) ]         ──> [ Memoria RAM del Sistema ] ──> [ Memoria GPU (VRAM) ]
  Archivos en 'blobs'               Descompresión/Carga            ¡AQUÍ SE GUARDAN!
   (En 16 bits = 7.5 GB)                                          (Cuantizados a 4 bits = ~2.5 GB)
```

1. **En el disco duro:** Los archivos de los `blobs` **NUNCA se modifican**. Siguen estando en 16 bits originales ocupando sus 7.5 GB.
2. **Durante la carga:** La librería `transformers` y `bitsandbytes` empiezan a leer esos bloques gigantescos del disco trozo a trozo.
3. **Al vuelo (On-the-fly):** A medida que lee los números del disco, el algoritmo los comprime a 4 bits (NF4).
4. **Destino final:** Los números cuantizados resultantes se colocan directamente en la **VRAM de tu tarjeta gráfica (GPU)**.

**En resumen:** 
Los valores cuantizados **solo viven en la memoria VRAM de la GPU mientras tu notebook esté encendido**. En lugar de ocupar los 7.5 GB en tu GPU, ocuparán únicamente alrededor de **2.5 GB a 3 GB de VRAM**, dejando todo el resto de la memoria libre para hacer el entrenamiento (los adaptadores LoRA, el optimizador y los textos que proceses).