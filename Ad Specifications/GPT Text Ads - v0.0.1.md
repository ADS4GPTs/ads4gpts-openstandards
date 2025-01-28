# GPT Text Ads

GPT Text ads appear as **textual messages** within a chat or messaging interface. They typically blend with the look and feel of user-generated messages but are clearly labeled to indicate sponsorship. These ads prioritize **minimal disruption**, **no file weight**, and **clear labeling**.

## Definitions and Terminology

- **GPT Text Ad:** A textual advertising message inserted within a chat or messaging flow.
- **Advertiser:** The party responsible for the ad content.
- **Publisher:** The environment or application hosting the GPT text ad. Most likely an AI application or GPT.

## Introduction

**1. Purpose and Scope**  
This specification outlines a standardized format for delivering GPT text advertisements. It aims to ensure consistency, transparency, and interoperability across different platforms and applications. By adhering to this spec, advertisers and publishers can foster user trust and provide a seamless, unobtrusive ad experience.

**2. Guiding Principles**
- **User Experience:** Keep ads contextually relevant and unobtrusive.
- **Transparency:** Clearly distinguish between user-generated content and sponsored messages.
- **Measurement & Reporting:** Provide consistent metrics and measurable campaign performance.


## GPT Text Ad Overview

| **Ad Type**                                       | **Ad Unit**                           | **Aspect Ratio** | **Recommended Dimensions (dp)** | **Max File Weight (kB)** | **Notes**                                                                                                                                                                                                                         |
|---------------------------------------------------|---------------------------------------|------------------|---------------------------------|--------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **GPT Text Ads**               | – “Sponsored Message” <br> – Inline Text Ads | N/A              | N/A (Text-based)               | N/A (Text-based)          | - **Text-Only:** Negligible K-weight.  <br>- **Recommended Length:** 120–360 characters (including spaces). Title and Body. <br>- **Disclosure Label:** Required.                  |

### Key Characteristics
1. **Textual Only Content:** May include optional CTA or short URL. Text format is Markdown. 
2. **No Imagery:** Purely chat.  
3. **Separated from Application Content**: In purely text applications this means the Ad should start after a line break.
3. **Clearly Marked as Sponsored Content**: Must contain the label such as “Sponsored,” “Ad,” or “Promoted.”  
4. **Lightweight Tracking:** One tracking pixel or server-to-server logging for impressions/clicks.

---

## Ad Specifications

Below is an expanded specification following the **IAB New Ad Portfolio** structure—covering dimensions, file weight, and user experience considerations.

### Creative & Content Specifications


| **Content Element**   | **Description**                                                                    | **Max Characters**      | **Required** | **Notes**                                                                                                                                                                                                                                             |
|----------------------|------------------------------------------------------------------------------------|-------------------------|-------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Ad Identifier (AdID)** | A unique identifier for tracking and reporting (e.g., string or UUID).                                         | 36–128 characters      | Yes         | Ensure it is unique and consistent for accurate tracking and reporting.                                                                                                                                                                               |
| **Advertiser Name**   | Name of the brand or sponsor.                                                      | 50–100 characters      | Yes         | Ensure the name is concise and recognizable. May be truncated if the platform enforces shorter limits.                                                                                                        |
| **Headline**          | Ad headline that is followed by a new line.                                        | 30–90 recommended      | Yes         | Keep short for chat environments.                                                                                                                                                                              |
| **Body**              | Core message text (intro, brand message).                                          | 70–280 recommended     | Yes         | Keep short for chat environments.                                                                                                                                                                              |
| **Disclosure Label**  | Required label for sponsored content followed by a line break.                     | 10–25                  | Yes         | E.g., “Ad,” “Sponsored.” May appear at the beginning or end of the message, or as a separate label element.                                                                                                    |
| **Call to Action (CTA)** | A short prompt or link text (optional).                                           | 25–35                  | No          | Examples: “Learn More,” “Get Offer,” “Shop Now.” If the chat environment supports clickable text, it may be hyperlinked or appended as a short URL.                                                            |
| **CTA URL**           | Shortened link or deep link.                                                        | 25–50 (typical short URL) | No          | Must comply with platform link formatting. Use https for encryption. It is bundled in markdown with CTA.
| **Blend**             | Indicates if the ad content should be adapted to blend into the chat conversation or kept as is. | Binary (Yes/No)         | Yes         | If set to "Yes," the ad content should be adapted to match the chat's tone and style. If "No," the ad content remains unchanged.                                                                                 |                                                                                                             |
| **Metadata** | Object with useful information for Ad Optimization                                 | –                       | No          | Similar to the ext object of OpenRTB protocol.                                                                                                                                                                 |

---

## Delivery and Positioning

**Placement Rules**
- **Contextual Placement:** Ads appear at natural conversation breaks after the last AI message. Never inject an GPT Ad into the AI response.

**Ad Rendering**
- **Styling:** Should not be distracting to the User taking away from the Experience.   
- **Ad Tailoring:** The text ad can be made to fit the flow of the existing conversation of the user and the AI. This has to be in line with the Advertiser though and the "Blend" indicator. Ad tailoring can happen before the Ad hit the Publisher in an Supply Side Platform or in the Publisher's systems.


**Considations**
- **Accessibility:** Provide text-based alternatives for users with screen readers and comply with accessibility standards (e.g., color contrast guidelines).
- **User Controls:** Provide ways to minimize or close the Ad.

---

## Implementation Guidance

**JSON Schema**

```json
{
  "ad_id": "123e4567-e89b-12d3-a456-426614174000",
  "advertiser": "BrandXYZ",
  "disclosure_label": "Sponsored",
  "headline": "Exclusive Offer!",
  "body": "Save 20% on your next purchase. Limited time only!",
  "cta": "Shop Now",
  "cta_url": "https://www.brandxyz.com/offer",
  "metadata": {"tags": ["promotion", "shopping", "discount"]},
}
```

**Ad Rendering**

There is a need to transform the JSON schema of the GPT Text Ad spec into markdown that can be done in various ways. One way would be to synthesise the Ad deterministically by weaving the headline, body, cta and cta_url elements. Another way would be to pass it through another AI to blend the text seamlessly into the conversation. For the latter we give detailed guidelines in the GPT Text Ad Delivery Prompt Principles.

---

## Summary

**GPT / Conversation Text Ads** offer a **lightweight, native-like** format that blends into messaging platforms while upholding the **IAB New Ad Portfolio**’s LEAN principles. This specification ensures:

- **Minimal Disruption:** Ads appear as standard chat messages, with clear labeling.  
- **High Transparency:** Users instantly see they are viewing a sponsored message.  
- **Performance Efficiency:** Negligible initial load impact and minimal resource usage.  
- **Consistency:** Aligns with broader IAB guidelines on respectful, user-friendly digital advertising experiences.

Publishers and chat-platform developers can adopt these recommendations to standardize GPT ad placements—balancing monetization with user experience in **dynamic, real-time messaging** environments.