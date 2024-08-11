import streamlit as st
import pandas as pd
import numpy as np
from PIL import Image

# Load dataset
fashion_data = pd.read_csv('fashion_data.csv')

# Set page title
st.set_page_config(page_title='Fashion and Retail App')

# Add title and subtitle
st.title('Fashion and Retail App')
st.subheader('Explore our fashion and retail data')

# Add image
image = Image.open('fashion_image.jpg')
st.image(image, caption='Fashion and Retail')

# Add sidebar
st.sidebar.title('Filter Data')

# Add filter options
brand = st.sidebar.multiselect('Select Brands', fashion_data['brand'].unique())
category = st.sidebar.multiselect('Select Categories', fashion_data['category'].unique())
price_range = st.sidebar.slider('Select Price Range', min_value=fashion_data['price'].min(), max_value=fashion_data['price'].max())

# Filter data based on user input
filtered_data = fashion_data[(fashion_data['brand'].isin(brand)) & (fashion_data['category'].isin(category)) & (fashion_data['price'] <= price_range)]

# Display filtered data
st.dataframe(filtered_data)

# Add charts and visualizations
st.subheader('Data Visualizations')

# Add bar chart for top selling brands
top_brands = fashion_data['brand'].value_counts().head(10)
st.bar_chart(top_brands)

# Add pie chart for category distribution
category_dist = fashion_data['category'].value_counts()
st.pie(category_dist, labels=category_dist.index)

# Add line chart for price trends
price_trends = fashion_data.groupby('date')['price'].mean()
st.line_chart(price_trends)
