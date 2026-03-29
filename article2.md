# Beyond Feature Importance: A Unified Perspective on Explaining AI Systems
*A blog-style analysis of the paper by Zhang et al.*

**Author:** Meriem Jelassi  
**Institution:** CentraleSupélec  
**Course:** Explainable AI  
**Paper:** *Towards Unified Attribution in Explainable AI, Data-Centric AI, and Mechanistic Interpretability*

---

## Why do we still struggle to understand AI?

Modern AI systems are remarkably powerful, but also increasingly difficult to interpret.

From large language models to deep neural networks, we are now able to train systems that achieve impressive performance across a wide range of tasks. Yet, despite these advances, a fundamental question remains surprisingly hard to answer:

> **Why did the model make this decision?**

This question lies at the core of interpretability research. Over the years, numerous techniques have been proposed to shed light on model behavior and make predictions more understandable.

However, as the field has evolved, these methods have largely developed in parallel, often within separate research communities. As a result, similar ideas are sometimes rediscovered under different names, creating a fragmented landscape of attribution methods.

This post discusses a recent paper that challenges this fragmentation. It puts forward a simple but compelling idea:

> **Many attribution methods are not fundamentally different, they reflect the same underlying principles, applied to different parts of the system.**

Rather than introducing a new method, the paper invites us to rethink how existing ones relate to each other.

---

## Three ways to explain a model

When trying to understand a model’s prediction, researchers typically adopt one of three perspectives, depending on which part of the system they choose to analyze.

### 1. Feature attribution: looking at the input

Feature attribution focuses on the input itself. It seeks to identify which parts of a given input contribute most to the model’s prediction.

> *Which elements of the input influenced the output?*

For example:
- In image classification, which pixels or regions led the model to predict “cat”?
- In natural language processing, which words or tokens contributed to a “positive” sentiment prediction?

Because of its intuitive nature, feature attribution is the most widely used form of interpretability in practice. It provides a direct link between model decisions and human-understandable inputs.

### 2. Data attribution: looking at the training data

Instead of focusing on the input, data attribution shifts attention to the training process. It asks:

> *Which training examples influenced the model’s behavior?*

This perspective is particularly useful for:
- diagnosing mislabeled data  
- understanding biases in datasets  
- auditing data sources  

In this sense, data attribution provides a way to trace a model’s behavior back to its origin, answering a slightly different question:

> *Where did this behavior come from?*

### 3. Component attribution: looking inside the model

A third perspective focuses on the internal structure of the model. Component attribution aims to identify which parts of the model itself are responsible for a given prediction.

> *Which neurons, layers, or internal mechanisms contributed to the output?*

This perspective is central to **mechanistic interpretability**, where researchers attempt to reverse-engineer neural networks and understand how information is processed internally.

Although these three perspectives focus on different aspects of the system, they ultimately address the same underlying goal: explaining how a model produces its predictions.

---

## A fragmented landscape

At first glance, these three approaches appear quite distinct.

Historically, they have indeed evolved in separate research communities:
- Feature attribution → explainable AI  
- Data attribution → data-centric AI  
- Component attribution → mechanistic interpretability  

Each of these areas has developed its own methods, terminology, and evaluation frameworks.

However, this separation comes at a cost.

> Similar ideas are often rediscovered across fields, sometimes under different names and with slightly different formulations.

As a result, the overall landscape of attribution methods has become fragmented, making it harder to compare approaches and build a coherent understanding of the field.

This observation serves as the starting point of the paper:

**What if these methods are not fundamentally different, but rather different views of the same underlying problem?**

The paper argues that this fragmentation is largely superficial, and that a unifying perspective can reveal deeper connections between these methods.

---

## A unifying perspective

The central claim of the paper is both simple and surprisingly far-reaching:

> **Feature, data, and component attribution rely on the same underlying techniques.**

What differs between these approaches is not the methodology, but the perspective they adopt. Each focuses on a different aspect of the system:
- input features  
- training data  
- internal components  

Yet, the mechanisms used to compute attribution remain largely the same.

In other words, these methods do not represent fundamentally different solutions, but rather different ways of applying the same core ideas to different parts of the model.

This observation suggests that attribution methods can be studied within a common framework, rather than as isolated techniques.

---

## One problem, one function

All attribution methods can be framed within the same general formulation.

We consider:
- a model \( f(x) \)  
- an input \( x \)  
- a prediction \( f(x) \)  

