# Stop Explaining Black Boxes: Why Interpretable Models Matter for High-Stakes Decisions

*A blog-style summary of the paper by Cynthia Rudin*

**Author:** Meriem Jelassi  
**Institution:** CentraleSupélec  
**Course:** Explainable AI  
**Paper:** *Stop Explaining Black Box Machine Learning Models for High Stakes Decisions and Use Interpretable Models Instead*

---

## **Introduction**

Machine learning systems increasingly influence decisions that deeply affect people's lives. Algorithms are now used in domains such as criminal justice, healthcare, finance, and public policy. Although these systems can improve efficiency and predictive performance, they often rely on complex models whose internal logic is difficult, and sometimes impossible, for humans to understand.

To address this lack of transparency, a large body of research has emerged under the umbrella of **Explainable Artificial Intelligence (XAI)**. The idea behind this approach is straightforward: if a model is too complex to interpret directly, additional techniques can be used to explain its predictions after they are produced.

In her influential paper *"Stop Explaining Black Box Machine Learning Models for High Stakes Decisions and Use Interpretable Models Instead,"* Cynthia Rudin challenges this approach. She argues that the real problem is not how to explain black-box models, but why we rely on them in the first place. Instead of explaining opaque models, we should focus on designing models that are interpretable by design.

---

## **The Rise of Explainable Machine Learning**

Modern machine learning models, particularly deep neural networks, are often described as **black boxes**. They can achieve impressive predictive performance, yet the reasoning behind their predictions is typically difficult for humans to understand.

To address this issue, researchers have developed a variety of explanation techniques. Common examples include:

- feature attribution methods such as SHAP and LIME  
- saliency maps used in computer vision  
- surrogate models that approximate the behavior of a complex model locally  

In many cases, these approaches rely on a secondary model whose purpose is to approximate the behavior of the original system. The overall process can be summarized as follows:
```
Black-box model → Explanation method → Human interpretation
```
The objective is to make model predictions easier to interpret while preserving the predictive performance of the underlying system.

Although this approach is intuitively appealing, Rudin argues that it relies on a questionable assumption: that complex black-box models are necessary to achieve accurate predictions in the first place.

---

## **The Myth of the Accuracy–Interpretability Trade-off**

A widely held belief in machine learning is that there exists an unavoidable trade-off between **accuracy and interpretability**. According to this view, simple models are easier for humans to understand but tend to be less powerful, while more complex models achieve higher predictive performance at the cost of transparency.

Rudin challenges this assumption and argues that the trade-off is often overstated.

In many real-world problems, especially those involving structured data, relatively simple models can achieve predictive performance comparable to that of more complex algorithms. Models such as logistic regression, decision trees, or rule-based systems frequently perform nearly as well as ensemble methods or deep neural networks once the data have been properly processed and meaningful features have been constructed.

In practice, improvements in model performance often result from better data preparation rather than from increasing model complexity. Important factors include:

- improved data preprocessing  
- careful feature engineering  
- deeper understanding of the problem domain  

These observations challenge the common assumption that interpretability necessarily comes at the expense of predictive accuracy.

---

## **The Problem with Post-hoc Explanations**

Even when black-box models achieve high predictive accuracy, relying on post-hoc explanations raises several important concerns.

### **Explanations are approximations**

Explanation models attempt to approximate the behavior of the original model. If an explanation were perfectly faithful, it would effectively reproduce the original model itself. In practice, however, explanation methods inevitably rely on simplifications.

As a result, explanations may only capture the model's behavior in certain regions of the input space. Outside those regions, the explanation may fail to represent the model accurately and can therefore be misleading.

### **Explanations may attribute importance to the wrong variables**

Because explanation models approximate the original system, they may rely on different features than the underlying model. Consequently, an explanation might suggest that a prediction was influenced by variables that were not actually used in the model's computation.

This issue becomes particularly problematic in sensitive contexts such as criminal justice or credit scoring, where explanations can shape how people perceive fairness, bias, and accountability.

### **Some explanations provide limited insight**

In domains such as computer vision, explanation methods often take the form of **saliency maps**, which highlight the regions of an image that influence the model's prediction.

Although these maps indicate where the model is focusing its attention, they do not explain how the information in those regions contributes to the final decision. Two different classes may even produce nearly identical saliency maps, offering little insight into the reasoning process of the model.

In such situations, explanations may create an illusion of understanding without actually revealing how the model operates.

---

## **Why Black Box Models Persist**

If interpretable models can often achieve performance comparable to that of black-box models, a natural question arises: why do opaque models remain so widely used in practice?

Several factors help explain their continued dominance.

### **Economic incentives**

Black-box models are frequently proprietary. Companies can protect them as intellectual property and sell them as commercial products or services. Transparent models, in contrast, are easier to replicate and therefore more difficult to monetize.

This creates a clear economic incentive for organizations to favor complex and opaque systems, even when simpler and more interpretable alternatives may exist.

### **Convenience and tooling**

