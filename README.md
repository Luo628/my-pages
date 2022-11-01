import pandas as pd
import matplotlib.pyplot as plt
import streamlit as st

df = pd.read_csv('c:/ds/homework/movies.csv')

#delete the 'min' and ','
df=df.rename(columns={'duration':'duration_in_min','stars':"casts_of_movies"})
df.dropna(inplace=True)
df['duration_in_min']=df['duration_in_min'].apply(lambda x:int(x[:-4]))
df['votes']=df['votes'].apply(lambda x :int(''.join([i for i in x if i!=','])))

#title
plt.style.use('seaborn')
st.title('Netflix popular movies dataset')

#select
certificates = df['certificate'].unique().tolist()
certificates_selection = st.multiselect('certificate:',\
                                        certificates,\
                                        default = certificates)
#slider
rates = df['rating'].unique().tolist()
rates_selection = st.slider('rating:',min_value = min(rates), \
                             max_value = max(rates), \
                            value = (min(rates),max(rates)))

#filter
mask = (df['rating'].between(*rates_selection)) &\
       (df['certificate'].isin(certificates_selection))
number_of_result = df[mask].shape[0]

#get the valid data
st.markdown(f'*Valid Data: {number_of_result}*')

#groupby and count the data base on the select
df_grouped = df[mask].groupby(by=['certificate']).count()[['rating']]
df_grouped = df_grouped.rename(columns={'rating': 'numbers'})
df_grouped = df_grouped.reset_index()

#subheader1
st.subheader('1.How many movies every certificate in this rating Range?')

#the picture which controled by filter and slider
st.bar_chart(df_grouped, x = 'certificate', y = 'numbers')

#subheader2
st.subheader('2.Will certificate influence their votes?')

#picture about certificate and votes
df1 = df.loc[:, ['certificate', 'votes']]
st.line_chart(df1.groupby('certificate').max())
st.line_chart(df1.groupby('certificate').mean())

#subheader3
st.subheader('3.What is the distribution of the certification ?')

#picture about the number of certificate
st.subheader('The histogram of the certificate')
fig, ax = plt.subplots(figsize=(13, 8))
val =df.certificate.hist(bins=30)
st.pyplot(fig)

#subheader4
st.subheader('4.Can we use some visualized ways to show the rating(we can easily see the average and the difference value)? ')

#picture about rating and quantity
st.subheader('The bar plot of the rating')
fig, ax = plt.subplots(figsize=(10, 10))
pop_sum = df.groupby('rating')['rating'].sum()
pop_sum.plot.bar(ax=ax)
st.pyplot(fig)

st.subheader('The Boxplot of the rating ')
fig,ax=plt.subplots(figsize=(20,10))
df.rating.plot.box(ax=ax)
st.pyplot(fig)

 
