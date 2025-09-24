# **Improving LLM Accuracy for SQL Generation with Memory Tuning**

This repository demonstrates an iterative process to significantly enhance the accuracy of Large Language Models (LLMs) in generating SQL queries. Using Llama3-8B-Instruct as the base model, we showcase how to systematically identify and correct hallucinations, improving valid SQL generation from an initial **30% to over 95%** accuracy.

## **The Problem: LLM Hallucination in SQL Generation**

When prompting a general-purpose LLM to generate SQL queries, it can often hallucinate, leading to errors that fall into two main categories:

1. **Invalid SQL:** The generated query is syntactically incorrect and will fail to execute. This can include using non-existent column names, incorrect function names, or improper formatting.  
2. **Malformed SQL:** The query is syntactically valid but semantically incorrect. It runs without error but does not actually answer the user's original question, leading to incorrect data or results.

This project tackles these issues head-on using an iterative data-centric approach.

## **The Solution: Iterative Memory Tuning**

We employ Lamini's Memory Tuning technique, which allows for rapid and efficient model improvement. The core idea is to iteratively refine the model by identifying its weaknesses, generating targeted training data to address them, and continuously evaluating its performance.

Our iterative workflow is as follows:

1. **Initial Evaluation:** Start with a small, manageable evaluation dataset (e.g., \~2200 question-SQL pairs) to quickly benchmark the base model's performance.  
2. **Tune the Model:** Fine-tune the base LLM on this initial dataset.  
3. **Diagnose Hallucinations:** Evaluate the tuned model's output. We use another LLM as a judge to programmatically identify where and how the model is failing.  
4. **Targeted Data Generation:** Use LLMs to generate new, synthetic data specifically designed to correct the identified hallucinations and weaknesses.  
5. **Expand and Refine Dataset:** Add the newly generated data to our training and evaluation sets. This step also includes filtering and manual cleaning to ensure data quality.  
6. **Re-iterate:** With the expanded and improved dataset, we tune the model again and repeat the cycle.

This process allows us to systematically "teach" the model the nuances of our database schema and the patterns of correct SQL queries.

## **Evaluation Strategy**

A robust evaluation framework is critical for measuring progress and guiding the iterative process. Our evaluation strategy is built on the following principles:

* **Quantitative:** Track clear metrics (e.g., percentage of valid SQL) to measure improvement over time.  
* **Diagnostic:** The evaluation should pinpoint specific areas of weakness and hallucination to inform the next round of data generation.  
* **Scalable & Automated:** Use LLMs as scorers to automate the evaluation process, allowing for rapid iteration on large datasets.  
* **Progressively Difficult:** As the model improves on easier examples, we intentionally expand the evaluation set with more complex and challenging hallucination examples to continue pushing its capabilities.  
* **Version Tracking:** We log evaluation results for each model iteration to track which changes led to the best performance gains.

## **Getting Started**

This repository provides the necessary code to:

1. Set up the Llama3-8B-Instruct model.  
2. Write full, structured prompts with special tokens to define roles and manage multi-turn conversations.  
3. Run the evaluation and tuning pipeline.

Example of a structured prompt for the model:

\<|begin\_of\_text|\>\<|start\_header\_id|\>system\<|end\_header\_id|\>

You are an expert SQL agent. Generate a valid SQL query based on the user's question.\<|eot\_id|\>\<|start\_header\_id|\>user\<|end\_header\_id|\>

{user\_question}\<|eot\_id|\>\<|start\_header\_id|\>assistant\<|end\_header\_id|\>

## **Future Work**

While this repository focuses on the core iterative improvement loop, the methodology can be extended to:

* **Enforce Specific Formatting:** Tune the model to always produce SQL in a particular style or format.  
* **Optimize Query Performance:** Teach the model to generate more efficient and performant SQL queries.

By following this iterative, data-centric approach, developers can build highly accurate and reliable LLM-powered applications.