The goal is to answer a simple but fundamental question:

> *How much did each part contribute to this prediction?*

This can be formalized as learning an attribution function \( g \), which assigns importance scores to different elements of the system.

Depending on the perspective, this function produces:
- feature attributions \( \phi_i(x) \)  
- data attributions \( \psi_j(x) \)  
- component attributions \( \gamma_k(x) \)  

Although these scores are defined over different objects, they all serve the same purpose: quantifying the contribution of individual elements to the model’s output.

In this sense, attribution can be viewed as a process of distributing responsibility across the different parts of the system.

This unified formulation suggests that, despite their apparent differences, attribution methods may rely on a shared set of computational principles.

---

## The three core techniques behind everything

A closer look reveals that most attribution methods rely on a small set of recurring ideas. In practice, they can be grouped into three main categories.

### 1. Perturbation: change something and observe

The core idea behind perturbation-based methods is straightforward:

> Modify part of the system and observe how the prediction changes.

This can take different forms:
- removing a word from a sentence  
- removing a training example and retraining the model  
- disabling a neuron or a layer  

Despite their simplicity, these methods can capture complex interactions. However, they are often computationally expensive, especially when many elements need to be evaluated.

This perspective naturally leads to more principled approaches such as **Shapley values**, where features (or data points) are treated as players in a cooperative game. Each element is assigned an importance score based on its marginal contribution across different subsets.

### 2. Gradients: measure sensitivity

Rather than modifying the system explicitly, gradient-based methods measure how sensitive the model is to small changes in the input:

$$
\nabla_x f(x)
$$

Gradients quantify how the output varies with respect to each input dimension:

> *If we slightly change this input, how much does the prediction change?*

These methods are computationally efficient and scale well to large models, as they typically require only a few forward and backward passes.

However, they can be noisy and sensitive to local variations, which may lead to unstable or hard-to-interpret explanations.

### 3. Linear approximation: simplify the model locally

A third approach consists in approximating the model locally around a point of interest using a simpler, interpretable function:

$$
g(x) = w^\top x + b
$$

In this setting, the coefficients \( w \) can be interpreted as importance scores, indicating how each feature contributes to the prediction.

Methods such as **LIME** follow this approach by fitting a local linear model to approximate the behavior of a complex model in the neighborhood of a given input.

Interestingly, these methods often rely on perturbations to generate local samples, highlighting that the boundaries between categories are not always strict.

Taken together, these three techniques, perturbations, gradients, and local approximations, form the foundation of most attribution methods, regardless of whether they focus on features, data, or model components.

This observation raises a natural question: are these methods fundamentally different, or simply different implementations of the same underlying idea?

---

## A deeper unification: local function approximation

One of the most insightful contributions of the paper is that many attribution methods can be understood as instances of a single underlying idea:

> **Approximating the model locally around a point of interest.**

Rather than analyzing the full complexity of the model, these methods focus on its behavior in a small neighborhood around a specific input. Within this local region, the model can often be approximated by a simpler and more interpretable function.

This perspective provides a unifying lens through which seemingly different methods can be understood. It helps explain:

- why different methods may produce conflicting explanations  
- how methods that appear unrelated are in fact closely connected  
- how attribution techniques can be compared within a common framework  

In this sense, attribution is not about uncovering a single “true” explanation, but about constructing local approximations of model behavior under different assumptions.

This shift in perspective transforms a diverse set of techniques into a coherent and theoretically grounded framework.

However, this shared foundation also implies that these methods inherit similar limitations.

---

## Shared challenges

Because attribution methods rely on the same underlying techniques, they also tend to share the same limitations. This unified perspective not only reveals common strengths, but also highlights recurring challenges across different approaches.

---

### Computational cost

A first major challenge is computational efficiency.

- Perturbation-based methods often scale poorly, as they require evaluating many variations of the input or retraining the model  
- Gradient-based methods are generally more efficient, but can become costly when higher-order information (such as Hessians) is involved  
- Linear approximation methods require generating a large number of samples to fit reliable local models  

As a result, scaling attribution methods to modern large-scale systems, such as large language models, remains a significant challenge.

### Instability

Another important issue is the lack of stability in attribution results.

Running the same method multiple times can produce different explanations. This variability arises from several sources:
- randomness in sampling procedures  
- stochastic optimization  
- sensitivity to hyperparameters  

This raises a fundamental question:

