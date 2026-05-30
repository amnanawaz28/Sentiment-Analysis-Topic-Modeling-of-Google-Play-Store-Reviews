Sentiment Analysis & Topic Modeling of Google Play Store Reviews
An end-to-end NLP pipeline applied to 1.8 million real-world user reviews from five major cloud storage applications
🧾 Project Information
FieldDetails📅 SubmittedJune 2026🎓 ProgramBS Business Data Analytics🏫 InstitutionCOMSATS University Islamabad, Lahore Campus📚 CourseMachine Learning / Natural Language Processing🗂️ SectionA — Department of Management Sciences

👥 Team Members
NameStudent IDGitHubLinkedInMuhammad Salman SaleemFA24-BBD-101@zzalmanLinkedInMehak FatimaFA24-BBD-049——Amna Nawaz GondalFA24-BBD-015@amnanawaz28LinkedInMuhammad Ahsan ShahzadFA24-BBD-099@Ahsanjutt099LinkedIn

📌 Abstract
This project presents a comprehensive Natural Language Processing (NLP) analysis of user reviews collected from the Google Play Store. The dataset — scraped entirely using automated tools — consists of over 1.8 million real-world reviews from five widely-used cloud storage applications.
The study applies two core machine learning techniques:

Sentiment Analysis — Classifying reviews as positive or negative based on star ratings, then analyzing the language patterns within each class
Topic Modeling (LDA) — Applying Latent Dirichlet Allocation to discover hidden themes and recurring discussion patterns buried inside unstructured text

The pipeline transforms raw, messy, user-generated text data into structured, interpretable insights that app developers and product teams can act upon. The final output reveals not only how users feel, but why — identifying which specific features drive satisfaction and which technical problems are destroying user experience.

🎯 Objectives

Collect large-scale real-world review data directly from the Google Play Store
Build a complete text cleaning and preprocessing pipeline for noisy user-generated content
Classify reviews into sentiment categories (positive / negative) using score-based labeling
Apply LDA topic modeling to extract hidden themes from both positive and negative review sets
Visualize topic distributions, keyword weights, and document-level assignments
Translate raw NLP findings into actionable product recommendations


🗃️ Dataset
Applications Analyzed
AppDeveloperReviews CollectedGoogle DriveGoogle LLC~1,280,000Microsoft OneDriveMicrosoft Corporation~323,000DropboxDropbox, Inc.~168,000BoxBox, Inc.~17,000pCloudpCloud AG~15,000

Note: Google Drive dominates the dataset with ~71% of all reviews, reflecting its massive global user base. This distribution mirrors real-world adoption patterns and provides a naturally imbalanced but realistic dataset.

Dataset Summary
MetricValueTotal reviews scraped1,803,014Empty/null reviews removed2,570Clean reviews used for analysis1,800,444LanguageEnglish (US)Data formatStructured CSVCollection methodAutomated scraping via google-play-scraper
Feature Description
ColumnTypeDescriptionapp_nameStringName of the application (e.g., "Google Drive")app_idStringUnique Play Store package identifierreviewsStringFull text of the user-written reviewscoreIntegerStar rating given by user (1 = worst, 5 = best)thumbsUpCountIntegerNumber of other users who found this review helpful

🛠️ Tech Stack
Python (Data Collection & Preprocessing)
LibraryVersionPurposegoogle-play-scraperLatestAutomated bulk review scraping from Play StorepandasLatestData manipulation, cleaning, and CSV handlingnumpyLatestNumerical operations and array processingnltkLatestTokenization, stopword removal, stemming, lemmatizationtqdmLatestProgress tracking during large data operationsreBuilt-inRegular expressions for text cleaninglangdetectLatestNon-English review filtering
R (Topic Modeling & Visualization)
PackagePurposetopicmodelsLDA model fitting and topic extractiontidytextText mining and tidy data operationstidyrData reshaping for tidy formatdplyrData manipulation and summarizationggplot2Topic weight and distribution visualizationwordcloudWord cloud generation per topictmText mining corpus creationreshape2Data transformation for plotting
Environment
ToolPurposeJupyter NotebookPrimary development environmentGoogle ColabCloud execution for large-scale scrapingGoogle DriveDataset storage and team collaboration

