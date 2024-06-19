# Med-Gemini MedQA Relabelling and Analysis

This repository contains data and code corresponding to the MedQA relabelling
performed as part of [1], specifically for the results in Figure 4b and appendix
C.2.

[1] Khaled Saab, Tao Tu, Wei-Hung Weng, Ryutaro Tanno, David Stutz, Ellery Wulczyn,
    Fan Zhang, Tim Strother, Chunjong Park, Elahe Vedadi, Juanma Zambrano Chaves,
    Szu-Yeu Hu, Mike Schaekermann, Aishwarya Kamath, Yong Cheng, David G.T. Barrett,
    Cathy Cheung, Basil Mustafa, Anil Palepu, Daniel McDuff, Le Hou, Tomer Golany,
    Luyang Liu, Jean-baptiste Alayrac, Neil Houlsby, Nenad Tomasev, Jan Freyberg,
    Charles Lau, Jonas Kemp, Jeremy Lai, Shekoofeh Azizi, Kimberly Kanada, SiWai Man,
    Kavita Kulkarni, Ruoxi Sun, Siamak Shakeri, Luheng He, Ben Caine, Albert Webson,
    Natasha Latysheva, Melvin Johnson, Philip Mansfield, Jian Lu, Ehud Rivlin,
    Jesper Anderson, Bradley Green, Renee Wong, Jonathan Krause, Jonathon Shlens,
    Ewa Dominowska, S. M. Ali Eslami, Katherine Chou, Claire Cui, Oriol Vinyals,
    Koray Kavukcuoglu, James Manyika, Jeff Dean, Demis Hassabis, Yossi Matias,
    Dale Webster, Joelle Barral, Greg Corrado, Christopher Semturs, S. Sara Mahdavi,
    Juraj Gottweis, Alan Karthikesalingam, Vivek Natarajan.
    [Capabilities of Gemini Models in Medicine](https://arxiv.org/abs/2404.18416).
    ArXiv, abs/2404.18416.

## Overview

Med-Gemini is a family of highly capable multimodal models that are specialized in
medicine with the ability to seamlessly use web search, and that can be efficiently
tailored to novel modalities using custom encoders. Med-Gemini particularly achieves
a new state-of-the-art performance of 91.1% accuracy on the popular MedQA (USMLE) benchmark.
However, as part of this evaluation, we noticed that not all questions in the MedQA test
set are reasonable to be evaluated. We suspected that various questions include label errors
or reference missing information such as figures or lab results that are not included.
In order to report reliable results, we thus conducted a full relabeling of MedQA using
at least 3 primary care physicians (PCPs) per question, asking for missinig information and
label errors. This repository includes the corresponding data and analysis code.

## Installation

1. Install [Conda](https://docs.conda.io/en/latest/) following the
   official instructions. Make sure to restart bash after installation.
2. Clone this repository using

    ```
    git clone https://github.com/google-health/med-gemini-medqa-relabelling
    cd med-gemini-medqa-relabelling
    ```

3. Create a new Conda environment from `environment.yml` and activate it
   (the environment can be deactivated any time using `conda deactivate`):

    ```
    conda env create -f environment.yml
    conda activate medqa_relabelling
    ```

   Alternatively, manually install `jupyter`, `numpy`, `pandas` and `matplotlib`.

These instructions have been tested with Conda version 23.7.4 (not miniconda)
on a 64-bit Linux workstation. We recommend to make sure that no conflicting
`pyenv` environments are activated or `PATH` is explicitly set or changed in
the used bash profile. After activating the Conda environment, the corresponding
Python binary should be first in `PATH`. If that is not the case (e.g.,
`PATH` lists a local Python installation in `~/.local/` first), this can
cause problems.

## Data

The MedQA questions with our annotations are available in `medqa_relabelling.csv` and
can easily be loaded using Pandas:

```
input_file = 'medqa_relabelling.csv'
with open(input_file, 'r') as f:
  df = pd.read_csv(f)
df.head()
```

The CSV file contains the individual ratings as rows, with the following columns:

* An index column;
* `time`: Time for the annotation task in milliseconds;
* `worker_id` an anonymized worker id;
* `qid`: a question id;
* `question`: the MedQA question;
* `A` through `D`: MedQA's answer options;
* `answer_idx`: MedQA's ground truth answer;
* `info_missing` and `important_info_missing`: whether the rater indicated that
  information in the question is missinig and whether this information was rated
  as important to answer the question;
* `blind_answerable` and `seen_answerable`: whether the rater determined that one
  or more of the options answers the question before (`blind_`) and after (`seen_`)
  revealing the ground truth answer;
* `blind_asnwers` and `seen_answers`: the selected answers if the question is answerable;
* `seen_change`: whether the rater updated their answer after revealing the ground truth.

Details on the exact study design can be found in the [paper](https://arxiv.org/abs/2404.18416), Appendix C.2.

## Analysis

Run `medqa_analysis.ipynb` to reproduce our results from the paper using dummy model predictions.
You can replace them with your model's predictions to reproduce Figure 4b in the paper.

## Citing this work

When using any part of this repository, make sure to cite the paper as follows:

```
@article{Saab2024CapabilitiesOG,
  title={Capabilities of Gemini Models in Medicine},
  author={Khaled Saab and Tao Tu and Wei-Hung Weng and Ryutaro Tanno and David Stutz and Ellery Wulczyn and Fan Zhang and Tim Strother and Chunjong Park and Elahe Vedadi and Juanma Zambrano Chaves and Szu-Yeu Hu and Mike Schaekermann and Aishwarya B Kamath and Yong Cheng and David G.T. Barrett and Cathy Cheung and Basil Mustafa and Anil Palepu and Daniel McDuff and Le Hou and Tomer Golany and Lu Liu and Jean-Baptiste Alayrac and Neil Houlsby and Nenad Toma{\vs}ev and Jan Freyberg and Charles Lau and Jonas Kemp and Jeremy Lai and Shekoofeh Azizi and Kimberly Kanada and SiWai Man and Kavita Kulkarni and Ruoxi Sun and Siamak Shakeri and Luheng He and Ben Caine and Albert Webson and Natasha Latysheva and Melvin Johnson and Philip Mansfield and Jian Lu and Ehud Rivlin and Jesper Anderson and Bradley Green and Renee Wong and Jonathan Krause and Jonathon Shlens and Ewa Dominowska and S. M. Ali Eslami and Claire Cui and Oriol Vinyals and Koray Kavukcuoglu and James Manyika and Jeff Dean and Demis Hassabis and Yossi Matias and Dale R. Webster and Joelle Barral and Gregory S. Corrado and Christopher Semturs and S. Sara Mahdavi and Juraj Gottweis and Alan Karthikesalingam and Vivek Natarajan},
  journal={ArXiv},
  volume={abs/2404.18416},
  year={2024},
}
```

## License and disclaimer

All software is licensed under the Apache License, Version 2.0 (Apache 2.0); you may not use this file except in compliance with the Apache 2.0 license. You may obtain a copy of the Apache 2.0 license at: https://www.apache.org/licenses/LICENSE-2.0

The provided annotations are licensed under the Creative Commons Attribution 4.0 International License (CC-BY). You may obtain a copy of the CC-BY license at: https://creativecommons.org/licenses/by/4.0/legalcode

Unless required by applicable law or agreed to in writing, all software and materials distributed here under the Apache 2.0 or CC-BY licenses are distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the licenses for the specific language governing permissions and limitations under those licenses.

This is not an official Google product.

The license for the original MedQA questions can be found in [jind11/MedQA](https://github.com/jind11/MedQA).