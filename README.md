# Topic 6 – Handling API Responses, Errors, and Rate Limits

## Overview

This project demonstrates how to handle API errors, configure retry options, fetch data using pagination, manage API request delays, and create a global error handler using n8n.

## Objectives

- Configure automatic retries for failed API requests.
- Handle API errors using Continue on Fail.
- Fetch user data using pagination.
- Add a delay between API requests.
- Create a Global Error Handler workflow.

## Tools Used

- n8n
- Postman
- Postman Echo API
- JSONPlaceholder API
- ReqRes API

## Workflow 1 – Main Workflow

### 1. Configure HTTP Request Retries

**Node Name:** Call Flaky API

- Method: GET
- URL: `https://postman-echo.com/status/500`
- Retry On Fail: Enabled
- Max Tries: 3
- Wait Between Tries: 2000 ms

This node retries the API request when the server returns an error.

### 2. Continue on Fail

**Node Name:** Call Flaky API

- Method: GET
- URL: `https://jsonplaceholder.typicode.com/invalid-route-9988`
- On Error: Continue (using error output)

This step handles API errors without stopping the entire workflow.

### 3. API Pagination

**Node Name:** Fetch User Page

- Method: GET
- URL: `https://reqres.in/api/users?page={{ $json.currentPage }}`

This node fetches user data from different API pages.

### 4. Increment Page

**Node Name:** Increment Page

This node increases the current page number to fetch the next page.

### 5. Wait Node

**Node Name:** Wait

- Wait Amount: 1
- Wait Unit: Seconds

This node adds a one-second delay between API requests to help avoid rate limits.

### 6. Loop Completion

The If node checks whether the API response contains user records.

- True: Continue to the next page.
- False: Stop the pagination loop.

A maximum of five iterations is configured as a safety limit.

## Workflow 2 – Global Error Handler

### Nodes

1. Error Trigger
2. Format Error Alert (Edit Fields)



This workflow captures unhandled errors from the main workflow.


All requests use No Auth and do not require a request body.

## Conclusion

In this topic, I learned how to configure API retries, handle errors, implement pagination, add delays between API requests, and create a global error handler workflow in n8n.