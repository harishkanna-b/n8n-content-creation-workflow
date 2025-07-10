# n8n Content Creation Workflow

This repository contains a powerful, multi-stage content creation system built with n8n. It automates the process of generating a personalized 4-week LinkedIn content plan based on a user's existing content and market research.

## Overview

The system is composed of three interconnected n8n workflows:

1.  **Workflow 1: Master Workflow** - This is the main workflow that orchestrates the entire process. It takes a LinkedIn profile URL as input, fetches the user's posts, and then calls the other workflows to perform analysis and content generation.
2.  **Workflow 2: Analysis Workflow** - This workflow analyzes the user's LinkedIn posts to create a "Content Manifesto," which is a detailed breakdown of their content strategy, voice, and style. It then performs market research on LinkedIn, Reddit, and Twitter to gather insights on relevant topics.
3.  **Workflow 3: Content Generation Workflow** - This workflow takes the Content Manifesto and the market research insights to generate a complete 4-week (20-day) LinkedIn content plan using a large language model.

## Features

*   **Automated Content Strategy:** Automatically analyzes a LinkedIn profile to define a content strategy.
*   **Multi-Platform Market Research:** Gathers insights from LinkedIn, Reddit, and Twitter.
*   **AI-Powered Content Generation:** Uses a large language model to create a personalized content plan.
*   **Airtable Integration:** Uses Airtable to store and manage data throughout the process.
*   **Modular Design:** The system is broken down into three separate workflows for better organization and maintainability.

## Prerequisites

Before you can use this workflow system, you will need the following:

*   **n8n:** An instance of n8n (local or cloud).
*   **Airtable:** An Airtable account.
*   **Google Gemini API Key:** An API key for the Google Gemini large language model.
*   **RapidAPI Account and API Key:** A RapidAPI account and an API key for the "linkedin-data-api".
*   **Apify Account and API Key:** An Apify account and an API key. You will also need to have the following Apify actors set up:
    *   `harvestapi/linkedin-post-search`
    *   `trudax/reddit-scraper-lite`
    *   `apidojo/twitter-scraper-lite`

## Setup Instructions

1.  **Import the Workflows:**
    *   Download the three workflow JSON files from this repository.
    *   In your n8n instance, go to the "Workflows" page and click the "Import from File" button.
    *   Import each of the three workflow files.

2.  **Configure Credentials:**
    *   In your n8n instance, go to the "Credentials" page.
    *   Add new credentials for the following services:
        *   **Airtable:** Enter your Airtable API key.
        *   **Google Gemini:** Enter your Google Gemini API key.
        *   **RapidAPI:** Create a new credential and add your RapidAPI key to the `X-RapidAPI-Key` header.
        *   **Apify:** Enter your Apify API key.

3.  **Set up the Airtable Base:**
    *   Create a new Airtable base.
    *   Create the following tables in your base:
        *   `Analysis Table`
        *   `Manifesto`
        *   `QueryTable`
    *   The specific fields for each table will be created automatically by the workflows when they run for the first time.

4.  **Update the Workflows & Credentials:**
    *   Open each of the three workflows in the n8n editor.
    *   **Airtable:** In all workflows, update the Airtable nodes to use your Airtable credentials and select the correct Base and Tables you created.
    *   **Google Gemini:** In all three workflows, update the Google Gemini nodes to use your Google Gemini credentials.
    *   **RapidAPI (LinkedIn Data):** In **Content Creation Workflow #1.json**, locate the **"Get LinkedIn posts"** node. In the "Headers" section of the node's parameters, find the parameter named `x-rapidapi-key`. Replace the placeholder value, `"Paste your RapidAPI key here"`, with your actual RapidAPI key.
    *   **Apify (Social Media Scraping):** In **Content Creation Workflow #2.json**, there are three HTTP Request nodes that call Apify actors: **"Call LinkedIn Actor"**, **"Call Reddit Actor"**, and **"Call X Actor"**. In each of these nodes, go to the "Headers" section and replace the placeholder `paste_your_apify_api_key_here` in the `Authorization` header with your actual Apify API key. The final value should be in the format `Bearer YOUR_APIFY_KEY`.

## How to Use

1.  **Run the Master Workflow:**
    *   Open "Content Creation Workflow #1" in the n8n editor.
    *   Click the "Execute Workflow" button.
    *   When prompted, enter the LinkedIn profile URL that you want to analyze.

2.  **Check the Results:**
    *   The workflow will take some time to run, as it is performing a lot of tasks.
    *   You can monitor the progress of the workflow in the n8n editor.
    *   Once the workflow is complete, you will find the generated content plan in the output of the final node in "Content Creation Workflow #3". The content plan will also be saved as a file.

## Customization

This workflow system is highly customizable. Here are a few ideas for how you can modify it to fit your needs:

*   **Change the Language Model:** You can easily swap out the Google Gemini model for another large language model, such as OpenAI's GPT-4.
*   **Add More Social Media Platforms:** You can add more social media platforms to the market research phase by adding more Apify actors to Workflow 2.
*   **Customize the Content Plan:** You can modify the prompt in the final AI Agent node in Workflow 3 to change the format or style of the generated content plan.
*   **Change the Output Format:** You can change the output format of the content plan by modifying the "Convert to File" node in Workflow 3.