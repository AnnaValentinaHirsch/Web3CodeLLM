NearData is made of:

- nearDapps: 3414 text versions of NEAR dApps repositories listed at [Electric Capital](https://github.com/electric-capital/crypto-ecosystems/blob/master/data/ecosystems/n/near.toml), collected between April 24th and May 17th using the GitHub scraper [RepoToText](https://github.com/JeremiahPetersen/RepoToText). 

- nearDappsCodeFiles: 100098 code files (after cleaning) in JavaScript (61262 unique files), Rust (13802 unique code files), Typescript (25034 unique code files) extracted in text formats from the near dApps files.
- nearDappsReadme: 23166 readme files extracted in text formats from the near dApps files.
- nearDappsTrees: 3414 text files representing the tree structure extracted from the nearDapps files.

- nearBlog: 481 blog articles from [Near Blog](https://near.org/blog) collected on May 13th 2024.
- nearBosWebEngine: 13 docs files from the [Near BOS Wen Engine](https://github.com/near/bos-web-engine) collected on May 21st 2024.
- nearDocs: 395 docs files from [Near Docs](https://docs.near.org) collected on May 13th 2024.
- nearNEPs: 124 docs files from the [NEAR Enhancement Protocol](https://github.com/near/NEPs) collected on May 21st 2024.
- nearNode: 40 docs files from the [Near Node Docs](https://github.com/near/node-docs) collected on May 21st 2024.
- nearPapers: 3 papers from the [Near Papers](https://near.org/papers) collected on May 21st 2024.
- nearWiki: 98 docs from the [Near Wiki](https://github.com/near/wiki) collected on May 21st 2024.

NearData has been published as three independent datasets on HuggingFace:
- [preTrainingNEAR](https://huggingface.co/datasets/jcarbonnell/preTrainingNEAR) for step 1- Continued Pre-Training of StarCoder2-3b with NEAR Protocol knowledge.
- [structTuningNEAR](https://huggingface.co/datasets/jcarbonnell/structTuningNEAR) for step 2- Structure-Aware Fine-Tuning of StarCoder2-3b with NEAR dApps trees and readme files converted into user prompts.
- [ASTCodeNEAR](https://huggingface.co/datasets/jcarbonnell/ASTCodeNEAR) for step 3- Fine-Tuning StarCoder2-3b with AST-segmented code files from NEAR dApps repositories.