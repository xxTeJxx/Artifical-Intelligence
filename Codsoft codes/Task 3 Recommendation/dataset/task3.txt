import pandas as pd
from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.metrics.pairwise import linear_kernel

link_small = pd.read_csv('/content/links_small.csv')
md = pd.read_csv('/content/movies_metadata.csv', error_bad_lines=False, engine="python")

print(link_small['tmdbId'].isnull().sum())
print(link_small.info())

link_small = link_small[link_small['tmdbId'].notnull()]['tmdbId'].astype('int')

md['id'] = pd.to_numeric(md['id'], errors='coerce', downcast='integer')

smd = md[md['id'].isin(link_small)]

smd['tagline'] = smd['tagline'].fillna('')
smd['overview'] = smd['overview'].fillna('')

smd['description'] = smd['overview'] + smd['tagline']

smd['description'] = smd['description'].fillna('')

tf = TfidfVectorizer(analyzer='word', ngram_range=(1, 2), min_df=0, stop_words='english')

tfidf_matrix = tf.fit_transform(smd['description'])

cosine_sim = linear_kernel(tfidf_matrix, tfidf_matrix)

smd = smd.reset_index()

indices = pd.Series(smd.index, index=smd['title'])

def get_recommendations(title):
    idx = indices[title]
    sim_scores = list(enumerate(cosine_sim[idx]))
    sim_scores = sorted(sim_scores, key=lambda x: x[1], reverse=True)
    sim_scores = sim_scores[1:31]
    movie_indices = [i[0] for i in sim_scores]
    return smd['title'].iloc[movie_indices]

recommended_movies = get_recommendations('The Godfather').head(10)
print(recommended_movies)
