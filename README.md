# Project-Gutenberg
_Analysis based on the result:_

The analysis of book popularity based on Kullback-Leibler (KL) divergence characteristics reveals interesting insights. Metrics like Average, Slope, Standard Deviation, Maximum and Number of books with kld higher than top 10% were considered and built for each book. These metrics were then regressed against log(downloads). The average KL divergence (avg_kld) had a positive coefficient, meaning that books with a higher average information reveal typically receive more downloads. This shows that readers enjoy a steady stream of new material. Books with different degrees of information revealed throughout sections are also more popular, according to the positive coefficient for the standard deviation of KL divergence (std_kld), which suggests that readers like the unpredictability and involvement that such variety delivers. However, novels with an excessive information reveal in a single part may be less popular, probably because it disturbs the reading flow, as indicated by the negative coefficient for maximum KL divergence (max_kld). Its interpretation when used alone may be complicated by the slope of KL divergence's (slope_kld) extremely negative coefficient, which may indicate multicollinearity with other KL measurements.


LASSO regression was used to identify the most important predictors of book downloads, highlighting avg_kld and max_kld as significant features. This underscores the importance of both consistent information revelation and notable information reveal spikes for book popularity. This however contradicts the previous results with the negative coefficient of regression with max_kld. An interesting point to note, the interaction terms involving max_kld with various genres suggest that significant moments of information introduction are important across genres like biography, history, and fiction. The analysis also points to genre-specific trends, such as the importance of consistent information flow in travel books. These results offer an advanced knowledge of the ways that genre and information dynamics interact to affect reader engagement and book popularity.



**Detailed Analysis**

_Regression of KLD characteristics with log(downloads)_

**avg_kld:** Coefficient: 0.34304126 (positive)
This positive coefficient suggests that books with a higher average KL divergence tend to have more downloads, on average. This implies that readers might prefer books with a consistent flow of new information revealed throughout the sections

**std_kld:** Coefficient: 2.62231218 (positive)
This positive coefficient suggests that books with a higher standard deviation of KL divergence tend to be more popular. This indicates that readers might enjoy books that alternate between sections with high and low information reveal, keeping them engaged and surprised.

**max_kld:** Coefficient: -0.47832542 (negative)
This negative coefficient suggests that books with a very high maximum KL divergence (extreme information reveal in one section) might be slightly less popular on average. This could be because such a drastic information shift might disrupt the reading flow for some readers.

**slope_kld:** Coefficient: -31.48056701 (negative)
The slope of KL divergence might be highly correlated with other KLD measures (e.g., average KL divergence) in the data.


_LASSO shrinkage of KLD measures_

**Without Genre:**
Important Features: 

LASSO identified *avg_kld* (average information reveal) and *max_kld* (maximum information reveal in a section) as the most important features for predicting log(downloads). This suggests that overall information flow and moments of significant new information introduction might be important for book popularity, regardless of genre.

**Specific Genre Insights:**

Max_kld Dominance:
Interestingly, most significant features involve *max_kld* interacting with various genres. This suggests that for many genres (e.g., biography, history, fiction), moments of significant information introduction might be particularly important for downloads.

Genre-Specific Patterns: 
Some specific interaction terms with "avg_kld" might be worth investigating further.
avg_kld - description, travel - This could indicate that a consistent flow of new information is important for travel books with descriptions.
avg_kld - fiction variations - These might reveal how information reveal patterns differ across fiction subgenres (juvenile, mystery, etc.).


## Building book level measures of characteristics of KL Divergence

Characteristics considered:
Average KL Divergence: Represents the average amount of information revealed per section 

Standard Deviation of KL Divergence: This measures the variability of information revelation across sections

Slope KL Divergence: Refers to the average rate of change in information revelation across different sections of a book.

Maximum KL Divergence: Identifies the section with the highest information revelation, potentially corresponding to a major plot twist or revelation.

Number of Sections with High KL Divergence: Counts how many sections fall above a particular threshold, indicating the frequency of significant information reveals.

Procedure Followed: 
Creating lists for characteristics of KL Divergence 
Appending them to the original data frame after calculation of each metric



## Relating book level KLD measures with log(downloads) by regressing the measures against downloads

Included Language as a control variable, as downloads can also depend on the language the book was written
There were 16 languages, and it was encoded using MultiLabelBinarizer
Languages: 'ang', 'cy', 'de', 'el', 'en', 'enm', 'eo', 'es', 'fr', 'grc', 'kha','la', 'myn', 'pt', 'tl', 'zh'


## Using LASSO to predict the most important variables that has significant effect on book downloads
Genres were stored together in the column “subjects” 

Did extensive data processing to retrieve top 50 genres used
   Identifying and filtering out stop words
   Tokenizing “subject” columns into genres
Appending these genres for specific books into a new column in the dataframe: merged[‘genres’]
All these genres are then encoded using get_dummies()

Using LASSO models only on KLD characteristics:
Important Features according to LASSO: ['avg_kld', 'max_kld']

Using LASSO for KLD measures along with the genres:
Important Features according to LASSO: ['max_kld', 'avg_kld*english', 'avg_kld*fiction', 'avg_kld*fiction, juvenile', 'avg_kld*periodicals', 'avg_kld*travel, description', 'max_kld*Other', 'max_kld*america,…………..]
