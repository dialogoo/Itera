# ThumbsDown

<img width="1024" height="1024" alt="ChatGPT Image Nov 18, 2025, 03_40_45 PM" src="https://github.com/user-attachments/assets/78f81fb2-e327-4372-9f07-d4acd26bf285" />

**Intelligent observability layer for human-AI conversations**

---

## ⚠️ Draft Notice
This README is a temporary draft until the publication of the ThumbsDown methodology paper. ThumbsDown is an open project—suggestions, contributions, and collaborations are very welcome.

---

## What is ThumbsDown?

ThumbsDown is an open-source observability layer for conversations between humans and LLMs or agents. It addresses a critical problem in AI feedback systems: traditional approaches capture biased samples, confuse correct responses with user error, and poison datasets—degrading agent quality over time.

## The Problem

Traditional RLHF (Reinforcement Learning from Human Feedback) has limitations:
- Only certain users provide feedback (selection bias)
- Only extreme situations get flagged (missing nuanced cases)
- Correct answers to unclear prompts get marked as failures
- No distinction between agent errors and user confusion
- Dataset quality degrades as agents learn from noisy data

## Solution

ThumbsDown introduces an intelligent observability layer that:

1. **Analyzes sentiment** in user responses automatically
2. **Classifies feedback** into a structured dataset
3. **Inspects negative responses** with an LLM to determine:
   - Was this an agent error?
   - Or was the user unclear/incorrect in their prompt?
4. **Filters out false negatives** to prevent contaminating the training data
5. **Generates corrected responses** for genuine errors
6. **Manages dataset versioning** for continuous evaluation and monitoring

## Installation
```bash
pip install ThumbsDown
```

## Quick Start
```python
from ThumbsDown import ObservabilityLayer

# Initialize the layer
observer = ObservabilityLayer(
    sentiment_threshold=-0.3,
    llm_inspector="gpt-4"
)

# Monitor a conversation
conversation = [
    {"role": "user", "content": "How do I reset my password?"},
    {"role": "assistant", "content": "I can help with that..."},
    {"role": "user", "content": "That doesn't work!"}  # Negative sentiment detected
]

# Analyze and filter
results = observer.analyze_conversation(conversation)

# Generate corrected responses for genuine errors
if results.is_agent_error:
    corrected = observer.generate_correction(conversation)
    observer.save_to_dataset(conversation, corrected, version="v1.2")
```

## Features

- **Sentiment Analysis**: Automatic detection of user satisfaction
- **LLM Inspection**: Intelligent filtering of false negatives
- **Dataset Management**: Versioning and tracking for continuous improvement
- **Eval Pipeline**: Monitor agent evolution over time
- **Quality Control**: Prevents dataset contamination with incorrect labels

## Architecture
```
User Response → Sentiment Analysis → Classification → LLM Inspection
                                                            ↓
                                            [Agent Error? / User Error?]
                                                            ↓
                                            Generate Correction (if needed)
                                                            ↓
                                            Save to Versioned Dataset
```

## Configuration
```python
config = {
    "sentiment_threshold": -0.3,
    "llm_inspector": "gpt-4",
    "min_confidence": 0.7,
    "dataset_version": "v1.2",
    "storage_path": "./datasets"
}

observer = ObservabilityLayer(**config)
```

## API Reference

### ObservabilityLayer
```python
class ObservabilityLayer:
    def __init__(self, sentiment_threshold: float, llm_inspector: str)
    def analyze_conversation(self, conversation: List[dict]) -> AnalysisResult
    def generate_correction(self, conversation: List[dict]) -> str
    def save_to_dataset(self, conversation: List[dict], correction: str, version: str)
```

### AnalysisResult
```python
class AnalysisResult:
    sentiment_score: float
    is_agent_error: bool
    confidence: float
    category: str
```

## Roadmap

- [ ] Multi-language sentiment analysis
- [ ] Custom LLM inspector integration
- [ ] Real-time monitoring dashboard
- [ ] Community-driven inspection rules
- [ ] Integration with popular agent frameworks
- [ ] Export to common ML formats (HuggingFace, JSON, CSV)

## Contributing

Contributions are welcome:
1. Fork the repo
2. Create a feature branch
3. Submit a PR with clear documentation
4. Join the discussion in Issues

## License

MIT License with Ethical Use Clause - This package must be used exclusively for ethical AI projects. See [LICENSE](LICENSE) for details.

## Contact

Questions? Ideas? Want to collaborate?
- GitHub Issues: [github.com/yourusername/ThumbsDown/issues]
- Email: [your-email]
- Documentation: [docs link]