Another important factor is the current machine learning ecosystem. Many popular frameworks and libraries are optimized for training complex models such as deep neural networks or ensemble methods. These tools make it easy to deploy powerful models with relatively little effort.

By contrast, interpretable machine learning methods often require more careful design and domain knowledge, and the available tools are less mature. As a result, practitioners may default to using black-box models simply because they are easier to implement.

### **The belief in “hidden patterns”**

Finally, there is a widespread perception that complex models are uniquely capable of discovering subtle patterns in data that would otherwise remain hidden.

While this assumption can sometimes hold, Rudin argues that interpretable models are often capable of capturing the same patterns when they are carefully constructed. The difficulty often lies not in the expressive power of interpretable models, but in the effort required to design them effectively.

---

## **Building Interpretable Models: Algorithmic Challenges**

Designing interpretable models is not always straightforward. Rudin emphasizes that building models which are both accurate and understandable often involves significant algorithmic challenges. In particular, several families of interpretable models require solving difficult optimization problems.

### **Logical models**

Logical models represent predictions through simple rules, such as:

```
IF condition THEN prediction
```
Examples include rule lists and decision trees. These models are naturally interpretable because their reasoning process can be expressed as a sequence of explicit conditions.

However, constructing an optimal set of rules can be computationally challenging. As the number of possible rules increases, the space of potential models grows extremely large, making exhaustive search impractical.

Algorithms such as CORELS address this challenge by efficiently exploring the space of rule-based models in order to identify optimal or near-optimal rule lists.

### **Sparse scoring systems**

Another important class of interpretable models consists of **scoring systems**, where predictions are obtained by summing a small number of weighted factors.

For example:
```
+1 if prior arrests ≥ 2
+1 if prior arrests ≥ 5
−1 if age ≥ 40
```

The resulting score is then converted into a risk estimate.

Scoring systems are widely used in fields such as medicine and criminal justice because they are easy to compute, easy to interpret, and can often be applied without complex software.

However, learning optimal scoring systems directly from data is difficult. The model must satisfy several constraints simultaneously, including sparsity and integer-valued coefficients. These requirements lead to challenging optimization problems.

Algorithms such as RiskSLIM have been developed to construct sparse scoring systems while maintaining strong predictive performance.

### **Domain-specific interpretability**

Interpretability is not defined in the same way across all domains. In some applications, traditional approaches such as sparsity may not be sufficient.

Computer vision provides a good example. In image classification, interpretability cannot simply rely on sparsity in pixel space. Instead, interpretable architectures may rely on **prototype-based reasoning**, where the model compares parts of an image to representative examples learned during training.

In prototype-based networks, the model identifies regions of the input image that resemble learned prototypes. The prediction is then based on the similarity between these regions and the stored prototypes.

In this setting, explanations are not generated after the prediction. Instead, the reasoning process of the model itself provides the explanation.

---

## **The Rashomon Effect in Machine Learning**

A central insight in Rudin's argument is the concept of the **Rashomon set**.

The Rashomon set refers to the collection of models that achieve nearly the same predictive performance on a given dataset. In many machine learning problems, several models with very different structures can produce predictions of comparable accuracy.

When this set of high-performing models is large, it increases the likelihood that at least one of these models will also be **interpretable**.

This perspective suggests that the search for interpretable models is not necessarily futile. Instead, it highlights an important point: many datasets may admit multiple equally accurate solutions, and some of those solutions may be both simple and understandable.

The real challenge, therefore, may lie less in the existence of interpretable models than in our ability to identify them within the broader space of high-performing models.

---

## **Implications for AI Governance**

The debate between explainability and interpretability has important implications for the governance and regulation of artificial intelligence.

Existing regulations, such as the European Union's **"right to explanation"**, focus primarily on providing explanations for automated decisions. However, if these explanations are merely approximations of black-box models, they may fail to deliver genuine transparency.

Rudin therefore argues for a stronger principle. In high-stakes decision-making contexts, **black-box models should not be used when an interpretable model with comparable performance is available**.

Adopting such a principle would encourage organizations to prioritize transparency, accountability, and trust when deploying machine learning systems that influence important aspects of people's lives.

---

## **Conclusion**

The growing field of explainable AI reflects an important concern: the need to make machine learning systems more transparent and trustworthy. However, the widespread assumption that complex black-box models are unavoidable may be misguided.

Rather than focusing primarily on explaining opaque models, we may need to reconsider how machine learning systems are designed in the first place.

In many high-stakes applications, interpretable models can provide a compelling alternative. They can achieve competitive predictive performance while remaining understandable to the people who rely on their decisions.

If this perspective gains wider acceptance, the future of trustworthy machine learning may not lie in explaining black boxes, but in **designing models that are interpretable from the outset.**


## **References**

Rudin, C. (2019). *Stop Explaining Black Box Machine Learning Models for High Stakes Decisions and Use Interpretable Models Instead*. Nature Machine Intelligence.
