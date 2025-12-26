# vancouver_housing_crisis

---

## Project Overview: Vancouver Housing Crisis Assistant

### Problem Context

Vancouver faces:

* **High housing prices and rents**
* **Low vacancy rates**
* **Power imbalance between landlords and tenants**

Many renters turn to **Reddit** (e.g., r/vancouver, r/canadahousing, r/legaladvicecanada) to:

* Share bad landlord experiences
* Ask what to do in stressful or confusing situations
* Learn from others’ past actions

However:

* This information is scattered
* Advice is hard to search or summarize
* Market data and lived experience are rarely combined

---

## Project Goal

Build a **two-part system**:

1. **Housing Price Prediction Model**

   * Predict housing prices or rent trends in Vancouver
   * Help users understand market pressure and affordability

2. **RAG-based Rental Assistant**

   * Uses Reddit comments and posts
   * Answers renter questions by surfacing **what others did in similar situations**
   * Focuses on experience-based guidance, not legal advice

Together, this system connects **data-driven market trends** with **real renter experiences**.

---

## System Architecture (High-Level)

```
                  ┌──────────────────────┐
                  │ Housing Market Data   │
                  │ (prices, rents, etc.) │
                  └──────────┬───────────┘
                             │
                    Predictive Model
                             │
                             ▼
                  Market Insight Outputs
                             │
                             ▼
┌──────────────┐     ┌─────────────────────┐
│ Reddit Data  │ ──▶ │ Vector Database     │
│ (posts &    │     │ (Embeddings)        │
│ comments)   │     └──────────┬──────────┘
└──────────────┘                │
                        RAG Pipeline
                                │
                                ▼
                     Rental Advice Assistant
```

---

## Sector 1: Predictive Model for Housing Prices

### Objective

Predict:

* Average rent or housing prices by area
* Trends over time (e.g., increasing/decreasing pressure)

### Possible Targets

* Monthly average rent
* Condo or detached home prices
* Price per square foot

### Features You Could Use

* Location (neighborhood, postal code)
* Property type (apartment, condo, house)
* Size (sq ft, bedrooms)
* Time (month/year)
* Interest rates (optional)
* Vacancy rates (optional)

### Output Examples

* “Rent in East Vancouver has increased ~X% over Y months”
* “Predicted average 1-bedroom rent next quarter: $____”

---

## Sector 2: RAG Over Reddit for Rental Advice

### Objective

Help renters ask questions like:

* “My landlord is trying to raise rent illegally”
* “They won’t fix mold—what did others do?”
* “I’m being pressured to move out—what are my options?”

The assistant responds with:

* Summarized past experiences
* Common actions people took
* Outcomes (filed complaint, negotiated, moved, etc.)

⚠️ **Important**:
This is **experience-based guidance**, not legal advice.

---

### Reddit Data Pipeline

#### 1. Data Collection

Target subreddits:

* r/vancouver
* r/canadahousing
* r/legaladvicecanada
* r/PersonalFinanceCanada

Data to collect:

* Post title
* Post body
* Top-level comments
* Timestamps
* Upvotes (optional)

#### 2. Cleaning & Filtering

* Remove deleted/empty posts
* Filter for rental-related keywords:

  * “landlord”
  * “rent increase”
  * “eviction”
  * “RTB”
  * “damage deposit”

#### 3. Chunking & Embeddings

* Split long posts/comments into chunks
* Generate embeddings
* Store in a vector database (e.g., FAISS, Chroma)

---

### RAG Query Flow

1. User asks a question:

   > “My landlord won’t return my damage deposit”

2. System:

   * Embeds the query
   * Retrieves similar Reddit posts/comments

3. LLM:

   * Summarizes what others did
   * Groups responses by common actions
   * Adds a safety disclaimer

#### Example Output

> “Based on similar renter experiences in Vancouver:
>
> * Many renters filed a claim with the RTB within 15 days
> * Some sent a formal written demand first
> * Several users reported success when documenting everything
> * A few chose to settle informally to avoid delays”

---

## Ethical & Design Considerations

* **No legal advice claims**
* Clear disclaimers
* Avoid naming or targeting individuals
* Focus on patterns, not single anecdotes
* Be careful with bias (Reddit skews younger, tech-savvy)

---

## Suggested Next Steps (Actionable)

### Phase 1: Scope & Planning

* Define exact prediction target (rent vs home price)
* Choose 3–4 renter issue categories (rent increase, eviction, repairs)

### Phase 2: Data Collection

* Gather housing market dataset
* Scrape Reddit posts/comments
* Store raw data cleanly

### Phase 3: Build MVP Models

* Train a simple price prediction model
* Create a basic RAG pipeline that answers 1–2 renter questions well

### Phase 4: Evaluation

* Compare predicted vs real prices
* Manually check RAG answers for relevance and safety

### Phase 5: Presentation / Deployment

* Simple web app or notebook demo
* Example queries + outputs
* Explain limitations clearly
