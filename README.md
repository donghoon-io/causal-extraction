# Causal Extraction

## Overview

Many scientific papers can be expressed in the format of a **causal relationship**. For example, a study on dietary interventions may propose specific behavioral changes and define causal pathways linking these interventions to observed outcomes.  

However, such causal relationships are rarely expressed in a standardized, machine-readable format. This project aims to bridge that gap by:  
1. **Identifying** papers that include “causal-diagram-like” representations (typically in figures),  
2. **Constructing** a ground-truth dataset from these diagrams, and  
3. **Training/Fine-tuning** a model capable of generating causal diagrams for papers that lack explicit ones.  

---

## Data Structure

The dataset is organized as follows:

```
representative_figure/
└── {keyword}/
  └── {batch_index}/

representative_figure_jsonified/
└── {keyword}/
  └── {batch_index}/

representative_tei/
└── {keyword}/
  └── {batch_index}/
```

### Structure Details

1. **Paper ID**  
   Each file follows a naming convention such as `W1490478993`, corresponding to the paper’s ID in [OpenAlex](https://openalex.org/). (e.g., W1490478993.tei.xml -> the paper text of the paper with the ID of W1490478993)

2. **Keyword and Batches**  
   Approximately eight keywords will be provided in the final dataset.
   Each keyword directory contains multiple **batches**, and each batch holds hundreds of papers.

3. **Representative Causal Figures**  
Using our custom retrieval and filtering pipeline, we have extracted one representative “causal-diagram-like” figure per paper.  
These are stored in the `representative_causal_figure/` directory and will serve as ground truth for model training.  

The corresponding metadata can be found in `representative_figure_metadata/`, where each JSON file includes:  
- `selected_index`: the figure’s index (starting from 1).  
- `rationale`: the reasoning behind its selection.  

To locate the corresponding figure:  
- Subtract 1 from the `selected_index` to match the figure’s position in `causal_figures/data/` (as `selected_index` starts from 1).  
- The figure’s image file can then be found in `figures/image/`, using the image name specified in the image URL of that json entity.

4. **Paper Text (TEI Format)**  
The `representative_tei/` directory contains the full text of each paper, formatted in TEI XML.
