# Towards a Unified View of Attribution in AI  
*Rethinking how we explain machine learning models*

**Author:** Meriem Jelassi  
**Institution:** CentraleSupélec  
**Course:** Explainable AI  
**Paper:** *Interpretability Beyond Feature Attribution: Quantitative Testing with Concept Activation Vectors (TCAV)*

---

## **Why do we still struggle to understand AI?**

Modern AI systems are incredibly powerful, but also increasingly opaque.

From deep neural networks to large-scale models, we can build systems that perform remarkably well, yet struggle to answer a simple question:

> **Why did the model make this decision?**

This question sits at the core of interpretability.

Over the years, researchers have developed a wide range of techniques to “open the black box.” Most of them attempt to explain predictions in terms of input features, pixels, words, or numerical variables.

But here lies a fundamental mismatch.

> Models operate on low-level features. Humans reason in high-level concepts.

We don’t think in pixels, we think in *textures*, *objects*, *patterns*, *ideas*.

And this gap is not just a technical detail, it is the reason why many explanations fail to be truly useful.

This paper starts from that observation and asks a simple but powerful question:

> **What if we explained models in the language humans actually use?**

---

## **From features to concepts**

Most existing explanation methods try to answer questions like:

- Which pixels matter?
- Which words influenced the prediction?

But these answers often fall short.

Knowing that a specific pixel contributed to a prediction does not really explain *why* the model made its decision, at least not in terms that humans naturally understand.

The issue is not a lack of information, but a mismatch in representation.

> Features are not concepts.

This is where TCAV introduces a crucial shift:

> **Instead of explaining predictions in terms of features, explain them in terms of concepts.**

Not pixels, but *textures*.  
Not tokens, but *ideas*.  
Not signals, but *meaning*.

---

## **What is a concept?**

In TCAV, a concept is not a predefined feature or label.

Instead, it is defined in a remarkably simple way:

> **a concept is a set of examples**

For instance:
- “striped” → images of striped textures  
- “furry” → images of animals with fur  
- “gender” → curated example datasets  

At first glance, this definition may seem almost too simple.  
But it is precisely what makes it powerful.

It removes the need to predefine concepts during training and instead allows them to emerge through examples.

As a result, users can define and refine concepts *after* the model has been trained, without modifying it.

Even more importantly, this makes interpretability accessible:  
non-experts can explore model behavior simply by providing a small set of representative examples.

---

## **Turning concepts into vectors**

How can a model understand a concept like “striped”?

The key idea is to translate this concept into the model’s internal representation.

For a given layer of a neural network:
- we collect activations for concept examples  
- we collect activations for random examples  
- we train a simple linear classifier to separate the two  

The result is a vector:

> **Concept Activation Vector (CAV)**

This vector captures a direction in the model’s internal space, the direction that best distinguishes the concept from non-concept examples.

In other words:

> A concept becomes a direction in the network’s representation space.

Moving along this direction increases the presence of the concept in the model’s internal representation.

---

## **Measuring the importance of a concept**

Once we have a concept vector, we can ask a natural question:

> *How much does this concept influence a prediction?*

Instead of measuring sensitivity with respect to individual features (like pixels), TCAV evaluates how the prediction changes when we move in the direction of the concept.

Formally, this is done using a **directional derivative** along the concept vector.

Intuitively, we are asking:

> *If we slightly shift the model’s internal representation toward this concept, does the prediction increase?*

- If yes → the concept supports the prediction  
- If no → the concept is irrelevant or even opposes it  

This produces a single scalar value, known as the **conceptual sensitivity**, which quantifies how strongly the prediction depends on the concept.

---

## **From local explanations to global understanding**

One of the main limitations of many interpretability methods is that they are **local**.

They explain individual predictions, but fail to capture how the model behaves more broadly.

TCAV addresses this limitation by introducing a global metric:

> **TCAV score**

For a given class, the TCAV score measures:

> the fraction of inputs for which a concept positively influences the prediction

In other words, it answers a class-level question:

> *Does this concept consistently matter for this class?*

For example:

- TCAV(striped, zebra) ≈ high → stripes are important  
- TCAV(dotted, zebra) ≈ low → dots are not  

This shifts interpretability from isolated explanations to a more systematic understanding.

Instead of analyzing one input at a time, we can now reason about patterns across an entire class.

As a result, interpretability becomes:

- quantitative  
- comparable  
- global  

---

## **Do concepts really exist inside neural networks?**

Before trusting TCAV, we need to address a critical question:

> Do these concept vectors actually correspond to meaningful concepts?

