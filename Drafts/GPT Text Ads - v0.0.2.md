# In-Chat Text Ads

In-chat text ads appear as **textual messages** within a chat or messaging interface. They typically blend with the look and feel of user-generated messages but are clearly labeled to indicate sponsorship. These ads prioritize **minimal disruption**, **no file weight**, and **clear labeling**.

## Overview

| **Ad Type**                                       | **Ad Unit**                           | **Aspect Ratio** | **Recommended Dimensions (dp)** | **Max File Weight (kB)** | **Notes**                                                                                                                                                                                                                         |
|---------------------------------------------------|---------------------------------------|------------------|---------------------------------|--------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **In-Chat / Conversation Text Ads**               | – “Sponsored Message” <br> – Inline Text Ads | N/A              | N/A (Text-based)               | N/A (Text-based)          | - **Text-Only:** Negligible K-weight.  - **Recommended Length:** 120–360 characters (including spaces). Title and Body. <br>- **Disclosure Label:** Required. (e.g., “Ad” / “Sponsored”)                   |

### Key Characteristics
1. **Primary Content is Text:** May include optional CTA or short URL.  
2. **No Imagery:** Purely chat like feeel.  
3. **Clearly Marked** as Sponsored Content: Must contain a label such as “Sponsored,” “Ad,” or “Promoted.”  
4. **Lightweight Tracking:** One tracking pixel or server-to-server logging for impressions/clicks.

---

## Recommended Usage Guidelines

Below is an expanded specification following the **IAB New Ad Portfolio** structure—covering dimensions, file weight, and user experience considerations.

### 1. Dimensions & Layout

- **Flexible Width:** Since the chat environment often scales text message bubbles to the screen width, there is no strict “pixel dimension” requirement.  
- **Density-Independent Pixels (dp):** Not directly applicable for pure text, but any optional icon or thumbnail (e.g., brand logo) should be sized around 20–30 dp in height/width.  
- **Aspect Ratio:** Typically **N/A** because the ad is purely textual. If an optional image or emoji is included, it should maintain a **1:1** aspect ratio and blend with the chat’s native styling.

### 2. File Weight and Requests

- **Text Payload:** Negligible for file weight.  
- **Optional Inline Icon:** Keep at or below **50 kB**.  
- **Number of Requests:** Ideally **1** request (or less) for the entire ad; if using any icon, compress it to minimize the load.  
- **Shared Libraries:** If the platform uses shared libraries for tracking or creative rendering, these typically remain outside the text ad’s load calculation (similar to the IAB guidance on exempt resources).

### 3. Character Count & Content Limits

| **Content Element**   | **Description**                                                                    | **Max Characters**      | **Notes**                                                                                                                                                                                                                                             |
|-----------------------|------------------------------------------------------------------------------------|-------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Headline / Main Text** | Core message text (intro, brand message).                                         | 70–280 recommended      | Keep short for chat environments. Could be truncated if the platform enforces shorter limits (similar to social media constraints).                                                                                                                   |
| **Disclosure Label**  | Required label for sponsored content.                                              | 10–15                   | E.g., “Ad,” “Sponsored.” May appear at the beginning or end of the message, or as a separate label element.                                                                                                                                            |
| **Call to Action (CTA)** | A short prompt or link text (optional).                                           | 25–35                   | Examples: “Learn More,” “Get Offer,” “Shop Now.” If the chat environment supports clickable text, it may be hyperlinked or appended as a short URL.                                                                                                   |
| **Short URL**         | Shortened link or deep link.                                                        | 25–50 (typical short URL) | Must comply with platform link formatting. Use https for encryption.                                                                                                                                                                                 |
| **Optional Emoji**    | Single or minimal use of emojis/brand icons.                                        | –                       | Must not be disruptive (avoid flashing/moving icons). If custom, see Max File Weight guidelines.                                                                                                                                                     |

### 4. Integration

Below is a high-level suggestion for an open-source **In-Chat Text Ad Specification**, loosely modeled after the Interactive Advertising Bureau (IAB) format conventions. It’s designed to be practical, clear, and adaptable for various chat platforms while ensuring reliable measurement, consistent display, and transparent operations.

---

## 1. Introduction

**1.1 Purpose and Scope**  
This specification outlines a standardized format for delivering in-chat text advertisements. It aims to ensure consistency, transparency, and interoperability across different platforms and applications. By adhering to this spec, advertisers and publishers can foster user trust and provide a seamless, unobtrusive ad experience.

**1.2 Guiding Principles**
- **User Experience:** Keep ads contextually relevant and unobtrusive.
- **Transparency:** Clearly distinguish between user-generated content and sponsored messages.
- **Measurement & Reporting:** Provide consistent metrics and measurable campaign performance.

---

## 2. Definitions and Terminology