🏗️ Pipeline Architecture
┌─────────────────────────────────────────────────────────────────┐
│                        DATA COLLECTION                          │
│  google-play-scraper → 5 apps → 1,803,014 raw reviews (CSV)    │
└─────────────────────────┬───────────────────────────────────────┘
                          ↓
┌─────────────────────────────────────────────────────────────────┐
│                      PREPROCESSING (Python)                     │
│  Lowercase → Remove punctuation/numbers/special chars           │
│  → Filter non-English → Remove stopwords → Tokenize             │
│  → Stem + Lemmatize → Strip whitespace → Clean DataFrame        │
└─────────────────────────┬───────────────────────────────────────┘
                          ↓
┌─────────────────────────────────────────────────────────────────┐
│                   SENTIMENT CLASSIFICATION                      │
│  Score 1–2 → Negative  |  Score 4–5 → Positive                 │
│  Score 3 (neutral) removed for clarity                          │
└──────────┬──────────────────────────────────┬───────────────────┘
           ↓                                  ↓
┌──────────────────────┐           ┌──────────────────────────────┐
│   NEGATIVE REVIEWS   │           │      POSITIVE REVIEWS        │
│   LDA in R           │           │      LDA in R                │
│   6 topics extracted │           │      6 topics extracted      │
└──────────┬───────────┘           └──────────────────┬───────────┘
           ↓                                          ↓
┌─────────────────────────────────────────────────────────────────┐
│                       VISUALIZATION                             │
│  Word clouds · Beta/Theta charts · Topic weight bars            │
│  Document-level assignments · Side-by-side comparisons          │
└─────────────────────────┬───────────────────────────────────────┘
                          ↓
┌─────────────────────────────────────────────────────────────────┐
│                   INSIGHTS & RECOMMENDATIONS                    │
│  High-priority fixes · Strength reinforcement · Strategy        │
└─────────────────────────────────────────────────────────────────┘

🔬 Methodology — Step by Step
Step 1: Data Scraping
Reviews were collected using the reviews_all() function from google-play-scraper, which fetches all available English-language reviews from the US store. A MAX_REVIEWS cap of 5,000,000 was set per app (none reached it), and a 2-second delay was added between apps for stability. Progress was tracked with tqdm.
pythonfrom google_play_scraper import reviews_all

app_reviews = reviews_all(
    app_id,
    sleep_milliseconds=0,
    lang='en',
    country='us'
)
After scraping, empty and null review entries were removed, reducing 1,803,014 total reviews to 1,800,444 clean records.

Step 2: Text Preprocessing
Each review was passed through a multi-stage NLP cleaning pipeline:
StepOperationExample1Lowercase"GREAT App!" → "great app!"2Remove punctuation & numbers"great app!" → "great app"3Remove special characters"can't sync 😤" → "cant sync"4Filter non-English reviewsRemoved using langdetect5Remove stopwords"this is a great app" → "great app"6Tokenize"great app" → ["great", "app"]7Lemmatize"running files" → ["run", "file"]8Stem"crashing" → "crash"9Rejoin & strip whitespace["crash", "upload"] → "crash upload"

Step 3: Sentiment Classification
Star ratings were used as proxy sentiment labels — a common and effective approach for review datasets:
ScoreSentiment LabelReasoning1 ⭐NegativeStrongly dissatisfied2 ⭐NegativeDissatisfied3 ⭐ExcludedAmbiguous / neutral4 ⭐PositiveSatisfied5 ⭐PositiveStrongly satisfied
This binary labeling strategy avoids ambiguity and allows clean separation of complaint language from praise language before applying topic modeling.

