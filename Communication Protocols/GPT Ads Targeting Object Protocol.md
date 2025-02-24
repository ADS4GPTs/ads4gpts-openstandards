**gpt_ads_targeting_object: protocol_specification**  
*version 0.0.1 – january 28, 2025*

---

## table_of_contents

1. [Introduction](#1-introduction)  
2. [Schema Definition](#2-schema-definition)  
3. [Field Explanation](#3-field-explanation)  
    - 3.1 [id](#31-id)  
    - 3.2 [user](#32-user)  
    - 3.3 [ad_recommendation](#33-ad_recommendation)  
    - 3.4 [undesired_ads](#34-undesired_ads)  
    - 3.5 [context](#35-context)  
    - 3.6 [num_ads](#36-num_ads)  
    - 3.7 [style](#37-style)  
4. [Categorical Fields](#4-categorical-fields)  
    - 4.1 [gender](#41-gender)  
    - 4.2 [age_range](#42-age_range)  
5. [Backwards Compatibility with OpenRTB](#5-backwards-compatibility-with-openrtb)  
6. [Privacy First](#6-privacy-first)  
7. [Example Object](#7-example-object)  

---

## 1. Introduction

The **gpt_ads_targeting_object** is a streamlined data structure designed to help AI-powered publishers communicate essential, **privacy-first** user insights to advertising demand platforms, specifically in the context of real-time bidding (RTB). By using **categorical** fields for certain user demographics and **rich-text** fields for more nuanced information, this specification ensures an **optimal balance** between **targeting accuracy** and **user privacy**.

**key_attributes**:
- **ai-first**: Designed to be automatically created by AI systems analyzing user interactions.
- **standardized** data format: Ensures consistent communication across ad platforms.
- **privacy-first**: Designed with data minimization and privacy protection as core principles. Minimizes the potential for direct user identification.
- **categorical** constraints on select fields: Prevents over-granularity in personally sensitive data.  
- **openrtb** backward compatibility: Possible integration with standard programmatic advertising frameworks.

---

## 2. Schema Definition

The gpt_ads_targeting_object consists of a simple JSON structure:

```json
{
  "id": "string",
  "user": {
     "gender": "string",
     "age_range": "string",
     "persona": "string"
  },
  "ad_recommendation": "string",
  "undesired_ads": "string",
  "context": "string",
  "num_ads": "integer",
  "style": "string"
}
```

Where:
- **id**: A unique identifier for the session or user (hashed or anonymized to ensure privacy).  
- **user**: Basic user attributes.  
- **ad_recommendation**: A free-text description of ads relevant to the user.  
- **undesired_ads**: A free-text or enumerated reference to ads the user does not wish to see.
- **context**: A summary of the context the ad is going to be in.
- **num_ads**: The number of ads desired.
- **style**: The style description of the AI application, defaults to "neutral".

---

## 3. Field Explanation

### 3.1 id  
- **type**: `string`  
- **description**: A **unique identifier** used by the publisher to differentiate sessions or anonymized users. It must **not** contain personally identifiable information (PII).  
- **example**: `"07f2c1139e4a1c8d"`

### 3.2 user  
#### `gender`  
- **type**: `string` (categorical)  
- **description**: A non-PII identifier for the user’s gender.  

#### `age_range`  
- **type**: `string` (categorical)  
- **description**: A bracketed or labeled range for the user’s age.  

#### `persona`  
- **type**: `string` (rich_text / informational)  
- **description**: A free-text descriptor providing a high-level view of user interests or behaviors.  
- **example**: `"fitness-focused frequent traveler with an interest in adventure sports"`

### 3.3 ad_recommendation  
- **type**: `string` (rich_text)  
- **description**: A potentially detailed free-text suggestion for the type of ads that would be relevant to the user.  
- **example**: `"budget airline promos, hiking gear discounts, local travel insurance offers"`

### 3.4 undesired_ads  
- **type**: `string` (rich_text)  
- **description**: A flexible free-text field listing categories, product types, or ad content the user does **not** want to see.  
- **example**: `"luxury travel packages, gambling, diet supplements"`

### 3.5 context  
- **type**: `string` (rich_text)  
- **description**: A summary of the context the ad is going to be in.  
- **example**: `"user browsing travel blogs"`

### 3.6 num_ads  
- **type**: `integer`  
- **description**: The number of ads desired.  
- **example**: `3`

### 3.7 style  
- **type**: `string`  
- **description**: The style description of the AI application, defaults to "neutral".  
- **example**: `"neutral"`

---

## 4. Categorical Fields

This protocol enforces categorical values for **gender** and **age_range** to limit excessive granularity that could lead to privacy risks.

### 4.1 gender
The publisher may restrict the gender field to a finite, commonly accepted set of strings, for example:
- `male`  
- `female`  
- `non_binary`  
- `undisclosed`

**note**: The AI agent may offer additional or alternate categories that align with its user base and regional regulations, but it should remain within a concise set to avoid exposing overly specific identity details.

### 4.2 age_range
Possible enumerations (though final naming and intervals are at the publisher’s discretion):
- `under_18`  
- `18-24`  
- `25-34`  
- `35-44`  
- `45-54`  
- `55-64`  
- `65_over`  
- `undisclosed`

---

## 5. Backwards Compatibility with OpenRTB

When integrating into standard OpenRTB workflows, the gpt_ads_targeting_object can be included within an OpenRTB `BidRequest` as an **extension** (e.g., `ext.gpt_targeting`). Below is a simplified illustration:

```json
{
  "id": "07f2c1139e4a1c8d", 
  "imp": [
     {
        "id": "1",
        "banner": {
          "w": 300,
          "h": 250
        }
     }
  ],
  "site": {
     "domain": "example.com"
  },
  "ext": {
     "gpt_targeting": {
        "id": "07f2c1139e4a1c8d",
        "user": {
          "gender": "female",
          "age_range": "25-34",
          "persona": "fitness-focused frequent traveler interested in adventure sports"
        },
        "ad_recommendation": "budget airline promos, hiking gear discounts, local travel insurance offers",
        "undesired_ads": "luxury travel packages, gambling",
        "context": "user browsing travel blogs",
        "num_ads": 3,
        "style": "neutral"
     }
  }
}
```

1. **id**: Matches the parent `BidRequest` (preferred) or can be a separate unique user/session ID.  
2. **user**: Core user traits are inserted in a structured format.  
3. **ad_recommendation** & **undesired_ads**: Passed in free-text form for the DSP to interpret.  
4. **context**: Provides context for the ad placement.
5. **num_ads**: Specifies the number of ads desired.
6. **style**: Describes the style of the AI application, defaults to "neutral".

The demand-side platform (DSP) can parse `ext.gpt_targeting` to tailor bidding decisions and creative selection accordingly, *while remaining compatible with standard IAB frameworks*.

---

## 6. Privacy First

1. **No direct PII**:  
    - The `id` field should be hashed or otherwise anonymized.  
    - `gender` and `age_range` are categorical to avoid pinpointing individuals.  

2. **Aggregated Data**:  
    - `persona` is high-level, interest-based text.  
    - The AI should avoid embedding any personal identifiers (e.g., names, emails) in the free-text fields.

3. **Compliance**:  
    - Publishers should adhere to GDPR, CCPA, or other relevant legislation.  
    - Obtain valid consent for data processing if required.
    - These are out of the scope of the targeting object since it is not a standalone Ad Request.

4. **Fallback for Undisclosed**:  
    - If a user opts out of sharing certain data, the AI can set `gender` or `age_range` to `"undisclosed"`.

---

## 7. Example Object

Below is a direct JSON example, **outside** an RTB bid request structure:

```json
{
  "id": "83291fda29cbe49",
  "user": {
     "gender": "female",
     "age_range": "25-34",
     "persona": "frequent yoga practitioner interested in eco-friendly products"
  },
  "ad_recommendation": "organic yoga mats, sustainable fitness apparel",
  "undesired_ads": "gambling, dietary supplements, extravagant luxury goods",
  "context": "user browsing fitness blogs",
  "num_ads": 2,
  "style": "neutral"
}
```

