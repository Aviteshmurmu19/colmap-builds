# COLMAP v3.14-dev (Portable Cloud Build)

This repository provides a **pre-compiled, portable version of COLMAP v3.14** (including the new Global Mapper/GLOMAP). It is specifically optimized for **Google Colab** and **Kaggle** environments using Tesla T4 GPUs.

### Why this exists?

Standard COLMAP installations in cloud notebooks often require long compilation times or face "missing library" errors. This "FAT" build includes all 200+ dependencies (SuiteSparse, Ceres, ONNX, etc.) bundled in a single archive.

---

## 🚀 Quick Setup (Colab / Kaggle)

Copy and paste this into a code cell to get running in seconds. This script automatically handles the download, extraction, and environment configuration.

```python
# 1. Download and Extract the FAT binary
!wget https://github.com/Aviteshmurmu19/colmap-builds/releases/download/v3.14-T4/colmap-v3.14-ubuntu22.04-cuda11.8-t4-FAT.tar.gz
!tar -xzvf colmap-v3.14-ubuntu22.04-cuda11.8-t4-FAT.tar.gz

# 2. Add to Environment Path (Works for both Kaggle and Colab)
import os
colmap_path = os.path.join(os.getcwd(), "colmap-v3.14-ubuntu22.04-cuda11.8-t4")

# Add binary to PATH
os.environ['PATH'] = f"{colmap_path}:{os.environ['PATH']}"

# Add bundled libraries to LD_LIBRARY_PATH (Appended to avoid breaking system tools)
os.environ['LD_LIBRARY_PATH'] = f"{colmap_path}/lib:{os.environ.get('LD_LIBRARY_PATH', '')}"

# 3. Verify Installation
!colmap -h

```

> [!TIP]
> **Pro-tip for Developers:** If system tools like `git` or `apt` show "symbol lookup errors" after running the setup above, it is due to library version conflicts. Simply run `%env LD_LIBRARY_PATH=` in a cell to reset your environment, then call COLMAP using:
> `!LD_LIBRARY_PATH={colmap_path}/lib colmap [args]`

---

## 🛠 Features Included

* **Integrated GLOMAP:** Use `colmap global_mapper` for rapid Global SfM.
* **CUDA Support:** Optimized for CUDA 11.8 (Standard on Colab/Kaggle).
* **Fully Portable:** No `apt-get install` required for math libraries.
* **Sift & ONNX:** Ready for modern feature extraction workflows.

## 📁 Usage in Notebooks

**Example: Running Automatic Reconstruction**

```bash
!colmap automatic_reconstructor \
    --workspace_path ./my_project \
    --image_path ./my_project/images \
    --use_gpu 1

```

**Example: Using Global Mapper**

```bash
!colmap global_mapper \
    --database_path ./project/database.db \
    --image_path ./project/images \
    --output_path ./project/sparse

```

## ⚠️ Notes

* **Environment:** This binary was built on Ubuntu 22.04 but is compatible with Ubuntu 24.04 (Kaggle/Codespaces) due to bundled libraries.
* **GPU:** Ensure your Notebook settings have "Hardware Accelerator" set to **GPU (T4)**.
