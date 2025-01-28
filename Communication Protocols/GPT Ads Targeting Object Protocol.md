**GPT Ads Targeting Object: Protocol Specification**  
*Version 0.0.1 – January 28, 2025*

---

## Table of Contents

1. [Introduction](#1-introduction)  
2. [Schema Definition](#2-schema-definition)  
3. [Field-by-Field Explanation](#3-field-by-field-explanation)  
   - 3.1 [id](#31-id)  
   - 3.2 [User](#32-user)  
   - 3.3 [Ad Recommendation](#33-ad-recommendation)  
   - 3.4 [Undesired Ads](#34-undesired-ads)  
4. [Categorical Fields](#4-categorical-fields)  
   - 4.1 [gender](#41-gender)  
   - 4.2 [age_range](#42-age_range)  
5. [Backwards Compatibility with OpenRTB/IAB Standards](#5-integration-with-openrtbiab-standards)  
6. [Privacy and Data Protection](#6-privacy-and-data-protection)  
7. [Example Targeting Object](#7-example-targeting-object)  

---

## 1. Introduction

The **GPT Ads Targeting Object** is a streamlined data structure designed to help AI-powered publishers communicate essential, **Privacy-First** user insights to advertising demand platforms, specifically in the context of Real-Time Bidding (RTB). By using **categorical** fields for certain user demographics and **rich-text** fields for more nuanced information, this specification ensures an **optimal balance** between **targeting accuracy** and **user privacy**.

**Key Attributes**:
- **AI-First**: Designed to be automatically created by AI systems analyzing user interactions.
- **Standardized** data format: Ensures consistent communication across ad platforms.
- **Privacy-First**: Designed with data minimization and privacy protection as core principles.  Minimizes the potential for direct user identification.
- **Categorical** constraints on select fields: Prevents over-granularity in personally sensitive data.  
- **OpenRTB** backward compatibility: Possible integration with standard programmatic advertising frameworks.

---

## 2. Schema Definition

The GPT Ads Targeting Object consists of a simple JSON structure:

```json
{
  "id": "string",
  "User": {
    "gender": "string",
    "age_range": "string",
    "persona": "string"
  },
  "Ad Recommendation": "string",
  "Undesired Ads": "string"
}
```

Where:
- **id**: A unique identifier for the session or user (hashed or anonymized to ensure privacy).  
- **User**: Basic user attributes.  
- **Ad Recommendation**: A free-text description of ads relevant to the user.  
- **Undesired Ads**: A free-text or enumerated reference to ads the user does not wish to see.

---

## 3. Field-by-Field Explanation

### 3.1 id  
- **Type**: `string`  
- **Description**: A **unique identifier** used by the publisher to differentiate sessions or anonymized users. It must **not** contain personally identifiable information (PII).  
- **Example**: `"07f2c1139e4a1c8d"`

### 3.2 User  
#### `gender`  
- **Type**: `string` (Categorical)  
- **Description**: A non-PII identifier for the user’s gender.  

#### `age_range`  
- **Type**: `string` (Categorical)  
- **Description**: A bracketed or labeled range for the user’s age.  

#### `persona`  
- **Type**: `string` (Rich text / Informational)  
- **Description**: A free-text descriptor providing a high-level view of user interests or behaviors.  
- **Example**: `"Fitness-focused frequent traveler with an interest in adventure sports"`

### 3.3 Ad Recommendation  
- **Type**: `string` (Rich text)  
- **Description**: A potentially detailed free-text suggestion for the type of ads that would be relevant to the user.  
- **Example**: `"Budget airline promos, hiking gear discounts, local travel insurance offers"`

### 3.4 Undesired Ads  
- **Type**: `string` (Rich text)  
- **Description**: A flexible free-text field listing categories, product types, or ad content the user does **not** want to see.  
- **Example**: `"Luxury travel packages, gambling, diet supplements"`

---

## 4. Categorical Fields

This protocol enforces categorical values for **gender** and **age_range** to limit excessive granularity that could lead to privacy risks.

### 4.1 gender
The publisher may restrict the gender field to a finite, commonly accepted set of strings, for example:
- `male`  
- `female`  
- `non_binary`  
- `undisclosed`

**Note**: The AI agent may offer additional or alternate categories that align with its user base and regional regulations, but it should remain within a concise set to avoid exposing overly specific identity details.

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

## 5. Backwards Compatibility with OpenRTB/IAB Standards

When integrating into standard OpenRTB workflows, the GPT Ads Targeting Object can be included within an OpenRTB `BidRequest` as an **extension** (e.g., `ext.gpt_targeting`). Below is a simplified illustration:

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
      "User": {
        "gender": "female",
        "age_range": "25-34",
        "persona": "Fitness-focused frequent traveler interested in adventure sports"
      },
      "Ad Recommendation": "Budget airline promos, hiking gear discounts, local travel insurance offers",
      "Undesired Ads": "Luxury travel packages, gambling"
    }
  }
}
```

1. **id**: Matches the parent `BidRequest` (preferred) or can be a separate unique user/session ID.  
2. **User**: Core user traits are inserted in a structured format.  
3. **Ad Recommendation** & **Undesired Ads**: Passed in free-text form for the DSP to interpret.  

The Demand-Side Platform (DSP) can parse `ext.gpt_targeting` to tailor bidding decisions and creative selection accordingly, *while remaining compatible with standard IAB frameworks*.

---

## 6. Privacy and Data Protection

1. **No Direct PII**:  
   - The `id` field should be hashed or otherwise anonymized.  
   - `gender` and `age_range` are categorical to avoid pinpointing individuals.  

2. **Aggregated Data Only**:  
   - `persona` is high-level, interest-based text.  
   - The AI should avoid embedding any personal identifiers (e.g., names, emails) in the free-text fields.

3. **Regulatory Compliance**:  
   - Publishers should adhere to GDPR, CCPA, or other relevant legislation.  
   - Obtain valid consent for data processing if required.
   - These are out of the scope of the targeting object since it is not a standalone Ad Request.

4. **Fallback for Undisclosed**:  
   - If a user opts out of sharing certain data, the AI can set `gender` or `age_range` to `"undisclosed"`.

---

## 7. Example Targeting Object

Below is a direct JSON example, **outside** an RTB bid request structure:

```json
{
  "id": "83291fda29cbe49",
  "User": {
    "gender": "female",
    "age_range": "25-34",
    "persona": "Frequent yoga practitioner interested in eco-friendly products"
  },
  "Ad Recommendation": "Organic yoga mats, sustainable fitness apparel",
  "Undesired Ads": "Gambling, dietary supplements, extravagant luxury goods"
}
```

