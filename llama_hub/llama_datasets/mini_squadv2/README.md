# Mini Squad V2 Dataset

[![arize (100 x 40 px)](https://github.com/nerdai/llama-hub/assets/92402603/eb4cb77a-1a1a-48a0-9f9d-277798832200)](https://arize.com/)

This dataset was prepared in collaboration with Xander Song of Arize AI.

## CLI Usage

You can download `llamadatasets` directly using `llamaindex-cli`, which comes installed with the `llama-index` python package:

```bash
llamaindex-cli download-llamadataset MiniSquadV2Dataset --download-dir ./data
```

You can then inspect the files at `./data`. When you're ready to load the data into
python, you can use the below snippet of code:

```python
from llama_index import SimpleDirectoryReader
from llama_index.llama_dataset import LabelledRagDataset

rag_dataset = LabelledRagDataset.from_json("./data/rag_dataset.json")
documents = SimpleDirectoryReader(
    input_dir="./data/source_files"
).load_data()
```

## Code Usage

You can download the dataset to a directory, say `./data` directly in Python
as well. From there, you can use the convenient `RagEvaluatorPack` llamapack to
run your own LlamaIndex RAG pipeline with the `llamadataset`.

```python
from llama_index.llama_dataset import download_llama_dataset
from llama_index.llama_pack import download_llama_pack
from llama_index import VectorStoreIndex

# download and install dependencies for benchmark dataset
rag_dataset, documents = download_llama_dataset(
  "MiniSquadV2Dataset ", "./data"
)

# build basic RAG system
index = VectorStoreIndex.from_documents(documents=documents)
query_engine = index.as_query_engine()

# evaluate using the RagEvaluatorPack
RagEvaluatorPack = download_llama_pack(
  "RagEvaluatorPack", "./rag_evaluator_pack"
)
rag_evaluator_pack = RagEvaluatorPack(
    rag_dataset=rag_dataset,
    query_engine=query_engine
)
benchmark_df = rag_evaluator_pack.run()  # async arun() supported as well
```

## Original data citation

```tex
@article{2016arXiv160605250R,
       author = {{Rajpurkar}, Pranav and {Zhang}, Jian and {Lopyrev},
                 Konstantin and {Liang}, Percy},
        title = "{SQuAD: 100,000+ Questions for Machine Comprehension of Text}",
      journal = {arXiv e-prints},
         year = 2016,
          eid = {arXiv:1606.05250},
        pages = {arXiv:1606.05250},
archivePrefix = {arXiv},
       eprint = {1606.05250},
}
```
