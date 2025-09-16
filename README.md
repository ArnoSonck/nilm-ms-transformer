# NILM — Transformer con Atención Multiescala

Este repositorio contiene un notebook con un modelo **Transformer multiescala** para **NILM** (desagregación no intrusiva). Incluye:
- Notebook: `notebooks/NILM_Transformer_Multiscale.ipynb`
- `environment.yml` (Conda) y `requirements.txt` (pip/venv)

> Recomendación: usa **Python 3.10/3.11**. Algunas librerías (p. ej., PyTorch) no ofrecen builds estables para 3.13 aún.

## 1) Crear ambiente virtual

### Opción A: Conda (recomendada)
```bash
conda env create -f environment.yml
conda activate nilm-ms-transformer
python -m ipykernel install --user --name nilm-ms-transformer --display-name "Python (nilm-ms)"
```

> GPU (opcional): consulta https://pytorch.org/get-started/ para instalar la variante con CUDA correcta para tu sistema.

### Opción B: venv + pip
```bash
python3 -m venv .venv
# Linux/macOS
source .venv/bin/activate
# Windows PowerShell
# .venv\Scripts\Activate.ps1

pip install --upgrade pip
pip install -r requirements.txt
python -m ipykernel install --user --name nilm-ms-transformer --display-name "Python (nilm-ms)"
```

## 2) Ejecutar el Notebook
```bash
jupyter lab
# Abrir notebooks/NILM_Transformer_Multiscale.ipynb
```

## 3) Conectar datos reales (UK-DALE/REFIT)
1. Re-muestrea la señal agregada y del aparato a **1 Hz** (o 6 s) de forma consistente.
2. Crea un `Dataset` tipo PyTorch que devuelva ventanas `[T, 1]` de entrada/salida.
3. Sustituye `DummyDataset` en el notebook por tu dataset real y ajusta `T`, `batch_size`, etc.
4. Añade métricas (MAE, RMSE, SAE/NDE, F1) en nuevas celdas.

## 4) Respaldo en GitHub

### A) Iniciar repo y primer commit
```bash
git init
git add .
git commit -m "Init NILM multiscale transformer demo"
```

### B) Crear repositorio remoto
- Opción 1 (GitHub CLI):  
  ```bash
  gh repo create nilm-ms-transformer --public --source=. --remote=origin --push
  ```
- Opción 2 (manual desde web): crea repo vacío en GitHub y luego:
  ```bash
  git remote add origin https://github.com/<tu-usuario>/nilm-ms-transformer.git
  git branch -M main
  git push -u origin main
  ```

### C) (Opcional) Git LFS para datos grandes
```bash
git lfs install
git lfs track "*.h5" "*.npz" "*.csv" "data/**"
git add .gitattributes
git commit -m "Track large data with LFS"
git push
```

> No subas datasets con licencias restrictivas si no está permitido. Considera ignorar carpetas `data/`.

## 5) Buenas prácticas
- Usa `.gitignore` para excluir `.venv`, `__pycache__`, `data/`, outputs pesados.
- Fija semillas y documenta versiones de paquetes.
- Exporta resultados a `results/` y guarda gráficos/metrics para trazabilidad.