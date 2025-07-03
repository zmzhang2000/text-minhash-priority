# 优先级文本最小哈希

[![中文](https://img.shields.io/badge/文档-中文-blue?logo=markdown)](./README.zh-CN.md)
[![English](https://img.shields.io/badge/Docs-English-green?logo=markdown)](./README.md)

## 安装

目前只经过Python 3.10的测试。

```bash
pip install git+https://github.com/zmzhang2000/text-minhash-priority
```

## 特性

本仓库实现**优先级文本最小哈希去重**算法。本算法与原始文本最小哈希去重算法不同点在于
* 支持根据优先级选择去重后的保留项

## 使用说明

1. 把需要去重的数据处理成`huggingface dataset`格式。我将会用`jsonl`格式的数据做演示。
```json
{"source": "ABC", "text": "What's your name?"}
{"source": "ABC", "text": "My name is John."}
```

2. 往数据集中添加`__keep__`或`__minhash_priority__`字段。
```json
{"source": "ABC", "text": "What's your name?", "__keep__": true}
{"source": "ABC", "text": "My name is John.", "__keep__": false}
```
或者
```json
{"source": "ABC", "text": "What's your name?", "__minhash_priority__": 20}
{"source": "ABC", "text": "My name is John.", "__minhash_priority__": 1}
```

3. 运行minhash去重脚本。使用`--column`指定去重所依据的列。
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

4. 结果文件将会保存为`huggingface dataset`格式。你可以使用`datasets.load_from_disk()`加载结果。

## 致谢

该仓库基于[text-dedup](https://github.com/ChenghaoMou/text-dedup)开发。更多详细信息请查看原始仓库。
