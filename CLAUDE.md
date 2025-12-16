# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This repository contains Anthropic's **Prompt Engineering Interactive Tutorial**, a comprehensive course teaching optimal prompt engineering techniques for Claude. The tutorial consists of 9 chapters with exercises, plus an appendix covering advanced topics.

The repository provides two parallel implementations of the same tutorial content:
- **Anthropic 1P/**: Uses Anthropic's direct API (via the `anthropic` Python SDK)
- **AmazonBedrock/**: Uses Amazon Bedrock to access Claude (via `boto3`)
  - `AmazonBedrock/anthropic/`: Bedrock implementation using Anthropic SDK style
  - `AmazonBedrock/boto3/`: Bedrock implementation using native boto3 style

## Tutorial Structure

The course is designed to be completed sequentially from Chapter 1 to Chapter 9:

### Beginner
- **Chapter 1:** Basic Prompt Structure
- **Chapter 2:** Being Clear and Direct
- **Chapter 3:** Assigning Roles

### Intermediate
- **Chapter 4:** Separating Data from Instructions
- **Chapter 5:** Formatting Output & Speaking for Claude
- **Chapter 6:** Precognition (Thinking Step by Step)
- **Chapter 7:** Using Examples (Few-Shot Prompting)

### Advanced
- **Chapter 8:** Avoiding Hallucinations
- **Chapter 9:** Building Complex Prompts (Industry Use Cases)

### Appendix
- Chaining Prompts
- Tool Use
- Empirical Performance Evaluations
- Search & Retrieval

## Setup Instructions

### For Anthropic 1P notebooks
1. Install dependencies:
   ```bash
   pip install anthropic
   ```

2. Start with `Anthropic 1P/00_Tutorial_How-To.ipynb` to set up your API key

3. In the setup notebook, configure:
   ```python
   API_KEY = "your_api_key_here"
   MODEL_NAME = "claude-3-haiku-20240307"
   ```

### For Amazon Bedrock notebooks
1. Install dependencies:
   ```bash
   pip install -r AmazonBedrock/requirements.txt
   ```

2. Start with the appropriate `00_Tutorial_How-To.ipynb` in either:
   - `AmazonBedrock/anthropic/`
   - `AmazonBedrock/boto3/`

## Working with the Tutorial

### Notebook Structure
Each tutorial notebook follows this pattern:
- **Setup section**: Loads API credentials and defines the `get_completion()` helper function
- **Lesson section**: Explains concepts with examples
- **Exercises section**: Interactive exercises with automated grading
- **Example Playground**: Area to experiment freely with prompts

### Helper Functions
The tutorials use a standard helper function throughout:

```python
def get_completion(prompt: str, system_prompt=""):
    message = client.messages.create(
        model=MODEL_NAME,
        max_tokens=2000,
        temperature=0.0,
        system=system_prompt,
        messages=[{"role": "user", "content": prompt}]
    )
    return message.content[0].text
```

### Hints System
Each exercise has associated hints available via the `hints.py` file:
- `Anthropic 1P/hints.py`: Hints and solutions for Anthropic API version
- `AmazonBedrock/utils/hints.py`: Hints and solutions for Bedrock version

To use hints in notebooks:
```python
from hints import exercise_X_Y_hint
print(exercise_X_Y_hint)
```

### Exercise Grading
Exercises include automated grading functions that check if Claude's response meets specific criteria (e.g., contains certain words, follows formatting rules, or meets length requirements).

## Key Technical Details

### API Configuration
- **Model**: Tutorial uses Claude 3 Haiku by default (smallest, fastest, cheapest model)
- **Temperature**: Set to 0 for deterministic responses
- **Max tokens**: 2000 tokens for most exercises

### Messages API Format
All prompts use the Messages API format requiring:
- Alternating `user` and `assistant` roles
- First message must be `user` role
- Each message has `role` and `content` fields
- Optional `system` parameter for system prompts

### CloudFormation (Bedrock only)
The `AmazonBedrock/cloudformation/` directory contains infrastructure-as-code templates for setting up AWS resources needed to run the Bedrock version.

## External Resources

- [Answer key (Google Sheets)](https://docs.google.com/spreadsheets/d/1jIxjzUWG-6xBVIa2ay6yDpLyeuOh_hR_ZB75a47KX_E/edit?usp=sharing): Complete solutions for all exercises
- [Google Sheets version](https://docs.google.com/spreadsheets/d/19jzLgRruG9kjUQNKtCg1ZjdD6l6weA6qRXG5zLIAhC8/edit?usp=sharing): More user-friendly version using Claude for Sheets extension
- [Anthropic Documentation](https://docs.anthropic.com/): Official API documentation
