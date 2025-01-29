# ADS4GPTs Protocol 
**Version 0.1**

---

# Table of Contents

1. [Overview](#overview)
    1. [ADS4GPTs Mission](#ads4gptsmission)
    2. [Executive Summary](#execsummary)
    3. [History & Motivation](#history)
2. [Regulatory Guidance](#regulatory)
3. [Architecture](#architecture)
    1. [Core Principles](#core_principles)
    2. [Terminology](#terminology)
    3. [Reference Model](#reference_model)
    4. [Protocol Layers](#protocol_layers)
4. [Specification](#specification)
    1. [Top-Level Structure](#21-top-level-structure)
    2. [Bid Request Schema](#bid_request)
        - [Object: Request](#object_request)
        - [Object: Item](#24-object-item)
        - [Object: Source](#object_source)
        - [Object: GPT Ads Placement](#object_gpt_ads_placement)
        - [Object: Deal](#object_deal)
        - [Object: Context](#27-object-context)
        - [Object: GPT Ads Targeting](#object_targeting)
        - [Example Request](#=example_request)
    3. [Bid Response Schema](#bid_response)
        - [Object: Response](#object_response)
        - [Object: Seatbid](#object_seatbid)
        - [Object: Bid](#object_bid)
        - [Object GPT Text Ad](#gpt_text_ad)
        - [Example Response](#example_response)
    4. [Event Notification](#event_notification)
        - [Event: Pending](#event_pending)
        - [Event: Billing](#event_billing)
        - [Event: Loss](#event_loss)
        - [Complex Event Chains](#complex_event_chains)
    5. [Inventory Authentication](#inventory_auth)
5. [Enumerations](#enumerations)
    1. [No-Bid Reason Codes](#nobid_reason)
    2. [Loss Reason Codes](#loss_reason)
---

# 1. Overview <a name="overview"></a>

## 1.1 ADS4GPTs Mission <a name="ads4gptsmission"></a>

**ADS4GPTs** extends programmatic advertising into AI-driven and text-focused environments such as ChatGPT-like platforms, large language model (LLM) chatbots, and other generative AI interfaces. 
The **ADS4GPTs** protocol serves as a **new** real-time bidding (RTB) standard for:

- **GPT/AI-driven** ad placements—particularly text-based or conversational formats,  
- **Privacy-first** user data handling, avoiding direct PII,  
- **Conversational** ads that are higly contextual and relevant.
- **Interoperable with OpenRTB**: Strong alignment with IAB Tech Lab’s OpenRTB 3.0.
- **AI-Safe Design**: Additional parameters (e.g., “blending_option”) to ensure the ad can adapt to or remain distinct from the AI’s textual style.

## 1.2 Executive Summary <a name="execsummary"></a>

Where legacy RTB protocols revolve around banner, video, and native ads—and often rely on user-level data—**ADS4GPTs** reimagines real-time bidding specifically for **AI** or **chat** contexts. It provides:

1. **Highly Relevant**: Powered by GPT-style natural language processing, enabling ads to match conversational context precisely.  
2. **Privacy-First** Targeting: Leveraging high-level or anonymized user/session signals.  
3. **UX Centered**: Ensures ad placements feel natural in conversational flows. Maintains high-quality user experience through granular control over the Ad placement, timing and content. The protocol ensures text ads can organically blend with or distinctly separate from AI responses, maintaining user experience quality.


## 1.3 History & Motivation <a name="history"></a>

As chat-based GPT applications gained popularity, it became clear that a dedicated RTB spec was needed to address:

- **Conversational ads** seamlessly integrated with AI assistant flows,  
- **Minimal** personal data usage to comply with evolving privacy regulations.
- **Addressing concerns** about the potential for AI responses to be overly influenced by advertisements, ensuring a balance between user experience and ad relevance.

**ADS4GPTs** is an **independent** standard, building on prior programmatic concepts but refocusing on AI-based experiences. In this document, we focus on **OpenRTB objects** that **ADS4GPTs** adopts and modifies. We also attribute the foundation of these objects to the [IAB Tech Lab’s OpenRTB specification](https://iabtechlab.com/standards/openrtb/).

---

# 2. Regulatory Guidance <a name="regulatory"></a>

Like OpenRTB, ADS4GPTs must comply with:

- GDPR, CCPA (or other applicable privacy laws).
- Consent management if the user’s data is used for targeting.

Because text-based ads can be more “personal,” we strongly recommend:

- Strict user data minimization (categorical age/gender).
- No direct PII in the GPT Ads Targeting Object.
- Clear disclosure that the user is seeing an ad (as per ADS4GPTs text-ad standards).
- Implementers must adhere to **GDPR**, **CCPA**, **COPPA**, or any local privacy laws. The specification:

Fortunately ADS4GPTs protocols and integrations:
- **Enforce** data minimization by default,  
- **Eliminate** direct personal data fields,  
- **Encourage** broad or “undisclosed” categories (gender, age_range) when specific data is not permitted.

---

# 3. Architecture <a name="architecture"></a>

## 3.1 Core Principles <a name="core_principles"></a>

1. **AI-First**: Focus on textual, minimal ad formats suitable for GPT chat or voice-based experiences.  
2. **Privacy-First**: Minimizes user data via the [GPT Ads Targeting Object](#object_targeting). 
3. **Flexibility**: Allow DSPs that only speak “vanilla” OpenRTB to ignore or partially parse ADS4GPTs extensions.
 


## 3.2 Terminology <a name="terminology"></a>

| **Term**                   | **Definition**                                                                 |
|----------------------------|-------------------------------------------------------------------------------|
| **GPT Ads**                | Text-based or chat-style ads in an AI environment.                            |
| **SSP (Supply-Side Platform)** | Integration partner for publishers, providing intelligence and the “Ad Agent.”  |
| **Ad Exchange**            | Brokers auctions, ensuring transparency and preventing fraud.                  |
| **DSP**                    | Demand-side platform or advertiser buying GPT ad slots.                        |
| **Ad Agent**               | The intelligent module (often from an SSP) that generates the request or helps shape the ad request/response. |
| **Placement**              | The text slot or impression opportunity.                                       |

## 3.3 Reference Model <a name="reference_model"></a>

A typical chain with role separation:

```
(Publisher/AI) --> [SSP / Ad Agent] --> [Ad Exchange] --> [DSP/Advertiser]
```

- **Publisher** calls into an **SSP** to generate an ADS4GPTs request. The SSP may add intelligence (floor pricing, brand safety signals, etc.) or even create the ad request from scratch with an Ad Agent.  
- The **Ad Exchange** receives the request from the SSP, runs an auction among multiple DSPs, ensures transaction transparency, and checks for fraud.  
- DSPs respond with bids; the exchange picks winners and notifies the SSP, which in turn notifies the publisher and helps serve or integrate the GPT ad.

## 3.4 Protocol Layers <a name="protocol_layers"></a>

1. **Layer-1: Transport**  
   - **HTTPS** (TLS 1.2+).  
2. **Layer-2: Format**  
   - **JSON** as default.  
3. **Layer-3: Transaction**  
   - The core ADS4GPTs request/response for real-time bidding.  
4. **Layer-4: Domain**  
   - GPT text ads domain objects.

---

# 4. Specification <a name="specification"></a>

## 4.1 Top-Level Structure <a name="21-top-level-structure"></a>

All **ADS4GPTs** bid request payloads must have a top-level JSON object:
```json
{
  "ads4gpts": {
    "ver": "1.0",
    "domainspec": "ads4gpts",
    "domainver": "1.0",
    "request": { ... },
    "response": { ... }
  }
}
```
Where:
- **`ver`**: ADS4GPTs version (e.g., `"1.0"`).  
- **`domainspec`**: Identifies that this domain specification is `"ads4gpts"`.  
- **`domainver`**: Domain schema version (`"1.0"`).  
- **`request`**: The core ad request object ([Object: Request](#22-object-request)).
- **`response`**: The core ad response object ([Object: Request](#22-object-response)).

---

## 4.2 Bid Request Schema <a name="bid_request"></a>

### Object: Request <a name="object_request"></a>


| Field   | Type              | Description                                                                                               |
|---------|-------------------|-----------------------------------------------------------------------------------------------------------|
| `id`    | string *(req)*    | Unique request ID.                                                                                        |
| `tmax`  | integer *(opt)*   | Maximum time (ms) allowed for the auction.                                                                |
| `at`    | integer *(opt)*   | Auction type (1=First Price, 2=Second Price, etc.). Default `2`.                                         |
| `cur`   | string[] *(opt)*  | Accepted currencies (e.g., `[ "USD", "EUR" ]`). Default `[ "USD" ]`.                                      |
| `source`| object *(opt)*    | A [Source](#23-object-source) object with transaction-level data.                                         |
| `item`  | array *(req)*     | One or more [Item](#24-object-item) objects describing the GPT ad placements.                             |
| `context` | object *(req)*  | A [Context](#27-object-context) object specifying environment and targeting details (e.g., `app`, `device`, etc.).      |             |

> **Note**: `tmax`, `at`, and `cur` are **optional** but recommended to ensure correct bidding logic.

SSPs typically create and populate this **Request** on behalf of the publisher, adding intelligence or brand safety signals. Then the SSP forwards it to an **Ad Exchange** for bidding.

---

### Object: Item <a name="24-object-item"></a>

Mirrors how an “impression” might be handled, but is **GPT-based**. At least one `item` object is required:

| Field     | Type        | Description                                                                                                |
|-----------|------------|------------------------------------------------------------------------------------------------------------|
| `id`      | string *(req)* | Unique ID for this item/placement.                                                                  |
| `qty`     | integer *(opt)*| Indicates how many billable events or “impressions.” Default `1`.                                    |
| `private` | integer *(opt)*| Flag for private deals: `0` = open, `1` = deals-only.                                                |
| `deal`    | array *(opt)*  | Array of Deal objects for PMP or direct deals.                                    |
| `spec`    | object *(req)* | The domain-specific object. Typically `spec.placement` referencing a GPT Ads Placement.             |

**Example**:
```json
{
  "id": "1",
  "qty": 1,
  "private": 0,
  "deal": [
    {
      "id": "1234",
      "flr": 1.50
    }
  ],
  "spec": {
    "placement": { /* see GPT Ads Placement */ }
  }
}
```

### Object: Source <a name="object_source"></a>

| **Attribute** | **Type** | **Description**                                                                                   |
|---------------|----------|---------------------------------------------------------------------------------------------------|
| `tid`         | string   | Transaction ID shared across the chain.                                                           |
| `ts`          | integer  | Timestamp in Unix ms.                                                                             |
| `ds`          | string   | Digital signature if relevant for authenticity.                                                   |
| `dsmap`       | string   | Describes the signature creation.                                                                  |
| `cert`        | string   | Public key cert.                                                                                  |
| `schain`      | object   | Supply chain details (e.g., node references).                                                     |
| `pchain`      | string   | Payment chain reference.                                                                          |
| `ext`         | object   | Any additional data from the SSP or Exchange.                                                    |

**Note**: If an **Ad Exchange** also wants to add an internal transaction ID before passing the request onward (in multi-exchange scenarios), it may do so in `ext`.

---

### Object: GPT Ads Placement <a name="object_gpt_ads_placement"></a>

Focus on text impressions for GPT or chat scenarios.


| **Field**               | **Type**  | **Description**                                                                                           |
|-------------------------|-----------|-----------------------------------------------------------------------------------------------------------|
| `placement_id`          | string    | Unique ID for the GPT text slot.                                                                          |
| `context`               | string    | e.g., `"assistant"`, `"chat"`, `"discussion"`.                                                            |
| `placement_type`        | string    | e.g., `"inline"`, `"suggested"`.                                                                            |
| `minimum_bid`           | float     | Optional floor price.                                                                                     |
| `text_requirements`     | object    | Character limits, CTA usage, link usage, disclosure label position.                                       |
| `blending_option`       | string    | `"none"`, `"partial"`, or `"full"`. If partial or full, text can be re-styled or reworded to match GPT.   |
| `metadata`              | object    | Additional info for brand safety, adjacency constraints, etc.                                            |

For more information about GPT Ads Placement read the ADS4GPTs [document](../Ad%20Placement%20Specifications/GPT%20Text%20Impression.md).

---

### Object: Deal <a name="object_deal"></a>

For private marketplace deals or direct deals, references can be included:

| **Attribute** | **Type** | **Description**                                        |
|---------------|----------|--------------------------------------------------------|
| `id`          | string   | Deal ID.                                               |
| `flr`         | float    | Deal floor.                                            |
| `flrcur`      | string   | Currency of the deal floor.                            |
| `at`          | integer  | Auction type override (1=First Price, etc.).          |
| `wseat`       | array    | Whitelisted buyer seats.                               |
| `wadomain`    | array    | Whitelisted advertiser domains.                        |
| `ext`         | object   | Custom fields.                                         |

---

### Object: Context <a name="27-object-context"></a>

Describes the environment in which ads are shown (e.g., app, device). It can reference adapted AdCOM or custom fields:

| Field          | Type   | Description                                                                                                  |
|----------------|--------|--------------------------------------------------------------------------------------------------------------|
| `app/site`          | object | Denotes an app environment or web app.  
| `targeting`          | object | The targeting data from the Ad Agent or the AI call from the Publisher.                                                                     |
| `device`       | object | Device info, e.g., OS, IP.                                                                                   |
| `regs`         | object | Regulatory signals (GDPR, CCPA).                                                                             |
| `restrictions` | object | Potential brand-safety or content restrictions.                                                              |
| `ext`          | object | Optional.                                                                                                     |

**Note**: If a site-based environment is used, you could replace `app` with `site`.

---


### Object: GPT Ads Targeting <a name="object_targeting"></a>

```json
{
  "id": "hashed-or-anon-user-id",
  "User": {
    "gender": "string",
    "age_range": "string",
    "persona": "string"
  },
  "Ad Recommendation": "string",
  "Undesired Ads": "string"
}
```

**Key Points**  
- Typically populated by the **SSP’s Ad Agent** or the publisher’s AI.  
- Avoids any PII, using broad categories for demographics.

For more information read the ADS4GPTs [document](../Communication%20Protocols/GPT%20Ads%20Targeting%20Object%20Protocol.md)

---

### Example Request <a name="=example_request"></a>

Below is a **full** sample request conforming to **ADS4GPTs 1.0**:

```json
{
  "ads4gpts": {
    "ver": "1.0",
    "domainspec": "ads4gpts",
    "domainver": "1.0",
    "request": {
      "id": "0123456789ABCDEF",
      "tmax": 150,
      "at": 2,
      "cur": [ "USD", "EUR" ],
      "source": {
        "id": "FEDCBA9876543210",
        "ts": 1541796182157,
        "cert": "ads-cert.1.txt"
      },
      "item": [
        {
          "id": "1",
          "qty": 1,
          "private": 0,
          "deal": [
            {
              "id": "1234",
              "flr": 1.50
            }
          ],
          "spec": {
            "placement": {
              "gpt_ads_placement": {
                "placement_id": "GPT-SLOT-01",
                "version": "1.0",
                "context": "assistant",
                "placement_type": "inline",
                "minimum_bid": 0.50,
                "text_requirements": {
                  "headline_max_length": 80,
                  "body_max_length": 200,
                  "disclosure_position": "top",
                  "cta_allowed": true,
                  "url_allowed": true
                },
                "blending_option": "partial"
              }
            }
          }
        }
      ],
      "context": {
        "app": {
          "app_name": "ExampleGPTApp",
          "app_bundle": "com.example.gptapp"
        },
        "device": {
          "ua": "Mozilla/5.0 (Linux; Android 9; ... )",
          "ip": "192.168.0.1"
        },
        "regs": {
          "gdpr": 1
        },
        "restrictions": {
          "adult": 0
        }
      },
      "targeting": {
        "id": "ANON-USER-12345",
        "User": {
          "gender": "female",
          "age_range": "25-34",
          "persona": "Fitness Enthusiast"
        },
        "Ad Recommendation": "Sports apparel, yoga mats",
        "Undesired Ads": "Weight loss pills, gambling"
      }
    }
  }
}
```

---

## 4.3 Bid Response Schema <a name="bid_response"></a>

### Object: Response <a name="object_response"></a>

| **Attribute** | **Type** | **Description**                                                           |
|---------------|----------|---------------------------------------------------------------------------|
| `id`          | string   | Echoes the request `id`.                                                  |
| `bidid`       | string   | DSP- or Exchange-generated identifier.                                    |
| `nbr`         | integer  | No-bid reason code if no valid bids.                                      |
| `cur`         | string   | Currency of bids, default `"USD"`.                                        |
| `seatbid`     | array    | Array of seat-level bids.                                                 |
| `ext`         | object   | Additional data from DSP or Exchange.                                     |

**Ad Exchanges** gather responses from multiple DSPs, pick the winning seat(s), and forward a simplified `Response` to the **SSP**, who notifies the publisher and helps deliver the GPT Ad.

---

### Object: Seatbid <a name="object_seatbid"></a>

| **Attribute** | **Type**      | **Description**                                                                                      |
|---------------|---------------|------------------------------------------------------------------------------------------------------|
| `seat`        | string        | Identifier for the buying entity.                                                                    |
| `package`     | integer       | 0=Individual items can win, 1=Package deal only.                                                     |
| `bid`         | array         | One or more [Bid](#object_bid) objects.                                                              |
| `ext`         | object        | DSP or Exchange extensions.                                                                          |

---

### Object: Bid <a name="object_bid"></a>

Detail of each bid for a GPT Ads Placement, which is a much simplified OpenRTB schema:

| Attribute | Type | Definition |
| --- | --- | --- |
| id | string; recommended | Bidder generated bid ID to assist with logging/tracking. |
| item | string; required | ID of the item object in the related bid request; specifically item.id. |
| price | float; required | Bid price expressed as CPM although the actual transaction is for a unit item only. Note that while the type indicates float, integer math is highly recommended when handling currencies (e.g., BigDecimal in Java). |
| deal | string | Reference to a deal from the bid request if this bid pertains to a private marketplace deal; specifically deal.id. |
| cid | string | Campaign ID or other similar grouping of brand-related ads. Typically used to increase the efficiency of audit processes. |
| tactic | string | Tactic ID to enable buyers to label bids for reporting to the exchange the tactic through which their bid was submitted. The specific usage and meaning of the tactic ID should be communicated between buyer and exchanges beforehand. |
| media | object | Layer-4 domain object structure that specifies the GPT Ad. Find the GPT Ads supported at the [repo](../Ad%20Specifications/)|
| ext | object | Optional demand source specific extensions. |
| purl | string | Pending notice URL called by the exchange when a bid has been declared the winner within the scope of an OpenRTB compliant supply chain (i.e., there may still be non-compliant decisioning such as header bidding). Substitution macros may be included. |
| burl | string; recommended | Billing notice URL called by the exchange when a winning bid becomes billable based on exchange-specific business policy (e.g., markup rendered). Substitution macros may be included. |
| lurl | string | Loss notice URL called by the exchange when a bid is known to have been lost. Substitution macros may be included. Exchange-specific policy may preclude support for loss notices or the disclosure of winning clearing prices resulting in ${OPENRTB_PRICE} macros being removed (i.e., replaced with a zero-length string). |


### Object GPT Text Ad: <a name="gpt_text_ad"></a>

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
| **Metadata** | Object with useful information for Ad Optimization                                 | –                       | No          | Similar to the ext object of OpenRTB protocol.  

For more information about the GPT Text Ad read more in the [documentation](../Ad%20Specifications/GPT%20Text%20Ads.md)

---

### Example Response <a name="example_response"></a>


```json
{
  "ads4gpts": {
    "ver": "1.0",
    "response": {
      "id": "REQUEST-12345",            // Echoes the request ID
      "bidid": "BID-ABC-789",          // DSP/Exchange-generated ID for tracking
      "cur": "USD",                    // Currency in which bids are expressed
      "seatbid": [
        {
          "seat": "DSP-001",          // Identifier for the DSP or seat
          "package": 0,               // 0=Individual items can win
          "bid": [
            {
              "id": "BID-xyz-1",      // Bid's unique ID
              "item": "1",            // Matches item.id from the request
              "price": 2.50,          // CPM bid price
              "deal": "DEAL-1234",    // If referencing a private marketplace deal from the request
              "cid": "CAMPAIGN-5678", // Campaign ID for auditing/creative bundling
              "tactic": "retargeting",// Tactic label or strategy identifier

              "media": {
                // GPT Text Ad: Inline text ad structure
                "gpt_text_ad": {
                  "ad_identifier": "TXT-AD-0001",
                  "advertiser_name": "SampleBrand",
                  "headline": "Upgrade Your Home Office!",
                  "body": "Discover ergonomic chairs and desks for a more productive workspace.",
                  "disclosure_label": "Sponsored",
                  "cta": "Shop Now",
                  "cta_url": "https://example.com/home-office",
                  "blend": "none",            
                  "metadata": {
                    "tags": ["ergonomic", "home_office"],
                    "someCustomHint": "Optimize for user interest in productivity"
                  }
                }
              },

              // Event Notification URLs
              "purl": "https://example-ssp.com/pending?bid=${OPENRTB_BID_ID}&price=${OPENRTB_PRICE}",
              "burl": "https://example-ssp.com/billing?bid=${OPENRTB_BID_ID}&price=${OPENRTB_PRICE}",
              "lurl": "https://example-ssp.com/loss?bid=${OPENRTB_BID_ID}&reason=${OPENRTB_LOSS}",

              // Additional Bid Extensions
              "ext": {
                "creative_rating": "PG",
                "thirdPartyVerification": true
              }
            }
          ]
        }
      ],
      // Top-level ext if the Exchange or DSP needs to pass extra data
      "ext": {
        "response_info": "Any additional metadata from DSP or Exchange"
      }
    }
  }
}
```

---


## 4.4 Event Notification <a name="event_notification"></a>

We are following the OpenRTB 3.0 protocol.

### Event: Pending <a name="event_pending"></a>

Fired when the ad wins at the exchange level. The exchange or SSP calls the `purl`.

### Event: Billing <a name="event_billing"></a>

Once the GPT ad is rendered or meets a **billable** condition, the exchange or SSP calls `burl`. This is typically **the** official transaction event for DSPs.

### Event: Loss <a name="event_loss"></a>

If a bid is definitively not served, the exchange or SSP may call `lurl` with the appropriate **Loss Reason Code**.

### Complex Event Chains <a name="complex_event_chains"></a>

Because we have both **SSPs** and **Ad Exchanges** in the chain, events can pass through multiple entities. Best practice: handle final signals (especially **billing**) server-side for accuracy.

---

## 4.5 Inventory Authentication <a name="inventory_auth"></a>

**ADS4GPTs** optionally supports digital signatures (similar to `ads.cert`). The **publisher** or **SSP** can sign certain immutable fields (e.g., domain, timestamp) to ensure authenticity. The **Ad Exchange** or DSP can verify them if needed. The details (signature creation, key distribution) are out of scope but align with the typical approach to cryptographic hashing and public key usage.

---


# 5. Enumerations <a name="enumerations"></a>

## 5.1 No-Bid Reason Codes <a name="nobid_reason"></a>

Examples:

| Value | Definition                        |
|-------|-----------------------------------|
| 0     | Unknown Error                     |
| 1     | Technical Error                   |
| 2     | Invalid Request                   |
| 15    | Insufficient Auction Time         |
| 500+  | Exchange/SSP-specific codes       |

## 5.2 Loss Reason Codes <a name="loss_reason"></a>

Examples:

| Value | Definition                              |
|-------|-----------------------------------------|
| 0     | Bid Won (for event clarity)             |
| 1     | Internal Error                          |
| 2     | Impression Opportunity Expired          |
| 102   | Lost to Higher Bid                      |
| 200+  | Creative Filtered (various subreasons)  |
| 500+  | Exchange/SSP-specific codes             |

---



