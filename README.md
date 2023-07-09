# The Abstraction and Reasoning Corpus (ARC)

This repository is a derivative of the original 2D ARC, modified to focus on 1D ARC problems. The shift towards 1D aligns better with contemporary algorithmic architectures and it doesn't compromise the integrity of reasoning or abstraction - an algorithm capable of 1D reasoning should eventually extend to 2D.

The impetus for this shift to 1D lies in its compatibility with existing methodologies, including transformer-based, DSL-based, and search-based approaches. Such architectures are more harmonious with a 1D model compared to a 2D one.

Moreover, the 1D focus allows for concentration on reasoning and abstraction within a more compact space, eliminating concerns associated with the complexities that come with a 2D approach. These include a loss of a well-defined order and an expanded problem space. Therefore, this 1D adaptation offers an advantageous starting point for exploring reasoning and abstraction.

*"ARC can be seen as a general artificial intelligence benchmark, as a program synthesis benchmark, or as a psychometric intelligence test. It is targeted at both humans and artificially intelligent systems that aim at emulating a human-like form of general fluid intelligence."*

The ARC A complete description of the dataset, its goals, and its underlying logic, can be found in: [On the Measure of Intelligence](https://arxiv.org/abs/1911.01547).

As a reminder, a test-taker is said to solve a task when, upon seeing the task for the first time, they are able to produce the correct output sequence for *all* test inputs in the task (this includes picking the dimensions of the output sequence). For each test input, the test-taker is allowed 3 trials (this holds for all test-takers, either humans or AI).


## Task file format

The `data` directory contains two subdirectories:

- `data/training`: contains the task files for training (400 tasks). Use these to prototype your algorithm or to train your algorithm to acquire ARC-relevant cognitive priors.
- `data/evaluation`: contains the task files for evaluation (400 tasks). Use these to evaluate your final algorithm. To ensure fair evaluation results, do not leak information from the evaluation set into your algorithm (e.g. by looking at the evaluation tasks yourself during development, or by repeatedly modifying an algorithm while using its evaluation score as feedback).

The tasks are stored in JSON format. Each task JSON file contains a dictionary with two fields:

- `"train"`: demonstration input/output pairs. It is a list of "pairs" (typically 3 pairs).
- `"test"`: test input/output pairs. It is a list of "pairs" (typically 1 pair).

A "pair" is a dictionary with two fields:

- `"input"`: the input "sequence" for the pair.
- `"output"`: the output "sequence" for the pair.

A "sequence" is a list of integers between 0 and 9 (inclusive). The smallest possible sequence length is 1 and the largest is 30.

When looking at a task, a test-taker has access to inputs & outputs of the demonstration pairs, plus the input(s) of the test pair(s). The goal is to construct the output sequence(s) corresponding to the test input sequence(s), using 3 trials for each test input. "Constructing the output sequence" involves picking a length of the output sequence, then filling each cell in the sequence with a symbol (integer between 0 and 9, which are visualized as colors). Only *exact* solutions (all cells match the expected answer) can be said to be correct.


## Usage of the testing interface

The testing interface is located at `apps/testing_interface.html`. Open it in a web browser (Chrome recommended). It will prompt you to select a task JSON file.

After loading a task, you will enter the test space, which looks like this:

![test space](https://arc-benchmark.s3.amazonaws.com/figs/arc_test_space.png)

On the left, you will see the input/output pairs demonstrating the nature of the task. In the middle, you will see the current test input sequence. On the right, you will see the controls you can use to construct the corresponding output sequence.

You have access to the following tools:

### Sequence controls

- Resize: input a sequence size (e.g. length "10" or "4") and click "Resize". This preserves existing sequence content (in the top left corner).
- Copy from input: copy the input sequence to the output sequence. This is useful for tasks where the output consists of some modification of the input.
- Reset sequence: fill the sequence with 0s.

### Symbol controls

- Edit: select a color (symbol) from the color picking bar, then click on a cell to set its color.
- Select: click and drag on either the output sequence or the input sequence to select cells.
    - After selecting cells on the output sequence, you can select a color from the color picking to set the color of the selected cells. This is useful to draw solid lines.
    - After selecting cells on either the input sequence or the output sequence, you can press C to copy their content. After copying, you can select a cell on the output sequence and press "V" to paste the copied content. You should select the cell in the top left corner of the zone you want to paste into.
- Floodfill: click on a cell from the output sequence to color all connected cells to the selected color. "Connected cells" are contiguous cells with the same color.

### Answer validation

When your output sequence is ready, click the green "Submit!" button to check your answer. We do not enforce the 3-trials rule.

After you've obtained the correct answer for the current test input sequence, you can switch to the next test input sequence for the task using the "Next test input" button (if there is any available; most tasks only have one test input).

When you're done with a task, use the "load task" button to open a new task.
