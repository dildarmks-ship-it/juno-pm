# AI Strategy One-Pager - Juno Automated Order Status

## 1. Problem & Workflow

Provide real time status of an order - Received, Processing, Prior Auth Required, Doctor Review, Packaging in Progress, Shipped, Cancelled (with reason)

## 2. Target Metrics

1. Member satisfaction increase by 10%
2. CSR Call reduction
3. Reduction in AHT
4. Common service API adoption by all front-end digital channels in first 30 days - this metrics leadership will say "do not touch"

## 3. Autonomy Level

Copilot or Agent
Not choosing Specify Assist

## 4. Data & Model Approach

LLM / RAG
I am not taking the fine-tune options because this is patient specific and time sensitive data related to health and I have to be accurate due to patient health and safety reasons

## 5. Risks & Mitigations

Orders presented only for the logged in member and dependants in the household
Apply the State specific rules - certain states do not allow Teen data to be presented to the Parent

## 6. V1 Scope

In: Surfaces real-time order status and flags orders where a Prior Authorization is required, needing patient follow-up
Out: Juno will not display order or health details for a dependant to a parent account in states where minor-consent law restricts that visibility.

## Why now

_Market timing + why this is defensible._

_____

## Success metric

_The single number that says the bet paid off._

_____
