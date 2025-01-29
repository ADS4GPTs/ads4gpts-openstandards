# Guiding Principles for GPT Ad Request Prompt

*Version 0.0.1*  

---

## 1. Introduction

The **ADS4GPTs** tool offers an automated, **privacy-first** mechanism for generating advertising recommendations—encoded in a standardized **GPT Ads Targeting Object**—based on AI-analyzed user data. This document provides **practical guidance** for developers on how to write a **prompt** that calls the ADS4GPTs tool from within an AI model, emphasizing the protection of **personally identifiable information (PII)** while enabling **richly detailed** contextual information through fields like **persona**, **Ad Recommendation**, and **Undesired Ads**.

---

## 2. Purpose of These Guidelines

1. **Privacy-First Output**: Ensure that no raw or inadvertent PII ever makes its way into the AI responses.  
2. **Rich Contextual Fields**: The fields `persona`, `Ad Recommendation`, and `Undesired Ads` can be **more descriptive** to capture user preferences and disinterests accurately, but without violating privacy.  
3. **Structured Consistency**: Provide a straightforward, predictable JSON schema that ads platforms can parse.  
4. **Few-Shot Learning Approach**: Include multiple **prompt/response** examples so developers can see how to guide the AI effectively.

---

## 3. Key Data Fields and Privacy Emphasis

### 3.1 The GPT Ads Targeting Object

The required structure for the **GPT Ads Targeting Object** is as follows:

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

**Important**:  
- **id** must be a hashed or anonymized identifier.  
- **gender** and **age_range** are **categorical** (not free-text) to limit over-targeting and reduce privacy risk.  
- **persona**, **Ad Recommendation**, and **Undesired Ads** are **rich-text** (free-text) fields.  
- **No raw PII** (names, emails, phone numbers, exact addresses) should ever appear in any of these fields.

### 3.2 Rich Text Fields

While `persona`, `Ad Recommendation`, and `Undesired Ads` can be more elaborate, follow these rules to **safeguard** user privacy:

- **Persona**: May include descriptive interests or behaviors (e.g., “avid hiker, frequently shops for eco-friendly travel gear”). *Avoid personal data like full name, contact details, precise location.*  
- **Ad Recommendation**: Can suggest specific brands, product categories, or service features that align with the persona. *Do not embed personally identifying info.*  
- **Undesired Ads**: Users can detail broader categories or explicit product lines they wish to avoid. *Refrain from including personal info or any data linking them to a unique identity.*

---

## 4. Few-Shot Learning Prompt Guidelines

This section demonstrates **how** to construct prompts that effectively call the ADS4GPTs tool. We’ll show multiple examples (few-shot learning) so the AI model learns the **desired format** and **privacy-preserving** output style.

### 4.1 Generic Prompt Template

Below is a **baseline** prompt you can adapt. Note the emphasis on protecting PII and returning a **valid** JSON structure.

```
You are an AI assistant that uses the ADS4GPTs tool to generate a GPT Ads Targeting Object.
We have the following user context (with no PII):

1) Anonymized session ID: [HASHED_SESSION_ID]
2) Gender: [male/female/non_binary/undisclosed]
3) Age Range: [under_18 / 18-24 / 25-34 / 35-44 / 45-54 / 55-64 / 65_over / undisclosed]
4) Persona: [Short paragraph describing the user’s interests or behaviors in a privacy-safe manner]
5) The user does NOT want to see ads related to [list categories or keywords]

Generate a **detailed** GPT Ads Targeting Object in JSON:
{
  "id": "",
  "User": {
    "gender": "",
    "age_range": "",
    "persona": ""
  },
  "Ad Recommendation": "",
  "Undesired Ads": ""
}

Rules:
- "id" = the hashed session ID from above
- Use one of the enumerated values for "gender" and "age_range"
- "persona": Provide a descriptive but privacy-safe overview
- "Ad Recommendation": Provide detailed categories, brands, or features matching the persona
- "Undesired Ads": Thoroughly list all ad categories the user wants to avoid
- Do NOT include any personal identifiers, like name, address, or unique identifiers beyond the hashed session
```

