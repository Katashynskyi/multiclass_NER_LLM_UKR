
# Named Entity Recognition in Ukrainian Texts: Implementation and Analysis Report

## Project Overview
This project implements Named Entity Recognition (NER) for Ukrainian texts using the Gemma 2 9B Instruct model through the DSPy framework. The implementation focuses on identifying 13 distinct entity classes while maintaining the original morphological forms of Ukrainian words, specifically designed to run within Kaggle notebook constraints.
- notebooks are kaggle notebook compatible (using of both T4), to run elsewhere - rewrite path for .csv files

## Technical Approach

### Model Selection Process and Rationale

1. **Initial Model Experiments**
   - Tested Llama-3.2-3B-Instruct initially
   - Found it inadequate for NER tasks due to:
     - Inconsistent handling of Ukrainian word endings
     - Token limit issues causing repetitive output
     - Insufficient capacity for complex NER reasoning

2. **Final Model Selection: Gemma 2 9B Instruct FP16**
   - Chosen for:
     - Optimal balance of size and performance for Kaggle constraints
     - Strong instruction-following capabilities
     - Efficient quantization (FP16) without significant quality loss
     - Superior handling of morphological variations in Ukrainian

### Server Implementation Evolution

1. **vLLM Exploration**
   - Initial attempts with vLLM server faced limitations:
     - Full `google/gemma-2-9b` exceeded notebook resources
     - First-generation `google/gemma-7b` lacked instruction tuning
     - Limited experience with vLLM's quantization options

2. **Ollama Solution**
   - Adopted Ollama server for advantages:
     - Out-of-box support for quantized models
     - Dual T4 GPU utilization via subprocess
     - Acceptable processing speed (1.5 hours for test set)
     - Better resource management in Kaggle environment

### Framework Integration

1. **DSPy Implementation**
   - Successful components:
     - Custom dataset creation using `dspy.Example`
     - Effective reasoning through `dspy.ChainOfThought`
     - Robust `Expert_Linguist` class for instruction handling
   
   - Challenges faced:
     - Incomplete documentation for `dspy.Evaluate`
     - Unresolved issues with `dspy.MIPROv2` implementation
     - Limited prompt optimization capabilities

### Data Processing Strategy

1. **Text Chunking Optimization**
   - Implemented dynamic chunk sizing:
     - 600 character limit per chunk
     - 5 sentences per sample
     - Visual analysis for consistent sample lengths

2. **Processing Pipeline**
   - Efficient chunking and reconstruction workflow
   - Preservation of morphological features
   - Batch processing for resource optimization

## Key Learnings and Observations

1. **Model Size Considerations**
   - 3B models proved insufficient for complex NER tasks
   - Better suited for simpler binary classification
   - Importance of instruction-tuned models for NER

2. **Server Selection**
   - Trade-off between vLLM's speed and Ollama's practicality
   - Resource constraints driving architectural decisions
   - Importance of quantization support

## Future Improvements

1. **Technical Enhancements**
   - Further exploration of vLLM capabilities
   - Investigation of additional quantization options
   - Optimization of prompt engineering techniques

2. **Framework Development**
   - Better integration with DSPy optimization modules
   - Enhanced documentation contributions
   - Development of robust evaluation metrics

## Results and Performance

### Initial Submission Results
- Achieved F1 score of 0.34 on the competition leaderboard
- Notable performance considering:
  - First submission without extensive optimization
  - Complex nature of Ukrainian NER task
  - Handling 13 distinct entity classes
  - Preservation of morphological forms

### Performance Analysis
1. **Baseline Achievement**
   - Score establishes a solid foundation for further improvements
   - Validates the viability of using Gemma 2 9B Instruct for Ukrainian NER
   - Demonstrates effective entity extraction across multiple categories

2. **Contributing Factors**
   - Effective chunking strategy maintaining context
   - Robust instruction design in Expert_Linguist class
   - Successful preservation of Ukrainian morphology
   - Balanced resource utilization within Kaggle constraints

### Areas for Score Improvement
1. **Model Optimization**
   - Fine-tune chunk size parameters
   - Optimize prompt engineering
   - Explore advanced DSPy features when documentation improves

2. **Processing Enhancements**
   - Refine entity boundary detection
   - Improve handling of overlapping entities
   - Enhance context understanding for ambiguous cases

This implementation demonstrates the practical challenges and solutions in developing a NER system within specific computational constraints while maintaining high accuracy in morphologically rich languages like Ukrainian. The initial score of 0.34 provides a promising starting point for further development and optimization of the NER system.
