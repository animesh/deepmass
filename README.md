# setup

```bash
sudo apt install python-pip
pip install numpy --user
pip install pandas --user
pip install tensorflow==1.7.0
echo "deb [signed-by=/usr/share/keyrings/cloud.google.gpg] http://packages.cloud.google.com/apt cloud-sdk main" | sudo tee -a /etc/apt/sources.list.d/google-cloud-sdk.list
curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key --keyring /usr/share/keyrings/cloud.google.gpg add -
sudo apt-get update && sudo apt-get install google-cloud-sdk
git clone https://github.com/animesh/deepmass
cd deepmass/
gcloud init
gcloud auth list
mkdir data
DATA_DIR="./data"
mv data prism/.
cd prism/
vim ./data/input_table.csv
python preprocess.py   --input_data="${DATA_DIR}/input_table.csv"   --output_data_dir="${DATA_DIR}"   --sequence_col="ModifiedSequence"   --charge_col="Charge"   --fragmentation_col="Fragmentation"   --analyzer_col="MassAnalyzer"
gcloud ai-platform predict     --model deepmass_prism     --project deepmass-204419     --format json     --json-instances "${DATA_DIR}/input.json" > "${DATA_DIR}/prediction.results"
python postprocess.py     --metadata_file="${DATA_DIR}/metadata.csv"     --input_data_pattern="${DATA_DIR}/prediction.results*"     --output_data_dir="${DATA_DIR}"     --batch_prediction=False
less data/outputs.tsv
```

# DeepMass

DeepMass is a suite of tools in development at Verily to enable mass
spectrometry data analysis using modern machine learning techniques.

## Prism

The first tool to be included in DeepMass is Prism, a computational framework to
predict MS2 fragmentation peak intensities. Developed in collaboration with the
Max Planck Institute, Prism's accuracy approaches the theoretical limit imposed
by experimental uncertainty.

We provide Prism as a service using [Google Cloud Machine Learning
Engine](https://cloud.google.com/ml-engine). This repository contains
instructions and utilities to get started.

## Updates

We plan on expanding & updating DeepMass into a comprehensive suite of tools for
a broad array of Mass Spec capabilities. For information, updates, or to
contribute, please reach out to us at DeepMass@google.com.
