# NEARCoder - Web3 Code LLM

![NearCoder draft training protocol](https://github.com/AnnaValentinaHirsch/Web3CodeLLM/blob/main/images/NearCoder.jpg).

## TL;DR

NEARCoder is a specialised language model designed to assist blockchain developers, 
particularly those working with the NEAR Protocol.
This project aims to bridge the gap in the nascent Web3 development landscape by providing a 
powerful coding assistant.

#### * Project Origin

NEARCoder Web3 Code LLM started as a [course project at the opencampus.sh](https://edu.opencampus.sh/en/course/477).

#### * Fine-tuning Process

Our fine-tuning protocol consists of three main steps:

1. **Continued Pre-Training**: General knowledge about the NEAR Protocol
2. **Structure-Aware Fine-Tuning**: Learning to produce directory structures from project descriptions
3. **Specialised Fine-Tuning**: Code generation based on AST-segmented NEAR dApps

#### * Datasets and Models

All datasets and models used in this project are open-sourced and available on [Hugging Face](https://huggingface.co/jcarbonnell).

#### * Presentation

A detailed presentation of NEARCoder was given at OpenCampus.sh on 17th June 2024. The slide deck is available [here](https://github.com/AnnaValentinaHirsch/Web3CodeLLM/blob/main/presentation/NEARCoder%20-%2010%20min%20presentation.pdf).

#### * Key Features

- Tailored assistance for NEAR Protocol development
- Generation of dApp structures and code
- Based on the state-of-the-art StarCoder2 model
- Trained on real-world NEAR Protocol projects and documentation



## 1. Introduction

Whilst Web2 technologies are well-supported with tutorials and code examples, 
Web3 remains a relatively new and rapidly evolving field. 
The blockchain industry faces challenges in hiring developers willing to start 
coding from scratch in this dynamic environment. NEARCoder addresses this need 
by offering tailored assistance for blockchain development.


NEARCoder is built on StarCoder2-3b, 
fine-tuned specifically for the NEAR Protocol using documentation, 
dApp structures, and full dApp repositories collected from open-source GitHub projects.


## 2. Background and Motivation

The advent of Web3 technologies, particularly blockchain-based systems, has introduced new paradigms in software development. However, the rapid evolution and fragmented nature of the Web3 landscape pose significant challenges for developers transitioning from traditional Web2 technologies. The NEAR Protocol, a layer-1 blockchain designed for usability and scalability, exemplifies the potential of Web3 whilst also highlighting the need for specialised developer tools.


### 2.1 The Web3 Development Landscape

Web3 technologies, including blockchain platforms, decentralised applications (dApps), and smart contracts, represent a paradigm shift in software development. Unlike Web2, which primarily deals with centralised systems and traditional programming paradigms, Web3 introduces concepts such as trustless interactions, decentralised consensus, and tokenomics.

### 2.2 Challenges in Web3 Development

The Web3 ecosystem faces several challenges:

1. Rapid evolution of technologies and standards
2. Fragmentation due to competing blockchain platforms
3. Steep learning curve for developers transitioning from Web2
4. Scarcity of experienced Web3 developers
5. Limited availability of comprehensive, up-to-date learning resources

### 2.3 The NEAR Protocol

The NEAR Protocol is a layer-1 blockchain platform designed to address some of the scalability and 
usability issues faced by earlier blockchain systems. 
Key features include:

- Sharding for scalability
- User-friendly account system
- WebAssembly-based smart contracts
- Proof-of-Stake consensus mechanism

##### 2.3.1. History

Started in 2017 as an AI project by 
Illia Polosukhin, co-author of “Attention is All You Need” [^1], Near.ai aimed to 
teach machines how to code before facing the challenge to pay worldwide contributors.
In 2018, Near creates the NEAR Protocol, a payment network on the blockchain,
promoting self-sovereignty of users’ data.
In May 2024, NEAR Protocol grew in a $8.2B market capitalisation, 
with millions of transactions every day, and announced the next big goal on their roadmap: 
**User-Owned AI**.

Despite its innovative features, NEAR Protocol development still faces the general challenges of the Web3 ecosystem.

### 2.4 The Need for Specialised Development Tools

Given the unique challenges of Web3 development and the specific features of the NEAR Protocol, there is a clear need for specialised tools to assist developers. Language models trained on general programming tasks often lack the specific knowledge required for effective Web3 development. This gap motivated the development of NEARCoder, a language model specifically tailored for NEAR Protocol development.

## 3. Methodology

NEARCoder is built on top of StarCoder2 (SOTA April 24)  and fine-tuned on the NEAR Protocol resources and GitHub repositories.

### 3.1 Data Collection and Preparation

Our data collection process began with scraping examples of NEAR dApps listed at Electric Capital. This extensive effort took approximately one month and resulted in the collection of 3,414 repositories, which were converted into text files after cleaning duplicates and removing empty projects.

From these text files of full repositories, we extracted:

- 3,414 dApp trees (for structure-aware fine-tuning)
- 23,166 dApp README files (translated into user prompts)
- 100,098 NEAR dApp code files (segmented with Abstract Syntax Trees)

This data was complemented by 1,154 documentation files providing general knowledge about NEAR, sourced from various official NEAR resources:

- nearDocs: 395 documents from Near Docs
- nearNode: 40 documents from Near Node Docs
- nearNEPs: 124 documents from Near Enhancement Protocol
- nearPapers: 3 papers from Near Papers
- nearWiki: 98 documents from Near Wiki

### 3.2 Dataset Publication

To ensure transparency and replicability, our training data have been prepared and published as three independent datasets on Hugging Face:

1. **preTrainingNEAR**: Used for the Continued Pre-Training of StarCoder2 on general NEAR Protocol knowledge.
2. **NEARdAppsPrompts**: Employed for the Structure-Aware Fine-Tuning of the NEAR-preTrainedStarCoder2, containing NEAR dApp trees and README files converted into user prompts.
3. **ASTCodeNEAR**: Utilized for the Specialised Fine-Tuning of the NEAR-structTunedStarCoder2, consisting of AST-segmented code files from NEAR dApps.

### 3.3 Training Process

Our training process consisted of three main steps:

#### 3.3.1 Continued Pre-Training

**Objective**: Train the model with general knowledge about the NEAR Protocol.

**Methods**:
- Supervised Fine-Tuning (SFT) of StarCoder2
- Hardware: NVIDIA A10G with 12 vCPU 46 GB RAM

**Result**: A new model "NEAR-preTrainedStarCoder2"

#### 3.3.2 Structure-Aware Fine-Tuning

**Objective**: Train the model to produce directory structures from project descriptions.

**Motivation**:
- The first challenge in development is often to structure a project well
- NEAR dApps often differ in structure from typical applications

**Methods**:
1. Create a new labelled dataset for Direct Preference Optimization (DPO)
2. Model Training with Direct Preference Optimization (DPO)
   - This method is more computationally efficient than reinforcement learning from human feedback (RLHF)

**Process**:
1. Use OpenAI API (GPT-4) to:
   - Extract key details from README files
   - Transform extracted details into user-friendly prompts
2. Generate "rejected" texts using NEAR-preTrainedStarCoder2 based on the created prompts
3. Use original tree structures as "accepted" texts

**Result**: A new model "NEAR-structTunedStarCoder2"

#### 3.3.3 Specialized Fine-Tuning

**Objective**: Fine-tune the model to produce actual code for NEAR dApps.

**Methods**:
- Supervised Fine-Tuning (SFT) of NEAR-structTunedStarCoder2 on AST-Segments
- Dataset: 100,098 code files (JS, RS, TS)
- Hardware: NVIDIA A10G with 12 vCPU 46 GB RAM

**Result**: The final model "NEARCoder"

## 4. Evaluation

To assess the performance and capabilities of NEARCoder, we designed a comprehensive evaluation framework. This framework consists of several tasks and metrics, each targeting different aspects of the model's functionality and relevance to NEAR Protocol development.

### 4.1 Evaluation Tasks

We employed the following tasks to evaluate NEARCoder:

1. **HumanEval**: A set of general code challenges in Javascript and Rust, designed to test the model's general coding abilities.

2. **NEAR Questions**: A collection of specific NEAR-related questions to assess the model's understanding of NEAR Protocol concepts and development practices.

3. **dApp Structures**: This task involves generating NEAR dApp trees with comments, testing the model's ability to create appropriate project structures for NEAR applications.

4. **Basic NEAR dApp**: A more complex task requiring the model to build a simple NEAR dApp, such as an event calendar or a snake game. This evaluates the model's capacity to generate functional NEAR-specific code.

### 4.2 Evaluation Metrics

For each task, we employed appropriate evaluation metrics:

1. **HumanEval**: We used the BLEU score to measure the similarity between the model's output and reference solutions.

2. **NEAR Questions**: We employed both BLEU and ROUGE-1 scores to assess the relevance and accuracy of the model's responses.

3. **dApp Structures**: We evaluated the structural similarity of the generated dApp trees compared to reference structures. Additionally, we assessed the quality and relevance of the generated comments.

4. **Basic NEAR dApp**: For this task, we evaluated the functionality, usability, and code quality of the generated dApp. This involved a combination of automated testing and expert review.


## 5. Results & Discussion

### 5.1 Performance Metrics

The performance results of the different models across various evaluation metrics are presented in the table below:

| Model                         | humanEval_bleu | nearQuestions_bleu | nearQuestions_rouge1 | dAppTrees_structure | dAppTrees_comment | dAppGen_functionality | dAppGen_usability | dAppGen_quality |
|-------------------------------|----------------|--------------------|----------------------|---------------------|-------------------|-----------------------|-------------------|-----------------|
| StarCoder2-3b                  | 0.076          | 0.53               | 0.28                 | 0.0240              | 0.0               | 0.0                   | 0.08              | 0.0830          |
| NEAR-preTrainedStarCoder2      | 0.080          | 0.52               | 0.29                 | 0.0275              | 0.0               | 0.0                   | 0.08              | 0.0830          |
| NEAR-structTunedStarCoder2     | 0.069          | 0.54               | 0.26                 | 0.0290              | 1.0               | 0.0                   | 0.08              | 0.0830          |
| NEARCoder-3b                   | 0.075          | 0.49               | 0.27                 | 0.0245              | 0.5               | 0.0                   | 0.00              | 0.1215          |

#### *Table 1: Performance Metrics*

The progression of model performance across different tasks during the training process is visualized in Image 1.

![NEARCoder training progress](https://github.com/AnnaValentinaHirsch/Web3CodeLLM/blob/main/images/evaluation.png)
### 5.2. Analysis by Steps

**Step 1:** The NEAR-preTrainedStarCoder2 model shows an improvement over the baseline StarCoder2-3b model on the NEAR_Questions test, indicating a successful enhancement in handling this specific evaluation task.

**Step 2:** The NEAR-structTunedStarCoder2 model demonstrates superior performance on the dApp_Trees test compared to the baseline. This suggests that structural tuning has had a positive impact on the model’s ability to perform tasks related to tree-based evaluations.

**Step 3:** However, the NEARCoder-3b model underperforms on the Basic_dApp_Development test, failing to meet the desired success criteria. Additionally, a decrease in accuracy is observed in the dApp_Trees test, indicating the need for further improvements to enhance the model’s performance across these evaluation metrics.

### 5.3. Impact of Training on Model Skills

- **Unaffected Skills:** 
  - General coding challenge
  - General NEAR knowledge

- **Improved Skills:** 
  - Structure tuning has led to improvements in dApp tree generation.

- **Decreased Skills:** 
  - The incorporation of AST-segments has resulted in diminished performance in dApp development tasks.

### 5.4. Hypotheses for Future Improvement

To address the observed shortcomings and further enhance model performance, we propose the following strategies:

- Enhance the AST-segments dataset to improve its effectiveness.
- Utilise AST-segments during the pre-training phase rather than fine-tuning.
- Replace the current -3b model with the more advanced StarCoder2-15b-instruct to leverage its capabilities.
- Develop a suitable evaluation metric specifically for coding tasks to better assess and tune the models' skillsets.

## 6. Conclusion

NEARCoder represents a significant step towards providing specialised support for NEAR Protocol developers. By fine-tuning a state-of-the-art language model on NEAR-specific data, we have created a tool that can assist developers in structuring projects, generating code, and understanding NEAR-specific concepts. However, the model still lacks in certain areas, particularly in generating functional code for NEAR dApps. Future work will focus on addressing these limitations and further enhancing the capabilities of NEARCoder to better serve the NEAR development community. Further research is needed to explore the potential of NEARCoder in other Web3 ecosystems and to develop more advanced models tailored to specific blockchain platforms.
