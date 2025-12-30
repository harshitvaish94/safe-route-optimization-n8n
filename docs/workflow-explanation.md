#Workflow Explanation – Safe Route Optimization (n8n)
This document explains the complete n8n workflow, decision logic, and sample output used in the Safe Route Optimization at Night project. The workflow is designed to prioritize route safety over shortest distance using multi-criteria evaluation.
#Workflow Overview
Webhook → Google Sheets → Code → Respond to Webhook
It accepts a route request, evaluates multiple candidate routes based on safety parameters, and returns the safest route.It accepts a route request, evaluates multiple candidate routes based on safety parameters, and returns the safest route.
Nodes Used and Their Roles
-> 1)Webhook Node

Purpose: Entry point of the workflow.

Accepts HTTP POST requests

Receives input parameters:

{
  "start": "Koramangala",
  "end": "MG Road"
}


Tested using Postman

Triggers the workflow execution

 This allows the workflow to behave like an API.
->2) Google Sheets – Get Row(s) Node

Purpose: Fetch route data and safety parameters.

Connected to a Google Sheet (demo → Sheet1)

Retrieves all rows matching the start and end locations

Each row represents a possible route option

Sheet Structure:

Start	End	Route	Time (min)	Safety Score
Koramangala	MG Road	Brigade Road	20	3
Koramangala	MG Road	Richmond Road	25	8

 Safety Score is the primary decision factor, not time.
->3) Code Node (JavaScript Logic)

Purpose: Compute the safest route using multi-criteria logic.

Runs once for all retrieved routes

Compares routes using Safety Score

Selects the route with the highest safety value
This ensures:

Distance/time is secondary

Safety is always prioritized
->4) Respond to Webhook Node

Purpose: Return the final response to the requester.

Sends the selected safest route as JSON

Acts as the API response layer
->Sample API Output
{
  "safestRoute": {
    "row_number": 3,
    "Start": "Koramangala",
    "End": "MG Road",
    "Route": "Richmond Road",
    "Time (min)": 25,
    "Safety Score": 8
  }
}
 Although Brigade Road is faster, Richmond Road is selected due to higher safety.

#Key Design Decisions

Safety Score takes precedence over distance

External data source (Google Sheets) allows easy updates

Webhook-based design enables API-style usage

No-code + minimal-code hybrid approach
->Tools & Technologies

n8n – Workflow orchestration

Google Sheets – Data storage

JavaScript (Code Node) – Decision logic

Postman – API testing

Webhooks – Integration layer
->Why This Matters

This project demonstrates how no-code automation can solve real-world safety problems using structured logic and external data, without building a full backend system.

