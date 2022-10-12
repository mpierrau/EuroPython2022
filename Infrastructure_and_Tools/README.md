# Tools and Infrasctructure/environment

## [PyArrow and the Future of Data Analytics](https://ep2022.europython.eu/session/pyarrow-and-the-future-of-data-analytics) (Alessandro Molina) (MP)
## [YouTube link](https://youtu.be/6vCejqZyxpA?list=PL8uoeex94UhFzv6hQ_V02xfMCcl8sUr4p&t=1965)
### tl;dr begin
PyArrow is a data format, efficient for data science. It is optimized in many ways but is still easy to use, like numpy or torch arrays. However, there is not yet a stable release which is compatible with the major ML-frameworks (PyTorch, Tensorflow etc.). There is a beta version of [TorchArrow](https://github.com/pytorch/torcharrow), but it is still v 0.1.0, so not very stable probably.

### Notes from talk

Apache Arrow is not just a format, but also:
- a data interchange standard
- an in-memory format
- a networking format
- a storage format
- an i/o library
- a vector computation library
- a dataframe like library
- a query engine
- a partitioned datasets manager

End goal: Allow you to write your code in any format you prefer (Python Java, C++, C¤, Ruby, R etc)

Aimed at providing a “write everywhere, run everywhere” experience.

PyArrow is the entry point to the Apache Arrow ecosystem for Python developers.

Fundamental type in PyArrow is a “column of data”, exposed by the pyarrow.Array object and its subclasses. Similar to NumPy single dimension arrays.
Complex objects put in arrow array are saved as _data_, not Python objects. This means: no additional Python induced overhead.
PyArrow arrays are optimized for in-memory storage and are intuitive to use! Much like a numpy or torch array. Masking is easy, lookup and transform is optimal. Scan through entire array, no lookup or evaluation necessary. Saves overhead.

Must use explicit function calls for now (For multiplication, addition etc), but will hopefully be dunder compatible in future.

[Compute functions (docs)](https://arrow.apache.org/docs/cpp/compute.html#compute-function-list)

Grouped PyArrow columns form a pyarrow.Table. It is cheap to append or augment a table. PyArrow is similar to Pandas Dataframe, but more optimized.

Tables are actually constituted by pyarrow.ChunkedArray so that appending rows to them is a cheap operation (only relevant chunks are updated).
Join, filter, aggregate data in tables. Acero compute engine.
Pyarrow supports zero-copy transformation from numpy/pandas to PyArrow format and vice versa.

Can be significantly faster than numpy in specific cases.

`pa.table(dict_of_numpy_arrays).to_pandas()`
is  faster than 
`pd.DataFrame(dict_of_numpy_arrays)`
(50.2 ms vs 82.5 ms)

alternatively:
`df = pd.read_csv(“large.csv”, engine=”pyarrow”)`
(~3 s vs ~13 s (!))

PyArrow has a dataset API (class). Allows for working with tabular data that is potentially bigger than memory and distributed across multiple files.
Datasets provide lazy access to the data, avoiding the need of loading it all in memory at once.

Fast read/write ops. and low marshaling (serialization-ish) cost.

[Getting started](https://arrow.apache.org/docs/python/getstarted.html)

Some work on supporting pyarrow for support pytorch ([TorchArrow](https://github.com/pytorch/torcharrow)!) but still Beta!

Has support for user specified functions.

## [Use animated charts to present & share your findings with ipyvizzu](https://ep2022.europython.eu/session/use-animated-charts-to-present-share-your-findings-with-ipyvizzu) (MP)

Main message: Now easier to present animated charts and graphs
The animation keeps the context

Focus on transitioning between types of graphs for the same data

Changes over time, zooming

Issues with data scienteists delivering insights to business stakeholders:
32% of time is spent with reporting and presentation
53% say business decisions are based on their insights
64% of them say that management have trouble understanding the stories they tell
Source: anaconda

jupyter and python support since march 2022 

can hover over data points

new update yesterday - presets and two-way compatibility

Everything is done locally (nothing towards a cloud)!

[vizzuhq](https://github.com/vizzuhq)

[ipyvizzu](https://github.com/vizzuhq/ipyvizzu)

[ipyvizzu-story](https://github.com/vizzuhq/ipyvizzu-story) <- for presentations (two-way)

[Examples](https://www.reddit.com/user/VizzuHQ/)

Can also make dashboards and combine charts

## [Docker, tips and tricks](https://ep2022.europython.eu/session/my-journey-using-docker-as-a-development-tool) (LK)
Intense tips-and-tricks lecture of Docker usage. Focused mainly on the usage of Docker compose and how to speed up the building of its parts. Claims pytest can be used within the container, completely isolating yourself within the docker environment.
Multistage-building of the image can save both save time and gives you full controll of the dependencies.

Easy setup, independent of OS.
Databases can be dockerized as well, keeping the development clean and free from changing behavior.
Manage multiple containers through docker-compose. Note, old syntax! New is docker SPACE compose, they removed the dash in the improved version. From the docker documentation:
“The new Compose V2, which supports the compose command as part of the Docker CLI, is now available.
Compose V2 integrates compose functions into the Docker platform, continuing to support most of the previous docker-compose features and flags. You can run Compose V2 by replacing the hyphen (-) with a space, using docker compose, instead of docker-compose.”

Various tips:
“--reload” flag reloads the server if the code has changed.
Use “docker compose down” to shut down all containers
### Tests in docker
“docker compose run app pytest”
in some file: run: docker compose run app pytest

### Speed up docker
Smaller images
    Remove redundant dependencies
        gives fewer security vectors
    Less storage
ex: FROM python.version-slim; imports msaller version of python in favor of building image speed.

### Dependencies
Dev dep in docker image
    You don’t need pytest in production, etc.
Multistage builts: gives the dev more options on how each container is being built. Does not save time but saves storage.
Can save time with the "cache from" flag, only reloads un-cached statements.

Slides:
https://docker-as-a-dev-tool.haseebmajid.dev/
Code example:
https://gitlab.com/haseeb-slides/docker-as-a-dev-tool



## Sponsors:
sourcery VSC extention


## [Managing complex data science experiments configurations with Hydra](https://ep2022.europython.eu/session/managing-complex-data-science-experiment-configurations-with-hydra) (MP)

Hydra & MLFlow

What is hydra? 
What’s the problem?
Managing complex configurations
Easy to confuse which values worked best (not traceable)
Results may be difficult to reproduce

hydra.cc

@hydra.main(version_base=”1.2”, config_path=”.”, config_name=”config”)
def my_experiment(cfg): …
    logger.info(cfg.model)

config.yaml content:
model:
    a: 1
    b: 2
    c: 3

adds a –help to the script

can then do 
`python script.py model.a = value`
to change value

Everytime the hydra based script is run, it creates an output directory with log, the config yaml file, hydra config file and any CL overrides.

gives traceability

has CL tab completion
eval “$(python my_experiment.py –shell-completion install=bash)”
then can tab

multirun allows experiment to run with a set of different parameters. 
(--multirun model.a=1,3 model.b=2,4)
parallellizable?

Hydra components:
OmegaConf
Can combine variables in config.yaml:

`baz: (${foo}, ${bar}!)`
Resolvers:
Some logic in yaml files 

```
${add:${foo},${bar}}
from omegaconf import OmegaConf
OmegaConf.register_new_resolver(“add”, lambda a,b : a+b)
```

Python Logging
Regular python logging, but configured for you
Launchers
Joblib, _Ray_, Redis Queue, Submit it! for Slurm
Sweepers
Adaptive Experimentation Platform (ax)
Nevergrad
_Optuna_
Plugins

Allows for composing configuration files from various config files.
Each subsection (“package”) has a subdirectory and namespace

my_experiment.yaml:
```
defaults:
training_settings

lr: 0.03
```

training_settings.yaml:
```
epochs: 20
optimizer_type: adam
lr: 0.01
early_stopping: false
```
```
@hydra.main(config_name=”my_experiment.yaml”)
def my_experiment(cfg): …	| Visualization 	| 9 	| [Link](https://youtu.be/uVD6B8WLMTo?list=PL8uoeex94UhFzv6hQ_V02xfMCcl8sUr4p&t=8762) 	| Talk 	| Peter Vid
```

Config groups and options!!

defaults:
dataset: imagenet

dataset/imagenet.yaml:
images: /…/imagenet/
labels: /…/labels.txt

dataset/cifar10.yaml:
images: /…/cifar10/
labels: /…/labels.txt

colorlog extention for hydra (coloring logs)

can specify classes in config!
loss_function:
 _target_: torch.nn.modules.loss.CrossEntropyLoss

import hydra
hydra.utils.instantiate(cfg.loss_function)

partial? what is a functools.partial?

there is also type checking.

Integration with mlflow
What is mlflow?

pip install mlflow
mlflow server

Can check progress of training here

just add `mlflow.log_metric(...,...)` to experiment script
`mlflow.log_artifact(“.hydra/config.yaml”)` <- integration part


Hydra makes your experiments:
Making experiments manageable
Traceable
Reproducible
This is science! 

## Data-warehouse meets Data-lakes (LK)
Very fast talk about the dynamics of data warehouses and data lakes. Gives an interesting take on why both are necessary, using the bottom-up and top-down approaches as foundations for the argument - both are indeed necessary and may even complement each-other.
Why move from warehouse to lake?
The unstructured nature of data lakes makes it possible for new interpretations of the data, a more flexible approach. Takes time to find what you are looking for, but when your “question/hypothesis” has been found in the data lake, it can be structured and stored in the warehouse.

Warehouse: Top-down approach, go from assuming question is right
Lake: Bottom-up approach: validate question through data?

Data lakes technologies recommended:
Delta Lake, hudi, ICEBERG.


