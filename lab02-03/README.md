# Deep Learning Labs 2 & 3
## From Supervised Learning to a Shallow Neural Network

##NOTE: Figures are generated within Jupyter Notebook without individual files, in case the oversized notebook cannot be sent to GitHub again

## Problem
The objective of this laboratory is to predict the net electrical
power output (PE) of a combined-cycle power plant using measured
environmental and operating conditions.

## Dataset
The model uses four input variables:

- AT: Ambient Temperature
- V: Exhaust Vacuum
- AP: Ambient Pressure
- RH: Relative Humidity

The target variable is PE, the net electrical power output.

## Models
Two regression models were compared:

- Linear Regression
- Shallow Neural Network using PyTorch

The selected neural-network configuration used 16 hidden neurons,
a learning rate of 0.1, and 300 epochs.

## Evaluation
Model performance was evaluated using:

- Mean Absolute Error (MAE)
- Root Mean Squared Error (RMSE)
- R² score

## Main Result
On the final test set:

| Metric | Linear Regression | Shallow NN |
|---|---:|---:|
| MAE | 3.5770 MW | 3.9102 MW |
| RMSE | 4.5393 MW | 5.2008 MW |
| R² | 0.9291 | 0.9069 |

Linear regression performed slightly better than the shallow neural
network on all three final test metrics.

## Engineering Conclusion
The neural network did not provide enough improvement to justify its
additional complexity for this dataset. Linear regression achieved
slightly better final test performance while remaining simpler,
easier to interpret, and less computationally demanding.

## Files
- `lab02_03.ipynb` — complete laboratory notebook
- `power_plant.csv` — power-plant dataset
