# Zooniverse Glitch Classification
This document provides an overview of the main functionalities and functions included in each Python file created for managing projects, subject sets, and data exports in Panoptes.

## Installation 

Make sure you have installed python3 by checking
`$ python --version && pip --version`

Then run the following to install zooniverse Python client
`$ pip install panoptescli`

## Project Structure 
## 1. Project Initializer

### Main Functionality:

Create a new instance of a Zooniverse project and upload an initial subject set to the project.

## 2. Data Export Generator

### Main Functionality:

Contains functions for requesting and generating exports for classifications and subjects.

## 3. Data Export Downloader

### Main Functionality:

Facilitates downloading of classification and subject export files from a specified Panoptes project.

## 4. Duplicate Removal

### Main Functionality:

Removes duplicate subjects from a subject set based on a specified metadata field.

## 5. Subject Set Metadata Update

### Main Functionality:

Updates the metadata for subjects in a subject set based on information from a CSV file.

## 6. active learning pipline

### Main Functionality:
Where we put everything together with a  `main` function orchestrating a series of operations that integrate local active learning processes with the Zooniverse platform to enable collaborative data labeling. It manages the entire workflow from data loading, active learning, image generation, to uploading data for public labeling.

### Main Function Logic:

### Initialization and Configuration
- **Logging Setup**: Configures logging to capture important events and errors throughout the execution of the pipeline.
- **Authentication**: Establishes a connection to the Zooniverse platform using environment-stored credentials. This step is critical to ensure that subsequent operations interacting with Zooniverse APIs are authenticated.

### Workflow Status Check
- **Workflow Retrieval**: Retrieves a specific workflow from Zooniverse to check the current labeling status.
- **Condition Check**: Determines if all data from a previous batch has been labeled (based on a count comparison). If not, it exits and logs that active subjects are still being labeled.

### Active Learning Execution
- **Subject Set Creation**: If the workflow is ready for new data (this includes both the case of first-time running the active learning or when all images from the previous batch have been labeled), the function prompts the user to input a name for a new subject set and creates it on Zooniverse.
- **Active Learning Process**:
  - **Data Loading**: Loads data sets specified by you.
  - **Active Learner Initialization and Querying**: Initializes the active learner and performs a series of queries to select instances for labeling, based on uncertainty and high probability.
  - **Image Generation**: Generates images from the selected data points, which are required for visual inspection and labeling on Zooniverse.
  - **Data Saving**: Compiles the labeled and unlabeled data, along with generated images, into a manifest file.
  
### Data Upload to Zooniverse
- **Manifest File Upload**: Uploads the manifest file, which contains metadata about the made images from previous step, to the newly created subject set on Zooniverse. This allows participants to access and label the data using Zooniverse's UI.
- **Upload Verification**: Checks the outcome of the upload process. Logs success or failure, providing feedback on the operation's result.

### Note on Data Export from Zooniverse
- Due to the limitations of the platform's current implementation, the best way to export data is to use Zooniverse's UI. You can navigate to your project builder, click on the Data Exports option, which will help you generate an export of all classifications made on the specific dataset.
  
### Summary
This notebook serves as the central coordinator for running the active learning pipeline, handling everything from setup, data processing, to integration with external platforms. It ensures that the data labeling process is seamless and efficient, leveraging both automated machine learning techniques and human input through the Zooniverse platform.