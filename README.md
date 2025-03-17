Key Points

User Instructions & Input: The script starts by printing detailed instructions and then prompts the user to enter the path to a folder containing the documents.

Document Processing: It scans for TXT, PDF, and DOCX files. Depending on the file extension, it uses appropriate libraries (e.g. PyPDF2 for PDFs, python-docx for DOCX).

Subject Parsing: The function determine_subject uses predefined keyword lists for each subject (e.g., Finance, HR, IT, etc.) and returns the subject with the highest keyword occurrence. If no keywords match, it defaults to "General".

Task Generation: For each subject, it “generates” tasks in batches. Each task contains a task description, domain, and recommended execution. In a real-world scenario, you would integrate more advanced NLP (using the loaded distilbert-base-uncased model) to extract these details. The task generation function simulates creating tasks in batches. After each batch, it checks for domain imbalance. If any domain dominates by more than 10% over an equal distribution, it rebalances the dataset.

Bias Detection & Rebalancing: After each batch, the script checks the distribution of domains. If any domain is over-represented by more than 10% relative to an equal split, it triggers a rebalancing step. If too many rebalances occur too early, it stops and asks for more data.

Dataset Size Monitoring: The script writes a temporary Parquet file to measure its size. Once the file exceeds 50GB, it notifies the user that the dataset is ready for LLM fine-tuning.

FP16 & Device Handling: The script loads the model with FP16 if available (unless disabled by the user) and automatically falls back to CPU if no GPU is found.

Output: Finally, it saves a separate Parquet file per subject into a subfolder (parquet_output), ready to be used in an Airflow DAG.

Permanent Parquet Files:
The function append_tasks_to_parquet checks if a Parquet file for a subject already exists. If so, it loads the existing data, appends the newly generated tasks, and overwrites the file. Otherwise, it creates a new Parquet file.


GPU/FP16 Support:
The script loads distilbert-base-uncased using FP16 if a CUDA-enabled GPU is available (unless disabled by the --no_fp16 flag) and falls back to CPU if necessary.
