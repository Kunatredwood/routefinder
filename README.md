# RouteFinder

[![arXiv](https://img.shields.io/badge/arXiv-2406.15007-b31b1b.svg)](https://arxiv.org/abs/2406.15007) [![OpenReview](https://img.shields.io/badge/⚖️-OpenReview-8b1b16)](https://openreview.net/forum?id=hCiaiZ6e4G) [![Slack](https://img.shields.io/badge/slack-chat-611f69.svg?logo=slack)](https://join.slack.com/t/rl4co/shared_invite/zt-1ytz2c1v4-0IkQ8NQH4TRXIX8PrRmDhQ)
[![License: MIT](https://img.shields.io/badge/License-MIT-red.svg)](https://opensource.org/licenses/MIT)[![Test](https://github.com/ai4co/routefinder/actions/workflows/tests.yml/badge.svg)](https://github.com/ai4co/routefinder/actions/workflows/tests.yml)

_Towards Foundation Models for Vehicle Routing Problems_


<table align="center">
  <tr>
    <td>
      <div align="center">
        <em>News: RouteFinder has been accepted as an <b>Oral</b> presentatation at the <a href="https://icml-fm-wild.github.io/"> ICML 2024 FM-Wild Workshop </a>! <a href="https://github.com/ai4co"> <img src="https://raw.githubusercontent.com/ai4co/assets/main/svg/ai4co_animated.svg" alt="AI4CO Logo" style="height: 1em; vertical-align: middle; transform: translateY(-20%);"> </a></em>
      </div>
    </td>
  </tr>
</table>



---

<div align="center">
    <img src="assets/overview.png" alt="RouteFinder Overview" style="width: 100%; height: auto;">
</div>


## 🚀 Installation

Install the package in editable mode:

```bash
pip install -e .
```

If you would like to install all dependencies including optional solvers, please install using `pip install -e '.[dev,solver]'`.


## 🏁 Quickstart

We recommend exploring [this quickstart notebook](examples/1.quickstart.ipynb) to get started with the `RouteFinder` codebase!

### Running

The main runner (example here of main baseline) can be called via:

```bash
python run.py experiment=main/rf/rf-100
```
You may change the experiment by using the `experiment=YOUR_EXP`, with the path under [`configs/experiment`](configs/experiment) directory.


## 🚚 Available Environments

<div align="center">
    <img src="assets/vrp.png" alt="VRP Problems" style="width: 100%; height: auto;">
</div>


We consider 24 variants, which include the base Capacity (C). The $k=4$ features O, B, L, and TW can be combined into any subset, including the empty set and itself (i.e., a *power set*) with $2^k = 16$ possible combinations. Finally, we study the additional Mixed (M) global feature that creates new Backhaul (B) variants in generalization studies, adding 8 more variants.

We have the following environments available:

| | Capacity<br>(C) | Open Route<br>(O) | Backhaul<br>(B) | Mixed<br>(M) | Duration Limit<br>(L) | Time Windows<br>(TW) |
|---|:---:|:---:|:---:|:---:|:---:|:---:|
| CVRP | ✔ | | | | | |
| OVRP | ✔ | ✔ | | | | |
| VRPB | ✔ | | ✔ | | | |
| VRPL | ✔ | | | | ✔ | |
| VRPTW | ✔ | | | | | ✔ |
| OVRPTW | ✔ | ✔ | | | | ✔ |
| OVRPB | ✔ | ✔ | ✔ | | | |
| OVRPL | ✔ | ✔ | | | ✔ | |
| VRPBL | ✔ | | ✔ | | ✔ | |
| VRPBTW | ✔ | | ✔ | | | ✔ |
| VRPLTW | ✔ | | | | ✔ | ✔ |
| OVRPBL | ✔ | ✔ | ✔ | | ✔ | |
| OVRPBTW | ✔ | ✔ | ✔ | | | ✔ |
| OVRPLTW | ✔ | ✔ | | | ✔ | ✔ |
| VRPBLTW | ✔ | | ✔ | | ✔ | ✔ |
| OVRPBLTW | ✔ | ✔ | ✔ | | ✔ | ✔ |
| VRPMB | ✔ | | ✔ | ✔ | | |
| OVRPMB | ✔ | ✔ | ✔ | ✔ | | |
| VRPMBL | ✔ | | ✔ | ✔ | ✔ | |
| VRPMBTW | ✔ | | ✔ | ✔ | | ✔ |
| OVRPMBL | ✔ | ✔ | ✔ | ✔ | ✔ | |
| OVRPMBTW | ✔ | ✔ | ✔ | ✔ | | ✔ |
| VRPMBLTW | ✔ | | ✔ | ✔ | ✔ | ✔ |
| OVRPMBLTW | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |

We additionally provide as baseline solvers for all baselines 1) [OR-Tools](https://github.com/google/or-tools) and 2) the SotA [PyVRP](https://github.com/PyVRP/PyVRP).

## 🔁 Reproducing Experiments

### Main Experiments
The `main` experiments on 100 nodes are (rf=RouteFinder) RF-POMO: [`rf/rf-100`](configs/experiment/main/rf/rf-100.yaml), RF-MoE-L: [`rf/rf-moe-L-100`](configs/experiment/main/rf/rf-moe-L-100.yaml), MTPOMO [`mtpomo-100`](configs/experiment/main/mtpomo/mtpomo-100.yaml) and MVMoE [`mvmoe-100`](configs/experiment/main/mvmoe/mvmoe-100.yaml). You may substitute `50` instead for 50 nodes. Note that we separate 50 and 100 because we created an automatic validation dataset reporting for all variants at different sizes (i.e. [here](configs/experiment/rfbase-100.yaml)).

Note that for MVMoE-based models, one can additionally pass the `model.hierarchical_gating=True` to enable hierarchical gating (L, Light version) - and similarly, `model.hierarchical_gating=False` for the full version.


Note that additional Hydra options as described [here](https://rl4co.readthedocs.io/en/latest/_content/start/hydra.html). For instance, you can add `+trainer.devices="[0]"` to run on a specific GPU (i.e., GPU 0).

### Ablations and more

Other configs are available under [configs/experiment](configs/experiment) directory.

### EAL (Efficient Adapter Layers)

To run EAL, you may use the following command:

```bash
python eal.py --path [MODEL_PATH]
```

with additional parameters that can be found in the [eal.py](eal.py) file.


### Known Bugs
- For some reason, there seem to be bugs when training on M series processors from Apple (but not during inference somehow?). We recommend training with a discrete GPU. We'll keep you posted with updates!


### 🤗 Acknowledgements

- https://github.com/FeiLiu36/MTNCO/tree/main
- https://github.com/RoyalSkye/Routing-MVMoE
- https://github.com/yd-kwon/POMO
- https://github.com/ai4co/rl4co


### 🤩 Citation
If you find RouteFinder valuable for your research or applied projects:

```
@inproceedings{berto2024routefinder,
    title={{RouteFinder}: Towards Foundation Models for Vehicle Routing Problems},
    author={Berto, Federico and Hua, Chuanbo and Zepeda, Nayeli Gast and Hottung, Andr{\'e} and Wouda, Niels and Lan, Leon and Tierney, Kevin and Park, Jinkyoo},
    booktitle={ICML 2024 Workshop on Foundation Models in the Wild (Oral)},
    year={2024},
    url={https://openreview.net/forum?id=hCiaiZ6e4G},
    note={\url{https://github.com/ai4co/routefinder}}
}
```

---

<div align="center">
    <a href="https://github.com/ai4co">
        <img src="https://raw.githubusercontent.com/ai4co/assets/main/svg/ai4co_animated_full.svg" alt="AI4CO Logo" style="width: 30%; height: auto;">
    </a>
</div>