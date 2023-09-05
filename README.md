# Diabetic Retinopathy Detection System

## Overview

This repository contains the setup and deployment instructions for the Diabetic Retinopathy Detection System, which helps in the early diagnosis and classification of diabetic retinopathy in retinal images. Below are the setup and deployment steps for different components of the system.

## Setup for Python

### Install Python

Follow the provided setup instructions to install Python on your system.

### Install Python Packages

Install the required Python packages for the project:

```shell
pip3 install -r training/requirements.txt
pip3 install -r api/requirements.txt
```

### Install Tensorflow Serving

Follow the provided setup instructions to install Tensorflow Serving.

## Setup for ReactJS

### Install Nodejs and NPM

Follow the provided setup instructions to install Nodejs and NPM on your system.

### Install Dependencies

Navigate to the frontend directory and install project dependencies:

```shell
cd frontend
npm install --from-lock-json
npm audit fix
```

### Configure Environment Variables

Copy the `.env.example` file as `.env` and update the API URL in the `.env` file.

## Setup for React-Native App

1. Set up the React Native environment as per the provided instructions in the React Native CLI Quickstart tab.

2. Install project dependencies:

```shell
cd mobile-app
yarn install
```

### For Mac Users

If you are using a Mac, also run:

```shell
cd ios && pod install && cd ../
```

### Configure Environment Variables

Copy the `.env.example` file as `.env` and update the API URL in the `.env` file.

## Training the Model

1. Download the data from Kaggle.

2. Keep only the folders related to potatoes.

3. Run Jupyter Notebook:

```shell
jupyter notebook
```

4. Open `training/potato-disease-training.ipynb` in Jupyter Notebook.

5. In cell #2, update the path to the dataset.

6. Run all the cells one by one.

7. Copy the generated model and save it with the version number in the `models` folder.

## Running the API

### Using FastAPI

1. Navigate to the `api` folder.

2. Run the FastAPI server using uvicorn:

```shell
uvicorn main:app --reload --host 0.0.0.0
```

The API is now running at `0.0.0.0:8000`.

### Using FastAPI & TF Serve

1. Navigate to the `api` folder.

2. Copy the `models.config.example` as `models.config` and update the paths in the file.

3. Run the TensorFlow Serving container (update config file path):

```shell
docker run -t --rm -p 8501:8501 -v /path/to/potato-disease-classification:/potato-disease-classification tensorflow/serving --rest_api_port=8501 --model_config_file=/potato-disease-classification/models.config
```

4. Run the FastAPI server using uvicorn (you can run it directly from your `main.py` or `main-tf-serving.py` using PyCharm run option or from the command prompt):

```shell
uvicorn main-tf-serving:app --reload --host 0.0.0.0
```

The API is now running at `0.0.0.0:8000`.

## Running the Frontend

1. Navigate to the `frontend` folder.

2. Copy the `.env.example` as `.env` and update `REACT_APP_API_URL` to the API URL if needed.

3. Run the frontend:

```shell
npm run start
```

## Running the App

1. Navigate to the `mobile-app` folder.

2. Copy the `.env.example` as `.env` and update the URL to the API URL if needed.

3. Run the app for Android or iOS:

```shell
npm run android
```

or

```shell
npm run ios
```

## Creating Public (Signed APK)

### Creating the TF Lite Model

1. Run Jupyter Notebook:

```shell
jupyter notebook
```

2. Open `training/tf-lite-converter.ipynb` in Jupyter Notebook.

3. In cell #2, update the path to the dataset.

4. Run all the cells one by one.

5. The model will be saved in the `tf-lite-models` folder.

### Deploying the TF Lite on GCP

1. Create a GCP account.

2. Create a Project on GCP (keep note of the project id).

3. Create a GCP bucket.

4. Upload the `potatoes.h5` model to the bucket in the path `models/potatos.h5`.

5. Install Google Cloud SDK (follow the provided setup instructions).

6. Authenticate with Google Cloud SDK:

```shell
gcloud auth login
```

7. Run the deployment script:

```shell
cd gcp
gcloud functions deploy predict_lite --runtime python38 --trigger-http --memory 512 --project project_id
```

Your model is now deployed. Use Postman to test the Google Cloud Function (GCF) using the Trigger URL.

### Deploying the TF Model (.h5) on GCP

1. Create a GCP account.

2. Create a Project on GCP (keep note of the project id).

3. Create a GCP bucket.

4. Upload the TensorFlow `.h5` model generated to the bucket in the path `models/potato-model.h5`.

5. Install Google Cloud SDK (follow the provided setup instructions).

6. Authenticate with Google Cloud SDK:

```shell
gcloud auth login
```

7. Run the deployment script:

```shell
cd gcp
gcloud functions deploy predict --runtime python38 --trigger-http --memory 512 --project project_id
```

Your model is now deployed. Use Postman to test the GCF using the Trigger URL.

For more information and inspiration on deploying models on Google Cloud, refer to the provided links in the original document.# Diabetic_Retinopathy_Detection_System