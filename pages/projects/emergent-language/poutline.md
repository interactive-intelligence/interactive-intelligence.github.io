---
layout: default
title: Technical Specifications
parent: Emergent Language
grand_parent: Projects
nav_order: 1
has_children: false
permalink: /projects/emergent-lang/poutline
---

# Project Outline
{: .no_toc }

Detailed ideas and development records
{: .fs-6 .fw-300 }

The description on the home page of the Emergent Language project page gives an overview of the motivation and directions of this project. This page contains additional details on key theoretical advancements and results we have obtained.

---

## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Overarching Goal
We want to create a deep learning system attempting to solve a task from which a 'language' that demonstrate properties of human language emerges. 

### Why do we want to do this?
{: .no_toc }

Such an emergent language is grounded in the model's actual understanding of the task and the semantics involved, and doesn't just rely upon syntactic dependencies. That is, it demonstrates *groundedness*.

![image](https://user-images.githubusercontent.com/73039742/164838769-93ee0f4b-2e5a-420c-9449-683c0aa4f008.png)


---

## Properties of Language

This section describes various properties of language we hope to see demonstrated by the created synthetic language. The first three ({discrete, sequential, variable-length}) are explicitly built into the design of the architecture, the remainder are metrics that may have auxiliary or indirect effect on the model.

### Discrete
Language is represented and understood in discrete symbols. An emergent language should possess properties of discreteness/quantization, as opposed to working with traditional real-valued vectors. The most explicit form of discreteness is generating language in one-hot form: the token in the vocabulary with the highest probability is selected at each time step. However, other acceptable forms of discreteness include Vector-Quantized Variational Autoencoder (VQ-VAE)-style mechanisms, in which continuous vectors are 'snapped' to the closest embedding in a limited and trainable set of embeddings. This can be interpreted as directly associating tokens with an embedding.

### Sequential
Language is inherently ordered and temporal. Thus, it must possess an intrinsic ordering - as opposed to a standard vector which is mapped to and from in a dense, fully-connected way. A language is sequential if the processes by which it is generated recognize temporality: for instance, generation and interpretation via recurrent or transformer-like methods. However, being sequential does not mean that the specific order is rigid/static. In English, larger sequences of tokens can be rearranged with minimal modification to semantics. (Larger seuences of tokens can be rearranged with minimal modification to semantics in English.) In fact, we hope to see evidence of similar behavior in an emergent language - low-level rigid ordering (the building blocks of syntax) with high-level interchangeability of semantic expression.

### Variable-Length


### Grounded
Large language models trained to model human language data learn sophisticated syntactic relationships between symbols, to the point of being able to perform seemingly complex operations with language. However, these models' lack of robustness has been well documented, and stems from a fundamental flaw within language models: lack of groundedness. That is, a language model may know the meaning of a token and its complex relationships with other tokens, but its understanding is restricted to the syntactic level of language. On the other hand, humans have a deeper semantic understanding of language. Language is a reference, rather than an ends, to understand semantics. A primary goal of this project is to observe robustness in an emergent language stemming from groundedness.

Groundedness is an abstract concept, but fundamentally it must demonstrate that the language's tokens are fundamentally rooted in the semantic rather than the syntactic level. This can be assessed by testing the relationship between tokens and the underlying semantic objects, for instance by perturbing the representation in ways that shouldn't affect the semantics and measuring the change in language activity.

### Compositional

### Hockett's Design Features

View complete descriptions [here](https://en.wikipedia.org/wiki/Hockett%27s_design_features#Design_features_of_language){:target="_blank"}

*Clearly irrelevant features have been ommitted.*

| Feature | Relevance |
| --- | --- |
| Interchangeability | Comes with the model; the model should be capable of generating every possible sequence. Language is accessible to all models. |
| Total Feedback | Recurrent models demonstrate this property. Models like the DLSM explicitly builds this listening-speaking design into the architecture. |
| Semanticity | This is a part of groundedness. |
| Arbitrariness | This is built into the experiment design; the language is initialized randomly and therefore emergence of symbols is arbitrary. |
| Discreteness | The discreteness itself is built into the expreriment design; that is, a limited set of vectors/tokens are accessible to construct the language. Additionally, the language should demonstrate the presence of a discrete syntax. |
| Displacement | This is perhaps a part of abstraction, but is not relevant to the current task scope. This may be demonstrable in a reinforcement learning context. |
| Productivity | The ability to develop essential attributes that can be combined in meaningful ways to form novel utterances (e.g. trained on all combinations of shapes except for green triangle, but is able to represent green triangle by virtue of having experience with green things and triangle things). |

Note: a lot of the features that were ommitted likely would have been modelable/demonstrable by an RL environment project.

---

## Tasks

### Autoencoding with Language Latent Space
Intuitively, we can understand the task of communication as one of autoencoding - one agent (an encoder) collaborates with another agent (a decoder) to structure the latent space such that information can reliably be encoded into it and decoded out of it. Lan et al. say as much in their paper ["On the Spontaneous Emergence of Discrete and Compositional Signals"](https://aclanthology.org/2020.acl-main.433.pdf){:target="_blank"}. 

In autoencoding task, the desired output is identical to the output. The decoder's objective is to perfectly reconstruct the input given a language-like latent representation. This language-like representation must satisfy at least some of the axioms of language (discussed more in-depth in the [Properties of Language](#properties-of-language) section). For the duration of our work on this task, we used a latent space satisfying two of the three core properties - discrete/symbolic and sequential. The intuition is that the encoder and decoder must collaboratively develop an understanding of core features in the image and represent it in important word-like tokens arranged in a meaningful language-like sequence.

Empirically, we observe good performance on the MNIST dataset with a sufficiently large vocabulary size and sequence length.

*A static demonstration of autoencoder reconstructive capabilities and internal + quantized language/'communication'.*

<center>
<img src="https://user-images.githubusercontent.com/73039742/164372921-41279cd8-cfeb-485d-8ec0-5a65206a4924.png" width="60%" />
</center>

*A dynamic demonstration of autoencoder reconstructive capabilities along the temporal/sequential language/'communication' axis.*

<center>
<img src="https://interactive-intelligence.github.io/files/languageProgression.gif" width="60%" />
</center>

*The autoencoder even generates auto-blurred genetalia in infancy!*

<center>
<img src="https://user-images.githubusercontent.com/73039742/164372535-b114c99c-f0d3-4d3c-a31b-f731d68600be.png" width="60%" />
</center>

However, the task of autoencoding with a language latent space becomes practically and philosophically dubious for any dataset more complex than MNIST, like CIFAR-10/100, ImageNet, or a semantic subset like the Stanford Dogs and Cats dataset. There really are only perhaps twenty or less actual digit forms; all remaining digits closely conform to one of these digit forms, since the dataset is low-resolution, centered, and grayscale. A network can reliably autoencode MNIST just be learning these digit form 'templates'. However, for more complex datasets, autoencoding as a task for emergent language becomes philosophically problematic. The purpose of language is not to encode information in a way that optimizes for minimum information loss; rather, it is inherently abstract. However, autoencoding rewards the development of explicit structures and information systems, which is in fact antithetical to the purpose of the project.

### VQ-AE-GAN
The VQ-AE-GAN (Vector-Quantized Autoencoder Generative-Adversarial-Network) system is an attempt to build a task that rewards abstractness by fitting the vector-quantized (i.e. discrete, language-like latent space) autoencoder into a Generative Adversarial Network system. In a traditional GAN system, the generator generates images and the discriminator classifies whether an input was pulled from the dataset or generated by the generator. The generator's goal is to fool the discriminator by maximizing its loss; the discriminator's goal is to minimize its loss. Such an 'adversarial' relationship leads to the development of realstic images that are sophisticated because they demonstrate abstract properties also present in real/true images.

If we replace the generator with a vector-quantized autoencoder (i.e. the same autoencoder architecture used for the previous task), then the vector-quantized autoencoder does not necessarily need to reconstruct the exact input pixel-for-pixel, but rather just develop a reconstruction that seems realistic to a discriminator. The discriminator is an entity that theoretically is capable of rewarding abstraction. To provide additional information to the discriminator as to force the generator to avoid trivial solutions, embeddings for each reconstruction are obtained form an auxiliary feed-forward neural network that are also passed (along with the reconstruction) into the discriminator for generated-or-real classification.

We have not seen much success with this approach.

### Object-Based Geometric Scene Similarity
Heavily inspired by Choi et al.'s paper ["Compositional Observer Communication Learning from Raw Visual Input"](https://arxiv.org/pdf/1804.02341v1.pdf){:target="_blank"}, the geometric scene similarity task is the most successful one to date, both philosophically and pragmatically. 

### Relation-Based Geometric Scene Similarity


---

## Models

### Dual Listener-Speaker Model

### Language Siamese Model

---


