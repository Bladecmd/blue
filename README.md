# Streamverse

This project is a full-stack streaming application—a “Streamverse”—that allows users to browse and watch movies and TV shows, manage subscriptions and user profiles, and receive personalized recommendations. The application is built using a three-tier architecture that separates concerns into a Frontend (client), a Backend (API server), and a Database Layer.

---

## Table of Contents

- [Project Overview](#project-overview)
- [Architecture Overview](#architecture-overview)
  - [Client Layer (React Frontend)](#client-layer-react-frontend)
  - [Application Layer (Express Backend)](#application-layer-express-backend)
  - [Database Layer (Oracle Database)](#database-layer-oracle-database)
  - [Security & Validation](#security--validation)
- [How It Works](#how-it-works)
- [Project Structure](#project-structure)
- [Summary](#summary)
- [Installation & Setup](#installation--setup)

---

## Project Overview

This project is a full-stack streaming application that mimics the core functionalities of Netflix. Key features include:

- **User Authentication:** Signup and login with secure password hashing (bcrypt) and JWT-based token authentication.
- **Profile Management:** Users can create and manage multiple profiles, each with its own watch history.
- **Content Browsing & Search:** Browse movies and TV shows by genre or perform dynamic searches based on title, genre, cast, year, or language.
- **Personalized Recommendations:** A recommendation engine powered by cosine similarity (using TF-IDF vectors) provides “more like this” suggestions.
- **Subscription Management:** Users can choose from multiple subscription plans that control the number of profiles and features available.
- **External Data Integration:** Data is periodically fetched from The Movie Database (TMDb) to update movies, TV shows, genres, and cast/crew information.

---

## Architecture Overview

The application is structured into three distinct layers:

### Client Layer (React Frontend)

- **Framework:** Built using [Create React App](https://github.com/facebook/create-react-app).
- **UI Components:** Organized into reusable components such as Header, Card, Accordion, and Player.
- **State Management:** Uses React hooks and the Context API (e.g., `AuthContext`) to manage global state (authentication, user data, subscription details).
- **Routing:** Utilizes React Router to navigate between pages (Home, Browse, Profile, Subscription History, etc.).
- **Styling:** Global styles are defined using styled-components and Normalize.css.

### Application Layer (Express Backend)

- **RESTful API:** Built with Node.js and Express. The API provides endpoints for user management, profile operations, content browsing, search, and subscription management.
- **Controllers:** Organized into modules (e.g., `users-controller.js`, `profile-controller.js`, `browse-controller.js`, `subscription-controller.js`) that implement the business logic.
- **Routing:** API routes are defined in separate files in the `/routes` directory.
- **Middleware:** Custom middleware (e.g., `check-auth.js`) enforces JWT-based authentication for protected routes.
- **Services:**
  - **Database Service:** A dedicated module (`database.js`) manages Oracle DB connections using connection pooling.
  - **Recommendation Engine:** The `cosine_similarity.js` service tokenizes content descriptions, creates TF-IDF vectors, and computes cosine similarity scores to generate content recommendations.
  - **TMDb Integration:** The `tmdb.js` module integrates with The Movie Database API to fetch movies, TV shows, genres, and credits, and then stores this data in the Oracle database.

### Database Layer (Oracle Database)

- **Persistence:** The Oracle database stores all persistent data such as user accounts, profiles, movies, TV shows, genres, watch history, watchlists, subscriptions, and similarity scores.
- **Stored Procedures & PL/SQL Blocks:** Critical operations such as inserting similarity data or updating subscriptions are encapsulated in PL/SQL blocks for atomicity and proper error handling.
- **Connection Pooling:** The backend database service uses connection pooling to ensure high performance and efficient resource usage.

### Security & Validation

- **JWT Authentication:** After login, a JSON Web Token is issued and used for accessing protected API endpoints.
- **Password Hashing:** Passwords are securely hashed using bcrypt before they are stored in the database.
- **Input Validation:** The Express backend employs `express-validator` to validate and sanitize user inputs.

---

## How It Works

1. **User Onboarding:**
   - **Sign Up / Login:** Users register or log in via the frontend. On successful login, the server issues a JWT token.
   - **Profile Management:** Authenticated users can create and manage multiple profiles.

2. **Content Discovery & Interaction:**
   - **Browsing & Search:** Users can browse by genre or use dynamic search features. The backend constructs dynamic SQL queries to return matching results.
   - **Recommendations:** The recommendation engine leverages cosine similarity (based on TF-IDF vectors computed on content descriptions) along with watch history and genre preferences to suggest similar content.
   - **Watchlist & Rating:** Users can add titles to their watchlists and rate content. The backend updates watch history and aggregates ratings and view counts.

3. **Subscription Management:**
   - **Subscription Plans:** Multiple subscription plans (e.g., Basic, Standard, Premium) are available that control the number of profiles and features.
   - **Billing & History:** Subscription details, including billing amounts and expiration dates, are managed using stored procedures and updated via the API.

4. **Data Integration:**
   - **TMDb API:** A dedicated service fetches updated information from TMDb (movies, TV shows, genres, and credits) and synchronizes the data with the Oracle database.

---

## Project Structure

```
/backend
├── app.js                   # Main Express application
├── controllers/
│      ├── users-controller.js
│      ├── profile-controller.js
│      ├── browse-controller.js
│      └── subscription-controller.js
├── middleware/
│      └── check-auth.js      # JWT authentication middleware
├── models/
│      └── http-error.js      # Custom HTTP error class
├── routes/
│      ├── users-routes.js
│      ├── profile-routes.js
│      ├── browse-routes.js
│      └── subscription-routes.js
└── services/
├── database.js       # OracleDB connection & execution helpers
├── cosine_similarity.js  # TF-IDF and cosine similarity based recommendation engine
└── tmdb.js           # TMDb API integration

/frontend
├── public/                  # Static assets (HTML, manifest, robots.txt, images)
├── src/
├── components/       # Reusable UI components (e.g., Header, Card, Accordion)
├── context/          # React context (e.g., AuthContext)
├── constants/        # Route definitions and other constants
├── pages/            # Application pages (Home, Browse, Profiles, etc.)
├── utils/            # Helper functions
├── App.js            # Main React application component
├── index.js          # Entry point for React app
├── global-styles.js  # Global styled-components styles
└── … (other configuration files)
```

## Installation & Setup

1. **Clone the Repository:**

   ```bash
   git clone https://github.com/yourusername/your-repo.git
   cd your-repo
   ```

	2.	**Backend Setup:**
	•	Navigate to the backend directory and install dependencies:
```bash
cd backend
npm install
```
•	Configure your OracleDB connection details in database.js.
	•	Create a .env file (if not already provided) for secret keys and other sensitive configuration (e.g., JWT secret, TMDb API key).
	•	Run any required SQL scripts to initialize your database schema and stored procedures.

3.	**Frontend Setup:**
	•	Navigate to the frontend directory and install dependencies:
```bash
cd ../frontend
npm install
```

## How to Run

### Running the Backend
	
 1.	Start the Oracle Database
Ensure that your OracleDB is running and that the connection details in backend/services/database.js are correct.
	2.	Run the Express Server:
```bash
cd backend
npm start
```
### Running the Frontend

1.  Start the React Application: 
```bash
cd frontend
npm start
```

## Testing the Application
	
 ### • Frontend:
Use the browser to navigate through the application pages (Home, Browse, Profile, etc.).
	
 ### •	Backend:
You can use tools like Postman or curl to test API endpoints (e.g., user login, profile creation, content search).








