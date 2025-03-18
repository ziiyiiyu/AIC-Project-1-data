# AIC-Project-1-data
### 1. **Data Source**

The data used in this experiment comes from two main sources: `TwitchQuotes` and `Reddit`. These datasets are publicly available and contain text data from different categories:

- **TwitchQuotes**: Data from the Twitch platform, which includes "Copypasta" texts (repetitive paragraphs or short texts found on the internet), often consisting of humorous, parody, or internet meme content.
- **Reddit**: Data from multiple subreddits on the Reddit platform, discussing various topics such as games like `league-of-legends` and `hearthstone`, as well as general social discussions. This data is used for comparison with the `Copypasta` category.

### 2. **Data Type**

- **`content`**: The content of each text entry, **string type**.
- **`is_copypasta`**: Label field, **binary type (0 or 1)**, indicating whether the text is a Copypasta (1 means Copypasta, 0 means not).
- **`non_english_ratio`**: The proportion of non-English characters in the text, **float type**, used to determine whether the text contains ASCII art.
- **`clean_content`**: The cleaned version of the text content, with URLs, punctuation, and numbers removed. This field helps in the subsequent text vectorization process.

### 3. **Data Volume and Composition**

- **TwitchQuotes Copypasta**:
    - Data from the Twitch platform, which contains a significant amount of Copypasta texts.
    - These texts are primarily related to internet culture, jokes, or memes, with the number of texts reaching thousands after preprocessing.
- **Reddit Dataset**:
    - Data used for comparison with the Copypasta category, collected from various subreddits. These texts are not part of the Copypasta category.
    - The Reddit data comes from subreddits such as `league-of-legends`, `hearthstone`, etc., with 744 test data and 1734 training data entries.

The **size** and **composition** of the dataset are as follows:

- **Training data**: A total of 1734 texts.
- **Test data**: A total of 744 texts.
- **`is_copypasta` labels**: Half of the texts belong to the Copypasta category, and the other half comes from Reddit discussions.

### 4. **Data Collection Process**

The data was collected using two tools: **Selenium** and **PRAW**.

- **Selenium**: Used to scrape data from the TwitchQuotes website. Selenium’s headless browser mode was employed to simulate human browsing and extract Copypasta texts from each page. The texts include categories such as `league-of-legends`, `classic`, etc.
    
    **Data collection code**:
    
    ```python
    driver = webdriver.Chrome(service=Service(ChromeDriverManager().install()), options=options)
    categories = {"classic": 18, "league-of-legends": 14, "hearthstone": 13}
    for category, page_count in categories.items():
        for page in range(1, page_count + 1):
            driver.get(f"https://www.twitchquotes.com/copypastas/labels/{category}?page={page}")
            # Extract data
    ```
    
- **PRAW (Python Reddit API Wrapper)**: Used to scrape data from Reddit. PRAW API was used to access various subreddits and collect popular posts and their comments, filtering out the Copypasta-tagged posts.
    
    **Data collection code**:
    
    ```python
    reddit = praw.Reddit(client_id=REDDIT_CLIENT_ID, client_secret=REDDIT_CLIENT_SECRET, user_agent=USER_AGENT)
    for subreddit_name in subreddits.values():
        for post in reddit.subreddit(subreddit_name).hot(limit=100):
            # Extract and process posts
    ```
    

### 5. **Data Storage and Format**

All collected data was stored in **CSV format**. Each text entry contains the `content` and `is_copypasta` label, along with other useful features for prediction, such as `clean_content` and `non_english_ratio`.

- **Storage Format**:
    - `copypasta_all_labels.csv`: Contains all the texts from TwitchQuotes, including their labels and categories.
    - `reddit_data.csv`: Contains posts and comments from Reddit, annotated with `is_copypasta`.
