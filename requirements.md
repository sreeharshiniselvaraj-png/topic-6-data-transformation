# Requirements Document

## Topic 6: Data Transformation (n8n)

## 1. Purpose

Define the functional and technical requirements for an n8n workflow
that cleans, reformats, and maps raw customer data into a standardized
JSON payload.

## 2. Objective

The workflow shall transform unformatted customer data by: - Renaming
raw input keys to standardized destination keys. - Removing leading and
trailing whitespace. - Converting the customer name to uppercase. -
Converting the email address to lowercase. - Excluding original raw
fields from the final output.

## 3. Scope

### 3.1 In Scope

-   Creating three mock string fields representing customer name, email,
    and phone number.
-   Mapping `Raw_Name` to `cleanName`.
-   Mapping `contact_email` to `cleanEmail`.
-   Mapping `phone_number` to `cleanPhone`.
-   Applying JavaScript `.trim()`, `.toUpperCase()`, and
    `.toLowerCase()` methods in n8n expressions.
-   Using an Edit Fields (Set) node with **Include Other Input Fields**
    disabled.
-   Producing a flat JSON object containing only the three clean
    properties.

### 3.2 Out of Scope

-   Regex-based email validation.
-   Phone-number normalization or country-code validation.
-   Nested array processing or object flattening.
-   Third-party JavaScript libraries for string manipulation.

## 4. Preconditions

-   The trainee has access to an active n8n instance.
-   The trainee is familiar with basic n8n workflow execution.
-   Input values are populated strings and are non-null.
-   The cleanup rules are statically defined in expressions.

## 5. Functional Requirements

  ------------------------------------------------------------------------
  ID                Requirement       Description        Priority
  ----------------- ----------------- ------------------ -----------------
  REQ-001           Dirty mock data   The workflow shall High
                                      create `Raw_Name`, 
                                      `contact_email`,   
                                      and `phone_number` 
                                      as strings         
                                      containing         
                                      leading/trailing   
                                      spaces and the     
                                      specified sample   
                                      casing.            

  REQ-002           Schema mapping    The workflow shall High
                                      map `Raw_Name` to  
                                      `cleanName`,       
                                      `contact_email` to 
                                      `cleanEmail`, and  
                                      `phone_number` to  
                                      `cleanPhone`.      

  REQ-003           Whitespace        The workflow shall High
                    trimming          remove leading and 
                                      trailing           
                                      whitespace from    
                                      all three values   
                                      using `.trim()`.   

  REQ-004           Case              The workflow shall High
                    standardization   convert            
                                      `cleanName` to     
                                      uppercase and      
                                      `cleanEmail` to    
                                      lowercase.         

  REQ-005           Final payload     The final output   High
                                      shall contain only 
                                      `cleanName`,       
                                      `cleanEmail`, and  
                                      `cleanPhone`;      
                                      original input     
                                      fields shall be    
                                      excluded.          
  ------------------------------------------------------------------------

## 6. Technical Requirements

  --------------------------------------------------------------------------
  ID                      Requirement             Description
  ----------------------- ----------------------- --------------------------
  TR-001                  Chained JavaScript      n8n expressions shall
                          methods                 support chained string
                                                  methods, including
                                                  `.trim().toUpperCase()`
                                                  and
                                                  `.trim().toLowerCase()`.

  TR-002                  Output selectivity      The final Edit Fields
                                                  (Set) node shall have
                                                  **Include Other Input
                                                  Fields** disabled so that
                                                  only the newly defined
                                                  clean fields are emitted.
  --------------------------------------------------------------------------

## 7. Sample Data

  Input key         Input value
  ----------------- --------------------
  `Raw_Name`        `Jane DOE`
  `contact_email`   `JANE@Example.com`
  `phone_number`    `123-456-7890`

## 8. Expected Output

``` json
[
  {
    "cleanName": "JANE DOE",
    "cleanEmail": "jane@example.com",
    "cleanPhone": "123-456-7890"
  }
]
```

## 9. Assumptions and Constraints

-   **ASM-001:** All incoming values are populated, non-null strings.
-   **CON-001:** Cleanup rules are statically defined in the expression
    fields and do not dynamically inspect input types.
-   The phone number is trimmed only; its internal formatting is
    preserved.
