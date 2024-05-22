# Web3 Code LLM

![NearCoder draft training protocol](https://github.com/AnnaValentinaHirsch/Web3CodeLLM/blob/main/NearCoder.jpg)

The Web3 Code LLM aims to help blockchain developers in their coding challenges. While most web2 technologies are quite well furnished in term of tutorials and examples of code on forums and online courses, the web3 is still a recent technology, with a rather scattered technological landscape due to competing ecosystems releasing their own solutions and trying to grow a user base. 

The fast-changing set of coding languages and the nascent stage of the web3 industry makes it hard for it to hire developers willing to start coding from scratch and we believe that a powerful coding assistant would be a significant help in that context. 

The Web3 Code LLM is a StarCoder2-3b fine-tuned on the Near Protocol documentation, dApp structure and full dApps repositories collected from open GitHub repositories.

The Web3 Code LLM has been started as a [course project at the opencampus.sh](https://edu.opencampus.sh/en/course/477). 

The training protocol includes several steps (see figure 1):
- 0. Pre-training
- 1. Continued Pre-Training
- 2. Structure-Aware Fine-Tuning
- 3. Specialized Fine-Tuning

The dataset used for step 1. Continued Pre-Training is available at [Hugging Face](https://huggingface.co/datasets/jcarbonnell/preTrainingNEAR).