# API Verification Tool Overview

This tool uses a Node.js application to automatically assess the status of various APIs defined in a validators JSON file. The process works as follows:

## 1. Retrieving the API List
- The application downloads a JSON file from GitHub that contains a list of validators along with their respective API endpoints.
- Each entry includes key information such as the validator's name and the base URL for its API.

## 2. Verification Using Puppeteer
- For each API with a defined endpoint, the tool uses **Puppeteer**—a headless browser—to simulate a real user accessing the URL.
- It constructs the verification URL by appending `/cosmos/auth/v1beta1/bech32` to the base API URL.
- Puppeteer then loads the page, waits for the network activity to settle, and extracts the page content.

## 3. Response Validation
- The tool attempts to parse the extracted content as JSON and checks for the presence of the `"bech32_prefix"` property.
- If this property is found, the API is marked as **Working**.
- If not, it is flagged as **Error** or **Offline** depending on the specific issue encountered.

## 4. Periodic Updates and Result Publication
- The service runs continuously (managed via **SystemD** or **PM2**) and updates the API statuses at regular intervals (e.g., every 5 minutes).
- The results are stored and exposed via an endpoint (e.g., `/check-apis`), which returns a JSON array with the current status of each API.
- This enables real-time monitoring of API availability.

## 5. Website Visualization
- The website makes AJAX requests to the `/check-apis` endpoint to fetch the latest results and displays them in a table.
- The table shows the validator's name, API endpoint, status (e.g., **Working** or **Offline**), and additional details.
- This allows users to quickly identify which APIs are operational and which ones are experiencing issues.

## Summary
The API verification tool combines automated JSON retrieval, real-world navigation simulation using Puppeteer, and periodic status updates to provide real-time monitoring of various APIs. This setup helps administrators quickly detect and address any issues with the API endpoints.
