# ChineseNMT

基于transformer的英译中翻译模型🤗。

项目说明参考知乎文章：[教你用PyTorch玩转Transformer英译中翻译模型！](https://zhuanlan.zhihu.com/p/347061440)

## Data Process

### 分词

- 工具：[sentencepiece](https://github.com/google/sentencepiece)
- 预处理：`./data/get_corpus.py`抽取train、dev和test中双语语料，分别保存到`corpus.en`和`corpus.ch`中，每行一个句子。
- 训练分词模型：`./tokenizer/tokenize.py`中调用了sentencepiece.SentencePieceTrainer.Train()方法，利用`corpus.en`和`corpus.ch`中的语料训练分词模型，训练完成后会在`./tokenizer`文件夹下生成`chn.model`，`chn.vocab`，`eng.model`和`eng.vocab`，其中`.model`和`.vocab`分别为模型文件和对应的词表。

## Model

采用Harvard开源的 [transformer-pytorch](http://nlp.seas.harvard.edu/2018/04/03/attention.html) ，中文说明可参考 [传送门](https://zhuanlan.zhihu.com/p/144825330) 。

## Requirements

This repo was tested on Python 3.6+ and PyTorch 1.5.1. The main requirements are:

- tqdm
- pytorch >= 1.5.1
- sacrebleu >= 1.4.14
- sentencepiece >= 0.1.94

To get the environment settled quickly, run:

```
pip install -r requirements.txt
```

## Usage

模型参数在`config.py`中设置。

- 由于transformer显存要求，支持MultiGPU，需要设置`config.py`中的`device_id`列表以及`main.py`中的`os.environ['CUDA_VISIBLE_DEVICES']`。

## Results

| Model | NoamOpt | LabelSmoothing | Best Dev Bleu | Test Bleu |
| :---: | :-----: | :------------: | :-----------: | :-------: |
|   1   |   No    |       No       |     24.07     |   24.03   |
|   2   |   Yes   |       No       |   **26.08**   | **25.94** |
|   3   |   No    |      Yes       |     23.92     |   23.84   |

实验结果在`./experiment/train.log`文件中，测试集翻译结果在`./experiment/output.txt`中。

## Model

训练好的 Model 2 模型（当前最优模型）可以在如下链接直接下载😊：

链接: https://pan.baidu.com/s/1cjZglVAgpVSwTo5TlhSwzQ  密码: ifj2

## Beam Search

当前最优模型（Model 2）使用beam search测试的结果

| Beam_size |   2   |   3   |   4   |     5     |
| :-------: | :---: | :---: | :---: | :-------: |
| Test Bleu | 26.59 | 26.80 | 26.84 | **26.86** |

