# StackUp Helper Bot

This repository contains a Retrieval-Augmented Generation (RAG) based Language Model (LLM) designed to assist users with their queries regarding the StackUp website. The model leverages data scraped from the StackUp Zendesk page to provide accurate and contextually relevant answers.

## Features

- **Data Scraping**: Extracts data from the StackUp Zendesk page.
- **Data Cleaning**: Processes and cleans the scraped data for embedding.
- **Data Embedding**: Stores the cleaned data in MongoDB Atlas for efficient retrieval.
- **Query Handling**: Uses RAG-based LLM to generate responses based on retrieved data.
- **Programming Query Rejection**: If users ask technical or programming-related questions, the LLM will deny answering to ensure the integrity of StackUp's platform policies.

## Repository Structure

```
├── get_and_clean_data_programs
│   ├── get-data.py
│   ├── clean-data.py
├── create-embedding-main.ipynb
├── data
│   ├── cleaned_data.txt
├── StackUp_Bot_Chatflow.json
├── requirements.txt
└── README.md
```

### Files

- **get_and_clean_data_programs/get-data.py**: Scrapes data from the StackUp Zendesk page and saves it as a JSON file.
- **get_and_clean_data_programs/clean-data.py**: Cleans the scraped JSON data and converts it into a text file.
- **create-embedding-main.ipynb**: Embeds the cleaned text data into MongoDB Atlas using the `togethercomputer/m2-bert-80M-8k-retrieval` model.
- **StackUp_Bot_Chatflow.json**: Contains a pipeline for Flowise AI, which integrates the model into the website.

## Setup and Usage

### Prerequisites

- Python 3.x
- MongoDB Atlas account
- Required Python packages (listed in `requirements.txt`)

### Installation

1. Clone the repository:

    ```sh
    git clone https://github.com/yourusername/stackup-query-assistant.git
    cd stackup-query-assistant
    ```

2. Install the required packages:

    ```sh
    pip install -r requirements.txt
    ```

3. Create a .env file and store ATLAS_URI & TOGETHER_API_KEY.

### Data Scraping and Cleaning

Run the `get-data.py` script to scrape data from the StackUp Zendesk page:

```sh
python get_and_clean_data_programs/get-data.py
```

Run the `clean-data.py` script to clean the scraped data:

```sh
python get_and_clean_data_programs/clean-data.py
```

### Data Embedding

1. Open and run the `create-embedding-main.ipynb` notebook to embed the cleaned data into MongoDB Atlas.

2. Navigate to Altas Search and Create a new **Vector Search** with name "idx_embedding" and the following body:
```json
{
  "fields": [
    {
      "numDimensions": 768,
      "path": "embedding",
      "similarity": "cosine",
      "type": "vector"
    }
  ]
}
```

3. Review and Create Vector Search.


### Flowise AI Integration

1. Install Flowise locally using NPM:

    ```sh
    npm install -g flowise
    ```

2. Start Flowise:

    ```sh
    npx flowise start
    ```

3. Open Flowise at [http://localhost:3000](http://localhost:3000).

4. Navigate to Workflows and click on the "Add New" button.

5. Click on the gear icon, load the workflow by uploading `StackUp_Bot_Chatflow.json`.

6. Input all required fields such as OpenAI API Key, MongoDB connection URI, database name, collection name, and index name, then save it.

7. Click on the code icon ("</>") and insert the generated script code into the `<body>` tag of your website.

## Demo

To see a live demo of the StackUp Query Assistant, visit the following link:

[Demo Link]()

## Important Note

### Programming Query Rejection

The StackUp Query Assistant is designed to maintain the integrity of StackUp's learn-to-earn platform. If users ask any technical or programming-related questions, the LLM will deny answering them. Similarly, if users ask about content not present in the knowledge base, it will also deny answering. This ensures that users complete their coding challenges and quests independently.
