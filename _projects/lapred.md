---
layout: page
title: Extended LaPred
description: Extension for Lane-Aware Prediction of Multi-Modal Future Trajectories of Dynamic Agents
img: assets/img/lapred/arch.png
importance: 3
category: work
github_reponame: beimingli0626/extended-LaPred
---

Restructured, modularized and extended framework, based on ideas proposed in [LaPred: Lane-Aware Prediction of Multi-Modal Future Trajectories of Dynamic Agents](https://arxiv.org/abs/2104.00249), and their [original implementation](https://github.com/bdokim/LaPred).

<div class="row">
    <div class="col-sm mt-3 mt-md-0" style="text-align: center;">
        {% include figure.html path="assets/img/lapred/arch.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

## Introduction
We implement original ideas presented in LaPred in a modularized way, along with our customized extensions. The difference with original implementation includes:
- Modularized codebase, which make it easier to test different encoder-decoder architectures, try different losses, and carry out ablation study.
- Implemented and tested a network variant which consider multiple nearby agents during feature extraction stage, while the original paper consider single nearby agent.
- Implemented a network variant which rank the likelihood of each modality naively and compute the modality selection loss with cosine similarity. Compared the performance with original implementation to justify the necessity of Modality Selection Block (check our report for details).

## Results
Most of the variations we implemented and experimented achieve minADE5 ~ 1.67 and minADE10 ~ 1.36. We also performs extensive ablation study and experiments to investigate the necessity and role of different submodules, you could refer to our report for experimental details and analysis.

<div class="row justify-content-sm-center" style="text-align: center;">
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.html path="assets/img/lapred/agent.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.html path="assets/img/lapred/laneselect.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

## Citation
Kudos to LaPred authors for their amazing results and original code implementations:
```
@InProceedings{Kim_2021_CVPR,
    author    = {Kim, ByeoungDo and Park, Seong Hyeon and Lee, Seokhwan and Khoshimjonov, Elbek and Kum, Dongsuk and Kim, Junsoo and Kim, Jeong Soo and Choi, Jun Won},
    title     = {LaPred: Lane-Aware Prediction of Multi-Modal Future Trajectories of Dynamic Agents},
    booktitle = {Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR)},
    month     = {June},
    year      = {2021},
    pages     = {14636-14645}
}

@misc{lapredCode,
    author={Kim, ByeoungDo and Park, Seong Hyeon and Lee, Seokhwan and Khoshimjonov, Elbek and Kum, Dongsuk and Kim, Junsoo and Kim, Jeong Soo and Choi, Jun Won},
    title = {LaPred Github Repository},
    howpublished = {\url{https://github.com/bdokim/LaPred}},
    note = {Accessed: 2022-12-17}
}
```