### 4.2 Example #1: Outdoor Enthusiast (Few-Shot)

**Prompt**  
```
You are an AI using the ADS4GPTs tool. Here's the user context:

- ID: "07f2c1139e4a1c8d"
- Gender: "female"
- Age Range: "25-34"
- Persona: "Loves hiking and outdoor photography, frequently shops for eco-friendly camping supplies, values sustainability."
- The user does NOT want ads about fast-food chains, gambling, or aggressive diet supplements.

Return a valid GPT Ads Targeting Object. Make sure persona, Ad Recommendation, and Undesired Ads are detailed yet privacy-safe.
```

**Ideal Response**  
```json
{
  "id": "07f2c1139e4a1c8d",
  "User": {
    "gender": "female",
    "age_range": "25-34",
    "persona": "Outdoor enthusiast focused on hiking, photography, and sustainable camping gear"
  },
  "Ad Recommendation": "Backpacking essentials, nature photography equipment, eco-friendly apparel, adventure tourism offers",
  "Undesired Ads": "Fast-food promotions, gambling services, aggressive diet supplement campaigns"
}
```

### 4.3 Example #2: Tech-Savvy Gamer (Few-Shot)

**Prompt**  
```
You are an AI using the ADS4GPTs tool. Here's the user context:

- Anonymized ID: "b1a23def980xyz"
- Gender: "non_binary"
- Age Range: "18-24"
- Persona: "Passionate about console gaming, enjoys eSports streams, frequently explores latest game releases and hardware reviews."
- The user does NOT want ads about high-end luxury products or financial services.

Please produce a GPT Ads Targeting Object with detailed fields, and do not include any personal data beyond what's provided. 
```

**Ideal Response**  
```json
{
  "id": "b1a23def980xyz",
  "User": {
    "gender": "non_binary",
    "age_range": "18-24",
    "persona": "Console gaming enthusiast, avid eSports viewer, frequently checks new game hardware and software updates"
  },
  "Ad Recommendation": "Discounted new release games, streaming platform bundles, next-gen console accessories, eSports merch",
  "Undesired Ads": "High-end luxury items, financial investment services"
}
```

### 4.4 Key Takeaways from Examples
- **Detailed** (but privacy-safe) text in persona, ad recommendations, and undesired ads.  
- **Strict** compliance with the enumerated fields for gender and age range.  
- **Consistent** JSON structure matching the GPT Ads Targeting Object.

---

## 5. Implementation Steps

1. **Collect Minimal Context**: Gather user session ID and interest data in a **privacy-first** manner.  
2. **Assemble Prompt**: Use the recommended structure and adapt the examples to your use case.  
3. **Call AI Model**: Send the prompt to your AI (e.g., ChatGPT or a custom LLM) with instructions to use the **ADS4GPTs** tool.  
4. **Validate JSON**: Ensure the model’s output is valid JSON and adheres to the GPT Ads Targeting Object schema.  
5. **Parse & Forward**: Supply the resulting JSON to your advertising pipeline or RTB integration to serve relevant ads.

---

## 6. Best Practices for Maintaining Privacy

1. **Never Request** Raw PII: Avoid prompting for sensitive identifiers like names, emails, phone numbers, or addresses.  
2. **Limit Persona Detail**: While rich, persona descriptions should remain **behavioral** or **interest-based**—avoid personal anecdotes or unique trackers.  
3. **Watch for AI Overreach**: The AI might attempt to “hallucinate” personal details if the prompt is not clear. Explicitly instruct it to **omit** any PII.  
4. **Monitor Outputs**: Use an automated checker that flags potential PII or out-of-schema responses.  
5. **Comply with Laws**: Ensure you have user consent for ad targeting; default to “undisclosed” for sensitive fields if consent is absent or unknown.
