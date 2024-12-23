
# House Price Prediction System - Machine Learning with Flask

## Project Overview

This project aims to build a **House Price Prediction System** using machine learning. The system uses a **Ridge Regression** model to predict house prices based on input features such as the number of bedrooms, bathrooms, size, and zip code of a property. The web application is built using **Flask** and provides an interactive user interface where users can enter these features and get an estimated price for a house.

### Key Features:
1. **Web Interface**: A Flask-powered web interface allows users to input property details.
2. **Machine Learning Model**: The system uses a pre-trained **Ridge Regression** model (`RidgeModel.pkl`) to predict house prices based on user input.
3. **Data Handling**: The system reads data from a CSV file (`final_dataset.csv`) and uses it for prediction and categorization.
4. **Real-Time Predictions**: The system provides real-time house price predictions based on user input.
5. **Data Validation**: It includes validation for input data to handle missing or unexpected values.

### Technologies Used:
- **Flask**: A lightweight Python web framework to build the web application.
- **pandas**: A powerful library for data manipulation and analysis.
- **pickle**: A Python module for serializing and deserializing objects, used to load the pre-trained machine learning model.
- **Machine Learning Model**: **Ridge Regression** (serialized in `RidgeModel.pkl`) to predict house prices.

## Prerequisites

Before running this project, ensure that you have the following installed on your system:

- **Python 3.x**: The project is developed using Python 3.
- **Required Libraries**: You can install all required libraries using the `requirements.txt` file or manually using pip.

### Install the required libraries:
Create a virtual environment (recommended) and install the dependencies:

```bash
pip install -r requirements.txt
```

### `requirements.txt`

```txt
Flask==2.2.2
pandas==1.5.3
scikit-learn==1.1.2
```


### **Explanation of key files**:
- **`app.py`**: This is the main Flask application that handles web routing, data processing, and prediction.
- **`final_dataset.csv`**: This dataset contains historical house prices along with other features (e.g., bedrooms, bathrooms, size, etc.), which are used for prediction.
- **`RidgeModel.pkl`**: This file contains the pre-trained Ridge Regression model that is used to predict the house prices.
- **`templates/index.html`**: The HTML page where users can input data to get the predicted house price.
- **`static/`**: Folder for storing static files (such as CSS files, JavaScript, or images).

## How to Run the Project

### Step 1: Clone the Repository

First, clone the repository to your local machine:

```bash
git clone https://github.com/your-username/House-price-prediction-system-MACHINE-LEARNING.git
cd House-price-prediction-system-MACHINE-LEARNING
```

### Step 2: Install the Dependencies

Make sure to install all required libraries by running:

```bash
pip install -r requirements.txt
```

### Step 3: Start the Flask Application

To run the Flask web application, execute the following command:

```bash
python app.py
```

The application will start running on `http://127.0.0.1:5000/` by default. Open this URL in your browser to interact with the application.

### Step 4: Interact with the Web Application

1. **Input Features**: On the home page, you will see form fields where you can enter values for:
   - **Bedrooms (beds)**
   - **Bathrooms (baths)**
   - **Size (size in sq. ft.)**
   - **Zip code (zip_code)**

2. **Get Prediction**: After entering the details, hit the **"Predict"** button to get the predicted house price.

3. The system will display the predicted price based on your input.

### Example Workflow:
1. The user selects the following options:
   - Bedrooms: `3`
   - Bathrooms: `2`
   - Size: `1500`
   - Zip code: `90210`
2. The application makes a prediction based on the Ridge Regression model, and the result might look something like: **$850,000**.

## Explanation of the Code

### `app.py` - Flask Application

The main Flask application consists of two routes:

1. **`/` (index route)**:
   - This route renders the homepage, which includes the form where users input the details for house price prediction.
   - It reads the possible values for bedrooms, bathrooms, size, and zip code from the dataset (`final_dataset.csv`) and passes them to the template.

2. **`/predict` (predict route)**:
   - This route handles the form submission. It reads the form data from the user, converts it into a DataFrame, and passes it through the pre-trained model (`RidgeModel.pkl`) for prediction.
   - If unknown categories (like unrecognized zip codes or sizes) are input, the application replaces them with the most frequent category in the dataset.
   - After prediction, the price is returned as a response to the user.

### Key Code Snippets:
- **Data Preprocessing**: Handles conversion of input data to the correct types and handles unknown categories.
  
  ```python
  # Handle unknown categories in the input data
  for column in input_data.columns:
      unknown_categories = set(input_data[column]) - set(data[column].unique())
      if unknown_categories:
          input_data[column] = input_data[column].replace(unknown_categories, data[column].mode()[0])
  ```

- **Model Prediction**:
  The pre-trained Ridge Regression model is loaded from the pickle file and used to predict the house price.

  ```python
  prediction = pipe.predict(input_data)[0]
  ```

### `index.html` - User Interface

The HTML file (`index.html`) contains the form for users to input the property details. It is a simple interface with dropdowns for selecting the number of bedrooms, bathrooms, etc., and a submit button to get the predicted price.

Example of the form (simplified):

```html
<form method="POST" action="/predict">
  <label for="beds">Bedrooms:</label>
  <select name="beds" id="beds">
    <option value="1">1</option>
    <option value="2">2</option>
    <option value="3">3</option>
    <!-- Add more options -->
  </select>

  <label for="baths">Bathrooms:</label>
  <select name="baths" id="baths">
    <option value="1">1</option>
    <option value="2">2</option>
    <option value="3">3</option>
    <!-- Add more options -->
  </select>

  <label for="size">Size (sq ft):</label>
  <input type="text" name="size" id="size">

  <label for="zip_code">Zip Code:</label>
  <input type="text" name="zip_code" id="zip_code">

  <button type="submit">Predict</button>
</form>
```

## Model Performance

The prediction is made using a **Ridge Regression** model, which was trained on historical data. The model is saved as a pickle file (`RidgeModel.pkl`). The performance of the model can be evaluated based on its **RMSE** and **R²** metrics, which were determined during model training.

## Conclusion

This project demonstrates the integration of a machine learning model (Ridge Regression) into a web application using **Flask**. The application allows users to easily predict house prices by inputting relevant features and receiving real-time predictions.
