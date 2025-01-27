# 1. Introduction

## 1.1 Purpose

The **GPT Ad Impression Schema** aims to standardize how an AI platform or supply-side partner declares an opportunity to insert a text-based ad within a conversational or generative environment. It ensures:

- **Clarity**: A uniform approach to describing key ad elements, such as maximum text lengths and mandatory sponsor disclosures.  
- **User Experience**: By mandating minimal intrusiveness and explicit labeling of sponsored messages, we preserve user trust.  
- **Flexibility**: Publishers and AI systems can define advanced features (like frequency capping, “blending,” or CTA usage) without the overhead of traditional banner or native ad specs.

## 1.2 Scope

- **GPT / Chat Ad Opportunities**: The spec covers text-only ad units shown in LLM chats, messaging apps, AI-driven Q&A experiences, and other conversation-driven interfaces.  
- **Ad Insertion**: Focuses on how an impression (i.e., “ad slot” or “opportunity”) is declared. **Does not** prescribe the final rendering approach—only the data required to serve an appropriate ad.

---

# 2. GPT Ad Impression Schema

Here is the generic schema of a GPT Ad Impression object:

```jsonc
{
    "gpt_ads_placement": {
        "placement_id": "string",
        "version": "string",
        "context": "string",
        "placement_type": "string",
        "minimum_bid": "number",
        "text_requirements": {
            "headline_max_length": "number",
            "body_max_length": "number", 
            "disclosure_position": "string",
            "cta_allowed": "boolean",
            "url_allowed": "boolean"
        },
        "blending_option": "string",
        "metadata": {
            "tags": ["string"],
            "custom_fields": {
                "string": "any"
            }
        }
    }
}
```

## Schema Reference Values

The following example shows possible values for each field in the GPT Ad Impression Schema based on the ADS4GPTs Ad Specification:



```jsonc
{
  "gpt_ads_placement": {
    "placement_id": "string",           // Unique placement/opportunity ID
    "version": "1.0",                   // Schema version
    "context": "chat|assistant|other",  // Environment in which ad will appear
    "placement_type": "inline|overlay|suggested",

    "minimum_bid": "number",                // Minimum bid floor for this placement

    "text_requirements": {
      "headline_max_length": 90,        // Allowed headline length
      "body_max_length": 280,          // Allowed body length
      "disclosure_position": "top|bottom|separate",
      "cta_allowed": true,
      "url_allowed": true
      /* 
        No "disclosure_required" flag here 
        since disclosure is mandatory per ADS4GPTs guidelines
      */
    },

    "blending_option": "none|partial|full",
    /* 
      "none": Ad remains distinct from chat style
      "partial": Some style blending or minimal rewriting 
      "full": The ad can be adapted to match the AI chat's tone or format 
    */

    "metadata": {
      "tags": [ ... ],
      "custom_fields": {
        "brandSafetyCategory": [ ... ],
        "contentRating": "string"
      }
    }
  }
}
```

## Explanation of Fields

1. **`placement_id`**  
   - A unique string or UUID that identifies this specific ad opportunity.

2. **`version`**  
   - The schema version for backward/forward compatibility within ADS4GPTs.

3. **`context`**  
   - Describes the general environment, such as `"chat"`, `"assistant"`, or any other applicable AI-driven interface.

4. **`placement_type`**  
   - **`inline`**: Placed directly in the conversation flow.  
   - **`overlay`**: Overlaid on top of or near the chat interface.  
   - **`suggested`**: Inserted as a suggestion or recommendation block.

5. **`minimum_bid`**  
   - The **bid floor** for this impression (e.g., `0.10` denotes a $0.10 floor).  
   - Useful in auctions to signal that bids below this value are not acceptable.

6. **`text_requirements`** (object)  
   - **`headline_max_length`**: The maximum number of characters for the ad’s headline.  
   - **`body_max_length`**: The maximum number of characters for the main ad text.  
   - **`disclosure_position`**: Where the mandatory sponsor label (e.g., “Sponsored” or “Ad”) appears. Options:  
     - **`top`**: The disclosure label precedes the headline.  
     - **`bottom`**: The disclosure label is appended at the end.  
     - **`separate`**: Shown in its own line or segment.  
   - **`cta_allowed`**: Whether the advertiser can include a call-to-action (e.g., “Learn More,” “Buy Now”).  
   - **`url_allowed`**: Whether the ad can display or link to a URL.

   > **Note**: Disclosure is **always required** under ADS4GPTs. The only variable is where it appears (`disclosure_position`).

