# Cloud-Enabled Crop Health Classification
This project works on predicting crop health using satellite imagery and cloud-based services like Google Cloud Storage (GCS) and Google Earth Engine (GEE). It calculates key vegetation indices (e.g., NDVI, EVI, NDWI, SAVI..etc) through satellite images for crop health analysis, then uses a weighted hybrid CNN-MLP model for classification to help farmers take suitable actions.

### Options to Run the Project

You can run the project in two ways:
1. **Use the Ready Data Provided (Images and data.csv):** Use the satellite images and the tabular dataset along with the pre-calculated indices that are available within this repo, we downloaded the images from the cloud and calculated their indices for quick runs.
2. **Use "Farm Boundaries Info at Subdistrict Level in Telangana State.csv":** This method is *not* recommended as it could take several hours. It lets you download the images from Google Earth Engine by yourself, and the indices we need for predictions will be calculated on-the-fly as the images are downloaded in parallel. Which is what we had to do the first time we wrote the code.

### Setup Instructions

To use Google Cloud Storage and Google Earth Engine for this project, follow these steps:

1. **Create a Google Cloud Project:**
   - Create a new project on [Google Cloud Console](https://console.cloud.google.com/).
   - Use your project ID in the code where applicable to access cloud resources.
   
2. **Enable Google Earth Engine API:**
   - Go to the [Google Earth Engine API page](https://developers.google.com/earth-engine) and ensure the API is enabled for       your Google Cloud project, or you should be able to enable it via Google Cloud Console too.

3. **Install Dependencies:**
   - Install necessary resources at the start of the code:
     ```
     pip install earthengine-api geemap rasterio geopandas tqdm scikit-image rasterstats
     pip install google-cloud-storage
     pip install tifffile
     ```

4. **Authentication and Credentials:**
   - Authenticate your Google Cloud and Earth Engine access using the provided credentials.
   - running the ee.initialize() cell will ask you to enter the key in a link they provide.
   - You will need your own credential/key for your Service Account which we will explain next.
     
5. **Download and Prepare Data:**
  
   1. **If using the ready data** >> ("data.csv" and attached satellite Images)
       - Explore the code cells that handle geospatial as they were very challenging and interesting, the additional                  features (Vegetation Indices, eg ndvi) in your dataset "data.csv" were calculated by running those cells previously.
       - You can run the snippets that upload the images to Google Cloud Storage, or similarly on Firestore.
       - To store the images on GCS you have to create a *Service Account* for your cloud Project.
       - Make sure the Service Account has the correct roles:
             a. Storage Admin (for creating buckets and uploading files)
             b. Storage Object Admin (for managing objects in GCS)
         If an error occurs, check your permissions, service account of GCS must be in the same project as Earth Engine.
       - A separate .ipynb for uploading the images on GCS will be provided, it can be run separately but the same code is   
         integrated in the main .ipynb
   2. **Downloading the images from the Cloud Earth Engine all over again** >> just run the main notebook from the start
       and make sure to substitue the credentials with your own, the images will be downloaded locally then uploaded to             Google Cloud Storage (for billing reasons).
    

### Model Overview
The project employs a **hybrid model** combining **Multi-Layer Perceptron (MLP)** for tabular data and **Convolutional Neural Network (CNN)**, specifically, **efficientNetB0**  to classify crop health based on the vegetation indices calculated from satellite imagery.

### Additional Notes

- This project is designed to be scalable by utilizing cloud infrastructure, allowing large number of images to be stored on a cloud and be processed easily.
- The vegetation indices (e.g., NDVI, EVI, etc.) are used to analyze crop health and make predictions.
- If you decided to download the images from the Cloud Earth Engine (which could take several hours), make sure to set the     "download" flag to "True" in the code.
- Do not forget to change the paths of the files according to your enviroment.
