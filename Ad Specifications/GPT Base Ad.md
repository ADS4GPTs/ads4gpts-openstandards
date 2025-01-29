 ADS4GPTs Base Ad Specification

## 1. Introduction

**Purpose and Scope**  
This specification defines the **core fields** and **structures** needed to represent an ad in the ADS4GPTs system. All specialized ad types (like text ads, rich media ads, video ads) should conform to these base requirements and extend them with type-specific details. That way we establish consistency and enable extensibility for multiple ad types within ADS4GPts.

---

## 2. Key Concepts & Entities

1. **Advertiser**  
   The brand or organization responsible for creating and funding the advertisement.

2. **Publisher / Platform**  
   The environment or application where ads are delivered.

3. **Ad Type**  
   A categorical label indicating the format of the advertisement (e.g., `text`, `image`, `video`). Each ad type may require additional or specialized fields.

4. **Ad Data**  
   A structured set of fields containing the actual content or instructions for how the ad is presented. This data structure will vary depending on the `ad_type`.

5. **Metadata**  
   Supplemental information (e.g., targeting tags, frequency capping flags, or custom fields) that can be used for optimizations or reporting.

---

## 3. Base Spec Fields

The following core fields are **required for every ad** in ADS4GPTs. Some fields may be optional (`No` in the “Required” column), but a minimal set is necessary for any valid ad record.

| **Field**         | **Description**                                                                  | **Type**  | **Required** | **Notes**                                                                         |
|-------------------|----------------------------------------------------------------------------------|-----------|-------------|-----------------------------------------------------------------------------------|
| **version**       | API version or spec version to ensure backward compatibility                     | `string`  | Yes         | Example: `"1.0"` or `"2025.1"`                                                    |
| **ad_id**         | Unique identifier (UUID or similar) for the ad                                   | `string`  | Yes         | Used for tracking, reporting, and ensuring no duplicates                          |
| **advertiser**    | Name of the advertiser or brand                                                 | `string`  | Yes         | Used to display brand identity, e.g., `"BrandXYZ"`                                |
| **disclosure_label** | Text labeling this as “Sponsored,” “Ad,” etc.                                   | `string`  | Yes         | Must be clear to end users that this is promoted content                          |
| **ad_type**       | The format or category of the ad (e.g., `text`, `image`, `video`)                | `string`  | Yes         | Guides how `ad_data` is interpreted                                               |
| **ad_data**       | An object containing the **format-specific** fields (text copy, images, video, etc.)  | `object`  | Yes         | **Varies by `ad_type`**. For example, text ads vs. image ads have different fields|
| **metadata**      | Additional data for targeting, analytics, or other custom fields                 | `object`  | No          | Use for advanced features; can be empty if not used                               |

### 3.1 Field Definitions

- **version**  
  - **Purpose:** Controls backward or forward compatibility. 
  - **Example Values:** `"1.0"`, `"2.1-beta"`.

- **ad_id**  
  - **Purpose:** Unique reference to track performance, impressions, clicks, etc. 
  - **Format Example:** `"123e4567-e89b-12d3-a456-426614174000"`.

- **advertiser**  
  - **Purpose:** Identifies the sponsor or company behind the ad. 
  - **Usage:** Display name, brand name, or an easily recognizable label.

- **disclosure_label**  
  - **Purpose:** Ensures transparency to users about the promotional nature of the content.
  - **Usage:** “Sponsored,” “Ad,” “Promoted,” etc.

- **ad_type**  
  - **Purpose:** Dictates which specialized specification or schema to use for `ad_data`.
  - **Example Values:** `"text"`, `"image"`, `"video"`, `"rich_media"`.

- **ad_data**  
  - **Purpose:** Holds all the creative material required to render the ad, such as textual copy, image URLs, or video embed codes.
  - **Structure:** Varies by `ad_type`. For a text ad, it might contain `headline`, `body`, and `cta`; for an image ad, it might contain `image_url`, `alt_text`, etc.

- **metadata**  
  - **Purpose:** Provides optional data for advanced use cases (e.g., retargeting tags, user segments, frequency capping, geo-info).
  - **Structure:** Typically a JSON object with key-value pairs.

---

## 4. Example JSON (Base)

Below is a basic JSON object conforming to the Base Ad Specification. Note that `ad_data` is empty here as a placeholder; real-world usage will populate it according to the chosen `ad_type`.

```json
{
  "version": "1.0",
  "ad_id": "123e4567-e89b-12d3-a456-426614174000",
  "advertiser": "BrandXYZ",
  "disclosure_label": "Sponsored",
  "ad_type": "text",
  "ad_data": {},
  "metadata": {}
}
```

**Notes**  
- In a **text ad** scenario, you would later replace `ad_data` with content fields specific to text ads.  
- For an **image ad**, you would include fields like `image_url`, `alt_text`, etc. within `ad_data`.


---

## 5. Extending the Base Specification

When introducing a new ad format (e.g., **Text Ads** or **Video Ads**), you build on the **ADS4GPTs Base Ad Specification** by:

1. Declaring `ad_type` = `"text"`, `"banner"`, or `"video"`.  
2. **Defining** additional properties within `ad_data` that are relevant to that format (e.g., `headline`, `body` for text ads; `video_url`, `thumbnail` for video ads).  
3. Ensuring you do **not conflict** with existing base fields (e.g., do not override `ad_type` with a different meaning).  

