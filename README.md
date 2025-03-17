# Predicting Binding Affinity API

## Overview
This Flask-based API predicts the binding affinity of a given molecular structure using a pre-trained deep learning model. The API supports multiple molecular formats, including SMILES, InChI, SDF, MOL2, and PDB.

## Features
- Accepts molecular input in **SMILES, InChI, SDF, MOL2, and PDB** formats.
- Converts molecules into **Morgan fingerprints** using RDKit.
- Predicts **binding affinity** using a **TensorFlow deep learning model**.
- Normalizes and scales the predictions using a **pre-saved scaler**.

## Installation

### Prerequisites
Ensure you have Python installed (>=3.7) and install the required dependencies:

```bash
pip install numpy tensorflow flask joblib rdkit
```

### Running the API
To start the Flask server, run:
```bash
python app.py
```
The API will be available at `http://localhost:5000`.

## API Usage

### **Endpoint: `/predict`**
- **Method:** `POST`
- **Request Body (JSON):**
  ```json
  {
      "molecule": "CC(C)Cc1ccc(O)cc1",
      "file_type": "smiles"
  }
  ```
- **Supported file types:**
  - `smiles` (SMILES string)
  - `inchi` (InChI string)
  - `sdf` (path to SDF file)
  - `mol2` (path to MOL2 file)
  - `pdb` (path to PDB file)

### **Example Request using cURL**
```bash
curl -X POST http://localhost:5000/predict \  
     -H "Content-Type: application/json" \  
     -d '{"molecule": "CC(C)Cc1ccc(O)cc1", "file_type": "smiles"}'
```

### **Example Response**
```json
{
    "predicted_binding_affinity": -6.42
}
```

## Code Explanation
1. **Loading Model & Scaler**
   - The model (`ppar_morgan_model.keras`) is loaded using TensorFlow.
   - The scaler (`scaler.pkl`) is loaded using `joblib` for target normalization.

2. **Processing Molecular Input**
   - The function `process_input()` converts molecular representations into RDKit molecules.
   - The function `molecule_to_fingerprint()` generates **Morgan fingerprints** (radius=2, 2048 bits).

3. **Prediction Workflow**
   - Converts molecule into a fingerprint vector.
   - Uses the trained model to predict binding affinity.
   - Applies inverse transformation using `scaler`.

## Future Improvements
- Support batch predictions.
- Implement molecular validation checks.
- Deploy on cloud services (AWS, GCP, etc.).

## License
This project is licensed under the MIT License.

