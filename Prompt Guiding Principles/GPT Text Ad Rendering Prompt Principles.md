# **Prompt Guiding Principles for AI Ad Rendering with ADS4GPTs**  

*Version 0.1*  

---

## **1. Overview**  

This document provides guidelines for structuring an AI Ad Rendering Prompt that takes an **ADS4GPTs-compliant GPT Text Ad JSON input** and converts it into **Markdown** for integration into conversational AI environments.  

The goal is to ensure that ads:  

- **Maintain brand integrity** by rendering exactly as provided, unless blending is explicitly allowed.  
- **Blend naturally into the conversation** when appropriate, following the `blend` parameter.  
- **Preserve formatting consistency** using Markdown to enhance readability.  
- **Comply with advertising and brand safety standards**, ensuring ads are neither misleading nor intrusive.  

---

## **2. Understanding the GPT Text Ad Schema**  

Each GPT text ad contains the following key fields required for rendering:  

| **Field**          | **Type**   | **Required** | **Description** |
|--------------------|-----------|-------------|----------------|
| `disclosure_label` | string    | Yes         | A required label indicating the ad is paid content (e.g., "Sponsored"). |
| `headline`        | string    | Yes         | A short, engaging headline for the ad. |
| `body`           | string    | Yes         | The main text of the ad, providing details about the offer. |
| `cta`            | string    | No          | A clear call-to-action (CTA), such as "Shop Now" or "Learn More." |
| `cta_url`        | string    | No          | A secure URL for the CTA button or link. |
| `blend`          | string    | No          | Determines how much the ad should be reworded to match the conversation. Accepted values: `"none"`, `"partial"`, `"full"`. |

**Blend Mode Descriptions:**  

- `"none"`: The ad **must be rendered exactly as provided**, with no rewording or stylistic adjustments.  
- `"partial"`: The ad’s **tone or phrasing may be slightly adjusted** to fit the context, but **core messaging must remain intact**.  
- `"full"`: The ad **can be significantly reworded** to match the AI's conversational style while keeping the key promotional elements.  

Example JSON input:  

```json
{
  "disclosure_label": "Sponsored",
  "headline": "Upgrade Your Tech!",
  "body": "Discover the latest gadgets at unbeatable prices. Limited-time deals available now!",
  "cta": "Shop Now",
  "cta_url": "https://www.techdeals.com",
  "blend": "partial"
}
```

---

## **3. Structuring the AI Ad Rendering Prompt**  

Since the same ad may appear in different conversational contexts, the AI must be able to integrate it smoothly while respecting the `blend` setting.  

### **Core Prompt Components**  

A well-structured prompt should include:  

1. **Current conversation context** – Helps the AI blend the ad naturally into the ongoing discussion.  
2. **Raw JSON ad data** – Ensures the AI renders the ad exactly as provided, unless blending is allowed.  
3. **Brand safety instructions** – Prevents unwanted modifications to advertiser messaging.  
4. **Formatting requirements** – Defines how the ad should be presented using Markdown.  

---

## **4. AI Ad Rendering Prompt Template**  

This is just a template, we do not guarantee that is the right prompt for your use case.

```
You are an AI responsible for rendering GPT text ads based on the ADS4GPTs specification while ensuring brand integrity.

### **Conversation context:**  
The following conversation is currently taking place:  

"[INSERT_CONVERSATION_CONTEXT_HERE]"  

### **Ad JSON Input:**  
[INSERT_AD_JSON_HERE]  

### **Instructions:**  
1. **Maintain brand integrity**  
   - If `blend` is `"none"`, do not modify the wording of the **headline, body, CTA, or CTA URL** in any way. Render the ad exactly as provided.  
   - If `blend` is `"partial"`, you may adjust **phrasing and style slightly** while keeping the core messaging and intent unchanged.  
   - If `blend` is `"full"`, you may reword the ad to match the conversational style, but all promotional elements (brand, offer, CTA) must remain intact.  

2. **Blend the ad naturally into the conversation**  
    - If `blend` is "none," present the ad exactly as provided.  
    - If `blend` is "partial," make subtle stylistic tweaks while preserving core messaging.  
    - If `blend` is "full," adapt the ad to match the conversation’s style while keeping key promotional elements.  
    - If the conversation is directly relevant, transition smoothly.  
    - If the conversation is neutral, introduce the ad subtly without forcing relevance.  
    - Use a neutral transition when needed, such as:  
      - "Here’s something that might interest you:"  
      - "Speaking of great deals…"  
      - "Check this out:"

3. **Format the ad using Markdown**  
   - The `disclosure_label` must appear at the top in bold (e.g., **Sponsored**).  
   - The `headline` should be bold or formatted as a Markdown heading.  
   - The `body` should be presented in a readable format.  
   - If a CTA and URL are provided, display the CTA as a **bold clickable link** in Markdown.  

4. **Ensure advertising transparency**  
   - The disclosure label must always be clearly visible.  
   - Do not modify the ad to appear as an organic recommendation from the AI.  
```

---

## **5. Few-Shot Examples**  

### **Example 1: No blending (blend: "none")**  

#### **Conversation context**  
*"I’m looking for a new laptop, but everything seems overpriced. I wish there were better deals."*  

#### **AI output (Markdown-formatted ad, no modification)**  
```
**Sponsored**  
### Upgrade Your Tech!  
Discover the latest gadgets at unbeatable prices. Limited-time deals available now!  

[**Shop Now**](https://www.techdeals.com)  
```

The AI **does not modify** the ad in any way.

---

### **Example 2: Partial blending (blend: "partial")**  

#### **Conversation context**  
*"I’ve been thinking about upgrading my home office. I need a better setup that feels modern."*  

#### **AI output (Markdown-formatted ad, minor rewording for flow)**  
```
**Sponsored**  
Looking to upgrade your tech?  

### Upgrade Your Tech!  
Discover the latest gadgets at unbeatable prices. Don’t miss out on limited-time deals!  

[**Shop Now**](https://www.techdeals.com)  
```

The **main messaging remains intact**, but the AI **adjusts the phrasing slightly** to make the transition smoother.

---

### **Example 3: Full blending (blend: "full")**  

#### **Conversation context**  
*"I just finished watching a sci-fi movie, and now I want to read a book with a similar theme!"*  

#### **AI output (Markdown-formatted ad, fully blended with context)**  
```
**Sponsored**  
If you love futuristic tech in movies, why not bring some into your own life?  

### Upgrade Your Tech!  
Get the latest gadgets at unbeatable prices. Limited-time deals are available now—don’t miss out!  

[**Shop Now**](https://www.techdeals.com)  
```

In this case, the AI **fully adapts** the ad to **match the context**, while keeping key promotional elements intact.

---

## **6. Brand Safety and Compliance**  

### **Preserve brand messaging**  
- The AI must not generate alternative wording, slogans, or reinterpretations of the ad.  
- Every word in the ad must appear exactly as in the JSON input.
- If `blend` is `"none"`, the AI **must not** alter any ad content.  
- If `blend` is `"partial"` or `"full"`, the AI may **adjust style**, but the **offer, CTA, and advertiser name must remain unchanged**.  

### **Ensure appropriate placement**  
- Ads should only be inserted when there is a logical opportunity within the conversation.  
- If an ad does not align with the discussion, a neutral introduction should be used.  

### **Maintain clear advertising disclosure**  
- The `disclosure_label` must always be included and clearly visible.  
- The AI should not present the ad as a natural suggestion or recommendation.  

