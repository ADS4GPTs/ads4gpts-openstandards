## ADS4GPTs AdCOM Extension

### 1. Overview

The **AdCOM `Ad` object** (IAB Tech Lab) is the root entity describing an ad creative. By default, it includes attributes such as `id`, `adomain`, `iurl`, and so on. To incorporate **ADS4GPTs GPT Ads**, we create a new `gpt` field in the **Ad Object**. This has the Ad info when a ADS4GPTs compatible placement is requested.

### 2. Extended `Ad` Object Structure


| **Attribute**  | **Type**                | **Definition**                                                                                                                                                                                                                                                |
|----------------|-------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **id**         | string (required)      | Unique ID of the creative. (Can mirror `ad_id` from ADS4GPT.)                                                                                                                                                                                                 |
| **adomain**    | string array           | Advertiser domain; top two levels (e.g., `"brandxyz.com"`).                                                                                                                                                                                                   |
| **bundle**     | string array           | App package name for in-app ads (optional).                                                                                                                                                                                                                   |
| **iurl**       | string                 | URL to a representative image of the ad for quality review.                                                                                                                                                                                                    |
| **cat**        | string array           | Content categories for the ad (per `cattax`).                                                                                                                                                                                                                 |
| **cattax**     | integer                | Indicates which taxonomy is used in `cat`. Default `2`.                                                                                                                                                                                                        |
| **lang**       | string                 | Language of the creative using ISO-639-1-alpha-2, or “xx” if none.                                                                                                                                                                                             |
| **attr**       | integer array          | Set of creative attributes.                                                                                                                                                                                                                                    |
| **secure**     | integer                | Flag indicating if the creative is HTTPS-based (0=No, 1=Yes).                                                                                                                                                                                                  |
| **mrating**    | integer                | Media rating per IQG guidelines.                                                                                                                                                                                                                               |
| **init**       | integer                | Unix timestamp (ms) of original ad instantiation.                                                                                                                                                                                                              |
| **lastmod**    | integer                | Unix timestamp (ms) of most recent modification.                                                                                                                                                                                                               |
| **gpt**    | object (required\*)    | Details for a GPT Ad if applicable (see ADS4GPTs Ad Specifications).                                                                                                     
| **display**    | object (required\*)    | Details for a display ad if applicable (see AdCOM).                                                                                                                                                                                                            |
| **video**      | object (required\*)    | Details for a video ad if applicable (see AdCOM).                                                                                                                                                                                                              |
| **audio**      | object (required\*)    | Details for an audio ad if applicable (see AdCOM).                                                                                                                                                                                                             |
| **audit**      | object                 | Depicts the audit status of the ad.                                                                                                                                                                                                                            |
| **ext**        | object                 | **Extension for vendor-specific fields.**                                                                                                                                                                                                                      |

> \*One media subtype (`gpt`, `display`, `video`, or `audio`) is generally required in this AdCOM extention unless another convention is used for specifying creative content.

---

### 3. `gpt` Schema

The `gpt` field forllows the GPT Ad Specifications for all types which is the base Specification. In the **ADS4GPTs Base Ad Specification**, the following core fields are required or optional.

| **Field**           | **Type**   | **Required** | **Description**                                                                         |
|---------------------|-----------|-------------|-----------------------------------------------------------------------------------------|
| **version**         | string     | Yes         | ADS4GPT spec version, e.g., `"1.0"`.                                                   |
| **ad_id**           | string     | Yes         | Unique ad identifier in ADS4GPT (can reuse AdCOM’s `id` if consistent).                |
| **advertiser**      | string     | Yes         | Name of the advertiser or brand (e.g., `"BrandXYZ"`).                                  |
| **disclosure_label**| string     | Yes         | Required label indicating sponsored content (e.g., `"Sponsored"`).                     |
| **ad_type**         | string     | Yes         | The type of ad (e.g., `"text"`, `"image"`, `"video"`) per ADS4GPT.                     |
| **ad_data**         | object     | Yes         | Format-specific data (text copy, image URLs, etc.)                                     |
| **metadata**        | object     | No          | Optional data for targeting, analytics, or advanced features.                          |

> **Note:** AdCOM’s top-level fields (e.g., `id`) **may** mirror or duplicate fields in ADS4GPT (e.g., `ad_id`) to preserve consistency across systems. In practice, you can choose to unify them or maintain separate IDs if required by different platforms.

Ad types and Ad Data follow the specifications of different ADS4GPTs types. [Read more.](.)

---

### 4. Example JSON

Below is a minimal **AdCOM `Ad` object** extended with **ADS4GPT** fields in `ext.ads4gpt`:

```json
{
  "id": "123e4567-e89b-12d3-a456-426614174000",
  "adomain": ["brandxyz.com"],
  "iurl": "https://img.adserver.com/creative.jpg",
  "cat": ["IAB2"],
  "cattax": 2,
  "lang": "en",
  "attr": [1, 2],
  "secure": 1,
  "init": 1706526000000,
  "lastmod": 1706579200000,
  "gpt": {
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
    },
  }
}
```

