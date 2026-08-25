# FineTuningLlama3
Fine Tuning Llama 3 for generating structured outputs to be used by Riskfolio library to produce optimal portfolio allocation strategies.


## System Functionality 

**Disclaimer:** This project is for testing the functionality of fine tuning Llama 3 to utilize tools and should not be relied upon to make investment decisions. 

User gives a list of specific stocks to be optimized along with an optimal indication as to what type of risk measure they desire to be used (Dispersion or Downside). 
```
Example interaction
```
User gives a list of qualificates related to the stock's Sector, dividend rate, and PE ratio. 

```
Example interaction
```

User requests a plot of the most recently created profile. 

```
Example interaction
```




## Folders 

**yf-downloads** - stock data used during optimal portfolio creation

## Files 
**training_set.json** - pairs of inputs and outputs for llm to be used in training script

**financials.csv** - contains SP500 stock financial information. 

**llama_FT.ipynb** - python notebook for fine tuning llama 3.2 on training examples. 
Adapted from the example code from Unsloth which can be found here: https://unsloth.ai/docs/get-started/fine-tuning-llms-guide/tutorial-how-to-finetune-llama-3-and-use-in-ollama

**llama_riskfolio_inference.ipynb** - querying the model with a prompt and receiving the tool based answer with the optimal portfolio. 

## Training Set Generation Process

A script was used to generate a list of portfolio combinations that include either a list of stocks or a list of qualifations and a optional risk measure indication. 

```
example portfolio 
```



These types of portfolio combinations where inputted into 3 LLMs  (list them), which outputted hypothetical input queries. 

The structured target outputs were created using a simple program. 

These pairs were then used to train Llama 3.2 to take a user's query and output a structured description of the desired portfolio. 

## LoRA Hyper Parameters

