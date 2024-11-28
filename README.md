![image](https://github.com/user-attachments/assets/6bbd69d5-80c1-45dc-8c45-d9d2fb606971)
# Twitter Scraping App

This project is designed to scrape data from Twitter profiles using Selenium in a Python Flask backend, store the data in an SQLite database, and display the data in a React frontend.

## Features

- Scrape user profile data from Twitter (username, profile name, bio, followers, following, location, website, profile image URL).
- Scrape posts from Twitter (up to 20 posts from the profile).
- Store the scraped data in an SQLite database (`data.db`).
- Fetch the scraped data upon clicking a 'Fetch' button in the React frontend.
- Display the scraped user profile and posts data in styled cards on the frontend.

## Prerequisites

Before getting started, ensure that the following software is installed:

- **Python 3.x**
- **Node.js** and **npm** (for React frontend)
- **SQLite** (SQLite comes pre-installed with Python, no need to install separately)
- **Google Chrome** (for Selenium WebDriver)

### Backend (Python Flask)

1. Clone this repository:

   ```bash
   git clone <repository_url>
   cd <repository_folder>
   ```

2. Install the necessary Python dependencies:

   ```bash
   pip install -r requirements.txt
   ```

   The `requirements.txt` should contain the following dependencies:

   ```text
   Flask==2.2.2
   flask-cors==3.1.1
   selenium==4.7.0
   pandas==1.5.3
   sqlite3==3.36.0
   ```

3. **Set up the SQLite database**:
   
   The database will be automatically created when the Flask backend starts. It will contain two tables:
   - `user_data`: Stores the scraped Twitter profile data.
   - `posts_data`: Stores the scraped Twitter posts.

4. **Run the Flask backend**:

   Navigate to the backend folder and run the Flask app:

   ```bash
   python app.py
   ```

   This will start the backend server on `http://127.0.0.1:7000`.

### Frontend (React)

1. Navigate to the frontend folder (React app) and install the required dependencies:

   ```bash
   cd frontend
   npm install
   ```

2. **Update the `fetchData` function in React**:
   
   Make sure the React app sends a POST request to the Flask backend to trigger the scraping. The request will include a list of Twitter profile URLs to scrape.

   Example API call in React:

   ```js
   const fetchData = async () => {
       const response = await fetch('http://127.0.0.1:7000/scrape', {
           method: 'POST',
           headers: {
               'Content-Type': 'application/json',
           },
           body: JSON.stringify({
               urls: ['https://twitter.com/exampleuser1', 'https://twitter.com/exampleuser2']
           })
       });
       const data = await response.json();
       setUserData(data.user_data);  // Set user data in state
       setPostsData(data.posts_data);  // Set posts data in state
   };
   ```

3. **Run the React app**:

   In the frontend folder, run the following command to start the React development server:

   ```bash
   npm start
   ```

   The app will run on `http://localhost:3000`.

### How It Works

1. **Backend**:
   - The backend is a Flask application that uses Selenium to scrape user profile data and posts from Twitter profiles.
   - When the `/scrape` route is hit with a POST request containing a list of URLs, it scrapes the data for each URL and stores it in an SQLite database (`data.db`).
   - The backend responds with the scraped data, which includes both user profile information and posts.

2. **Frontend**:
   - The React frontend has a simple button labeled 'Fetch' that, when clicked, sends a POST request to the backend with the list of Twitter profile URLs to scrape.
   - Once the backend returns the scraped data, it displays the data on the frontend using styled cards.

### Folder Structure

```
.
├── app.py               # Flask backend app
├── requirements.txt     # Python dependencies
└── frontend/            # React frontend
    ├── public/
    ├── src/
    └── package.json     # React dependencies
```

### Sample Workflow

1. The user enters Twitter profile URLs into the frontend (React app).
2. Upon clicking the "Fetch" button, the React app sends the URLs to the Flask backend via a POST request.
3. The Flask backend uses Selenium to scrape data from the provided URLs.
4. The scraped data (user profile and posts) is saved to the `data.db` SQLite database.
5. The backend sends the scraped data back to the frontend.
6. The frontend displays the data in a structured format (cards) for the user.

### Example Output in Frontend

- **User Profile Card**:
  - Username
  - Profile Name
  - Bio
  - Followers and Following Count
  - Location and Website
  - Profile Image

- **Posts Card**:
  - Post content (first 20 posts)

### Error Handling

- If an error occurs during scraping, the backend will return an error message with details.
- If invalid data is sent from the frontend (e.g., URLs are missing or not in the correct format), the backend will return an appropriate error response.

### Troubleshooting

- **WebDriver Issues**: If you face issues with the WebDriver, make sure you have the correct version of Google Chrome and the ChromeDriver installed and accessible from your system's PATH.
- **Cross-Origin Request Issues**: If you face issues with CORS, ensure that `flask-cors` is correctly installed and configured in the backend.

### Contributing

If you'd like to contribute to the project, please fork the repository and create a pull request with your changes.

