# 🔬 Tutorial Base – Calcolo del potenziale elettrostatico con NextGenPB

## 🎯 Obiettivo
Calcolare il potenziale elettrostatico di una proteina a partire da un file `.pqr` utilizzando NextGenPB.

---

## 📦 Requisiti

### Software
- Docker **oppure** Singularity
- ParaView (opzionale per visualizzare i risultati)

### File richiesti
- `1ABC.pqr` – File PQR della proteina
- `options.pot` – File di configurazione del calcolo

---

## 🚀 Step 1 – Preparazione

Crea una cartella per il progetto:

```bash
mkdir nextgenpb_tutorial
cd nextgenpb_tutorial
