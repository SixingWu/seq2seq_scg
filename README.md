<h1>Seq2Seq Neural Machine Translation model with different optimization techniques</h1>

This is our implementation of seq2seq neural machine translation model with different optimization techniques. For details, see the corresponding [NIPS'17 workshop paper](https://arxiv.org/abs/1711.07724)

<h2>Setting up</h2>

1) Run the script (borrowed from [Harvard NLP repo](https://github.com/harvardnlp/BSO/tree/master/data_prep/MT)) to download and preprocess IWSLT'14 dataset:
```shell
$ cd preprocessing
$ source prepareData.sh
```
NOTE: this script requires Lua and luaTorch. As an alternative, you can download all necessary files from [this repo](https://github.com/pcyin/pytorch_nmt/tree/master/data)

2) Run this command to create vocab.bin:
```shell
$ python utils/vocab.py --train_src data/nmt_iwslt/train.de-en.de --train_tgt data/nmt_iwslt/train.de-en.en --output data/nmt_iwslt/vocab.bin
```


3) Download pretrained word vectors and run preprocessing script:
```shell
$ mkdir data/fasttext
$ cd data/fasttext
$ wget https://s3-us-west-1.amazonaws.com/fasttext-vectors/wiki.de.vec
$ wget https://s3-us-west-1.amazonaws.com/fasttext-vectors/wiki.en.vec
$ python pretrained_embeddings.py
```


<h2> Usage </h2>

1) Specify train and model parameters in ```utils/param_provider.py```

2) Run training

To train on CPU just execute ```train.py```:
```shell
$ python train.py
```

To train on GPU:
```shell
$ source train_on_gpu.sh gpu_id
```

for example:
```shell
$ source train_on_gpu.sh 0
```
to run on GPU with id 0.

By default, the script ```train.py``` do not evaluates the model during training. If you want to evaluate, run it with key ```--do_eval```:
```shell
$ python train.py --do_eval
```

Or, run the following script to train and eval on GPU:
```shell
$ source train_and_eval_on_gpu.sh gpu_id
```

3) Evaluate your model on test dataset

First, specify the model you want to evaluate as a 'start_model' in ```utils/param_provider.py```.

Then, run the previously trained model in inference mode:
```shell
$ python inference.py
```
Or, to infer on GPU:
```shell
$ source inference_on_gpu.sh gpu_id
```

Last, run this command to evaluate BLEU score:
```shell
$ python compute_bleu.py
```
