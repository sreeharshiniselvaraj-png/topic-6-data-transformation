# Implementation Document

## Topic 6: Data Transformation (n8n)

## 1. Workflow Details

**Workflow name:** `Topic 6 Data Transformation Workflow`

**Node sequence:**

``` text
Manual Trigger -> Dirty Raw Data -> Reformat & Map
```

## 2. Node 1: Manual Trigger

1.  Add the **Manual Trigger** node (shown in n8n as **When clicking
    "Test workflow"**).
2.  Use it to start the workflow manually during testing.
3.  Connect it to the `Dirty Raw Data` node.

## 3. Node 2: Dirty Raw Data

1.  Add an **Edit Fields (Set)** node after the Manual Trigger.
2.  Rename it to `Dirty Raw Data`.
3.  Set the node to **Manual Mapping**.
4.  Add the following three fields. Set each field type to **String**.

  Field name        Value
  ----------------- --------------------
  `Raw_Name`        `Jane DOE`
  `contact_email`   `JANE@Example.com`
  `phone_number`    `123-456-7890`

The leading and trailing spaces are intentional. If the normal text
input removes them, use the node's Expression mode to create the string
value. In Expression mode, enter an expression such as
`{{ '   Jane DOE   ' }}`. Ensure the resulting value does not contain
literal quote characters.

5.  Execute this node and inspect its JSON output. Verify that all three
    values retain their leading and trailing spaces.

## 4. Node 3: Reformat & Map

1.  Add a second **Edit Fields (Set)** node after `Dirty Raw Data`.
2.  Rename it to `Reformat & Map`.
3.  Set the node to **Manual Mapping**.
4.  Set **Include Other Input Fields** to **Off/Disabled**.
5.  Add the following three fields, each with type **String**.

### Field A: cleanName

**Name:** `cleanName`

**Expression:**

``` javascript
{{ $json.Raw_Name.trim().toUpperCase() }}
```

This reads `Raw_Name`, removes leading/trailing whitespace, and converts
the remaining text to uppercase.

### Field B: cleanEmail

**Name:** `cleanEmail`

**Expression:**

``` javascript
{{ $json.contact_email.trim().toLowerCase() }}
```

This reads `contact_email`, removes leading/trailing whitespace, and
converts the remaining text to lowercase.

### Field C: cleanPhone

**Name:** `cleanPhone`

**Expression:**

``` javascript
{{ $json.phone_number.trim() }}
```

This reads `phone_number` and removes leading/trailing whitespace. The
internal hyphens are preserved.

## 5. Execute and Verify

1.  Close the node configuration panel.
2.  Click **Test workflow**.
3.  Confirm that the Manual Trigger, Dirty Raw Data, and Reformat & Map
    nodes execute successfully.
4.  Open the `Reformat & Map` node output and select JSON.
5.  Confirm that the output contains exactly the following fields and
    values:

``` json
[
  {
    "cleanName": "JANE DOE",
    "cleanEmail": "jane@example.com",
    "cleanPhone": "123-456-7890"
  }
]
```

6.  Confirm that `Raw_Name`, `contact_email`, and `phone_number` are not
    present in the final output.
7.  Save the workflow.

## 6. Verification Checklist

-   [x] Manual Trigger added.
-   [x] `Dirty Raw Data` node configured with three string fields.
-   [x] Raw values contain leading and trailing spaces.
-   [x] `Reformat & Map` node configured with three string output
    fields.
-   [x] `.trim()` applied to all three input values.
-   [x] `.toUpperCase()` applied to the name.
-   [x] `.toLowerCase()` applied to the email.
-   [x] **Include Other Input Fields** disabled.
-   [x] Final output contains only the three clean keys.
-   [x] Complete workflow execution verified successfully.

## 7. Execution Result

The completed workflow was executed in n8n. The final observed JSON
output matched the expected output shown above, and all three workflow
nodes displayed successful execution indicators.
