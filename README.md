# GitHub Stars Crawler

**Author:** Anas Jafri

This project is a Python-based GitHub Stars Crawler that uses GitHub’s GraphQL API to fetch repositories and their star counts, storing the data into a PostgreSQL database. It was developed as part of a Software Engineer Take-Home Assignment and demonstrates clean, modular design and practical software engineering practices.

---

## What This Project Does

The crawler connects to GitHub’s GraphQL API to fetch repositories (by default targeting those with at least one star ⭐) and saves their `name_with_owner` and `stargazer_count` into a PostgreSQL database.

Key features include:

- **Upsert support:**  
  - If a repository already exists, only its star count and timestamp are updated.  
  - If it’s new, it is inserted.  
- **CSV export:** Easily dump the collected data for sharing or further analysis.

---

## Why Only 1000 Rows?

Although the assignment suggested fetching up to 100,000 repositories, this implementation currently handles 1,000 rows due to time constraints and GitHub API rate limits.  
The code is fully scalable: you can adjust the `target_count` parameter in `crawl_stars.py` to process larger datasets.

---

## Project Architecture

This project follows **clean and modular design**:

- **`crawl_stars.py`** – Fetches data from GitHub and inserts/updates records in PostgreSQL.
- **`dump_db.py`** – Exports database content to CSV.
- **`crawl.yml`** – GitHub Actions workflow that automates setup, crawling, and exporting steps.

**Design principles applied:**

1. **Separation of concerns:** Crawling, database management, and CI/CD are modular and independent.  
2. **Error handling:** Retry logic and exception management for API failures.  
3. **Immutability:** Fetched data is not mutated beyond preparing upserts.

---

## Assignment Questions Answered

**1. How would you handle 500 million repositories?**  

- Use distributed crawling with multiple workers to parallelize API requests.  
- Introduce batch processing and message queues (e.g., Kafka, RabbitMQ) for scalable task distribution.  
- Switch to asynchronous API calls for speed.  
- Implement sharded or partitioned database tables.  
- Store raw data in cloud object storage (S3, GCS) before inserting into DB.  
- Fetch only incremental updates daily using GitHub’s `updatedAt` field.  

**2. How would the schema evolve for additional metadata (issues, PRs, comments, etc.)?**  

- Create separate tables for issues, pull requests, comments, and reviews.  
- Maintain foreign key relationships linking these tables to repositories.  
- Store timestamps and counts for efficient incremental updates.  
- Use partitioning and indexing for large datasets to ensure query efficiency.  

**3. Why is duration important?**  

- Crawling speed affects both efficiency and API rate limit compliance.  
- The crawler batches requests (100 repositories per query), uses retries with a 1-second delay, and performs efficient upserts instead of inserting records individually.  

**4. What software engineering practices were applied?**  

- Clean, modular architecture  
- Logging for debugging and performance monitoring  
- Retry mechanisms and error resilience  
- Environment variables for configuration flexibility  
- CI/CD integration with GitHub Actions

---

## How to Run Locally

### Prerequisites

- Python 3.9+  
- PostgreSQL running locally  
- GitHub Personal Access Token with public repo access

### Steps

1. **Clone the repository**

```bash
git clone https://github.com/Anas-Jaffery/SofsticaAssesment.git
cd SofsticaAssesment