Step 4: LDA Topic Modeling (in R)
Latent Dirichlet Allocation (LDA) is a generative probabilistic model that treats each document as a mixture of topics, and each topic as a mixture of words. It discovers hidden thematic structure in a corpus without any labeled supervision.
The model was applied separately on:

The negative review corpus
The positive review corpus

Each model produced 6 topics, chosen after evaluating coherence scores. Output included:

Beta weights — probability of each word belonging to each topic
Theta weights — probability of each document belonging to each topic
Top keywords per topic for labeling and interpretation

rlibrary(topicmodels)

lda_model <- LDA(dtm, k = 6, control = list(seed = 42))
topics <- tidy(lda_model, matrix = "beta")

Step 5: Visualization
Multiple visualization types were produced to interpret LDA output:

Word clouds — Visual representation of top keywords per topic
Beta bar charts — Top 10 keywords ranked by probability per topic
Theta distribution plots — How strongly each document belongs to each topic
Average topic weight charts — Relative importance of each topic across the corpus
Document-level assignment tables — Sample reviews tagged with their dominant topic


📈 Results
Negative Reviews — 6 Discovered Topics
TopicAssigned LabelTop KeywordsInterpretation1App functionality failuresneed, even, still, problem, fixGeneral dissatisfaction with broken features2Update-triggered crashesupdate, work, crash, open, fixApp stops working after updates3Upload/download issuesupload, download, still, slow, failTransfers stuck, slow, or failing4Storage anxietystorage, full, delete, photo, spaceRunning out of space, forced deletions5Google Drive sync problemsgoogle, drive, account, login, syncAuth failures and sync inconsistency6General performanceapp, open, load, crash, slowGeneral slowness and loading failures

All 6 topics carry nearly equal weight (~16.7% each), meaning users are frustrated across multiple dimensions simultaneously — not just one dominant problem.


Positive Reviews — 6 Discovered Topics
TopicAssigned LabelTop KeywordsInterpretation1Storage praiseapp, storage, best, good, helpAppreciation for storage capacity and management2Ease of useuse, easy, love, app, greatUsers find the app simple and intuitive3Overall qualitygreat, app, work, love, likeGeneral praise for reliability and performance4File & photo savingfile, save, phone, photo, likeStrong appreciation for saving and backup features5Google Drive integrationdrive, google, one, thank, likePraise for seamless Drive backup and sync6Helpfulnessapp, good, nice, help, likeBroad utility and everyday helpfulness

Positive topics are also equally weighted (~16.7% each), indicating well-rounded, balanced satisfaction — no single feature is a standout but all are appreciated.


Side-by-Side Comparison
DimensionNegative ThemesPositive ThemesUpdatesApp breaks after updatesUpdates rarely mentioned positivelyStorageRunning out, forced deletionsFree storage appreciatedEase of useAbsent in complaintsTop strengthUploadsSlow, stuck, failingAssumed to work — not explicitly praisedGoogle DriveSync and login failuresBackup and integration praisedHelpfulness"Please help fix""Helpful app"Topic balanceEqual weight across all 6Equal weight across all 6

🔍 Recommendations
🚨 High Priority — Critical Fixes
ProblemRoot CauseRecommended SolutionExpected ImpactApp crashes after updatesUnstable release processStaged rollouts + auto-rollback if crash rate > thresholdReduce update-triggered 1-star reviewsUpload/download failuresBackend reliability + poor UX on errorsResume upload queue + descriptive error messagesImprove perceived reliabilityPhoto deletion anxietyInsufficient storage + no smart cleanup"Free device space only" feature with undoReduce uninstall riskGoogle Drive sync issuesAuth token expiry + sync race conditionsImproved OAuth stability + manual sync triggerIncrease user trust
✅ Medium Priority — Strengthen What's Working
StrengthRecommendationEase of useKeep the UI simple; hide advanced settings behind menusFree storageSurface storage availability more prominently in UIFile protectionAdd visible virus scan / encryption status indicatorsHelpfulnessIntroduce in-app guided tooltips and smart help suggestions
🔮 Long-Term Strategic Moves

