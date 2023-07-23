# EvalRS-KDD-2023
Official Repository for EvalRS @ KDD 2023, the Second Edition of the workshop on well-rounded evaluation of recommender systems.

<a href="https://colab.research.google.com/drive/1QeXglfCUEcscHB6L0Gch2qDKDDlfwLlq?usp=sharing">  <img src="https://colab.research.google.com/assets/colab-badge.svg"> </a>

[//]: # (TODO: we really need a new image!)
![https://reclist.io/kdd2023-cup/](images/back_evalrs.png)

## Status

_The repository is mostly stable (i.e. you can start hacking!), but check back often for additional material and tools, and possibly minor modifications to the sample code!_

## Quick Start

| Name            | Link     | 
|-----------------|----------|
| Tutorial 1 - Exploring the EvalRS Dataset | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1VXmCpL0YkLkf5_GTgJmLtd2b2A57CNjm?usp=sharing)|
| Tutorial 2 - A Dummy Model In RecList on the EvalRS Dataset  | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1QeXglfCUEcscHB6L0Gch2qDKDDlfwLlq?usp=sharing)|
| Tutorial 3 - A Song Embedding Model on the EvalRS Dataset | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1hZJj5akr1cMP3QWWKXvD0MNoX4GimNb7?usp=sharing)|
| Appendix - How to Write a RecList | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1GVsVB1a3H9qbRQvwtb0TBDxq8A5nXc5w?usp=sharing)|


## Overview

