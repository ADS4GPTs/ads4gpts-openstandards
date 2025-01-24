# GPT Text Ads

GPT Text ads appear as **textual messages** within a chat or messaging interface. They typically blend with the look and feel of user-generated messages but are clearly labeled to indicate sponsorship. These ads prioritize **minimal disruption**, **no file weight**, and **clear labeling**.

## Definitions and Terminology

- **In-Chat Text Ad:** A textual advertising message inserted within a chat or messaging flow.
- **Advertiser:** The party responsible for the ad content.
- **Publisher:** The environment or application hosting the in-chat text ad. Most likely an AI application or GPT.

## Introduction

**1. Purpose and Scope**  
This specification outlines a standardized format for delivering in-chat text advertisements. It aims to ensure consistency, transparency, and interoperability across different platforms and applications. By adhering to this spec, advertisers and publishers can foster user trust and provide a seamless, unobtrusive ad experience.

**2. Guiding Principles**
- **User Experience:** Keep ads contextually relevant and unobtrusive.
- **Transparency:** Clearly distinguish between user-generated content and sponsored messages.
- **Measurement & Reporting:** Provide consistent metrics and measurable campaign performance.


## In-Chat Text Ad Overview

| **Ad Type**                                       | **Ad Unit**                           | **Aspect Ratio** | **Recommended Dimensions (dp)** | **Max File Weight (kB)** | **Notes**                                                                                                                                                                                                                         |
|---------------------------------------------------|---------------------------------------|------------------|---------------------------------|--------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **In-Chat Text Ads**               | – “Sponsored Message” <br> – Inline Text Ads | N/A              | N/A (Text-based)               | N/A (Text-based)          | - **Text-Only:** Negligible K-weight.  <br>- **Recommended Length:** 120–360 characters (including spaces). Title and Body. <br>- **Disclosure Label:** Required.                  |

### Key Characteristics
1. **Textual Only Content:** May include optional CTA or short URL. Text format is Markdown. 
2. **No Imagery:** Purely chat like feel.  
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
- **Contextual Placement:** Ads appear at natural conversation breaks after the message of the AI or .  
- **Frequency Capping:** Suggested best practice is to limit in-chat ads to one for every 10–15 user messages or as platform context dictates.

**Ad Rendering**
- **Styling Constraints:** Must be visually distinct (e.g., different background shade or text color) without disrupting the conversation flow.  
- **Accessibility:** Provide text-based alternatives for users with screen readers and comply with accessibility standards (e.g., color contrast guidelines).

**User Controls**
- **Minimize or Close button?

---

## 5. Tracking, Measurement, and Reporting

**5.1 Impression Measurement**
- An impression is counted only when the ad enters the user’s active chat window or is otherwise confirmed as viewable (according to IAB viewability standards).

**5.2 Click Measurement**
- A click is recorded when a user taps or clicks on the clickable portion of the ad (e.g., CTA or URL).  
- Redirection should occur via a tracking URL or server to support real-time analytics.

**5.3 Key Metrics**
- **Impressions Served:** Total number of ads delivered.  
- **Viewable Impressions:** Ads viewed or scrolled into view.  
- **Clicks/Engagement:** Count of user interactions.  
- **CTR (Click-Through Rate):** Ratio of clicks to impressions.

---

## 6. Compliance and Privacy

**6.1 Privacy Standards**
- Collect and store only necessary user data; comply with region-specific data regulations (GDPR, CCPA, etc.).  
- Provide an opt-out mechanism if any user-level data is being used for targeting.

**6.2 Security Measures**
- Use secure channels (HTTPS) for ad requests, impressions, and click tracking.  
- Validate ad content to minimize malicious or deceptive advertising.

**6.3 Content Standards**
- No misleading, harmful, or offensive text.  
- Clearly delineate sponsored content from user-generated content to preserve trust.

---

## 7. Implementation Guidance

**7.1 Example JSON Schema**

```json
{
  "adID": "123e4567-e89b-12d3-a456-426614174000",
  "advertiserName": "BrandXYZ",
  "disclosureLabel": "Sponsored",
  "headline": "Exclusive Offer!",
  "bodyText": "Save 20% on your next purchase. Limited time only!",
  "cta": "Shop Now",
  "clickthroughURL": "https://www.brandxyz.com/offer",
  "metadataTags": ["promotion", "shopping", "discount"],
  "frequencyCap": {
    "messagesInterval": 15
  },
  "styling": {
    "textColor": "#000000",
    "backgroundColor": "#F5F5F5"
  }
}
```
**7.2 Example System Prompt**


**7.2 Implementation Steps**
1. **Identify Natural Breaks:** Determine points in the chat to insert ads (based on message count or user activity).  
1. **Always after AI or Bot response is finished:** Determine points in the chat to insert ads (based on message count or user activity).  
2. **Render Ads Seamlessly:** Distinguish from user chat but maintain consistent style.  
3. **Enable Tracking:** Implement event listeners for impression and click metrics.  
4. **Refresh and Rotation:** Rotate ads based on campaign or time intervals to avoid repetitive user exposure.


### 5. Interaction & Tracking

1. **Impression Tracking:** Typically fired when the ad text is scrolled into view (the chat environment can define “viewability” as at least 50% of the ad bubble in the viewport).  
2. **Click / Tap Tracking:** If a CTA or link is included, a click redirect can be tracked via a single measurement pixel or server-to-server log.  
3. **Engagement (Optional):** Some platforms may support likes, emojis, or replies to the ad. These metrics are platform-specific.



## Alignment with Other IAB Ad Formats

Like the **Emoji and Sticker Content** or **Native Ads** within the IAB New Ad Portfolio, **In-Chat Text Ads**:

- **Fit within an existing content experience** (the conversation) rather than taking over the screen.  
- **Maintain a lightweight footprint** and adhere to the performance guidelines (initial vs. subload file weights, minimal CPU overhead).  
- **Respect user context** by avoiding disruptive auto expansions, forced countdowns, or overlays.  
- **Allow for basic creative elements** (headline, optional icon, disclosure, CTA), paralleling the data assets model for native ads.

---

## Additional Notes & Best Practices

1. **Frequency Capping:** In a conversation flow, showing an ad every single time a user sends a message is disruptive. A recommended approach is 1 ad after every 10–15 user messages or at a natural break in conversation.  
2. **Placement Timing:** Ads can appear when the user opens a chat, after a certain idle time, or upon receiving new content—always ensure it *does not interrupt* an active user chat exchange mid-message.  
3. **Accessibility:** Maintain readable font sizes and color contrast. If an icon is present, provide accessible text for screen readers (e.g., alt text “Brand Logo”).  
4. **Prohibited Practices:** No pop-ups, no forced reading time, no auto-play audio, no full-screen takeovers, no blinking or flashing text.  
5. **Privacy & Transparency:** If interest-based targeting is used, incorporate an **AdChoices** link (icon or text) that leads to user opt-out or preference management.

---

## Summary

**In-Chat / Conversation Text Ads** offer a **lightweight, native-like** format that blends into messaging platforms while upholding the **IAB New Ad Portfolio**’s LEAN principles. This specification ensures:

- **Minimal Disruption:** Ads appear as standard chat messages, with clear labeling.  
- **High Transparency:** Users instantly see they are viewing a sponsored message.  
- **Performance Efficiency:** Negligible initial load impact and minimal resource usage.  
- **Consistency:** Aligns with broader IAB guidelines on respectful, user-friendly digital advertising experiences.

Publishers and chat-platform developers can adopt these recommendations to standardize in-chat ad placements—balancing monetization with user experience in **dynamic, real-time messaging** environments.