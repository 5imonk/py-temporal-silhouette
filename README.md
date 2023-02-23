## Temporal Silhouette for Stream Clustering Validation
### Instructions for experiment replication

**author:** Félix Iglesias

**contact:** felix.iglesias@tuwien.ac.at


The TS index and the experiments here implemented are from the paper:

*Temporal Silhouette: Validation of Stream Clustering Robust to Concept Drift* (under review...)

*Feb., 2023*

If you use Temporal Silhouette and/or the experiments described here, please cite the repository and the paper (when published).


### 0. Requirements

Experiments require the following general-purpose Python packages:

- numpy
- pandas
- scipy
- sklearn
- glob
- re
- sys
- subprocess
- ntpath

Stream clustering algoriths used are:

- Birch from sklearn
- DenStream from [https://github.com/issamemari/DenStream](https://github.com/issamemari/DenStream)
- CluStream and Streamkm from [https://github.com/giuliano-oliveira/clusopt_core](https://github.com/giuliano-oliveira/clusopt_core)


### 1. Temporal Silhouette Index (TS)

Use TS in your codes by importing the "tempsil" function:

        from TSindex import tempsil

        tempsil(t,x,l,s,kn,c)
        # INPUTS
        # t: 1D-array with timestamps
        # x: 2D-array with data vectors
        # l: 1D-array with labels
        # s: window-size of the simple-moving-average (SMA), default s=200 
        # kn: number-of-neighbors of other clusters for calculating beta, default kn=200
        # c: sigma parameter to weight the penalization over contextual outliers [0...1], default=1
        #
        # OUTPUTS
        # k: 1D-array with cluster-labels
        # ts2: 1D-array with quadratic cluster temporal silhouettes
        # TS: global Temporal Silhuette

For a simple example, open a terminal and run:

        $ python3 toy_tests.py c

where "c" is a real number for the TS sigma parameter. In the paper, Section 4.6 uses this script for the examples, with c=1 and c=0.

### 2. Datasets

**Synthetic datasets** are located within the [dataS] folder. They are also publicly available in **Mendeley**: 

“Data for Evaluation of Stream Data Analysis Algorithms”. Mendeley Data, V1, doi: 10.17632/c43kr4t7h8.1, [https://data.mendeley.com/datasets/c43kr4t7h8/1](https://data.mendeley.com/datasets/c43kr4t7h8/1)

The *clean* (without outliers) versions and the versions with *out-of-phase* outliers are created online during the runing of the experiments

**Real datasets** are located within the [dataR] folder (they are preprocessed versions in arff format). Original data has been obtained from:

- HealthData.gov - COVID-19 Estimated ICU Beds Occupied by State Timeseries: [https://healthdata.gov/dataset/COVID-19-Estimated-ICU-Beds-Occupied-by-State-Time/7ctx-gtb7](https://healthdata.gov/dataset/COVID-19-Estimated-ICU-Beds-Occupied-by-State-Time/7ctx-gtb7)
- kaggle - Retail and Retailers Sales Time Series Collection: 
[https://www.kaggle.com/datasets/census/retail-and-retailers-sales-time-series-collection](https://www.kaggle.com/datasets/census/retail-and-retailers-sales-time-series-collection)


### 3. Replicating experiments

For the synthetic data, open a terminal in the current folder. Run:

        $ python3 run_analysis_synthetic.py

A *results_synthetic.csv* file will be created in the [results/] folder. 

For the real data, open a terminal in the current folder. Run:

        $ python3 run_analysis_real.py

Three files (*covid_labels.csv*, *results_real.csv* and *retail_labels.csv*) will be created in the [results/] folder.

**Warning!** The [results/] folder already contain files with the results published in the paper. Remove, rename or copy these files in a different folder before running the scripts; otherwise, results will be appended in the synthetic case and overwriten in the real case.

### 4. Comparing performances of internal validation indices

For a comparison with the Ground Truth (GT) as benchmark: 

        $ python3 compare_results.py results/results_synthetic.csv gt

For a comparison with the best clustering according to external validation (AMI) as benchmark: 

        $ python3 compare_results.py results/results_synthetic.csv ami

### 5. Plotting peformances for real data cases

To visualize performances of real data cases, run:

        $ python3 plots_retail.py dataR/retail.arff results/retail_labels.csv 

        $ python3 plots_covid.py dataR/covid.arff results/covid_labels.csv 
