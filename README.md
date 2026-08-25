# Fine Tuning Llama 3 for Riskfolio Library Interaction
Fine Tuning Llama 3 for generating structured outputs to be used by Riskfolio library to produce optimal portfolio allocation strategies.


## System Functionality 

**Disclaimer:** This project is for testing the functionality of fine tuning Llama 3 to utilize tools and should not be relied upon to make investment decisions. 

**First type of interaction:**

User gives a list of specific stocks to be optimized along with an optimal indication as to what type of risk measure they desire to be used (Dispersion or Downside). 
```
Please make for me an optimized stock portfolio using the following tickers: AAPL, NVDA, MSFT, ALLE, ALGN, AMG, AET, AES. I would like the portfolio to be optimized to reduce dispersion.
```
Generated response: 
```
Sure, I'll generate an optimized stock portfolio for you.
The portfolio will consist of the following stock tickers: AAPL, NVDA, MSFT, ALLE, ALGN, AMG, AET, AES
Portfolio will be optimized using a dispersion-based risk measure.
**Disclaimer:** AI-generated information is provided for educational purposes only and should not be considered financial advice.
Applying Riskfolio Library...
Here is the optimal portfolio distribution:
	NVDA 	58%
	AMG  	37%
	AAPL 	4%
```

**Second type of interaction:**

User gives a list of qualifications related to the stock's Sector, dividend rate, and PE ratio. 
```
Please find stocks in the utilites, technology, and healthcare sectors, to make an optimal portfolio to minimize potential downsides. I would also like these stocks to have a diviend rate higher than 3% and a PE ratio less than 40. 
```
Generated response: 
```
Absolutely, I can generate the portfolio you're looking for.
The portfolio will contain stocks with the following qualities:
	Belonging to the following sectors: Utilities, Technology, Healthcare
	Dividend rate above 3.0%
	PE ratio below 40
Portfolio will be optimized using a downside-based risk measure.
**Disclaimer:** AI-generated information is provided for educational purposes only and should not be considered financial advice.
Applying Riskfolio Library...
Here is the optimal portfolio distribution:
	ETR  	59%
	FE   	21%
	CNP  	12%
	NI   	7%
```

**Third type of interaction:**

User requests a plot of the most recently created profile using **!** as a hard command indicator. 

```
!plot allocation
```

<img width="1011" height="611" alt="pieChart" src="https://github.com/user-attachments/assets/29ebd5e4-c653-41fa-a2a0-7a0df588b880" />

```
!plot frontier
```

<img width="1011" height="611" alt="frontier" src="https://github.com/user-attachments/assets/df869df0-e0c4-4fba-bba9-ff4b56a8615c" />

```
!plot frontier area
```
<img width="1011" height="611" alt="structure" src="https://github.com/user-attachments/assets/6359db69-b3d5-48d3-aa23-13cdabafc82a" />





## Folders 
These folders are not present on this page due to memory constraints but they have been uploaded to Kaggle and can be downloaded into your working directory. 

**yf-downloads** - Historical stock data downloaded using the yfinance library. Currently includes stocks in the SP500 which had historical stock data for at least 5 years. Can be downloaded from: https://www.kaggle.com/datasets/bandrewensinger/historicaldata  
In order for a symbol to be used by the tool, it must be present in this folder. 

**riskfolioAdapter** - this contains the LoRA Adapter to be applied to the LLM during inference. Can be downloaded from:  https://www.kaggle.com/models/bandrewensinger/riskfolioft/. 

## Files 
**training_set.json** - Pairs of text inputs and outputs for fine tuning an LLM. The input is a user asking for an optimized portfolio using either a given set of stocks or a set of qualifications for what stocks should be used. The output is a structured explanation of the problem to solve using the Riskfolio library. 

**fundamentals.csv** - Contains SP500 stock fundamental data. Used by the system to filter by Dividend rate, PE ratio, and Sector.  
Downloaded from https://www.kaggle.com/datasets/franoisgeorgesjulien/s-and-p-500-companies-with-financial-information 

**llama_FT.ipynb** - Jupyter notebook for fine tuning llama 3.2 on the training examples. 
Adapted from the example code from Unsloth which can be found here: https://unsloth.ai/docs/get-started/fine-tuning-llms-guide/tutorial-how-to-finetune-llama-3-and-use-in-ollama

**llama_riskfolio_inference.ipynb** - Jupyter notebook to query the fine-tuned model with a prompt, and receiving the tool-based answer with the optimal portfolio. 

## Training Set Generation Process

A script was used to generate a list of portfolio combinations that include either a list of stocks or a list of qualifications and a optional risk measure indication. 

Example: 
```
{
	stock_list: ["ICE", "RF", "ISRG", "AVB", "NTRS", "AEP", "COST", "ITW", "ROST"],
	sectors: null,
	dividend: null,
	PE: null,
	risk_measure: "dispersion"
}

{
	stock_list: null,
	sectors: ["Consumer Discretionary", "Utilities", "Health Care", "Consumer Staples"],
	dividend: 6.0,
	PE: 24,
	risk_measure: "downside"
}
```

These types of portfolio combinations where inputted into 3 LLMs  (Gemma, Llama, Phi), which outputted hypothetical input queries given an input query such as:
```
"Please make a natural sounding query from a user asking for an optimal portfolio allocation strategy to an ai system. \nThey would like to invest exclusively in the following tickers: ['ICE', 'RF', 'ISRG', 'AVB', 'NTRS', 'AEP', 'COST', 'ITW', 'ROST']. The elements in this list must be included but the list can take any form, such as a numbered list, comma separated values, etc.  \nThey would like the portfolio to limit dispersion. Make sure that dispersion and downside are differentiated. So if it sounds like they want to limit downside, do not make it also sound like they want to limit dispersion. And vice versa. \nThis information does not need to be in any particular order or have any set number of sentences."
```

The structured target outputs were created using a simple program that didn't require an LLM because no variance in structure was desired. 

## Future Work 
* Adding more stock historical data and fundamentals for system access
* Fine tuning on more fundamental data types
* Fine tuning on more precise risk measure requests