7. **`blending_option`**  
   - Controls if the ad text can mimic or blend into the AI platform’s style or tone. Possible values:  
     - **`none`**: The ad should remain visually/textually distinct.  
     - **`partial`**: Minimal style adjustments (e.g., tone, language) to fit the chat context.  
     - **`full`**: The ad content can be rephrased or adapted entirely to match the chat’s tone, brand, or style guidelines (subject to advertiser permissions).

8. **`metadata`**  
   - **`tags`**: Simple string tags describing the nature of the placement or intended ad context (e.g., “discount,” “holiday-sale”).  
   - **`custom_fields`**: Additional metadata for brand safety, content rating, or any proprietary flags needed by the AI platform or AdTech partners.

---

# 3. Rationale for a New Schema

### 3.1 AI-First Use Cases

**Traditional** OpenRTB or Native Ad specs were built around web banners, in-feed image/video, or mobile display. They don’t map neatly to:

- **Inline text** within generative AI dialogues.  
- **Conversational** formatting, which is purely textual and might need “blended” styles.  
- **Real-Time** insertion after natural breaks in an AI conversation, with minimal disruption.

A **new schema** (GPT Ad Impression) addresses these **unique** constraints of AI chat environments from the ground up.

### 3.2 Streamlined Text-Only Requirements

The legacy IAB Native spec covers image, video, data fields, sponsor logos, brand icons, etc. In GPT ads, everything is text-based—**no images** or heavy creative assets. Hence, this streamlined schema:

- Reduces overhead.  
- Clarifies required text lengths.  
- Imposes **disclosure** by design.

### 3.3 Minimal Intrusion & Clear Labeling

User trust is **paramount** in conversational AI. This schema highlights:

- Mandatory “Sponsored” or “Ad” labeling.  
- Clear indication of CTA usage.  
- Frequency capping to preserve conversation integrity.

### 3.4 Enhanced Flexibility for AI Systems

AI-driven ad insertion may require dynamic rewording, style adaptation, or adjacency controls that standard specs don’t address. Our new approach:

- Incorporates a **`blend_style`** flag for adjusting tone or wording.  
- Provides robust **`metadata`** fields for flexible targeting and brand safety.  
- Decouples from legacy banner or display specs that might hamper agility in an LLM context.

### 3.5 Design Choices

1. **AI-First Design**  
   - Traditional specs revolve around banners or multi-asset native ads; here, we emphasize pure text, disclaimers, and an optional CTA—ideal for chat or voice-based systems.

2. **Minimum Bid for Monetization Control**  
   - The `minimum_bid` field addresses a direct need for GPT-based auctions to define a **bid floor**. This ensures the platform’s monetization targets are met.

3. **Mandatory Disclosure**  
   - ADS4GPTs guidelines stipulate that **all** GPT text ads must include a sponsor label. By **removing** the optional “disclosure_required” field, we make it **inherent** that a disclosure label is always present.

4. **Blending Flexibility**  
   - The `blending_option` acknowledges that some AI experiences prefer a subtle, integrated approach, while others require a distinct, obvious ad block. This field explicitly conveys how flexible the placement can be in adapting ad content.

5. **Simplicity & Extensibility**  
   - The core fields are minimal—ensuring easy adoption and minimal confusion.  
   - Additional data can be stored in `metadata > custom_fields` for advanced use cases (brand safety checks, extra targeting logic, etc.).

---

# 4. Implementation Example: GPT Ads Placement Request

Below is how a request might look when the AI application is ready to fill a text-ad slot:

```json
{
  "gpt_ads_placement": {
    "placement_id": "GPT-PL-001",
    "version": "1.0",
    "context": "chat",
    "placement_type": "inline",
    "minimum_bid": 0.25,
    "text_requirements": {
      "headline_max_length": 60,
      "body_max_length": 200,
      "disclosure_position": "top",
      "cta_allowed": true,
      "url_allowed": true
    },
    "blending_option": "partial",
    "metadata": {
      "tags": ["promo", "flash_sale"],
      "custom_fields": {
        "contentRating": "G"
      }
    }
  },
  "device": { ... },
  "user": { ... }
}
```

