# Reliable Vision
### Confidence Calibration and View Agreement for Image Classification under Degradation

When should an image classifier trust its prediction, and when should it ask for help?

Reliable Vision explores this question using a CNN trained on CIFAR-10. It compares predicting every image with two selective classification policies that flag less reliable predictions for human review.

## Overview

Image classifiers can make confident mistakes, particularly when images are blurred or noisy. This project combines calibrated confidence with prediction consistency across transformed views to investigate the trade-off between automated coverage and prediction accuracy.

The workflow includes:
- Training a CNN for image classification.
- Calibrating confidence using temperature scaling.
- Measuring agreement across transformed image views.
- Selecting review thresholds on a separate policy split.
- Evaluating frozen review rules on clean and degraded test images.

Flagging an image for review does not automatically correct its prediction. This project evaluates the routing decision; it does not measure human reviewer performance.

## Methods

Three approaches are compared:

| Method | Decision rule |
|---|---|
| Predict every image | Accept all predictions |
| Calibrated confidence | Accept predictions with calibrated confidence ≥ 0.75 |
| Confidence + view agreement | Accept predictions with combined score ≥ 0.70 |

View agreement measures the fraction of three transformed-view predictions that match the original prediction.

The combined score is:

**Combined score = calibrated confidence × view agreement**

Predictions below the selected threshold are flagged for review.

## Dataset

CIFAR-10 contains 32 × 32 RGB images from 10 classes.

| Split | Images | Purpose |
|---|---:|---|
| Training | 35,000 | Learn model parameters |
| Validation | 5,000 | Monitor model performance |
| Calibration | 5,000 | Fit temperature scaling |
| Policy selection | 5,000 | Select review thresholds |
| Final test | 10,000 | Evaluate frozen policies |

Calibration and policy selection are separated from final test evaluation.

## Evaluation Conditions

The final test evaluates five conditions:

- Clean images
- Gaussian blur: sigma = 1
- Gaussian blur: sigma = 2
- Gaussian noise: standard deviation = 0.05
- Gaussian noise: standard deviation = 0.10

The review thresholds remain fixed across all test conditions.

## Evaluation Metrics

- **Coverage:** proportion of predictions accepted automatically.
- **Accepted accuracy:** accuracy among accepted predictions.
- **Accepted errors:** incorrect predictions accepted automatically.
- **Review workload:** number of images flagged for review.
- **95% confidence interval:** uncertainty around accepted accuracy.
- **Error AUROC:** ability of the reliability score to distinguish correct and incorrect predictions.

Coverage and accepted accuracy should be interpreted together: higher accepted accuracy may require reviewing more images.

## Confirmed Baseline Result

On the clean final test set, predicting every image achieved:

| Metric | Result |
|---|---:|
| Test images | 10,000 |
| Accuracy | 84.34% |
| 95% confidence interval | 83.61–85.04% |
| Incorrect predictions | 1,566 |
| Coverage | 100% |

The notebook contains the full comparison of review policies across clean, blurred, and noisy images.

## Technology

- Python
- PyTorch
- torchvision
- NumPy
- Matplotlib
- scikit-learn
- Google Colab

## Run the Project

Open the notebook in Google Colab:

[Open Reliable Vision in Colab](https://colab.research.google.com/drive/1Az1TWZLeuRQzrPp5SM40-Z39nk4BpHo3?usp=sharing)

1. Select a GPU runtime if available.
2. Run the setup and dataset preparation cells.
3. Run training and confidence calibration.
4. Run policy selection.
5. Run the final evaluation and inspect the tables and plots.

## Limitations

This is a research prototype evaluated on CIFAR-10 and selected synthetic degradations. Its results do not establish reliability on other datasets or real-world deployment conditions.

## Author

**Jaber Mobarak**

Master’s student in Artificial Intelligence and Big Data  
MGTU STANKIN
## Report

[Read the project report](Reliable_Vision_project_report.pdf)
