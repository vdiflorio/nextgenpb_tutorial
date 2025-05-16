### 📄 `run.md`

# ⚙️ Running NextGenPB

You can run NextGenPB using either **Docker** or **Singularity**.

---

## 🐳 Using Docker

```bash
sudo docker build -f Dockerfile -t <name_of_image>:latest .

sudo docker run -v "$(pwd)":/App <name_of_image>:latest \
  mpirun -np <number_of_processors> ngpb --potfile /App/options.pot --pqrfile /App/file.pqr
```

## ❄️ Using Apptainer

```bash
sudo apptainer build NextGenPB.sif recipe.def

apptainer exec --pwd /App   --bind $(pwd):/App /path/to/sif/NextGenPB.sif \
  mpirun -np <number_of_processors> ngpb --potfile /App/options.pot --pqrfile /App/file.pqr
```
