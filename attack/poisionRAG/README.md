##  PoisionRAG for LLM
Attact method form https://github.com/sleeepeer/PoisonedRAG

## Acknowledgement
* [corpus-poisoning.](https://github.com/princeton-nlp/corpus-poisoning)
* [beir benchmark](https://github.com/beir-cellar/beir).
* [contriever](https://github.com/facebookresearch/contriever) for retrieval augmented generation (RAG).

## Poision data generate with Corpus Poisoning Attack 

```bash
MODEL=contriever
DATASET=nq-train
OUTPUT_PATH=results/advp
k=10

mkdir -p $OUTPUT_PATH

for s in $(eval echo "{0..$((k-1))}"); do

python src/attack_poison.py \
   --dataset ${DATASET} --split train \
   --model_code ${MODEL} \
   --num_cand 100 --per_gpu_eval_batch_size 64 --num_iter 5000 --num_grad_iter 1 \
   --output_file ${OUTPUT_PATH}/${DATASET}-${MODEL}-k${k}-s${s}.json \
   --do_kmeans --k $k --kmeans_split $s

done
```


## Evaluate the attack
`python run.py`
