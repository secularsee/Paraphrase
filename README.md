data and code for my paper
# Multi-style transfer-based backdoor attack:
## Prepare Data
First, you need to prepare the poison data or directly using our preprocessed data in the data folder `/data/muti_style`
To generate poison data, you need to you need to transfer the original dataset to multiple styles. We implement it based on the [code](https://github.com/martiansideofthemoon/style-transfer-paraphrase). Please move to check the details.
## Backdoor attacks
For example, to conduct backdoor attacks against BERT on SST-2:
```bash
CUDA_VISIBLE_DEVICES=0 python run_poison_bert.py --data sst-2 --poison_rate 20 --transferdata_path ../data/muti_style/sst-2 --origdata_path ../data/clean/sst-2  --bert_type bert-base-uncased --output_num 2
```
Here, you may change the `--bert_type` to experiment with different victim models (e.g. roberta-base, distilbert-base-uncased). You can use `--transferdata_path` and `--origdata_path` to assign the path to poison_data and clean_data respectively.
# Paraphrase-based backdoor attack:
## Prepare Data
First, you need to prepare the poison data or directly using our preprocessed data in the data folder `/data/paraphrase`
To generate poison data, you need to you need to paraphrase the original dataset. 
## Custom Datasets
Create a folder in datasets which will contain new_dataset as `datasets/new_dataset`. Paste your plaintext `train/dev/test` splits into this folder as `train.txt`, `dev.txt`, `test.txt`. Use one instance per line (note that the model truncates sequences longer than 50 subwords). Add `train.label`, `dev.label`, `test.label` files (with same number of lines as `train.txt`, `dev.txt`, `test.txt`). These files will contain the style label of the corresponding instance.
1. To convert a plaintext dataset into it's BPE form run the command,
```bash
python datasets/dataset2bpe.py --dataset datasets/new_dataset
```
2. Next, for converting the BPE codes to `fairseq` binaries and building a label dictionary, first make sure you have downloaded RoBERTa and setup the `$ROBERTA_LARGE global` variable in your `.bashrc`. Then run,
```bash
datasets/bpe2binary.sh datasets/new_dataset
```
3. Paraphrase the dataset by using pretrained Gpt_2_large model.
```bash
python datasets/paraphrase_splits.py --dataset datasets/new_dataset
```
4. Convert the BPE file back into its raw text form.
```bash
python datasets/bpe2text.py --input datasets/new_dataset --output datasets/paraphrase
```

## Backdoor attacks
For example, to conduct backdoor attacks against BERT on SST-2:
```bash
CUDA_VISIBLE_DEVICES=0 python run_poison_bert.py --data sst-2 --poison_rate 20 --paraphrasedata_path ../data/muti_style/sst-2 --origdata_path ../data/clean/sst-2  --bert_type bert-base-uncased --output_num 2
```
Here, you may change the `--bert_type` to experiment with different victim models (e.g. roberta-base, distilbert-base-uncased). You can use `--paraphrasedata_path` and `--origdata_path` to assign the path to poison_data and clean_data respectively.
