# ADS4GPTs GPT Text Ad Specification

## 1. Introduction

**Purpose**  
The **ADS4GPTs GPT Text Ad Specification** sets additional requirements for delivering **text-based ads** in a chat or AI-centric environment. It **extends** the **ADS4GPTs Base Ad Specification** by defining fields specific to GPT text ads, ensuring a **clear, consistent**, and **user-friendly** approach.

---

## 2. Relationship to the Base Spec

All GPT Text Ads must first comply with the **ADS4GPTs Base Ad Specification**. Specifically, they must include the **base fields**:

1. **version** (string; required)  
2. **ad_id** (string; required)  
3. **advertiser** (string; required)  
4. **disclosure_label** (string; required)  
5. **ad_type** (string; required)  
6. **ad_data** (object; required)  
7. **metadata** (object; optional)

In this GPT Text Ad Spec:

- **ad_type** is set to `"text"` or another relevant keyword (e.g., `"gpt_text"`).
- **ad_data** includes **GPT text-specific fields**, as outlined below.

---

## 3. GPT Text Ad Fields

Within `ad_data`, GPT Text Ads add or refine the following **required** and **optional** fields. This structure **extends** (rather than replaces) the base spec.

| **Field**  | **Type**   | **Required** | **Description**                                                                                                                                          |
|------------|-----------|-------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| **headline** | string   | Yes         | A short, attention-grabbing line introducing the ad’s main message (30–90 recommended characters).                                                        |
| **body**     | string   | Yes         | The main text of the ad, providing more detail or context (70–280 recommended characters).                                                               |
| **cta**      | string   | No          | A concise **call to action** (25–35 recommended characters). E.g., "Learn More," "Shop Now."                                                             |
| **cta_url**  | string   | No          | A URL to which the user is directed if they click or tap the CTA link. Must be secure (HTTPS) and typically shortened (25–50 characters if possible).     |

> **Note:** The **headline** and **body** fields are mandatory, while **cta** and **cta_url** are optional. These constraints help maintain consistency in GPT-based text ads that appear in chat streams.

---

## 4. Example: GPT Text Ad

Here is a minimal JSON example illustrating the **base fields** plus the **GPT Text Ad** fields inside `ad_data`:

```json
{
  "version": "1.0",
  "ad_id": "123e4567-e89b-12d3-a456-426614174000",
  "advertiser": "BrandXYZ",
  "disclosure_label": "Sponsored",
  "ad_type": "text",
  "ad_data": {
    "headline": "Exclusive Offer!",
    "body": "Save 20% on your next purchase. Limited time only!",
    "cta": "Shop Now",
    "cta_url": "https://www.brandxyz.com/offer"
  },
  "metadata": {
    "tags": ["promotion", "discount"]
  }
}
```

---

## 5. Guidelines & Best Practices

1. **Length and Readability**  
   - Keep **headline** concise (30–90 characters).  
   - Keep **body** brief but informative (70–280 characters).  
   - Respect any character limits imposed by your platform or UI constraints.

2. **CTA Usage**  
   - If you include a CTA, ensure it is clear, action-oriented, and relevant.  
   - Include `cta_url` if you want to direct users to an external link.  

3. **Markdown Support**  
   - If your GPT environment supports Markdown, you may format **headline** and **body** with basic Markdown (e.g., **bold** or *italic*) to enhance readability.  
   - Verify any restrictions on clickable links or formatting within your specific platform.

4. **Disclosure**  
   - The **disclosure_label** (from the Base Spec) must be present and clearly displayed, e.g., “Sponsored” or “Ad.”  
   - Position it in line with platform policies (e.g., at the top or bottom of the ad message).

5. **Tracking & Metrics**  
   - Use `ad_id` for impression/click tracking.  
   - If additional analytics or targeting data is needed, store it in the **metadata** object.

---

## 6. Rendering & Delivery

- **Placement**  
  - GPT Text Ads typically appear after natural conversation breaks or clearly separated from user-generated text.  
- **Formatting**  
  - The environment or application (e.g., chat-based UI) can adapt the text to blend with conversation style (subject to any “blend” rules in the base spec).  
- **Extensibility**  
  - If you require advanced text ad features (e.g. custom placeholders and specific personalization), you can add those fields to `metadata` or within `ad_data`.

---

