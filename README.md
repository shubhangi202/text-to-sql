# text-to-sql
---

# Agentic Text-to-SQL Workflow

This project demonstrates an **agentic workflow** that converts natural language queries into SQL, executes them against a database, and returns the results in **user-friendly natural language**. The workflow leverages **Large Language Models (LLMs)** and can handle complex queries, including multi-level joins.

---

## Table of Contents

* [Overview](#overview)
* [Features](#features)
* [Architecture](#architecture)
* [Database Schema](#database-schema)
* [Setup](#setup)
* [Usage](#usage)
* [Supported LLMs](#supported-llms)
* [Examples](#examples)
* [Contributing](#contributing)
* [License](#license)

---

## Overview

This workflow allows **non-technical users** to interact with databases without writing SQL. Users input natural language queries, and the workflow:

1. Analyzes the query.
2. Uses LLMs to translate the query into a syntactically correct SQL statement.
3. Executes the SQL query against a database.
4. Returns the results in natural language.

This approach supports **complex queries** involving multiple joins across tables, making it suitable for real-world scenarios.

---

## Features

* Convert natural language to SQL queries.
* Execute SQL queries against a database automatically.
* Handle multi-level joins and complex queries.
* Return user-friendly responses in natural language.
* Easily switch between LLMs (OpenAI GPT-4 or open-source models from Hugging Face).
* Debugging enabled with printed SQL queries.

---

## Architecture

```
User (Natural Language Query)
          │
          ▼
     LLM Agent (Understands DB schema)
          │
          ▼
   Generates SQL Query
          │
          ▼
      SQL Database
          │
          ▼
   Returns SQL Response
          │
          ▼
  LLM Agent (Formats response)
          │
          ▼
User-friendly Natural Language Response
```

---

## Database Schema

This project uses the **Chinook Database** (local SQL database):

* **Artist**: `artist_id`, `name`
* **Album**: `album_id`, `title`, `artist_id`
* **Track**: `track_id`, `name`, `album_id`, `genre_id`
* **Genre**: `genre_id`, `name`
* **PlaylistTrack**: `playlist_id`, `track_id`

**Example Relationship:**
To fetch all tracks of an artist, the workflow joins `Artist → Album → Track`.

---

## Setup

1. Clone the repository:

```bash
git clone https://github.com/shubhangi202/text-to-sql.git
cd text-to-sql
```

2. Install dependencies:

```bash
pip install -r requirements.txt
```

3. Ensure the **Chinook database** is available locally and update the URI in the code:

```python
db_uri = "sqlite:///path/to/chinook.db"
```

---

## Usage

1. Import the workflow:

```python
from agentic_workflow import answer_user_query
```

2. Run a natural language query:

```python
query = "Give me 10 artists"
response = answer_user_query(query, use_openai=True)  # True for GPT-4, False for Quen
print(response)
```

3. Switch LLM models easily:

* **OpenAI GPT-4**: `use_openai=True`
* **Quen 7B Instruct (Hugging Face)**: `use_openai=False`

---

## Supported LLMs

* **OpenAI GPT-4** (paid API)
* **Quen 7B Instruct** (open-source, free, hosted on Hugging Face)

---

## Examples

**1. Simple query:**

```text
"Give me the name of 10 artists"
```

**SQL generated:**

```sql
SELECT name FROM artist LIMIT 10;
```

**Response:**

```
The names of 10 artists are AC/DC, Aerosmith, Alenus, ...
```

**2. Multi-level join:**

```text
"Give me some tracks by artist name 'Audio Slave'"
```

**SQL generated:**

```sql
SELECT track.name
FROM track
JOIN album ON track.album_id = album.album_id
JOIN artist ON album.artist_id = artist.artist_id
WHERE artist.name = 'Audio Slave';
```

**Response:**

```
The tracks by the artist Audio Slave include "Show Me How to Live", "Gasoline", "What You Are", ...
```

---

