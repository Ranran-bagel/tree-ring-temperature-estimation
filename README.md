# Tree-Ring Temperature Estimation

## Description

This project explores whether historical temperatures can be reconstructed from tree-ring width data using statistical regression models.

The analysis focuses on *Quercus crispula* (Mizunara oak) trees from eastern Hokkaido, Japan, and examines the relationship between tree-ring widths and historical annual mean temperatures recorded in Kushiro.

The project originated from a first-year university group assignment in Python-based data analysis. It was subsequently reorganized and extended to improve reproducibility, statistical validation, and documentation.

**Research Question:**
Can historical temperatures be reliably estimated from tree-ring width data?

**Conclusion:**
The regression models developed in this study did not demonstrate sufficient predictive accuracy for reliable historical temperature reconstruction.

## Technologies

- Python
- pandas
- NumPy
- Matplotlib
- statsmodels
- Jupyter Notebook

## Methodology

The analysis follows six main steps:

1. **Data Collection**
   - Obtain publicly available tree-ring width data from J-STAGE Data.
   - Collect historical temperature records from the Japan Meteorological Agency (JMA).

2. **Data Preprocessing**
   - Aggregate tree-ring measurements from individual cores into tree-level measurements.
   - Handle missing values and align datasets by year.
   - Apply an approximate temperature adjustment based on elevation differences.

3. **Exploratory Data Analysis**
   - Visualize historical tree-ring width and temperature trends.
   - Calculate Pearson correlation coefficients.
   - Examine relationships between individual trees and annual temperatures.

4. **Regression Modeling**
   - Perform simple linear regression.
   - Perform multiple linear regression.
   - Estimate regression coefficients using Ordinary Least Squares (OLS).
   - Examine R², adjusted R², and statistical significance.

5. **Model Validation**
   - Divide the available observations chronologically into training and validation periods.
   - Evaluate predictions in two directions:
     - **Validation A:** Earlier 60% for training, later 40% for validation.
     - **Validation B:** Later 60% for training, earlier 40% for validation.
   - Evaluate predictive performance using correlation coefficients (r), Reduction of Error (RE), and Coefficient of Efficiency (CE).

6. **Interpretation**
   - Compare model performance across tree groups.
   - Examine limitations of the regression models.
   - Discuss potential improvements to historical temperature reconstruction.

## Results

The analysis includes tree-ring measurements from:

- 15 large-sized trees
- 6 medium-sized trees

### Correlation and Regression Results

| Metric | Large-sized Trees | Medium-sized Trees |
|---|---:|---:|
| Maximum individual correlation | 0.52 (Qm246) | 0.48 (Qms546) |
| Correlation of mean tree-ring width | -0.08 | -0.01 |
| Multiple regression R² | 0.42 | 0.29 |
| Adjusted R² | 0.34 | 0.24 |
| Regression coefficients with p < 0.05 | 2 | 1 |

### Model Validation

| Metric | Large-sized Trees | Medium-sized Trees |
|---|---:|---:|
| Validation r (A) | -0.17 | -0.09 |
| Validation r (B) | -0.40 | -0.17 |
| Validation RE (A) | -0.81 | +0.16 |
| Validation RE (B) | -1.37 | -4.12 |
| Validation CE (A) | -4.85 | -0.89 |
| Validation CE (B) | -5.85 | -10.19 |

**Validation A:** Training on the earlier 60% of observations and evaluating on the later 40%.

**Validation B:** Training on the later 60% of observations and evaluating on the earlier 40%, which more closely resembles historical climate reconstruction.

RE compares predictive performance against a reference based on the training-period mean, while CE uses the validation-period mean.

Negative values indicate that the model performs worse than the corresponding mean-based reference.

### Key Findings

- Some individual trees showed moderate correlations with annual temperature.
- Multiple regression models achieved moderate explanatory power within the fitted dataset.
- However, predictive performance deteriorated substantially during chronological validation.
- CE values were negative across all evaluated models and both validation directions.
- Even when RE was positive, low validation correlations suggested that the models did not reliably capture year-to-year temperature variations.

These findings demonstrate that moderate in-sample regression performance does not necessarily translate into reliable out-of-sample predictions.

Detailed results and visualizations are available in the Jupyter Notebook.

## Repository Structure

```text
tree-ring-temperature-estimation/
├── README.md
├── requirements.txt
├── .gitignore
├── data/
│   └── Kushiro_kion.csv
└── notebooks/
    └── tree_ring_analysis.ipynb
```

The original tree-ring datasets are not included in this repository.

The notebook downloads the required datasets from their original source and stores them locally in the `data/` directory.

The analysis notebook and its explanations are written in Japanese.

## Installation and Usage

Clone the repository:

```bash
git clone https://github.com/Ranran-bagel/tree-ring-temperature-estimation.git
cd tree-ring-temperature-estimation
```

Create and activate a virtual environment:

```bash
python -m venv .venv
source .venv/bin/activate
```

For Windows:

```bash
.venv\Scripts\activate
```

Install the required dependencies:

```bash
pip install -r requirements.txt
```

Open the Jupyter Notebook:

```bash
jupyter lab notebooks/tree_ring_analysis.ipynb
```

Run all cells using **Restart & Run All**.

An internet connection is required for the initial download of the tree-ring datasets.

If automatic downloading fails, the datasets can be downloaded manually from J-STAGE Data and placed in the `data/` directory.

## Data Sources and Licensing

### Tree-Ring Width Data

**Dataset:**

Terazawa, K., Aoki, N., Tokita, K., Sakai, K., Niida, T., & Ohno, Y. (2025).

*Tree-ring widths of large- and medium-sized Quercus crispula trees spanning 1663–2019 in eastern Hokkaido, Japan.*

J-STAGE Data.

DOI:
https://doi.org/10.50853/data.jjfs.28592693.v1

Dataset:
https://jstagedata.jst.go.jp/articles/dataset/Tree-ring_widths_of_large-_and_medium-sized_em_Quercus_crispula_em_trees_spanning_1663_2019_in_eastern_Hokkaido_Japan/28592693

Related publication:
https://doi.org/10.4005/jjfs.107.103

**License:** Creative Commons Attribution-NonCommercial-NoDerivatives 4.0 International (CC BY-NC-ND 4.0).

The following original datasets are not redistributed in this repository:

- `trw_l.csv`
- `trw_m.csv`
- `info_tree.csv`
- `info_core.csv`

Instead, the notebook retrieves the original files directly from the data provider.

The project is intended for educational and non-commercial portfolio purposes.

### Historical Temperature Data

**Source:** Japan Meteorological Agency (JMA)

Historical monthly temperature records from Kushiro:

https://www.data.jma.go.jp/stats/etrn/view/monthly_s3.php?prec_no=19&block_no=47418&year=1879&month=&day=&view=

The temperature dataset (`Kushiro_kion.csv`) was manually compiled and processed from publicly available JMA records.

It is not an official CSV dataset produced by JMA.

The source is acknowledged in accordance with the applicable public data usage terms.

JMA usage information:
https://www.jma.go.jp/jma/kishou/info/coment.html

Missing or insufficient observations are handled during preprocessing.

## Limitations

Several limitations should be considered when interpreting the results.

### 1. Tree Age and Growth Trends

Raw tree-ring widths were used without removing age-related growth trends.

Tree-ring growth is influenced by multiple factors, including tree age, precipitation, soil conditions, and competition.

Consequently, variations in ring width cannot be attributed exclusively to temperature.

### 2. Temperature Adjustment

An approximate temperature adjustment of -3°C was applied to account for elevation differences between the sampling area and the Kushiro observation station.

This adjustment assumes a temperature lapse rate of approximately 0.6°C per 100 meters.

It is a simplified assumption rather than a site-specific, independently validated correction.

### 3. Model Generalization

Although some regression models achieved moderate in-sample R² values, chronological validation showed poor predictive performance.

This suggests that the observed statistical relationships were insufficient for reliable temperature reconstruction.

### 4. Environmental Variables

The models primarily use tree-ring width as the explanatory variable.

Other environmental factors, such as precipitation and previous-year climate conditions, were not explicitly modeled.

## Future Improvements

Potential improvements include:

- Applying dendrochronological detrending methods to remove age-related growth patterns.
- Constructing standardized tree-ring width indices.
- Investigating multicollinearity among tree-ring variables.
- Incorporating additional climate variables.
- Comparing linear regression with alternative statistical and machine learning models.
- Improving model validation and uncertainty estimation.

## Project Background and Contributions

This project originated from a first-year university group assignment involving Python-based data analysis.

The original research idea—investigating whether historical temperatures could be estimated from tree-ring width data—was proposed by me.

The use of regression analysis was developed through discussions with generative AI.

The original analysis was completed as a group assignment, with AI assistance in implementing Python code.

The project was subsequently reorganized and extended with additional model validation and documentation.

The original coursework and subsequent improvements should be understood as distinct stages of the project.

## AI Usage

ChatGPT and Claude were used in preparing this repository.

**My work**
- I wrote the original analysis (correlation, simple regression and multiple regression of tree-ring width against temperature) as a first-year university assignment.
- The research question and dataset were chosen by me.

**AI assistance**
- Cleaning up the notebook for publication: removing Colab dependencies, unifying file paths, simplifying the code, and adding an automatic data download step.
- Adding chronological validation. Claude added the early-to-late split; the reverse (late-to-early) split was added following a suggestion from ChatGPT. The RE and CE metrics were added at that stage.
- Checking data licenses and citations, and writing the README.
- Discussing statistical approaches.

All AI-generated code and text were reviewed and run by me before being included.

## Acknowledgments

This project was originally completed as part of a university group assignment.

The tree-ring dataset was provided through J-STAGE Data, and historical temperature observations were obtained from the Japan Meteorological Agency.