The paper provides several pieces of evidence to support this.

First, images can be ranked according to their similarity to a given concept.  
The highest-ranked images consistently align with human intuition, suggesting that the learned directions capture meaningful structure.

Second, visualization techniques such as DeepDream can be used to generate patterns that maximize a concept direction.  
The resulting images exhibit recognizable features associated with the concept.

Taken together, these observations suggest that:

> Concepts are not artificially imposed on the model, they are already encoded within its internal representations.

---

## **What models actually learn**

Using TCAV, the authors analyze standard image classification models.

Some results align with intuition:
- zebras rely on “striped”  
- fire trucks rely on “red”  

These findings act as a sanity check, confirming that the method captures meaningful signals.

But more interestingly, TCAV also reveals less obvious patterns.

It uncovers hidden biases in the model:
- certain objects are correlated with race  
- some classes are associated with gender  

Importantly, these concepts were never explicitly provided during training.

> The model learned them implicitly from the data.

This highlights a deeper issue:

> Models don’t just learn tasks, they learn *correlations present in the data*, including unintended or undesirable ones.

---

## **When explanations fail**

To evaluate TCAV more rigorously, the authors design a controlled experiment.

They construct a dataset in which each image contains both:
- visual content  
- textual captions  

By introducing controlled noise, they can determine whether the model relies more on:
- the image itself  
- or the caption  

This setup provides an approximate *ground truth* for what the model is actually using.

### Results

- TCAV correctly identifies which concept the model relies on  
- saliency maps fail to communicate this distinction clearly  

But the most striking result comes from a human study.

Participants were asked to interpret saliency maps and determine which concept mattered most. Their accuracy was only slightly above random chance, yet they often reported high confidence in their answers.

> Explanations can be misleading, even when they appear convincing.

This result highlights a critical limitation:

> An explanation is not useful if it cannot be reliably interpreted by humans.

---

## **From explanation to action**

TCAV is not just a diagnostic tool, it enables intervention.

In a real-world medical application (diabetic retinopathy), TCAV was used to analyze model predictions.

It revealed that the model sometimes relied on misleading or inappropriate features, leading to systematic errors.

This type of insight is actionable.

It allows practitioners to:
- identify flawed reasoning patterns  
- adjust or constrain model behavior  
- improve reliability and alignment with domain knowledge  

In this case, TCAV even highlighted discrepancies between the model’s reasoning and expert medical judgment, opening the door to targeted corrections.

> Interpretability becomes a tool for *debugging* and improving models, not just understanding them.

---

## **A different way to think about interpretability**

TCAV introduces a subtle but important shift.

Instead of asking:

> “Which features matter?”

It asks:

> **“Which concepts matter?”**

This reframes interpretability in a way that aligns more closely with how humans actually reason about decisions.

Explanations are no longer tied to low-level signals, but to meaningful abstractions.

As a result, the role of interpretability changes:

- from visualization → to reasoning  
- from local → to global  
- from passive → to actionable  

This is not just a methodological improvement, it is a shift in perspective.

> Understanding a model is no longer about inspecting its components, but about relating them to concepts we understand.

---

## **Limitations and future directions**

While powerful, TCAV is not a complete solution.

It relies on user-defined concepts, which may be incomplete, subjective, or biased.  
Some concepts may overlap, be difficult to define precisely, or fail to capture the full complexity of the model’s behavior.

More fundamentally, the quality of the explanation depends on the quality of the concepts themselves.

However, this perspective also opens several promising directions for future research:

- extending concept-based explanations to other modalities, such as text, audio, or multimodal models  
- automatically discovering meaningful concepts from data  
- using concept attribution to improve robustness, fairness, and safety  
- leveraging models to reveal patterns or structures that humans might not easily identify  

These directions suggest that concept-based interpretability is not just a tool for explanation, but a foundation for deeper interaction between humans and machine learning systems.

---

## **Final thoughts**

This paper does not simply propose a new method.

It introduces a different way of thinking about interpretability.

> Interpretability should not be about translating models into features, 
> but about expressing them in the language of human concepts.

By bridging the gap between model representations and human understanding, TCAV moves us closer to explanations that are not only accurate, but truly meaningful.

More broadly, it suggests a shift in perspective:

> Understanding AI is not just about looking inside models,  
> but about asking the right questions, in the right language.

---

## **References**

Kim, B., Wattenberg, M., Gilmer, J., Cai, C., Wexler, J., Viegas, F., & Sayres, R. (2018).  
*Interpretability Beyond Feature Attribution: Quantitative Testing with Concept Activation Vectors (TCAV)*.
