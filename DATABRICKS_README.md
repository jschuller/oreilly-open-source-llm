# Running these notebooks on Azure Databricks

This guide provides instructions for setting up an Azure Databricks environment to run the example notebooks in this repository.

## 1. Choosing a Databricks Runtime

The notebooks in this repository have several dependencies on libraries that require a specific hardware and software environment. Based on the `requirements.txt` file, the following is recommended:

*   **Databricks Runtime**: `15.3 LTS for Machine Learning`
*   **Instance type**: A GPU-enabled instance (e.g., `Standard_NC4as_T4_v3` on Azure)
*   **CUDA Version**: The `15.3 ML` runtime comes with CUDA 12.1, which is compatible with the project's dependencies.

### Python Version Mismatch

**Important**: The `pyproject.toml` file in this repository specifies a requirement for `Python >= 3.13`. However, the recommended Databricks Runtime `15.3 ML` comes with **Python 3.11**.

It is possible that the code will run without modification on Python 3.11. You should proceed with the setup and see if you encounter any Python-related errors. If you do, you may need to:
a) Find a newer Databricks runtime that supports Python 3.13 or newer (these may be in Public Preview).
b) Adjust the code in the notebooks to be compatible with Python 3.11.

## 2. Uploading the Repository and Data

Before you can run the notebooks, you need to upload the contents of this repository to your Databricks workspace.

1.  **Upload the repository**: Use the "Repos" feature in Databricks to clone this repository into your workspace.
2.  **Upload the data**: The notebooks in `Day 1` depend on the data in the `un/` directory. You need to upload this directory to a location accessible from your Databricks cluster. The recommended approach is to upload it to the Databricks File System (DBFS). You can use the Databricks UI or the [Databricks CLI](https://docs.databricks.com/en/dev-tools/cli/index.html) to do this. For example, you could upload the `un/` directory to `/dbfs/data/un/`.

## 3. Installing Libraries

The required Python libraries are listed in `requirements.txt`. You can install them on your Databricks cluster.

1.  Navigate to your cluster's configuration page.
2.  Go to the "Libraries" tab.
3.  Click "Install new".
4.  Choose "PyPI" and enter the package names and versions from `requirements.txt`.

Alternatively, you can install the libraries using `%pip` magic commands at the beginning of your notebooks. For example:

```python
%pip install -r requirements.txt
```

Some notebooks also download models on the fly (e.g., from `spacy` or `Hugging Face`). Ensure that your cluster has internet access to allow these downloads.

## 4. Modifying the Notebooks

The notebooks in this repository use relative paths to access data files. You will need to modify them to point to the location where you uploaded the data on DBFS. The notebooks will be modified to include a setup cell at the top where you can specify the data path.

For example, if you uploaded the `un/` directory to `/dbfs/data/un/`, you would set the `data_path` variable in the notebook to `"/dbfs/data/"`.

This `DATABRICKS_README.md` file is the first step in making these notebooks easier to run on Azure Databricks. The next step in the plan is to modify the notebooks themselves.
