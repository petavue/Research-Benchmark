# LLM-INFERENCE

This repository hosts a collection of scripts designed to facilitate the inference of different Large Language Models (LLMs) across various platforms, including Anyscale, OpenAI, Amazon Bedrock, Anthropic, Google Gemini, and self-hosted models in Runpod servers. The scripts aim to streamline the process of running LLM models on a dataset containing a total of 360 rows(can be changed).

The [BIRD](https://bird-bench.github.io/) dataset consists of natural language questions related to a SQL database schema, along with their corresponding correct SQL queries. Leveraging this dataset, the scripts orchestrate the execution of LLMs on identical questions and schemas provided in the dataset. Subsequently, the SQL queries generated by the LLMs are compared against the ground truth queries from the dataset.

Additionally, the repository empowers users to assess LLM models comprehensively by considering various performance metrics. By analyzing these metrics, users can make informed decisions regarding model selection, optimization, and deployment strategies.

## Usage
## Inferencing
For inferencing diffrerent LLM models across different platform clone this repository,
```bash
git clone https://github.com/petavue/llm-research.git
```
The benchmark_scripts folder contains scricts which are specific to the each platform and use appropriate scripts for inference.
Eg: For inferencing GPT-3.5 with Open-AI api use open-ai.py script:
```bash
python open-ai.py -h 
```
The above command will give all the instructions which will be needed to run the inference, The script asks for model name, number of instruction to be passed in the prompt, dataset size. When these are not provided the default values are take. default value for dataset size is 360, if you want to run for different dataset lenght make sure that there is a csv file with  name in the format bird_equal_split_<dataset_length>.csv, use data_extraction.py and Final_equal_split.py to get create a dataset of different size, These are also explained in the Data section.

The default bird_equal_split_360.csv will have 360 rows which contain natural language question, schema, correct SQL query, hardness in each row, Also the dataset is made in suah a way that there are equal number of SQL query for each hardness catogory(120 simple, 120 moderate, 120 challenging). This dataset is used in our benchmark. The default instruciton used in the benchmark can also be changed in the common_functions.py file.

Command to run GPT-3 in OpenAI api using 5 instruction and 360 as dataset size:
```bash
python open-ai.py --models gpt-3 --inst 5 --il 360 
```

The scripts creates folders and files which are needed for the evaliation scripts,

inference_results
    \__ environment_name
            \__ short_size
                    \__ model_name
                            \__ instruction_size

Each instruction_size file will contain dev_gold.sql, dev.json, execution-log.jsonl, predict_dev.json, metrics.csv, gold.text, predicted.txt which are used by evaluation scripts.

## Evaluation

For evaluation scripts clone the Github repo [DAMO-ConvAI](https://github.com/AlibabaResearch/DAMO-ConvAI/tree/main). Detete folders other than 'bird', this folder contains all the inference scripts which are needed for the evaluating the accuracy of the llm in SQL generation, Now replace the eavluation.py which is present in the DAMO-ConvAI/bird/llm/src/ with the evaluation.py which is present in helper_scripts folder of this repo. Before running the evaluation script we need to create a folder called evaldata in DAMO-ConvAI/bird/llm/ which will be used by the accuracy.py script present in this repo.

Now to evaluate all the inference run be the infrerence script we use accuracy.py which will take all the files created by the inference script and give accuracy of each model for a specific shot, instruction, dataset size. To run accuracy.py we have to make some changes in the code. In accuracy.py in line 12 provide the path of the DAMO-ConvAI and also the path of the bird_equal_split_360.csv file.

use below command to run the accuracy.py script(Note: make use that accuracy.py is present inside inference_results folder which was created by the inference script because  the accuracy.py will recursively search the folders to evaluate all the models which has been run using the inference script)
```bash
python accuracy.py 
```
## Metrics
For finding the metrics like throughput, cost, total_input_tokens, total_output_tokensuse metrics_to_excel.py which will create excel file for all the metrics calculated for the inferencing, in line 10 provide the path of the bird_equal_split_360.csv for the script to work properly.
```bash
python metrics_to_excel.py 
```
## Data

