# DCASE 2019 Challenge: Task 5 - Urban Sound Tagging

This repository contains code and annotation files to reproduce our output.csv file system outputs that was submitted to take part in the [Task 5 (Urban Sound Tagging)](http://dcase.community/challenge2019/task-urban-sound-tagging) of the [DCASE 2019 Challenge](http://dcase.community/challenge2019).

## Download the dataset
The dataset used to reproduce the system outputs can be found at the following [link](https://drive.google.com/file/d/1_FnfVw8AxwcKXHh1-rvvdloF9tfioY3U/view?usp=sharing).

The dataset consists of the Task 5 development set and evaluation audio files, as well as audio files extracted from several sound classes from the following open external datasets: [FSDKaggle2018](https://zenodo.org/record/2552860#.XP4vMKMRXOS), [FSDnoisy18k](https://zenodo.org/record/2529934#.XP4vTKMRXOS), [UrbanSound8k](https://urbansounddataset.weebly.com/urbansound8k.html), [Urban-SED](http://urbansed.weebly.com/), [ESC-50-master](https://dataverse.harvard.edu/dataset.xhtml?persistentId=doi:10.7910/DVN/YDEPUT).


The audio data extracted from various sound classes of the open external datasets were stitched and split into 10-seconds audio files to fit the model training for this challenge.

## Installation
Please follow the installation guide that can be found at [DCASE 2019 Challenge: Task 5 - Urban Sound Tagging Baseline system](https://github.com/sonyc-project/urban-sound-tagging-baseline) and ensure that your system is able to get the baseline system output.

### Setting up
This setup guide assumes that your sonyc-ust directory is located at your home directory and that you have already run the following command:
```shell
export SONYC_UST_PATH=~/sonyc-ust
```

Extract the aforementioned dataset you downloaded into the sonyc-ust/data directory.
```shell
tar xf ./sonyc_ust_linus_all_files.tar.gz -C ~/sonyc-ust/data/
```

Activate the sonyc-ust environment
```shell
source activate sonyc-ust
```

Once you are in the sonyc-ust environment, pip install the [OpenL3](https://pypi.org/project/openl3/) python library.
```shell
pip install openl3
```

Extract the audio embeddings with OpenL3
```shell
mkdir ~/sonyc-ust/features/l3mel256emb512
cd ~/sonyc-ust/data
openl3 ./sonyc_ust_linus_all_files --content-type env --input-repr mel256 --embedding-size 512 --output ~/sonyc-ust/features/l3mel256emb512
```

Clone this repository
```shell
git clone https://github.com/linusng/sonyc-ust-challenge-2019.git
cd sonyc-ust-challenge-2019
```

Train our submitted model 1
```shell
# Fine-level
python classify_l3.py ./annotations_1.csv $SONYC_UST_PATH/data/dcase-ust-taxonomy.yaml $SONYC_UST_PATH/features/l3mel256emb512 $SONYC_UST_PATH/output baseline_fine --label_mode fine --num_hidden_layers 4

# Coarse-level
python classify_l3.py ./annotations_1.csv $SONYC_UST_PATH/data/dcase-ust-taxonomy.yaml $SONYC_UST_PATH/features/l3mel256emb512 $SONYC_UST_PATH/output baseline_coarse --label_mode coarse --num_hidden_layers 4
```

Train our submitted model 2
```shell
# Fine-level
python classify_l3.py ./annotations_2.csv $SONYC_UST_PATH/data/dcase-ust-taxonomy.yaml $SONYC_UST_PATH/features/l3mel256emb512 $SONYC_UST_PATH/output baseline_fine --label_mode fine --num_hidden_layers 3

# Coarse-level
python classify_l3.py ./annotations_2.csv $SONYC_UST_PATH/data/dcase-ust-taxonomy.yaml $SONYC_UST_PATH/features/l3mel256emb512 $SONYC_UST_PATH/output baseline_coarse --label_mode coarse --num_hidden_layers 3

```

Train our submitted model 3
```shell
# Fine-level
python classify_l3.py ./annotations_3.csv $SONYC_UST_PATH/data/dcase-ust-taxonomy.yaml $SONYC_UST_PATH/features/l3mel256emb512 $SONYC_UST_PATH/output baseline_fine --label_mode fine --num_hidden_layers 3 --num_epochs 20

# Coarse-level
python classify_l3.py ./annotations_3.csv $SONYC_UST_PATH/data/dcase-ust-taxonomy.yaml $SONYC_UST_PATH/features/l3mel256emb512 $SONYC_UST_PATH/output baseline_coarse --label_mode coarse --num_hidden_layers 3 --num_epochs 20
```

Train our submitted model 4
```shell
# Fine-level
python classify_l3.py ./annotations_3.csv $SONYC_UST_PATH/data/dcase-ust-taxonomy.yaml $SONYC_UST_PATH/features/l3mel256emb512 $SONYC_UST_PATH/output baseline_fine --label_mode fine --num_hidden_layers 3 --hidden_layer_size 256 --num_epochs 20

# Coarse-level
python classify_l3.py ./annotations_3.csv $SONYC_UST_PATH/data/dcase-ust-taxonomy.yaml $SONYC_UST_PATH/features/l3mel256emb512 $SONYC_UST_PATH/output baseline_coarse --label_mode coarse --num_hidden_layers 3 --hidden_layer_size 256 --num_eopchs 20
```
