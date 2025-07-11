# Text-MinHash-Priority

[![中文](https://img.shields.io/badge/文档-中文-blue?logo=markdown)](./README.zh-CN.md)
[![English](https://img.shields.io/badge/Docs-English-green?logo=markdown)](./README.md)

## Installation

Only tested with Python 3.10 so far.

```bash
pip install git+https://github.com/zmzhang2000/text-minhash-priority
```

## Features

This repository implements the **MinHash Near Deduplication with Priority** algorithm. Specifically, this algorithm differs from the original MinHash Near Deduplication algorithm in that
* it support to select the item to keep from duplicated items according to the priority

## Usage

1. Process your dataset into `huggingface dataset` format. I will provide a sample dataset with `jsonl` format.
```json
{"source": "ABC", "text": "What's your name?"}
{"source": "ABC", "text": "My name is John."}
```

2. Add `__keep__` or `__minhash_priority__` key to your dataset.
```json
{"source": "ABC", "text": "What's your name?", "__keep__": true}
{"source": "ABC", "text": "My name is John.", "__keep__": false}
```
or
```json
{"source": "ABC", "text": "What's your name?", "__minhash_priority__": 20}
{"source": "ABC", "text": "My name is John.", "__minhash_priority__": 1}
```

3. Run minhash deduplication script. Use `--column` to specify the column to deduplicate.
```bash
python -m text_dedup.minhash \
  --path "json" \
  --data_files "dataset.jsonl" \
  --split "train" \
  --cache_dir "./cache" \
  --output "dataset_deduplicated" \
  --column "text" \
  --ngram 4 \
  --threshold 0.8 \
  --batch_size 10000 \
  --use_auth_token true
```

4. The results will be saved with the `huggingface dataset` format. You can load the results with `datasets.load_from_disk()`.


## Acknowledgements
This repository is developed based on [ChenghaoMou/text-dedup](https://github.com/ChenghaoMou/text-dedup). More details can be found in the original repository.
