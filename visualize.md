### 📄 `visualize.md`

# 📊 Visualizing Results with ParaView

Once the `.vtk` file is generated, you can view it using **ParaView**.

---

## 🧭 Steps

1. Open **ParaView**
2. Go to `File > Open`, select the `.vtk` file
3. Click **Apply** in the Properties panel
4. Use "Color By" → `Potential`
5. Add filters such as:
   - `Slice` to inspect cross sections
   - `Contour` for isopotential surfaces
   - `Volume` to explore in 3D

---

## 🖼️ Example Visualization

ParaView allows overlaying the molecule structure (e.g., from `.pdb`) with the potential field for detailed inspection.

Go back to [Run Instructions](run.md) or return to [Tutorial Home](index.md).
