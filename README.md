# IndicTransTokenizer

The goal of this repository is to provide a simple, modular, and extendable tokenizer for [IndicTrans2](https://github.com/AI4Bharat/IndicTrans2) and be compatible with the HuggingFace models released. 

## Pre-requisites
 - `Python 3.8+`
 - [Indic NLP Library](https://github.com/VarunGumma/indic_nlp_library)
 - Other requirements as listed in `requirements.txt`

## Configuration
 - Installation from pip : `pip install indictrans-tokenizer`
 - Editable installation:
    - `git clone https://github.com/VarunGumma/IndicTransTokenizer`
    - `cd IndicTransTokenizer`
    - `pip install --editable ./`

## Usage
```python
import os
import json
import torch
from transformers import AutoModelForSeq2SeqLM
from IndicTransTokenizer import IndicTransTokenizer, IndicProcessor

tokenizer = IndicTransTokenizer(direction="en-indic")
ip = IndicProcessor(inference=True)
model = AutoModelForSeq2SeqLM.from_pretrained("ai4bharat/indictrans2-en-indic-dist-200M", trust_remote_code=True)

sentence = "This is a test sentence"

batch = ip.preprocess_batch([sentence], src_lang="eng_Latn", tgt_lang="hin_Deva")
batch = tokenizer(batch, src=True, return_tensors="pt")

with torch.inference_mode():
    outputs = model.generate(**batch, num_beams=5, num_return_sequences=1, max_length=256)

outputs = tokenizer.batch_decode(outputs, src=False) 
outputs = ip.postprocess_batch(outputs, lang="hin_Deva")
print(outputs)

>>> ['यह एक परीक्षण वाक्य है']
```

For using the tokenizer to train/fine-tune the model, just set the `inference` argument of IndicProcessor to `True`.

## Authors
 - Jay Gala (jaygala24@gmail.com)
 - Pranjal Agadh Chitale (pranjalchitale@gmail.com)
 - Varun Gumma (varun230999@gmail.com)
 - Raj Dabre (prajdabre@gmail.com)


## Bugs and Contribution
In case you encounter any bugs or want additional functionalities, please feel free to raise Issues/Pull Requests or contact the authors. 


## Citation
If you use our codebase, models or tokenizer, please do cite the following paper:
```
@article{
    gala2023indictrans,
    title={IndicTrans2: Towards High-Quality and Accessible Machine Translation Models for all 22 Scheduled Indian Languages},
    author={Jay Gala and Pranjal A Chitale and A K Raghavan and Varun Gumma and Sumanth Doddapaneni and Aswanth Kumar M and Janki Atul Nawale and Anupama Sujatha and Ratish Puduppully and Vivek Raghavan and Pratyush Kumar and Mitesh M Khapra and Raj Dabre and Anoop Kunchukuttan},
    journal={Transactions on Machine Learning Research},
    issn={2835-8856},
    year={2023},
    url={https://openreview.net/forum?id=vfT4YuzAYA},
    note={}
}
```