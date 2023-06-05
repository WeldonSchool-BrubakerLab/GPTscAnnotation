# Project Title
This is a simple tool for single-cell people to gain insights about cell identities from GPT. Although it is still controversial whether the inference made by GPT is reliable, its reasoning can greatly help the analyst with relatively less knowledge of the biological context of the data.

## Installation
```
pip install gptscannotation
```

## Usage
```
from gptscannotation import chat_workflow
chat_workflow(adata, openaikey, openaikey, cluster_key, organism = 'unknown', tissue = 'unknown', DE_dfs = None, rank_key = None, display_conversation = True, 
                  double_verification = True, prior_knowledge = '', log_counts_layer = None, use_raw = False, ngenes = 20,
                  model_name = 'gpt-4', temperature = 0, top_p = 1, 
                  frequency_penalty = 0, presence_penalty = 0, sleep_time = 5, prompt_only = False)
```
adata: AnnData
Annotated data matrix.

openaikey: your openai key. 
The OpenAI API uses API keys for authentication. Visit your API Keys page to retrieve the API key you'll use in your requests.

cluster_key: str
The key of the cluster assignment under adata.obs (e.g., input 'leiden' if the cluster info is stored under adata.obs['leiden'])

organism: str (default: unknown)
A string describe the organism from which the data is obtained. Input 'unknown' if such info is unknown.

tissue: str (default: unknown)
A string describe the tissue from which the data is obtained. Input 'unknown' if such info is unknown.

DE_dfs: list of pandas DataFrame (default: None)
This function uses the sc.tl.rank_genes_groups ('wilcoxon' method) to run a quick DE analysis among all clusters. 

If you use other methods for DE analysis, please organize your result following the manner below in order to use the function:

Create 2 pandas dataframes, one for upregulations, one for down regulations.
The columns of the datdaframes are clusters, and the entries in each columns are the DE genes in each cluster, sorted descendingly by the magnitude of their fold change. 
The final DE_dfs should be a list like:
```
DE_dfs = [df_up, df_down]
```
rank_key: str (default: None)
If sc.tl.rank_genes_groups has been run, indicate the key where the result is stored under adata.uns

display_conversation: bool (default: True)
display the prompt and GPT's response

double_verification: bool (default: True)
The initial inference is made based on the up/down-regulated genes in all clusters. If true, the initial inference will be verified with additional expression information provided to GPT.

prior_knowledge: str (default: '')
If you have any prior knowledge you want GPT to know, describe it here in a concise way.

log_counts_layer: str (default: None)
This function takes the log transformed counts. By default, it will look for log counts at adata.X. 
If you stored the log counts under adata.layers, enter the key here.

use_raw: bool (default: False)
This function takes the log transformed counts. By default, it will look for log counts at adata.X. 
If the log counts is stored at adata.raw, enter 'True' here while keeping the log_counts_layer to default 'None'

ngenes: int (default: 20)
number of up- and down-regulated genes GPT should use for the inference of each cluster.

model_name: str (default: 'gpt-4')
ID of the model to use.  Please see the OpenAI's API reference https://platform.openai.com/docs/api-reference/introduction

temperature: float (default: 1.0)
The randomness of the response. Value ranges between 0 and 2. Higher values means more random answer. Please see the OpenAI's API reference https://platform.openai.com/docs/api-reference/introduction


top_p: float (default: 1.0)
An alternative to sampling with temperature, called nucleus sampling, where the model considers the results of the tokens with top_p probability mass. Please see the OpenAI's API reference https://platform.openai.com/docs/api-reference/introduction


frequency_penalty: float (default: 0.0) 
Number between -2.0 and 2.0. Positive values penalize new tokens based on their existing frequency in the text so far, decreasing the model's likelihood to repeat the same line verbatim. Please see the OpenAI's API reference https://platform.openai.com/docs/api-reference/introduction


presence_penalty: float (default: 0.0)
Number between -2.0 and 2.0. Positive values penalize new tokens based on whether they appear in the text so far, increasing the model's likelihood to talk about new topics. Please see the OpenAI's API reference https://platform.openai.com/docs/api-reference/introduction

sleep_time: float (default: 5.0)
Waiting time between each inquiry in seconds. Increase this number if reach the speed limit.

prompt_only: bool (default: False)
Set to 'True' if you only want to copy the prompt and paste it into the chatbox, which is a solution to use gpt-4 but you don't have access to the gpt-4 API.

## Publication
TBA