This is the official repository for [EvalRS @ KDD 2023](https://reclist.io/kdd2023-cup/): _a Well-Rounded Evaluation of Recommender Systems_.

Aside from papers and talks, we will host a hackathon, where participants will pursue innovative projects for the rounded evaluation of recommender systems. The aim of the hackathon is to evaluate recommender systems across a set of important dimensions (accuracy being _one_ of them) through a principled and re-usable set of abstractions, as provided by [RecList](https://github.com/jacopotagliabue/reclist) 🚀. At the end of the workshop, organizers will sponsor a social event for teams to finalize their projects, mingle with like-minded practitioners and received the monetary prizes for best papers and projects: a link to the event will be added soon!

This repository provides an open dataset and all the tools necessary to partecipate in the hackathon: everything will go back to the community as open-source contributions. Please refer to the appropriate sections below to know how to get the dataset and run the evaluation loop properly.

All accepted papers for EvalRS are available [here](https://github.com/RecList/evalRS-KDD-2023/tree/main/papers).

### Important dates

Check the [EvalRS website](https://reclist.io/kdd2023-cup/) for the official timeline, including the start date, paper submission, and workshop schedule.

### Quick links

* 🛖 [EvalRS website](https://reclist.io/kdd2023-cup/)
* 📚 [EvalRS paper](https://arxiv.org/abs/2304.07145)
* 📖 [RecList website](https://reclist.io/)


## Dataset and target scenario

This hackathon is based on the [LFM-1b Dataset, Corpus of Music Listening Events for Music Recommendation](http://www.cp.jku.at/datasets/LFM-1b/). The use case is a typical user-item recommendation scenario: at _prediction time_, we get a set of users: for each user, our model recommends a set of songs to listen to, based on historical data on previous music consumption.

We picked LFM as it suits the spirit and the goal of this event: in particular, thanks to [rich meta-data on users](http://www.cp.jku.at/people/schedl/Research/Publications/pdf/schedl_ijmir_2017.pdf), the dataset allows us to test recommender systems among many non-obvious dimensions, on top of standard Information Retrieval Metrics (for the philosophy behind behavioral testing, please refer to the original [RecList paper](https://arxiv.org/abs/2111.09963)).

Importantly, the dataset of this workshop is a _new_, augmented version of the one used [last year at CIKM](https://github.com/RecList/evalRS-CIKM-2022): to provide richer [item meta-data](https://arxiv.org/abs/1912.02477), we extended the LFM-1b dataset with content-based features and user-provided labels from the [WASABI dataset](https://github.com/micbuffa/WasabiDataset) (see below).

### Dataset overview

When you run the evaluation loop below, the code will automatically download _a chosen subset of the LFM dataset_, ready to be used (the code will download it only the first time you run it). There are three main objects available from the provided evaluation class:

_Users_: a collection of users and available meta-data, including patterns of consumption, demographics etc... In the Data Challenge scenario, the user Id is the query item for the model, which is asked to recommend songs to the user.

![http://www.cp.jku.at/datasets/LFM-1b/](images/users.png)

_Tracks_: a collection of tracks and available meta-data. In the Data Challenge scenario, tracks are the target items for the model, i.e. the collection to choose from when the model needs to provide recommendations.

![http://www.cp.jku.at/datasets/LFM-1b/](images/tracks.png)

[//]: # (TODO: add one more picture on the extra fields and their coverage)

_Historical Interactions_: a collection of interactions between users and tracks, that is, listening events, which should be used by your model to build the recommender system for the Data Challenge.

![http://www.cp.jku.at/datasets/LFM-1b/](images/training.png)

To enrich track-related metadata, four addditional objects are provided holding features derived from the WASABI dataset:

_Social and Emotion Tags_: a collection of social tags and emotion tags collected on last.fm, together with a weight that expresses how much they have been used for a given song.

![https://github.com/micbuffa/WasabiDataset/](images/tags.png)


_Topics_: a collection of 60-dimensional descriptors representing the topic distribution of a LDA topic model trained on English lyrics (model is available [here](https://github.com/micbuffa/WasabiDataset/)).


![https://github.com/micbuffa/WasabiDataset/](images/topics.png)


_Song Embeddings_: 768-dimensional SentenceBERT embeddings calculated, using the `all-mpnet-base-v2` pretrained model, on song lyrics. For each of the tracks for which lyrics were available (47% of the total number of unique songs), both embeddings calculated on the full *song* and concatenation of embeddings calculated on individual *verses* are available.

![https://sbert.net/docs/pretrained_models.html](images/embeddings.png)



For in-depth explanations of the code and the template scripts, see the instructions below and check the provided examples and tutorials in `notebooks`.

For information on how the original datasets were built and what meta-data are available, please refer to these papers: [LFM-1b](http://www.cp.jku.at/people/schedl/Research/Publications/pdf/schedl_ijmir_2017.pdf), [WASABI](https://dl.acm.org/doi/10.1007/s10579-022-09601-8).


## Hack with us

You can refer to our collab notebooks to start playing with the dataset and to run a first, very simple model, with RecList.

| Name            | Link     | 
|-----------------|----------|
| Tutorial 1 - Exploring the EvalRS Dataset | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1VXmCpL0YkLkf5_GTgJmLtd2b2A57CNjm?usp=sharing)|
| Tutorial 2 - A Dummy Model In RecList on the EvalRS Dataset  | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1QeXglfCUEcscHB6L0Gch2qDKDDlfwLlq?usp=sharing)|

## Hackathon Structure and Rules

### How the Hackathon runs

We will ask participants to come up with a contribution for the rounded evaluation of recommender systems, based on the dataset and tools available in this repository. Contribution details will be intentionally left open-ended, as we would like participants to engage different angles of the problem on a shared set of resources. 

Examples could be operationalizing important notions of robustness, applying and discussing metric definitions from literature, quantifying the trade-off between privacy and accuracy, and so on. The hackathon is a unique opportunity to **live and breathe** the workshop themes, increase chances of multi-disciplinary collaboration, network and discover related work by peers, and contribute valuable materials back to the community. 

### Rules

* The hackathon will start during the workshop and continue at the social gathering.
* Remote teams that are not able to join KDD in person can participate to the hackathon if willing to operate during the workshop hours: please send a message to `claudio dot pomo at poliba dot it` and `fede at stanford dot edu` if you're interested in partecipating remotely.
* Teams can start working on their project before KDD, provided they will also work during the event and engage the other participants during the workshop.
* The only dataset that can be used is the one provided with this repository (you can, of course, _augment_ it if you see fit): given the open-ended nature of the challenge, we are happy for participants to choose whatever tool they desire: for example, you can bring your own model or use the ones we provide if the point you are making is independent from any modelling choice. Please note that if you focus on offline code-based evaluation, re-using and extending the provided RecList provides bonus points, as our goal is to progressively standardize testing through a common library.
* The deliverables for each team are two: 1) a public GitHub repository with an open source license containing whatever has been used for the project (e.g. code, materials, slides, charts); 2) a elevator pitch (video duration needs to be less than 3 minutes) to explain (using any narrative device: e.g. a demo, a demo and some slides, animations) the project: motivation, execution, learnings and why it is cool.
* Based on the materials submitted and the elevator pitch, the organizers will determine the winners at their sole discretion and award the prizes at the social event the evening of Aug. 7th (location TBD).

### Prizes

Here is the breakdown of the prizes that will be awarded:

* $500 best student paper
* $500 best paper
* $500 runner-up best hackathon project
* $2000 best hackathon project

The prizes are generously offered by _mozilla.ai_: winners should contact `fahd at mozilla dot ai` for the delivery.

## Organizers 

This event focuses on building in the open, and adding lasting artifacts to the community. _EvalRS @ KDD 2023_ is a collaboration between practitioners from industry and academia, who joined forces to make it happen:

* [Federico Bianchi](https://www.linkedin.com/in/federico-bianchi-3b7998121/), Stanford 
* [Patrick John Chia](https://www.linkedin.com/in/patrick-john-chia/), Coveo
* [Jacopo Tagliabue](https://www.linkedin.com/in/jacopotagliabue/), NYU / Bauplan
* [Claudio Pomo](https://www.linkedin.com/in/claudiopomo/), Politecnico di Bari
* [Gabriel de Souza P. Moreira](https://www.linkedin.com/in/gabrielspmoreira/), NVIDIA
* [Ciro Greco](https://www.linkedin.com/in/cirogreco/), Bauplan
* [Davide Eynard](https://www.linkedin.com/in/deynard/), mozilla.ai
* [Fahd Husain](https://www.linkedin.com/in/fahdhusain/), mozilla.ai

## Sponsors

This Data Challenge is open and possible thanks to the generous support of these awesome folks. Make sure to add a star to [our library](https://github.com/jacopotagliabue/reclist) and check them out!


<a href="https://mozilla.ai/" target="_blank">
    <img src="images/mozai.svg" width="200"/>
</a>

<a href="https://snap.com/en-US" target="_blank">
    <img src="images/snap.png" width="200"/>
</a>

<a href="https://www.bauplanlabs.com/" target="_blank">
    <img src="images/bauplan.png" width="200"/>
</a>

<a href="https://costanoa.vc/" target="_blank">
    <img src="https://costanoa.vc/wp-content/themes/costanoa/img/logo-wide-dark2@2x.svg" width="200"/>
</a>

## How to Cite

If you find our code, datasets, tests useful in your work, please cite the original WebConf contribution as well as the EvalRS paper.

_RecList_

```
@inproceedings{10.1145/3487553.3524215,
    author = {Chia, Patrick John and Tagliabue, Jacopo and Bianchi, Federico and He, Chloe and Ko, Brian},
    title = {Beyond NDCG: Behavioral Testing of Recommender Systems with RecList},
    year = {2022},
    isbn = {9781450391306},
    publisher = {Association for Computing Machinery},
    address = {New York, NY, USA},
    url = {https://doi.org/10.1145/3487553.3524215},
    doi = {10.1145/3487553.3524215},
    pages = {99–104},
    numpages = {6},
    keywords = {recommender systems, open source, behavioral testing},
    location = {Virtual Event, Lyon, France},
    series = {WWW '22 Companion}
}
```

_EvalRS_

```
@misc{https://doi.org/10.48550/arXiv.2304.07145,
  doi = {10.48550/ARXIV.2304.07145},
  url = {https://arxiv.org/abs/2304.07145},
  author = {Federico Bianchi and Patrick John Chia and Ciro Greco and Claudio Pomo and Gabriel Moreira and Davide Eynard and Fahd Husain and Jacopo Tagliabue},
  title = {EvalRS 2023. Well-Rounded Recommender Systems For Real-World Deployments},
  publisher = {arXiv},
  year = {2023},
  copyright = {Creative Commons Attribution 4.0 International}
}
```
