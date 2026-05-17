# Demand Review AI Agent

Final project for the Building AI course.

## Summary

An AI agent that automates the preparation of monthly Demand Review meetings 
for supply chain and demand planning teams. It analyzes forecast accuracy, 
detects anomalies, drafts narrative insights, and generates ready-to-present 
slides, reducing meeting prep time from 8 hours to 30 minutes.

## Background

Monthly Demand Review meetings are a critical ritual in modern supply chain 
organizations: they align Sales, Marketing, Finance, Operations, and Supply 
Chain around a consensus demand plan. Yet preparing them is painful: demand 
planners spend 1 to 2 full days per affiliate, every month, pulling data 
from multiple systems (SAP, APS, Excel, Power BI), formatting slides, 
chasing comments, and writing the same kind of narrative over and over.

Problems this idea addresses:
* Demand planners burn 20 to 40% of their time on meeting prep instead of 
  actual planning analysis.
* Insights are inconsistent across affiliates (each planner has a different 
  format and depth).
* Anomalies in forecast bias, MAPE, or stock projections are often missed 
  because no one has time to look at every SKU.
* Knowledge is trapped in slide decks instead of structured data.

My personal motivation: as a Demand and Procurement Manager managing demand 
planners across multiple affiliates, I see this gap every month. An AI 
agent that consolidates data, surfaces anomalies, and writes a first draft 
of the narrative would free planners to focus on what humans do best: 
judgment, negotiation, and creativity.

## How is it used?

The user is a demand planner or demand manager preparing a monthly Demand 
Review for one or several affiliates. The workflow:

1. The agent connects to the company's data sources (forecast database, 
   sales history, stock levels, customer orders).
2. It computes the standard KPIs automatically (MAPE, WMAPE, BIAS, forecast 
   accuracy by SKU group, top movers, top contributors to error).
3. It detects anomalies using statistical thresholds and simple ML 
   (forecasts deviating more than X sigmas from baseline, sudden trend 
   breaks, unusual SKU launches or discontinuations).
4. It writes a draft executive narrative in the company's tone, highlighting 
   the 3 to 5 key insights worth discussing in the meeting.
5. It generates a slide deck following the company template, including 
   charts, KPI tables, and the narrative.
6. The planner reviews, edits, and finalizes the deck in 30 minutes 
   instead of 8 hours.

The solution should be designed with respect for the planner's expertise: 
it is an assistant, not a replacement. The planner remains in control and 
is responsible for final decisions.

## Data sources and AI methods

Data sources:
* Forecast database (from APS systems such as Bevolta, SAP IBP, Kinaxis)
* ERP transactional data (sales orders, deliveries, stock movements)
* Sales history (anonymized customer orders by SKU, channel, geography)
* Marketing calendar (promo events, product launches, NPI dates)
* External signals where relevant (weather data, industry indices)

AI methods:
* Statistical analysis (MAPE, WMAPE, BIAS, forecast accuracy decomposition)
* Anomaly detection (z-score, isolation forest for multivariate outliers)
* Time series forecasting baseline (exponential smoothing, Prophet)
* Large Language Models (Claude API or similar) for narrative generation 
  and document drafting
* Retrieval-Augmented Generation (RAG) to ground the narrative in actual 
  company data and previous meeting minutes
* Prompt engineering to enforce the company's tone of voice and template

| Component             | Method                                       |
| --------------------- | -------------------------------------------- |
| KPI computation       | Pandas, NumPy, statistical formulas          |
| Anomaly detection     | Isolation Forest, z-score, business rules    |
| Narrative generation  | LLM with RAG over historical decks           |
| Slide generation      | python-pptx with branded company template    |

## Challenges

What this project does not solve:
* It does not replace human judgment. Many demand decisions depend on 
  context the AI cannot access (a salesperson's intuition about a customer, 
  a marketing decision still under discussion, a strategic priority shift).
* It does not improve the underlying forecast accuracy by itself. It 
  surfaces problems faster but the root causes still need human resolution.
* It depends entirely on the quality of the input data. Garbage in, garbage 
  out remains true.

Ethical considerations:
* Risk of over-trust: planners may accept AI-drafted narratives without 
  critical review, propagating mistakes.
* Risk of skill atrophy: if junior planners never write their own narrative, 
  they may lose the analytical instinct that makes a senior planner valuable.
* Data privacy: sales data is sensitive and must stay within the company's 
  controlled environment. The LLM provider must offer enterprise-grade 
  data protection.
* Bias in anomaly detection: the model may systematically over-flag certain 
  SKU categories or affiliates if the training data is unbalanced.

## What next?

Possible evolutions of the project:
* Extend to other recurring meetings: S&IOP, supplier reviews, procurement 
  performance reviews.
* Multilingual support to scale across affiliates (French, English, Spanish, 
  Italian, German).
* Voice interface to allow planners to interact with the agent during the 
  meeting itself.
* Integration with Microsoft Teams or Slack to deliver KPIs and alerts 
  inline in daily workflows.
* Closed-loop learning: capture the human edits to the AI draft and use 
  them to fine-tune future drafts.

Skills and assistance needed:
* Stronger Python and data engineering skills.
* Hands-on experience with LLM orchestration frameworks (LangChain, 
  LlamaIndex, or direct API integration).
* Collaboration with a data engineer to handle real production data pipelines.
* Feedback from demand planners across multiple companies to validate the 
  approach beyond a single organization.

## Acknowledgments

* Inspired by the Elements of AI and Building AI courses by the University 
  of Helsinki and Reaktor Innovations.
* DDMRP methodology insights from Carol Ptak and Chad Smith.
* Anthropic Claude API documentation and best practices for prompt 
  engineering.
* The supply chain community on LinkedIn for many discussions about the 
  friction in monthly planning rituals.