**Response** (Ad Server/Exchange → Publisher/AI):
```json
{
    "gpt_ads_placement_response": {
        "placement_id": "GPT-PL-001",
        "creative": {
            "headline": "Early Access to Holiday Deals!",
            "body": "Enjoy exclusive discounts—up to 30% off. Limited time!",
            "disclosure_label": "Sponsored",
            "cta": "Shop Now",
            "cta_url": "https://example.com/holiday"
        },
        "bid": {
            "amount": 1.25,
            "currency": "USD"
        },
        "metadata": {
            "ad_id": "AD-123",
            "advertiser": "ExampleBrand",
            "creative_id": "CR-456"
        }
    }
}
```

### Explanation of Use

- The publisher or AI system includes the **gpt_ads_placement** object in the request to an ad server or marketplace.  
- The ad server sees **`minimum_bid: 0.25`** and ensures any bids below $0.25 are disqualified.  
- Because **`disclosure_position`** is `"top"`, the returned creative must contain the sponsor label before the headline.  
- **`blending_option`** = `"partial"` tells the ad server it can slightly modify style or wording if the advertiser allows, but must remain recognizable as an ad.


---

# 5. Backwards Compatibility with OpenRTB

While the **GPT Ad Impression Schema** is purpose-built for AI environments, it can be integrated with OpenRTB for existing AdTech ecosystems:

## 5.1 Integration with OpenRTB

Publishers or SSPs can embed the GPT Ad Impression within OpenRTB's extensibility points:

```jsonc
{
    "imp": [{
        "id": "imp1",
        "ext": {
            "gpt_ads_placement": {  // Our schema goes here
                "placement_id": "GPT-001",
                "version": "1.0",
                // ... other fields as defined above
            }
        }
    }]
}
```

## 5.2 Native Ad Format Mapping

For DSPs supporting only OpenRTB Native formats, key fields map as follows:

```jsonc
{
    "native": {
        "request": {
            "assets": [
                {
                    "id": 1,
                    "required": 1,
                    "title": {
                        "len": 90  // maps to headline_max_length
                    }
                },
                {
                    "id": 2,
                    "required": 1,
                    "data": {
                        "type": 2,  // body text
                        "len": 280  // maps to body_max_length
                    }
                },
                {
                    "id": 3,
                    "required": 1,
                    "data": {
                        "type": 1,  // sponsored label
                        "len": 15   // reasonable length for "Sponsored" or "Ad"
                    }
                }
            ]
        }
    }
}
```

## 5.3 Unique GPT Features

Some GPT-specific fields have no direct OpenRTB equivalents and should remain in `ext`:
- `blending_option`
- `disclosure_position`
- Custom metadata fields

Example of GPT-specific fields in OpenRTB:

```jsonc
{
    "imp": [{
        "id": "imp1",
        "ext": {
            "gpt_ads": {
                "blending_option": "partial",
                "disclosure_position": "top",
                "metadata": {
                    "custom_fields": {
                        "ai_context": "chat_assistant",
                        "brand_safety_level": "high"
                    }
                }
            }
        }
    }]
}
```

This hybrid approach enables gradual adoption while maintaining compatibility with existing AdTech infrastructure.

---


# 4. Final Thoughts

The **GPT Ad Impression Schema** is purpose-built for **AI chat and LLM-driven** advertising:

1. **Why a New Schema?**  
   - Legacy specs revolve around banner, display, or multi-asset native ads. GPT Ads are strictly textual, conversation-driven, and revolve around minimal intrusion with high transparency.

2. **Key Differentiators**  
- **Mandating Disclosure**: No field to toggle—disclosure is always on.  
- **Offering Monetization Control**: Through the `minimum_bid` floor.  
- **Allowing or Forbidding Blending**: With a clear `blending_option` field.  
- **Eliminating Unnecessary Fields**: E.g., `frequency_cap` is removed for simpler concurrency management.  

3. **Backwards Compatibility**  
   - Though the new schema stands on its own, it can be **embedded** into OpenRTB structures (e.g., `imp.ext`) for those requiring transitional workflows. A partial mapping to OpenRTB Native is possible, though not recommended for fully AI-first solutions.

The net result is a **clean, AI-centric** blueprint for text-based ad placements, ensuring clarity for all parties—**publisher, advertiser,** and **end user**—while providing enough flexibility to handle advanced scenarios in next-generation GPT or chat environments.