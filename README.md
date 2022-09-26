# Paraphrase
data and code for my paper
Multi-style transfer-based backdoor attack:
For example, to conduct backdoor attacks against BERT on SST-2:
CUDA_VISIBLE_DEVICES=0 python run_poison_bert.py --data sst-2 --poison_rate 20 --transferdata_path ../data/muti_style/sst-2 --origdata_path ../data/clean/sst-2  --bert_type bert-base-uncased --output_num 2
Paraphrase-based backdoor attack:
For example, to conduct backdoor attacks against BERT on SST-2:
CUDA_VISIBLE_DEVICES=0 python run_poison_bert.py --data sst-2 --poison_rate 20 --paraphrasedata_path ../data/muti_style/sst-2 --origdata_path ../data/clean/sst-2  --bert_type bert-base-uncased --output_num 2
