# Content Based Recommender System

Content Based Recommender System learns to recommend items that are similar to the ones that the user liked in the past. 
In contrast, collaborative filtering recommendation identifies users whose preferences are similar to those of the given user and recommends items they have liked.

## 1.1	Dataset

The datasets used are from Kaggle, https://www.kaggle.com/kandij/job-recommendation-datasets/.
Upon examining the datasets, an assumption is made that the datasets originated from an online job recruiting portal based in United States. The datasets consist of details on the jobs availability and the applicants’ profiles and activity log in the portal for the period 2013 – 2015.

## 1.2	Job Profile

The text column of job profile is created by joining the columns Slug, Title, Position, Company, City, Employment.Type, Job.Description and Education.Required of the Combined_Jobs_Final.csv.
These fields are selected as these are the attributes that applicants look out for. For example, an applicant may prefer to consider part time employment due to family commitments or prefer a role that is located near his/her residence. 
Once the columns are combined, data pre-processing is done on the text column such as removal of unnecessary characters, punctuation and stopwords, separation of each word with space, case lowering and lemmatisation are performed. This text column is subsequently joined with the Job.ID resulting in a dataset with 2 columns; Job.ID and text. 

## 1.3	User Profile

To build a user profile,  applicants’ background, experience, indication of positions and interest are used. This information, which are defined explicitly by the applicant in the online recruiting platforms, are captured in Experience.csv and Positions_Of_Interest.csv. 
The activity logs of the applicant such as job views are used as it provides information on the type and nature of jobs that the applicant is interested in, which is not captured by the explicit feedback previously.

## 1.4	Similarity metrics 

Based on job views dataset, only active users with more than 5 job views are used to build the user profile.
The ‘Profile’ column is then vectorised using tf-idf weighting resulting in a sparse matrix stored in user_tfidf. 
Cosine Similarity is chosen out of the other similarity methods to create the top 15 recommended jobs for the active users as the value of cosine will increase with decreasing value of the angle which signifies more similarity.

## 1.5	Evaluation Methodology

In order to create a user profile, there must be sufficient interactions with the online job platform when building a content-based recommender system. Otherwise, the recommendations might not be accurate in depicting an applicant’s interest in the job.

Hence, based on the number of job views, users with at least 5 page view counts are selected to build the user profile. These users will be termed as active users and their viewed items (Job.ID) are split into train set (70%) and test set (30%).

The evaluation techniques used here were adopted from the techniques used in Recommendation Systems (i.e. hits@K, recall@K, precision@K) Based on the Job_Views dataset, active users and their viewed items (Job.ID) are split into train set (70%) and test set (30%). The train set is input into the Content-Based Recommender Algorithm to find top N job recommendations for each applicant. These job recommendations are exclusive of the jobs which already appeared in the train set of each applicant. They are derived by computing the cosine similarity for each applicant’s tfidf profile in user_tfidf, and each Job’s tfidf profile in job_tfidf, and sorted according to magnitude to form the Top 5, Top 10, Top 15 and Top 20 job recommendations for each applicant.

These TopN recommendations are then matched against Job.ID viewed by each applicant in the test set. We find the number of matches for the Top 5, 10, 15 and 20 recommendations.

## 1.6 Performance of model

The performance of the model is impacted by the number of job views per user in the train set. With a larger view count, we can get a better user profile. For this exercise, active users with at least 5 page view counts are used and their viewing profile is split into train and test set. This could have resulted in the training sample per user to be further reduced for user profiling. A higher view count threshold can be considered to improve the user profiling for every active user.

### Strengths

As content-based recommender system is independent of user ratings, it is able to recommend new jobs which had not been viewed or liked by other applicants and yet similar to the ones that the applicant has viewed before. As long as the job’s profile matches the applicant’s profile, the new job available for application can be recommended to applicants.

### Limitations

While content-based recommender system allows for explanation to users on why such recommendations are made, it is hard to recommend jobs that are not similar to those that are already seen by the user. Applicants like the concept of serendipity where jobs of differing context can be recommended to them and at the same time, the jobs are still of relevance to them. This can be solved by introducing randomness into the dataset. Applicants also tend to be more satisfied with recommendations of higher diversity i.e. jobs in different industries to give them more job options.

Content-based recommender system is also dependent on user profile. There must be enough interactions to form a user profile. Else, the recommendations might not be accurate in depicting an applicant’s interest in the job.

Semantics can also be brought into the recommendation using knowledge resources such as taxonomy and lexicon to improve the accuracy of the model.
