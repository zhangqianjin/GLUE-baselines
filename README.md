# GLUE Baselines
This repo contains the code for baselines for the [Generalized Language Understanding Evaluation](https://gluebenchmark.com/) (GLUE) benchmark.
See [our paper](https://openreview.net/pdf?id=rJ4km2R5t7) for more details about GLUE or the baselines.


## Downloading GLUE

We provide a convenience python script for downloading all GLUE data and standard splits.

After downloading GLUE, point ``PATH_PREFIX`` in  ``src/preprocess.py`` to the directory containing the data.

If you are blocked from s3.amazonaws.com (as may be the case in China), downloading MRPC will fail, instead you can run the command below:

```
#git clone https://github.com/wasiahmad/paraphrase_identification.git
python download_glue_data.py --data_dir glue_data --tasks all --path_to_mrpc=paraphrase_identification/dataset/msr-paraphrase-corpus
```

## Running

To run our baselines, use ``src/main.py``.
Because preprocessing is expensive (particularly for ELMo) and we often want to run multiple experiments using the same preprocessing, we use an argument ``--exp_dir`` for sharing preprocessing between experiments. We use argument ``--run_dir`` to save information specific to a particular run, with ``run_dir`` usually nested within ``exp_dir``.


```
python main.py --exp_dir EXP_DIR --run_dir RUN_DIR --train_tasks all --word_embs_file PATH_TO_GLOVE
```

NB: The version of AllenNLP used has [issues](https://github.com/allenai/allennlp/issues/342) with tensorboard. You may need to substitute calls ``from tensorboard import SummaryWriter`` to ``from tensorboardX import SummaryWriter`` in your AllenNLP source files.


## GloVe, CoVe, and ELMo

Many of our models make use of [GloVe pretrained word embeddings](https://nlp.stanford.edu/projects/glove/), in particular the 300-dimensional, 840B version.
To use GloVe vectors, download and extract the relevant files and set ``word_embs_file`` to the GloVe file.
To learn embeddings from scratch, set ``--glove`` to 0.

We use the CoVe implementation provided [here](https://github.com/salesforce/cove).
To use CoVe, clone the repo and fill in ``PATH_TO_COVE`` in ``src/models.py`` and set ``--cove`` to 1.

We use the ELMo implementation provided by [AllenNLP](https://github.com/allenai/allennlp/blob/master/tutorials/how_to/elmo.md).
To use ELMo, set ``--elmo`` to 1. To use ELMo without GloVe, additionally set ``--elmo_no_glove`` to 1.

## Reference

If you use this code or GLUE, please consider citing us.

```
 @unpublished{wang2018glue
     title={{GLUE}: A Multi-Task Benchmark and Analysis Platform for
             Natural Language Understanding}
     author={Wang, Alex and Singh, Amanpreet and Michael, Julian and Hill,
             Felix and Levy, Omer and Bowman, Samuel R.}
     note={arXiv preprint 1804.07461}
     year={2018}
 }
```

Feel free to contact alexwang _at_ nyu.edu with any questions or comments.
