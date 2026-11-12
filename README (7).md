# Topic 6: Data Transformation (n8n)

## Overview

This workflow cleans and standardizes unformatted customer data using
n8n's **Manual Trigger** and **Edit Fields (Set)** nodes.

It demonstrates: - Mapping raw input keys to a standard output schema -
Removing leading and trailing whitespace with JavaScript `.trim()` -
Converting a customer name to uppercase with `.toUpperCase()` -
Converting an email address to lowercase with `.toLowerCase()` -
Excluding original input fields from the final payload

## Workflow

``` text
Manual Trigger
     |
     v
Dirty Raw Data (Edit Fields)
     |
     v
Reformat & Map (Edit Fields)
     |
     v
Final cleaned JSON
```

## Sample Input

The `Dirty Raw Data` node produces these three string fields:

  Field             Sample value
  ----------------- --------------------
  `Raw_Name`        `Jane DOE`
  `contact_email`   `JANE@Example.com`
  `phone_number`    `123-456-7890`

The spaces at the beginning and end of each value are intentional test
data.

## Expected Output

The `Reformat & Map` node should return only these three properties:

``` json
[
  {
    "cleanName": "JANE DOE",
    "cleanEmail": "jane@example.com",
    "cleanPhone": "123-456-7890"
  }
]
```

## Nodes

1.  **Manual Trigger** --- starts the workflow when the user clicks
    **Test workflow**.
2.  **Dirty Raw Data** --- creates the three mock input fields.
3.  **Reformat & Map** --- trims and standardizes the values, maps them
    to the destination keys, and excludes the original fields.

## How to Run

1.  Open the workflow in n8n.
2.  Click **Test workflow**.
3.  Confirm that all nodes execute successfully.
4.  Open the `Reformat & Map` node's output and select JSON.
5.  Compare the result with the expected output above.

## Requirements

See [`requirements.md`](requirements.md) for functional and technical
requirements.

## Implementation

See [`implementation.md`](implementation.md) for node configuration and
expressions.

## Test Summary

The completed workflow was manually executed. The observed output
matched the expected JSON structure and values. The three workflow nodes
showed successful execution.

The accompanying test results recorded 12 test cases as PASS based on
the completed exercise.