Controlled update channels — Allow users to opt into "stable-only" update track
Personalized storage recommendations — Suggest upgrade plans based on actual usage behavior
Real-time sentiment monitoring — Track keywords like fix, crash, stuck, plea for early issue detection dashboards
Convert satisfied users into power users — Loyalty features and rewards for heavy free-tier users


📁 Repository Structure
📦 sentiment-topic-modeling-playstore/
 ┣ 📓 semster_ml_project.ipynb        ← Main notebook (all stages)
 ┣ 📄 Cloud_Storage_Reviews.csv       ← Dataset (not tracked due to size — see instructions)
 ┣ 📊 topic_overall_weights.csv        ← LDA average topic weight summary
 ┣ 📊 topic_weights_per_document.csv   ← Per-document dominant topic assignments
 ┣ 🖼️ graph_topic_weights.png          ← Topic weight bar chart (positive reviews)
 ┗ 📄 README.md                        ← This file

🚀 Getting Started
Prerequisites
Python dependencies:
bashpip install google-play-scraper pandas numpy nltk tqdm langdetect
Download NLTK data (run once):
pythonimport nltk
nltk.download('stopwords')
nltk.download('wordnet')
nltk.download('punkt')
R dependencies:
rinstall.packages(c("topicmodels", "tidytext", "tidyr", "dplyr",
                   "ggplot2", "wordcloud", "tm", "reshape2"))
Running the Project

Clone this repository

bash   git clone https://github.com/your-username/sentiment-topic-modeling-playstore.git
   cd sentiment-topic-modeling-playstore

Open the notebook

bash   jupyter notebook semster_ml_project.ipynb
Or upload to Google Colab for cloud execution.

Run cells in order:

Section 2: Data scraping (produces Cloud_Storage_Reviews.csv)
Section 3: Preprocessing
Section 4: Sentiment analysis + LDA (requires R)
Section 5: Conclusions and visualizations




⚠️ Scraping note: Collecting 1.8M reviews takes significant time. Set MAX_REVIEWS to a smaller number (e.g., 10000) per app for testing. Google may rate-limit heavy scraping — add delays if needed.


⚠️ Dataset size: The full CSV (~400MB+) is not included in this repository. Run the scraping section to regenerate it, or contact the authors.


🧪 Sample Output
Word Cloud (Positive Reviews — Topic: Ease of Use)
Shows dominant terms: easy, love, great, use, app, simple, work, amazing
Beta Weight Chart (Negative Reviews — Topic: Upload Issues)
Top keywords by probability: upload, still, trying, slow, download, fail, stuck, work, keep, error
Average Topic Weights
SentimentAll TopicsEqual Distribution?Negative6 topics~16.7% each ✅Positive6 topics~16.7% each ✅

📚 References & Further Reading

Blei, D. M., Ng, A. Y., & Jordan, M. I. (2003). Latent Dirichlet Allocation. Journal of Machine Learning Research, 3, 993–1022.
Bird, S., Klein, E., & Loper, E. (2009). Natural Language Processing with Python. O'Reilly Media.
Silge, J., & Robinson, D. (2017). Text Mining with R: A Tidy Approach. O'Reilly Media.
Google Play Scraper Python Library: https://github.com/JoMingyu/google-play-scraper


📄 License
This project was developed for academic purposes as a final semester project at COMSATS University Islamabad. All review data was collected from publicly available sources on the Google Play Store.

🔗 Connect With the Authors
Muhammad Salman Saleem
LinkedIn · GitHub · Kaggle · Instagram
Amna Nawaz Gondal
LinkedIn · GitHub · Instagram
Muhammad Ahsan Shahzad
LinkedIn · GitHub · Instagram
