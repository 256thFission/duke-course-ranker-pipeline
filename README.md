# Course Evaluation Analysis Pipeline

A pipeline to rank Duke courses & professors, sourced from https://eval-duke.evaluationkit.com (data not included for copyright reasons).

## Project Overview

The format is ordinal survey data, grouped by indivual course section and with a ranking of 1-5 for most quantitive questions. The pipeline provides:

- **Modular pipeline architecture** 
- **Statistical rigor** with Wilson confidence intervals and appropriate ordinal data methods
- **Confidence scores** to indicate reliability of results based on sample sizes
- **Comprehensive analysis** including correlations between evaluation questions
- **Reproducible workflow** from raw data to final reports

## Quick Start

### Prerequisites

- R (>= 4.0.0)
- Required packages: `dplyr`, `binom`, `stringr`, `polycor`, `corrplot`, `psych`, `tidyverse`, `ggplot2`, `DT`

### Installation

1. Clone this repository
2. Open the R project file: `courses.Rproj`
3. Install dependencies: `renv::restore()` (after initialization)
4. Run the data preparation pipeline: `source("data-prep/00_run_all_data_prep.R")`
5. Generate analysis report: `quarto::quarto_render("analysis/full_analysis_demo.qmd")`

## Repository Structure

```
├── R/                      # Core pipeline modules (in pipeline/)
│   ├── main_pipeline.R     # Main interface and coordination
│   ├── data_aggregation.R  # Data aggregation functions
│   ├── ordinal_metrics.R   # Ordinal response calculations
│   ├── statistical_analysis.R # Statistical methods
│   ├── tiering_and_summary.R  # Confidence tiers
│   └── visualization.R     # Plotting functions
├── data-prep/              # Data preparation scripts
│   ├── 00_run_all_data_prep.R # Master data preparation script
│   ├── courses.qmd         # Data combination script
│   └── course_codes.qmd    # Course code processing
├── data/                   # Clean datasets
│   ├── [DEPT]_evaluations.csv # Raw department data
│   ├── all.csv            # Combined raw data
│   ├── all_combine.csv     # Enriched with course codes
│   └── treqs.csv          # Final prepared dataset
├── analysis/               # Analysis and reporting scripts
│   ├── full_analysis_demo.qmd    # Main analysis report
│   ├── correlation_analysis_demo.qmd # Correlation analysis
│   └── sta_analysis_demo.qmd     # Statistical analysis
├── outputs/                # Generated plots, tables, and reports
├── utils/                  # Utility and testing scripts
├── doc/                   # Documentation
│   └── pipelineReadme.md  # Detailed pipeline documentation
├── tables/                # Additional data tables
├── .gitignore             # Git ignore rules
├── README.md              # This file
└── courses.Rproj          # R project file
```

## Usage

### Basic Analysis

```r
# Load the pipeline
source("pipeline/main_pipeline.R")

# Load your data
data <- read.csv("data/all.csv")

# Run instructor analysis
instructor_results <- run_instructor_analysis(data)

# Run subject analysis  
subject_results <- run_subject_analysis(data)

# Custom analysis
custom_results <- run_evaluation_pipeline(
  data = data,
  group_var = "instructor",
  question_stem = "Q6_overall_instructor_rating",
  include_correlations = TRUE
)
```

### Full Reproducible Workflow

1. **Data Preparation**: `source("data-prep/00_run_all_data_prep.R")`
2. **Analysis**: `quarto::quarto_render("analysis/full_analysis_demo.qmd")`
3. **View Results**: Check the `outputs/` directory for generated files

## Key Analysis Components

### Confidence Tiers

Results are automatically categorized by reliability:

- **High Confidence**: Top quartile of sample sizes - suitable for high-stakes decisions
- **Moderate Confidence**: 50th-75th percentile - reliable for most purposes  
- **Low Confidence**: 25th-50th percentile - interpret carefully
- **Preliminary Only**: 10+ responses but below low tier - suggestive only
- **Insufficient Data**: <10 responses - unreliable

### Statistical Methods

- **Wilson Confidence Intervals**: More accurate than normal approximation for proportions
- **Polychoric Correlations**: Appropriate method for ordinal survey data
- **Sample Size Awareness**: All analyses account for response count reliability

### Visualizations

- Correlation matrices with hierarchical clustering
- Confidence interval plots
- Distribution analyses
- Publication-ready formatting

## Data Requirements

Your evaluation data should contain:

- **Grouping variables**: instructor, subject, course_title, etc.
- **Level counts**: `[question]_level_1`, `[question]_level_2`, etc. for 5-point scales
- **Class information**: ClassSize or similar sample size indicators
- **Optional**: Pre-computed means and rates

Example structure:
```
instructor | course_title | ClassSize | Q6_level_1 | Q6_level_2 | Q6_level_3 | Q6_level_4 | Q6_level_5
-----------|--------------|-----------|------------|------------|------------|------------|------------
Smith_J    | MATH 101     | 25        | 1          | 2          | 5          | 10         | 7
Johnson_M  | PHYS 201     | 30        | 0          | 1          | 3          | 12         | 14
```

## Dependencies

The project uses `renv` for package management. Install with:

```r
install.packages("renv")
renv::restore()
```

Core packages:
- `dplyr` - Data manipulation
- `binom` - Wilson confidence intervals  
- `stringr` - String processing
- `polycor` - Polychoric correlations
- `corrplot` - Correlation visualization
- `psych` - Psychological statistics
- `tidyverse` - Data science toolkit
- `ggplot2` - Visualization
- `DT` - Interactive tables

## Citation

If you use this pipeline or the raw data  in academic work, please contact me at phillip.lin@duke.edu

## License
apache 2.0
