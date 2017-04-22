# TF Stage

A fast and canonical project setup for TensorFlow models. The most difficult part of getting started with TensorFlow isn't deep learning, it's putting together hundreds of API calls into a cohesive model.

```
$ tfstage my_project
Project created: ./my_project
```

## Workflow

Roughly, our workflow consists of the following:

1. Start a new project with `tfstage`
2. Replace the stock input pipeline with real data
3. Replace the model with the simplest possible baseline
4. Train it
5. Iterate!

## Setup

1. Clone the tfstage repo:

    ```
    $ git clone https://gitlab/fomoro/tfstage 
    ```

2. Install tfstage with pip:

    ```
    $ pip install -e tfstage/
    ```

3. Create a new empty project directory

    ```
    $ mkdir my_project/
    $ cd <my_project>/
    ```

4. Run `tfstage`:

    ```
    $ tfstage my_project
    Project created: ./my_project
    ```

## Environment

High-level description of a new project:

- main.py: defines command-line arguments and sets up [`learn_runner`](https://goo.gl/I6TwxA)
- experiment.py: defines a [`tf.contrib.learn.Experiment`](https://goo.gl/nMvwLx) for training
- inputs.py: defines the input pipeline for training and evaluation
- model.py: defines the model, loss, and training optimization
- augment.py: defines any data augmentation or feature engineering
- serve.py: defines placeholders for [TensorFlow Serving](https://goo.gl/bM3jpA) and [Google Cloud ML Engine predictions](https://goo.gl/yTBv2e).
- prep.py: utility script for any data pre-processing (such as creating [TFRecords](https://goo.gl/bpm3zW))

In addition, several common files are created including:

- README.md
- requirements.txt
- .gitignore

### Local Deployment

```
PROJECT_NAME=my_project
MODULE_NAME="${PROJECT_NAME}.main"
PACKAGE_PATH="${PROJECT_NAME}/"
JOB_DIR=logs/

gcloud ml-engine local train \
  --module-name $MODULE_NAME \
  --package-path $PACKAGE_PATH \
  --job-dir $JOB_DIR \
  -- \
  [args]
```

### Cloud Deployment

```
MODULE_NAME="${PROJECT_NAME}.main"
PACKAGE_PATH="${PROJECT_NAME}/"
JOB_NAME="${PROJECT_NAME}_1"
JOB_DIR="gs://${PROJECT_NAME}/${JOB_NAME}"
REGION=us-east1

gcloud ml-engine jobs submit training $JOB_NAME \
  --job-dir $JOB_DIR \
  --runtime-version 1.0 \
  --module-name $MODULE_NAME \
  --package-path $PACKAGE_PATH \
  --region $REGION \
  -- \
  [args]
```
