# my pages
 import pandas as pd
import matplotlib.pyplot as plt
import streamlit as st

df = pd.read_csv('c:/ds/homework/movies.csv')
df=df.sample(1000)

plt.style.use('seaborn')
st.title('Netflix popular movies dataset(We get 1000 data points from the Kaggle)')
# create a multi select
genre_filter = st.sidebar.multiselect(
     'Genre Selector',
     df.genre.unique(),  # options
     df.genre.unique())  # defaults

# create a input form
form = st.sidebar.form("certificate")
certificate_filter = form.text_input('The certification (enter ALL to reset)', 'ALL')
form.form_submit_button("Apply")

st.subheader('1.What is the distribution of the certification ?')
df[['certificate']]

st.subheader('The histogram of the certificate')
fig, ax = plt.subplots(figsize=(13, 8))
val =df.certificate.hist(bins=30)
st.pyplot(fig)

st.subheader('2.Based on the 1000 data points,can we use some visualized ways to show the rating(we can easily see the average and the difference value)? ')
df[['rating']]

st.subheader('The bar plot of the rating')
fig, ax = plt.subplots(figsize=(10, 10))
pop_sum = df.groupby('rating')['rating'].sum()
pop_sum.plot.bar(ax=ax)
##df.plot.scatter(x='votes', y='rating')
st.pyplot(fig)

st.subheader('The Boxplot of the rating ')
fig,ax=plt.subplots(figsize=(20,10))
df.rating.plot.box(ax=ax)
st.pyplot(fig)

 
