# Data Science Roadmap Diagrams

Reusable visual learning assets for the [CubicNova Data Science Roadmap](https://github.com/cubicnova/Data-Science-Roadmap).

These SVG diagrams explain core workflows and architectures across Data Science, Machine Learning, Deep Learning, Generative AI, and MLOps. SVG files are text-based and can be reviewed or edited in a vector graphics editor.

## Diagram Index

| Diagram | Description |
|---|---|
| [Data Science Lifecycle](./data-science-lifecycle.svg) | End-to-end data science process, including monitoring and iteration |
| [Machine Learning Workflow](./machine-learning-workflow.svg) | Data splitting, preprocessing, training, evaluation, and deployment |
| [Supervised vs. Unsupervised Learning](./supervised-vs-unsupervised.svg) | Major learning paradigms and example task types |
| [Neural Network Architecture](./neural-network-architecture.svg) | Input, hidden, and output layers in a fully connected network |
| [LLM and RAG Architecture](./llm-rag-architecture.svg) | Document retrieval and context-grounded LLM response workflow |
| [MLOps Lifecycle](./mlops-lifecycle.svg) | Model training, registry, deployment, monitoring, and improvement |

## Use a Diagram in Markdown

From the repository root:

```markdown
![Data Science Lifecycle](assets/diagrams/data-science-lifecycle.svg)
```

From a Markdown file inside a topic folder, adjust the relative path based on that file's location.

## Design and Accuracy Notes

- These diagrams are conceptual overviews, not exhaustive technical specifications.
- Real workflows may vary by project, organization, and technology stack.
- In machine learning, avoid data leakage by fitting preprocessing only on training data.
- RAG can improve grounding but does not guarantee that every generated answer is correct.
- Retraining in an MLOps workflow should be evaluated before a new model is released.

## Contribution Guidelines

Contributions are welcome. Please keep labels readable, verify technical accuracy, use descriptive lowercase hyphenated filenames, and include editable source files when practical.

If you modify a diagram, preview it before committing and update this index if you add or rename files.

Part of the [CubicNova Data Science Roadmap](https://github.com/cubicnova/Data-Science-Roadmap).