> **To what extent can we trust these explanations?**

### Evaluation is difficult

Evaluating attribution methods is inherently challenging, as there is no clear ground truth for explanations.

Several approaches are commonly used:
- removing features with high attribution scores and measuring the impact on performance  
- evaluating explanations through downstream tasks (e.g., debugging or data cleaning)  
- relying on human judgment  

However, each of these approaches has limitations. In practice, different evaluation metrics may lead to conflicting conclusions, making it difficult to assess the reliability of a given method.

Taken together, these challenges suggest that improving attribution methods requires not only better algorithms, but also more principled ways of evaluating and stabilizing their outputs.

At the same time, this shared structure also creates opportunities: if these methods face similar challenges, they may also benefit from shared solutions.

---

## New research directions

The unified perspective not only clarifies existing methods, but also opens up new directions for future research.

---

### Cross-attribution ideas

If attribution methods share common foundations, then ideas developed in one setting can be transferred to others.

For instance:
- Shapley values, originally used in feature attribution, have already been successfully extended to data attribution  
- advanced gradient-based techniques, widely used in feature and data attribution, could be further explored for component attribution  

This suggests that many potential connections remain unexplored. Bridging these gaps could lead to more powerful and general attribution methods.

---

### Combining perspectives

Another important direction is to move beyond isolated analyses and consider multiple perspectives simultaneously.

Each type of attribution provides a partial view of model behavior:
- Feature attribution explains what the model focuses on  
- Data attribution explains where this behavior originates  
- Component attribution explains how the model internally computes it  

Taken individually, each perspective offers limited insight. Together, they provide a more complete understanding of the system.

> Understanding a model likely requires integrating all three perspectives, rather than relying on a single one.

This shift from isolated explanations to integrated analysis reflects a broader move toward more holistic understanding of AI systems.

---

## Beyond interpretability: practical impact

Attribution methods are not only useful for understanding models — they also enable us to act on them.

### Model editing

Once a problematic behavior has been identified, attribution methods can help locate its source within the system.

This makes it possible to intervene directly:
- by modifying specific components  
- by correcting targeted behaviors  
- without retraining the entire model  

This is particularly valuable for large models, where retraining can be prohibitively expensive.

### Model steering

Rather than modifying model parameters, steering techniques aim to guide model behavior at inference time.

In this setting, attribution plays a key role by identifying *what should be influenced* in order to achieve a desired outcome.

For example, it can help determine which features, internal representations, or data patterns are associated with undesirable behaviors, and therefore should be adjusted.

### AI regulation

As AI systems are increasingly deployed in high-stakes contexts, issues of transparency and accountability become critical.

Attribution methods provide complementary forms of insight:
- feature attribution reveals how inputs are processed  
- data attribution highlights the influence of training data  
- component attribution exposes the role of internal model structures  

Together, these perspectives support more informed auditing, debugging, and regulation of AI systems.

These applications illustrate how a unified view of attribution can move us from understanding models to actively shaping their behavior.

---

## A balanced perspective

The paper does not claim that all attribution methods are identical, nor that existing distinctions should be ignored.

There are valid reasons to consider these approaches separately:
- they address different objectives  
- they rely on different evaluation frameworks  
- they originate from distinct research communities  

Moreover, not all attribution methods fit neatly within the proposed categorization, and some approaches may require alternative perspectives.

At the same time, focusing exclusively on these differences can obscure deeper connections.

> Looking at attribution methods in isolation may hide important structural similarities and limit our ability to transfer ideas across domains.

The contribution of the paper is therefore not to eliminate distinctions, but to complement them with a unifying perspective.

In this sense, unification should be seen as a tool for understanding, not as a replacement for domain-specific insights.

---

## Final thoughts

This paper does not introduce a new algorithm.

Instead, it proposes something arguably more valuable:

> A new way of organizing what we already know.

By highlighting the common structure underlying attribution methods, it shows how ideas that were previously developed in isolation can be brought together within a unified framework.

This perspective makes it possible to:
- simplify the conceptual landscape  
- transfer ideas across domains  
- and develop more coherent approaches to interpretability  

In a field as fragmented as AI interpretability, this kind of clarity is both rare and valuable.

**Understanding AI may not require entirely new tools, but a better way of connecting the ones we already have.**

## **References**

Zhang, et al. (2024). *Towards Unified Attribution in Explainable AI, Data-Centric AI, and Mechanistic Interpretability*.
