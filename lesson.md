```md
# 📌 Lesson: Introduction to Reddit API

## 🎯 Learning Objectives
By the end of this lesson, students will:
1. Understand what an API is and how it works.
2. Learn how to access and authenticate with the **Reddit API**.
3. Use Python and the `praw` library to retrieve and analyze subreddit data.
4. Implement basic API requests using the `requests` library as an alternative.
5. Explore use cases such as scraping top posts and analyzing comment sentiment.

---

## 🏗️ 1. What is an API?
- **API (Application Programming Interface)** is a way for different programs to communicate with each other.
- APIs allow developers to retrieve data from external sources without needing direct database access.
- The **Reddit API** lets us interact with Reddit programmatically, accessing posts, comments, and user data.

---

## 🔑 2. Reddit API Authentication
Before using the Reddit API, we need **credentials** to authenticate.

### Steps:
1. Go to **[Reddit Apps](https://www.reddit.com/prefs/apps)**.
2. Click **"Create an app"** at the bottom.
3. Fill in:
   - **App name**: `MyRedditApp`
   - **App type**: Select "script"
   - **Redirect URI**: `http://localhost:8080`
   - **Permissions**: Select necessary scopes.
4. Save the **Client ID** and **Client Secret** for authentication.

---

## 🛠️ 3. Setting Up Python for Reddit API
### 🔹 Install Dependencies
```bash
pip install praw requests
```

### 🔹 Configure API Access
Create a file `config.py` to store credentials:
```python
CLIENT_ID = "your_client_id"
CLIENT_SECRET = "your_client_secret"
USER_AGENT = "my_reddit_app"
USERNAME = "your_reddit_username"
PASSWORD = "your_reddit_password"
```

---

## 📜 4. Using `praw` to Fetch Data
`praw` (Python Reddit API Wrapper) makes it easy to interact with Reddit.

### 🔹 Connect to the Reddit API
```python
import praw
import config

# Authenticate
reddit = praw.Reddit(
    client_id=config.CLIENT_ID,
    client_secret=config.CLIENT_SECRET,
    user_agent=config.USER_AGENT,
    username=config.USERNAME,
    password=config.PASSWORD
)

print("Authenticated as:", reddit.user.me())
```

### 🔹 Fetch Top Posts from a Subreddit
```python
subreddit = reddit.subreddit("learnpython")

for post in subreddit.hot(limit=5):
    print(f"Title: {post.title}")
    print(f"Upvotes: {post.score}")
    print(f"URL: {post.url}\n")
```

### 🔹 Fetch Comments from a Post
```python
post = reddit.submission(url="https://www.reddit.com/r/learnpython/comments/example")

print("Post Title:", post.title)
print("Comments:")
for comment in post.comments[:5]:
    print("-", comment.body)
```

---

## 📡 5. Using `requests` for API Calls
An alternative to `praw` is making direct API requests using `requests`.

### 🔹 Fetch Data Using `requests`
```python
import requests

url = "https://www.reddit.com/r/learnpython/top.json?limit=5"
headers = {"User-Agent": "my_reddit_app"}

response = requests.get(url, headers=headers)
data = response.json()

for post in data["data"]["children"]:
    print("Title:", post["data"]["title"])
    print("Upvotes:", post["data"]["score"])
    print("URL:", post["data"]["url"], "\n")
```

---

## 📊 6. Use Case: Analyzing Sentiment of Reddit Comments
We can analyze comment sentiment using `TextBlob`.

### 🔹 Install `textblob`
```bash
pip install textblob
```

### 🔹 Analyze Comments
```python
from textblob import TextBlob

comments = ["I love Python!", "This is the worst!", "Not bad, but could be better."]

for comment in comments:
    sentiment = TextBlob(comment).sentiment.polarity
    print(f"Comment: {comment} | Sentiment: {sentiment}")
```
- **Positive sentiment** → Closer to **1**.
- **Negative sentiment** → Closer to **-1**.
- **Neutral sentiment** → Around **0**.

---

## 🎯 7. Assignment: Build Your Own Reddit Bot
**Task**: Create a bot that:
1. Fetches the **top 10** posts from `r/python`.
2. Finds the **most upvoted comment** on each post.
3. Analyzes the sentiment of the comment.
4. Prints the results.

---

## ✅ 8. Summary
- The **Reddit API** allows interaction with Reddit programmatically.
- `praw` makes accessing Reddit easier.
- API requests using `requests` provide an alternative.
- **Authentication is required** for full API access.
- Sentiment analysis can provide insights into comments.

🚀 **Next Steps**: Extend your bot to automatically reply to comments based on sentiment!

---

## 🔗 Useful Links
- 🔗 [Reddit API Docs](https://www.reddit.com/dev/api/)
- 🔗 [PRAW Documentation](https://praw.readthedocs.io/en/stable/)
```
