# Mobile Device Attribute Anomaly Detection

## Problem Statement

The nature of fraud is dynamic and ever-changing. Finding patterns and identifying anomalies are essential in this industry. Given a set of mobile device attributes (for example, brand, model) data, design a model to find patterns or anomalies in these data. Take into account that not all device attributes are readily available all the time and there is no historical data available.

The goal of this project is to design a model that can identify patterns and anomalies in mobile device attribute data (e.g., brand, model, RAM, processor) to aid in fraud detection. The model needs to handle challenges such as the dynamic nature of fraud, incomplete data (missing attributes), and the lack of historical fraud data for training.

## Dataset

The analysis uses a dataset containing attributes of various smartphones (`smartphones - smartphones.csv`). The dataset includes features like:
* model
* price
* rating
* sim (details like 3G/4G/5G, VoLTE, Wi-Fi, NFC)
* processor (name, cores, clock speed)
* ram (RAM size, inbuilt storage)
* battery (capacity, fast charging details)
* display (size, resolution, refresh rate, notch type)
* camera (megapixels for rear and front cameras)
* card (memory card support)
* os (operating system version)

## Methodology

1.  **Data Collection:**
    * Loaded the smartphone attribute data from a CSV file.

2.  **Data Preparation & Feature Engineering:**
    * Handled missing values using various imputation techniques (KNN, Median, Mode, Rule-based) based on feature characteristics.
    * Cleaned and transformed raw features into usable formats:
        * **Price:** Extracted numerical value from string.
        * **SIM:** Extracted binary features for network types (3G, 4G, 5G), VoLTE, NFC, Wi-Fi, IR Blaster, and SIM type (Dual/Single).
        * **Processor:** Extracted Processor Type (e.g., Snapdragon, Helio, Dimensity) and Core Type (Octa, Hexa, Quad). Brands with low frequency were grouped into "Other".
        * **RAM:** Extracted RAM size and Inbuilt Memory size. Categorized these into bins (e.g., <1GB, 1-8GB, 9-16GB for RAM; <1GB, 1-48GB, 49-128GB etc. for storage).
        * **Battery:** Extracted battery capacity (mAh) and charging wattage (W). Categorized these into bins (e.g., <2000mAh, 2001-4000mAh; <25W, 25-50W etc.).
        * **Display:** Extracted display size (Inches), Resolution Width, Resolution Height, Refresh Rate (Hz), and Notch Type. Refresh rates were categorized.
        * **Card:** Categorized memory card support into "Supported," "Not Supported," or "Hybrid".
        * **OS:** Extracted the base OS (Android, iOS, Other OS). Brands with low frequency were grouped into "Other OS".
        * **Model:** Extracted the Brand name. Brands with low frequency were grouped into "Other".
    * Applied One-Hot Encoding to categorical features.
    * Applied appropriate scaling/transformations (RobustScaler, MinMaxScaler, Yeo-Johnson) to numerical features based on their distribution analysis.

3.  **Modeling:**
    * Used the **Isolation Forest** algorithm, an unsupervised learning technique suitable for anomaly detection, especially with high-dimensional data and without historical labels. A contamination factor of 5% was used.

4.  **Anomaly Interpretation:**
    * Applied **SHAP (SHapley Additive exPlanations)** to understand which features contribute most to a data point being classified as an anomaly.

## Key Findings (Based on SHAP Analysis)

* **Features increasing anomaly score (more likely anomalous):** High front camera MP (17-32MP), presence of IR Blaster, high charging wattage (51-100W), Helio processors, certain brands (OPPO, Samsung, Realme), specific notch types, lack of memory card support.
* **Features decreasing anomaly score (more likely normal):** Octa-core processors, common inbuilt storage (49-128GB), memory card support.

## How to Run

1.  Ensure you have Python and necessary libraries installed (pandas, numpy, scikit-learn, seaborn, matplotlib, shap).
2.  Place the `smartphones - smartphones.csv` file in the correct directory as specified in the notebook.
3.  Run the Jupyter Notebook `solution.ipynb` cell by cell.

## Next Steps

* Experiment with other anomaly detection algorithms like Local Outlier Factor (LOF), One-Class SVM, DBSCAN, KMeans, or Autoencoders.
* Tune hyperparameters for the chosen models.
* Compare the performance of different models.