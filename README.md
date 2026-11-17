# Cattle Health Prediction

Can a computer tell if a cow is healthy or sick just by looking at its body measurements? This small project tries to answer that question with machine learning.

I built it as a learning project in Python. I wrote the code step by step, and I tried to explain every choice, including the mistakes I found along the way.

## What I did

1. Took a public cow health dataset from Kaggle.
2. Cleaned it (fixed a spelling mistake and removed 89 copied rows).
3. Drew charts to see how healthy and sick cows are different.
4. Trained two models: a **decision tree** and a **random forest**.
5. Tested them in a fair way, including 5-fold cross-validation.
6. Checked which measurements help the models most.

## The data

The data is called **Cow Health Prediction** by Khush Upadhyay, on Kaggle. I did not upload it here. Please download it from Kaggle (check the license on the dataset page), save it as `cattle_dataset.xlsx`, and put it in a folder called `data`.

The file has 178 rows and 14 columns. Each row is one cow. The last column, `health_status`, says if the cow is healthy or unhealthy. That is the answer the model tries to guess.

## Two problems I found in the data

**1. Copied rows.** Half of the file is a copy of the other half. So there are not 178 cows, only **89 different cows** (45 healthy and 44 unhealthy). This matters because if one copy of a cow is used to teach the model and another copy is used to test it, the model has already seen the test cow. The score looks great, but it is fake. I removed the copies.

**2. A shortcut column.** Every healthy cow has `ideal` in the column `faecal_consistency`, and no sick cow has it. A model that only reads that one column gets 100%, but it has not learned anything about cows. So I removed that column and tested the models again. I also kept the results with the column, so you can compare.

## Results

I tested each model 5 times on different groups of cows (5-fold cross-validation). Every cow was tested exactly once.

| Model | Accuracy (average) | How much it jumps |
|---|---|---|
| Decision tree, without the shortcut column | 95.5% | +/- 4.2% |
| Random forest, without the shortcut column | 96.6% | +/- 4.5% |
| Decision tree, with the shortcut column | 100% | 0% (too perfect, so I do not trust it) |

The tree and the forest are **tied**. The difference between them is about one cow, which is too small to say one is better.

**Which measurements matter most?** Five measurements did most of the work: walking capacity, body temperature, heart rate, sleeping time and lying-down time. When I changed the random seed, the same five stayed at the top, but their order changed. So I do not say that one of them is "the most important". Body condition score and breed gave almost no help.

**The mistake.** One sick cow (row 3 in the table) looks healthy on many measurements. Both models missed it. On a real farm, a missed sick cow is the worst kind of mistake.

## How to run it

You need Python 3.13 and Windows, Mac or Linux. These are the steps for Windows:

```
python -m venv cattle_env
cattle_env\Scripts\activate
pip install -r requirements.txt
```

Then:

1. Put the Excel file in the `data` folder.
2. Open `02_cattle_health_final.ipynb` in VS Code (with the Python and Jupyter extensions).
3. Choose the `cattle_env` kernel.
4. Click **Run All**.

## Files

```
cattle-health-project/
├── README.md                       <- this file
├── requirements.txt                <- the libraries I used
├── 02_cattle_health_final.ipynb    <- the full project, with notes
├── .gitignore                      <- files that should not be uploaded
└── data/                           <- put the Excel file here
```

## What I learned

- How to clean and explore data with pandas.
- That a very high score can be a warning sign, not a success.
- How to split data fairly, and why the test cows must stay hidden.
- How to read a confusion matrix, precision and recall.
- How decision trees and random forests work, and what feature importance can and cannot tell us.
- Why one test is not enough, and how cross-validation helps.

## Limits (please read)

- There are only 89 cows, from one dataset. This is a small learning project, not a tool for real farms.
- The data looks unusually clean. Healthy and sick cows are very easy to separate, so the data may be partly simulated. Real farm data is usually messier.
- The file does not say what units some columns use, so I did not guess them.
- Feature importance shows what the model used. It does not prove what causes sickness.
- I never tested the model on a different farm.

## Future work

This project works on one table of measurements, taken once per cow, and everything runs offline. Here is how it could grow:

- **Real-time monitoring.** A natural next step is continuous, sensor-based monitoring, as used in precision livestock farming. Wearable sensors would send measurements all day, and a model would watch for changes and give an early warning.
- **More and better data.** Use data from several farms, and test the model on a farm it has never seen.
- **Check the missed cows.** Ask a vet to look at the cows the models get wrong, to see if their labels are right.
- **Changes over time.** Look at how measurements change over days (for example, a drop in rumination) instead of one number per cow.
- **Images.** Use camera images, for example to spot lameness or behaviour, with computer vision.

## About me

Belhachemi Chaimaa/ 知媛, Jingdezhen Ceramic University. Contact: chaimaebelhachemi47@gmail.com

Next I want to learn computer vision, so I can work with camera images of animals.
