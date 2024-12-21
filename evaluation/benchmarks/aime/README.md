# AIME (American Invitational Mathematics Examination) Benchmark

## Overview

This benchmark evaluates the performance of AI agents on solving problems from the American Invitational Mathematics Examination (AIME). The AIME is a challenging high school mathematics competition in the United States, known for its difficult problems that require advanced problem-solving skills.

## Dataset

The dataset used for this benchmark is the [AIME 1983-2024 dataset](https://huggingface.co/datasets/gneubig/aime-1983-2024) available on Hugging Face. It contains AIME problems from 1983 to 2024, covering a wide range of advanced mathematical topics.

## Evaluation Process

The evaluation script (`run_infer.py`) performs the following steps:

1. Loads the AIME dataset from Hugging Face.
2. For each problem:
   - Presents the question to the AI agent.
   - Allows the agent to use Python and numerical libraries (e.g., numpy, sympy) to solve the problem.
   - Compares the agent's final answer with the correct answer.
3. Calculates the overall performance metrics.

## Running the Evaluation

To run the evaluation using Poetry, use the following command from the root directory of the OpenHands project:

```bash
poetry run python -m evaluation.benchmarks.aime.run_infer --llm-config <your_llm_config> --agent-cls CodeActAgent --max-iterations 50 --eval-n-limit 10 --eval-num-workers 1 --data-split train
```

You can adjust the parameters as needed:
- `--llm-config`: Specify the LLM configuration to use.
- `--agent-cls`: The agent class to evaluate (currently supports CodeActAgent).
- `--max-iterations`: Maximum number of iterations for each problem.
- `--eval-n-limit`: Number of problems to evaluate (use a smaller number for testing).
- `--eval-num-workers`: Number of worker processes for parallel evaluation.
- `--data-split`: The dataset split to use (e.g., 'train', 'test').

Note: Make sure you're in the root directory of the OpenHands project when running this command, as it uses relative imports.

## Analyzing the Results

After running the evaluation, you can analyze the results using the `eval_infer.py` script. This script calculates overall accuracy and accuracy by year. To run the analysis, use the following command:

```bash
poetry run python -m evaluation.benchmarks.aime.eval_infer --output-dir <path_to_output_directory>
```

Replace `<path_to_output_directory>` with the directory where `output.jsonl` was saved (this is printed at the end of the `run_infer.py` execution).

The script will output:
- Overall accuracy
- Total number of problems evaluated
- Accuracy for each year

This provides a comprehensive view of the model's performance on the AIME benchmark, both overall and across different years.

### Remote Runtime Support

This evaluation script supports running with a remote runtime. To use the remote runtime, set the following environment variables:

- `RUNTIME`: Set this to 'remote' to use the remote runtime (default is 'eventstream').
- `ALLHANDS_API_KEY`: Your API key for accessing the remote runtime.
- `SANDBOX_REMOTE_RUNTIME_API_URL`: The URL of the remote runtime API.

Example:

```bash
export RUNTIME=remote
export ALLHANDS_API_KEY=your_api_key_here
export SANDBOX_REMOTE_RUNTIME_API_URL=https://your-remote-runtime-url.com

python run_infer.py --llm-config <your_llm_config> --agent-cls CodeActAgent --max-iterations 50 --eval-n-limit 10 --eval-num-workers 1 --data-split train
```

Using the remote runtime can provide additional resources and potentially improve performance for complex mathematical computations.

## Output

The evaluation script generates an output file (`output.jsonl`) containing detailed results for each problem, including:
- The problem statement
- The agent's solution process
- The final answer provided by the agent
- Whether the answer was correct

## Metrics

The main metric for this benchmark is the accuracy of the agent's answers. An answer is considered correct if it exactly matches the integer answer provided in the dataset. The script calculates and reports:

1. Individual problem results: For each problem, the script determines whether the agent's answer is correct.

The detailed results for each problem are saved in the `output.jsonl` file in the evaluation output directory. To calculate the overall accuracy, you can process this file to compute the percentage of correctly answered problems across the entire evaluation set.

## Importance

The AIME benchmark is significant for several reasons:
1. It tests advanced mathematical problem-solving skills.
2. Problems often require creative thinking and novel approaches.
3. It covers a wide range of mathematical topics, including algebra, geometry, number theory, and combinatorics.
4. Success on this benchmark would indicate strong capabilities in mathematical reasoning and computation.

## Limitations

- The benchmark focuses on final answers and may not fully capture the quality of the problem-solving process.
- It requires the agent to format answers in a specific way, which may not always reflect real-world use cases.
- The problems are specific to the AIME format and may not generalize to all types of mathematical problem-solving.

## Future Work

- Implement a more detailed scoring system that considers partial credit for correct approaches.
- Expand the evaluation to include other mathematics competitions or problem sets.
- Develop methods to evaluate the quality and efficiency of the problem-solving process, not just the final answer.