- **In-Chat Text Ad:** A textual advertising message inserted within a chat or messaging flow.
- **Advertiser:** The party responsible for the ad content.
- **Publisher/Platform:** The environment or application hosting the in-chat text ad.
- **Viewability:** Measurement standard indicating how many users have meaningfully viewed the ad (e.g., ad enters the visible window).

---

## 3. Ad Content Specification

**3.1 Required Elements**
1. **Ad Identifier (AdID):** A unique identifier for tracking and reporting (e.g., string or UUID).  
2. **Advertiser Name:** Name of the brand or sponsor.  
3. **Headline/Text Body:** Core textual content of the ad, limited to a recommended character count (e.g., 140–280 characters) to ensure clarity.  
4. **Disclosure Label:** A required label (“Sponsored”, “Ad”, etc.) clearly indicating promotional content.

**3.2 Optional Elements**
1. **Clickthrough URL:** A link, if applicable, providing additional information, redirecting to a landing page or an offer.  
2. **Call to Action (CTA):** A concise prompt urging user action (“Learn More”, “Sign Up”, etc.).  
3. **Rich Text Formatting:** Bold or italic text for emphasis, if supported by the chat platform’s formatting rules.  
4. **Metadata Tags:** Optional keywords or segments indicating target audience or contextual relevance (e.g., “#tech”, “#travel”).

**3.3 Character Count and Formatting Recommendations**
- **Headline:** Maximum 50 characters (including spaces) for optimum clarity.  
- **Body Text:** Maximum 280 characters to remain concise in chat contexts.  
- **Formatting:** Follow platform guidelines for line breaks, bolding, or italics.

---

## 4. Ad Delivery and Positioning

**4.1 Placement Rules**
- **Contextual Placement:** Ads appear after a certain number of user messages or at natural conversation breaks.  
- **Frequency Capping:** Suggested best practice is to limit in-chat ads to one for every 10–15 user messages or as platform context dictates.

**4.2 Ad Rendering**
- **Styling Constraints:** Must be visually distinct (e.g., different background shade or text color) without disrupting the conversation flow.  
- **Accessibility:** Provide text-based alternatives for users with screen readers and comply with accessibility standards (e.g., color contrast guidelines).

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

**7.2 Implementation Steps**
1. **Identify Natural Breaks:** Determine points in the chat to insert ads (based on message count or user activity).  
2. **Render Ads Seamlessly:** Distinguish from user chat but maintain consistent style.  
3. **Enable Tracking:** Implement event listeners for impression and click metrics.  
4. **Refresh and Rotation:** Rotate ads based on campaign or time intervals to avoid repetitive user exposure.


### 4. User Experience & LEAN Principles

1. **Non-Invasive Display:**  
   - The ad must not block user messages or appear as a pop-up overlay.  
   - Frequency: 1 ad per ~10–15 user messages or at natural conversation breaks.  
2. **Clear Labeling:**  
   - Must be visually distinct, typically with a “Sponsored” or “Ad” label.  
   - Provide clarity so users can differentiate between user-generated messages and promotional content.  
3. **No Auto-Play Media:**  
   - Audio or video auto-play is disallowed in a purely text-based environment.  
   - If any link leads to video content, it should open outside the conversation or in a separate chat card (user-initiated).  
4. **Lightweight & Secure:**  
   - Ads served via HTTPS.  
   - Minimal CPU usage (text only; no advanced animations).  
5. **Respect Privacy & Data Regulations:**  
   - Comply with local and global data protection rules (GDPR, CCPA, etc.).  
   - Provide an opt-out if interest-based targeting is used.  

### 5. Interaction & Tracking

1. **Impression Tracking:** Typically fired when the ad text is scrolled into view (the chat environment can define “viewability” as at least 50% of the ad bubble in the viewport).  
2. **Click / Tap Tracking:** If a CTA or link is included, a click redirect can be tracked via a single measurement pixel or server-to-server log.  
3. **Engagement (Optional):** Some platforms may support likes, emojis, or replies to the ad. These metrics are platform-specific.

### 6. Creative Specifications & Examples

| **Creative Element** | **Specification**                                                           | **Example**                                                                                                                                                  |
|----------------------|---------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Headline/Text**    | 70–280 characters recommended, with brand name or short pitch.             | “Try our new fitness app – now 20% off! Start your healthy journey today.”                                                                                  |
| **Disclosure**       | “Sponsored,” “Ad,” or “Promoted” label.                                    | “**Ad**: Try our new fitness app – now 20% off!”                                                                                                            |
| **CTA**              | Keep short (25–35 chars).                                                   | “Get Offer” or “Learn More”                                                                                                                                 |
| **Optional Icon**    | PNG/JPG/GIF ≤ 50 kB, ~20–30 dp size.                                        | ![company_logo_small.png (20×20 dp)](example.com/logo.png)                                                                                                  |
| **Link**             | Shortened URL or deep link. Must be secure (HTTPS).                         | “Visit https://bit.ly/xyz to learn more.”                                                                                                                   |

---

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