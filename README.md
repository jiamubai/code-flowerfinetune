# FlowerTune LLM on Code Dataset

This directory conducts federated instruction tuning with a pretrained [Qwen/Qwen2.5-Coder-7B-Instruct](https://huggingface.co/Qwen/Qwen2.5-Coder-7B-Instruct) model on a [Code dataset](https://huggingface.co/datasets/flwrlabs/code-alpaca-20k).
We use [Flower Datasets](https://flower.dev/docs/datasets/) to download, partition and preprocess the dataset.
Flower's Simulation Engine is used to simulate the LLM fine-tuning process in federated way,
which allows users to perform the training on a single GPU.


## Methodology

This baseline performs federated LLM fine-tuning with [LoRA](https://arxiv.org/pdf/2106.09685) using the [🤗PEFT](https://huggingface.co/docs/peft/en/index) library.
The clients' models are aggregated with FedAvg strategy.
This provides a baseline performance for the leaderboard of Code challenge.


## Environments setup

Project dependencies are defined in `pyproject.toml`. Install them in an activated Python environment with:

```shell
cd flower-code
pip install -e .
```

## Experimental setup

The dataset is divided into 10 partitions in an IID fashion, a partition is assigned to each ClientApp.
We randomly sample a fraction (0.2) of the total nodes to participate in each round, for a total of `10` rounds.
```shell
flwr run
```
All settings are defined in `pyproject.toml`. To run experiments with flexlora, set `use_flexlora` to `1` through:
```shell
flwr run --run-config "use_flexlora=1
```

## Evaluation Result


|          | humaneval | mbpp  | multiple-cpp | multiple-js | Average |
|:--------:|:---------:|:-----:|:------------:|:-----------:|:-------:|
|  FedAvg  |   27.43   | 64.00 |    60.24     |    72.67    |  56.08  |
| FlexLoRA |   23.17   | 64.00 |    60.86     |    70.18    |  54.55  |

All experiments are conducted using 2 A5000(20GB memory) GPUs.
## Model saving

The PEFT checkpoint can be found in: https://drive.google.com/file/d/1KxaLwblBeBLqz-kBfXn8JhDQ_5G5SCvm/view?usp=sharing
