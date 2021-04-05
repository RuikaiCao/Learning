# Install and Setup spaCy

## Install pip, virtualenv on Ubuntu 18.04 (with Python 3.6 pre-installed)

```bash
sudo apt install python3-pip
sudo apt install build-essential libssl-dev libffi
sudo apt install python3-venv
```

## Create and activate virtualenv "spacy"

```bash
cd
mkdir python-env
cd py_env
python3 -m venv spacy
source ~/py_env/spacy/bin/activate
```

## Install spaCy

```bash
pip install -U pip setuptools wheel
pip install -U spacy[transformers,lookups]
python -m spacy download zh_core_web_sm
python -m spacy download zh_core_web_trf
python -m spacy download en_core_web_sm
python -m spacy download en_core_web_trf
```

## Install other useful packages

```bash
pip install numpy scipy sklearn matplotlib seaborn pandas statsmodels fastcluster jupyterlab
```