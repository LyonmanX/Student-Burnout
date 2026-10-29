# Student Burnout and Dropout Risk

What factors are linked to a student being at risk of dropping out? This project cleans a dataset of 800 students,
compares dropout risk across groups with significance tests, and builds logistic regression and random forest
models to see which factors matter most when considered together.

> **Note:** The data is **synthetic** (computer-generated), not real student records. The project practices data
> cleaning, statistical testing and modeling. Results describe the generated data and shouldn't be used for real
> decisions about students.

| File | What it is |
|---|---|
| `BurnoutAnalysis.ipynb` | The full analysis: profiling, cleaning, comparisons, models and conclusions |
| `student_burnout_dropout_dataset_2.csv` | The raw dataset |
| `archive/v1/` | Earlier cleaning and visualization notebooks, kept for reference |

---

## Key findings

| Factor | Dropout risk rate | Gap | p-value |
|---|---|---|---|
| **Burnout level** | 92.0% (high) vs. 23.8% (low) | **68.2 pts** | < 0.001 |
| **Attendance** | 70.1% (low) vs. 42.7% (high) | **27.4 pts** | < 0.001 |
| Family support | 63.4% (low) vs. 46.5% (high) | 16.9 pts | < 0.001 |
| Part-time job | 62.3% (yes) vs. 51.0% (no) | 11.3 pts | 0.005 |
| Counseling access | 57.6% (no) vs. 48.7% (yes) | 8.9 pts | 0.021 |
| Backlogs | 61.0% (3+) vs. 53.5% (0) | 7.5 pts | 0.634 (no clear link) |

- **Burnout is the strongest factor by far.** In the random forest, shuffling burnout lowers accuracy by 22.6
  percentage points, more than 20 times any other factor.
- **Habits linked to burnout:** high-burnout students study about 39 minutes less per day and spend about 45 minutes
  more on screens (both p < 0.001). Exercise has a smaller link (p = 0.006). Sleep and social activity show no clear
  link.
- **Models:** the random forest reaches 75.5% accuracy and logistic regression 75.0%, about 21 percentage points
  better than always guessing "at risk" (54.5%). Without burnout, accuracy drops to 70.5–73.5%, and attendance, study
  hours and motivation become the top factors.

---

## Method

**Data:** [Student Burnout and Dropout Risk Dataset](https://www.kaggle.com/datasets/harpartapsingh13/student-burnout-and-dropout-risk-dataset)
on Kaggle. 800 students, 25 columns: demographics, academics, daily habits, finances, mental-health scores,
burnout level and dropout risk.

1. **Profiling:** measured missing values per column (1.6–6.4%) and checked value ranges and categories.
2. **Cleaning:** missing text answers filled with "Unknown", missing numbers with each column's median. The outcome
   columns (`Burnout_Level`, `Dropout_Risk`) were never used for filling, so the cleaning doesn't build in the
   relationships being tested.
3. **Group comparisons:** dropout risk rate per group, with chi-square tests. Habits across burnout levels were
   compared with Kruskal-Wallis tests.
4. **Models:** baseline, logistic regression and random forest, trained on 75% of students and tested on the other
   25%. Missing values are filled inside the model pipeline using the training data only. Permutation importance
   shows which factors the random forest relies on, with and without burnout.

## Limitations
- The data is synthetic, which likely explains burnout's dominance and the missing link between backlogs and GPA
  (correlation 0.006).
- The data shows links, not causes. For example, it can't tell whether less studying leads to burnout or burnout
  leads to less studying.
- The models were tested on a single 25% split of 800 students. A different split could shift accuracy by a few
  points.

## How to run
```bash
pip install pandas scipy scikit-learn matplotlib seaborn jupyterlab
jupyter lab BurnoutAnalysis.ipynb
```
Keep `student_burnout_dropout_dataset_2.csv` in the same folder as the notebook and run all cells.
