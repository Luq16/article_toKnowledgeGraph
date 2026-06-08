# Text → Knowledge Graph → Neo4j (demo)

A minimal, reproducible notebook that builds a knowledge graph from unstructured
text with an LLM, loads it into **Neo4j**, and queries it back with Cypher.

**Notebook:** [`knowledge_graph_demo.ipynb`](knowledge_graph_demo.ipynb)

Pipeline: raw text → LLM structured distillation → entity/relation extraction
(`iText2KG`) → load into Neo4j → query with Cypher → visualize.

## Prerequisites

- Python 3.8+
- An **OpenAI API key**
- A **Neo4j** instance — a free [Neo4j Aura](https://neo4j.com/cloud/aura-free/)
  database is easiest (or local Neo4j via `bolt://localhost:7687`)

## Setup

```bash
# 1. create / activate a virtualenv, then install deps
pip install -r requirements_kg_demo.txt

# 2. provide credentials (copied from the template, then edited)
cp .env.example .env
#   edit .env and fill in OPENAI_API_KEY, NEO4J_URI, NEO4J_USERNAME, NEO4J_PASSWORD
```

`.env` is gitignored — keep your real keys there; never commit them.

## Run

Open the notebook and run the cells top to bottom:

```bash
jupyter lab knowledge_graph_demo.ipynb
# or use the VS Code / Jupyter extension
```

Cell 1 loads `.env` automatically (falls back to prompting if a value is missing).
By default it runs on a short inline sample text; uncomment the `PyPDFLoader`
block in section 2 to use your own PDF instead.

## Notes

- **Pinned versions matter.** `itext2kg==0.0.3` takes `openai_api_key` / `model_name`
  directly; newer releases change the API. Install from `requirements_kg_demo.txt`.
- **Aura URI** uses the `neo4j+s://` (TLS) scheme; local Neo4j uses `bolt://`.
- **Cost:** distillation + extraction call GPT-4o (a few cents for short text).
- **Empty graph?** Use longer, fact-dense source text — sparse text yields few relations.
