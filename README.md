# FinReflectKG Multi-Hop QA Dataset

## Overview

**Paper**: FinReflectKG - MultiHop: Financial QA Benchmark for Reasoning with Knowledge Graph Evidence

**Authors**:
- Abhinav Arun, Domyn, New York, US (abhinav.arun@domyn.com)
- Reetu Raj Harsh, Domyn, India (reeturaj.harsh@domyn.com)
- Bhaskarjit Sarmah, Domyn, Gurugram, India (bhaskarjit.sarmah@domyn.com)
- Stefano Pasquali, Domyn, New York, US (stefano.pasquali@domyn.com)

The **FinReflectKG Multi-Hop QA Dataset** is a comprehensive collection of financial domain question-answer pairs designed for evaluating multi-hop reasoning capabilities in knowledge graphs. This dataset contains **555 high-quality questions** derived from financial documents (10-K filings) of S&P 100 companies across 3 years (2022-2024) and across GICS sectors.

## Dataset Statistics

- **Total Questions**: 555
- **Multi-hop Coverage**: 2-hop (290 questions) and 3-hop (265 questions) reasoning patterns
- **Document Relationships**:
  - Intra-document: 270 questions (48.6%)
  - Inter-document same company: 231 questions (41.6%) 
  - Inter-document cross company: 54 questions (9.7%)
- **Data Sources**: S&P 100 company 10-K filings (2022-2024)
- **Domain**: Financial/Corporate data
- **Language**: English

## GICS Sector Coverage

The dataset covers companies across all major GICS (Global Industry Classification Standard) sectors:

- **Financials**: 18 companies
- **Information Technology**: 17 companies  
- **Health Care**: 15 companies
- **Industrials**: 13 companies
- **Communication Services**: 10 companies
- **Consumer Staples**: 10 companies
- **Consumer Discretionary**: 9 companies
- **Energy**: 3 companies
- **Utilities**: 3 companies
- **Real Estate**: 2 companies
- **Materials**: 1 company

## Files Description

### Dataset Files

**`final_master_dataset.json`** - Complete dataset
- Contains all 555 questions
- File size: ~5.11 MB
- Ready for research and evaluation

## Data Structure

Each question entry contains the following fields:

```json
{
  "question_id": 1,
  "question": "What was the year-over-year change in...",
  "answer": "The provision decreased from $159 million...",
  "pattern": "ORG_REG -> Regulates -> ORG -> Has_Stake_In -> SEGMENT -> Discloses -> FIN_METRIC",
  "entities": {
    "start": "FRB",
    "node_2": "GS", 
    "node_3": "Asset & Wealth Management",
    "end": "Provision for Credit Losses"
  },
  "entity_types": {
    "start": "ORG_REG",
    "node_2": "ORG",
    "node_3": "SEGMENT", 
    "end": "FIN_METRIC"
  },
  "hop_count": 3,
  "document_relationship": "inter_document_cross_company",
  "path_data": {
    "start_node": {...},
    "end_node": {...},
    "hop_1_rel": {
      "chunk_text": "...",
      "page_id": "page_21",
      "chunk_id": "chunk_1",
      "source_file": "GS_10k_2024.pdf"
    },
    "hop_2_rel": {...},
    "hop_3_rel": {...}
  }
}
```

### Key Fields Explained

- **`question_id`**: Unique identifier (1-555)
- **`question`**: Natural language question
- **`answer`**: Comprehensive answer with reasoning
- **`pattern`**: Multi-hop reasoning pattern (entity types and relationships)
- **`entities`**: Specific entities involved in the reasoning path
- **`entity_types`**: Entity type classifications (ORG, PERSON, FIN_METRIC, etc.)
- **`hop_count`**: Number of reasoning hops (2 or 3)
- **`document_relationship`**: Relationship between source documents
- **`path_data`**: Evidence chunks from original documents supporting the reasoning

## Multi-Hop Patterns

The dataset covers various multi-hop reasoning patterns including:

1. **Regulatory Oversight**: `ORG_REG -> Regulates -> ORG -> Discloses -> FIN_METRIC`
2. **Corporate Hierarchy**: `ORG -> Has_Stake_In -> SEGMENT -> Discloses -> FIN_METRIC`
3. **Person-Organization**: `PERSON -> Member_Of -> ORG -> Discloses -> FIN_METRIC`
4. **Cross-Entity Dependencies**: `SEGMENT -> Depends_On -> ORG -> Has_Stake_In -> SEGMENT`
5. **And more complex 2-hop and 3-hop patterns**

## Document Relationship Types

1. **Intra-document**: Questions where all evidence comes from the same document
2. **Inter-document same company**: Questions spanning multiple years of the same company
3. **Inter-document cross company**: Questions requiring information from different companies

## Usage Examples

### Loading the Dataset

```python
import json

# Load dataset
with open('final_master_dataset.json', 'r') as f:
    dataset = json.load(f)

questions = dataset['questions']
metadata = dataset['metadata']

print(f"Total questions: {metadata['total_questions']}")
print(f"First question: {questions[0]['question']}")
```

### Filtering by Hop Count

```python
# Get all 3-hop questions
three_hop_questions = [q for q in questions if q['hop_count'] == 3]
print(f"3-hop questions: {len(three_hop_questions)}")
```

### Filtering by Document Relationship

```python
# Get cross-company questions
cross_company = [q for q in questions if q['document_relationship'] == 'inter_document_cross_company']
print(f"Cross-company questions: {len(cross_company)}")
```

## Evaluation Metrics

This dataset is designed for evaluating:

- **Multi-hop Reasoning**: Ability to connect information across multiple entities
- **Financial Domain Knowledge**: Understanding of financial concepts and relationships
- **Cross-document Understanding**: Reasoning across different documents
- **Entity Relationship Modeling**: Understanding of corporate structures and regulations

## Data Quality

- **Evidence-Grounded**: All answers supported by source document chunks
- **Diverse Patterns**: Multiple reasoning patterns and complexity levels

## Citation

If you use this dataset in your research, please cite:

```bibtex
@article{finreflectkg2024,
  title={FinReflectKG - MultiHop: Financial QA Benchmark for Reasoning with Knowledge Graph Evidence},
  author={Arun, Abhinav and Harsh, Reetu Raj and Sarmah, Bhaskarjit and Pasquali, Stefano},
  year={2024},
  organization={Domyn}
}
```
