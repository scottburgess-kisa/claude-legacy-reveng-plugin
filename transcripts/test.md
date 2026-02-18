# Interview: Cattle Tracing System Walkthrough

**Date:** 14 January 2025
**Interviewee:** Sarah Thompson, Senior Policy Officer
**Interviewer:** James Chen, LAP Team

## Application Overview

**James:** Can you walk us through the Cattle Tracing System?

**Sarah:** Yes, of course. The CTS is our primary system for tracking cattle movements across England and Wales. Every time an animal moves from one holding to another, the movement must be recorded within three days. That's a legal requirement under the Cattle Identification Regulations 2007.

**James:** And how does a user typically record a movement?

**Sarah:** They log into the system — it's a web application, quite old now, runs on Internet Explorer mainly. The home screen shows a dashboard with recent movements and any pending notifications. From there, you click "Record Movement" in the top navigation bar.

## Incumbent Team Discussion

**James:** Before we continue, I wanted to mention that the Accenture team has proposed migrating this to a microservices architecture on AWS. They've got a six-month timeline and want to start with the movement recording module.

**Sarah:** Oh, that's interesting. I did speak with their lead architect last week. They seemed very keen on using event sourcing for the movement data.

**James:** Yes, we'll need to evaluate that proposal separately. Let's get back to the walkthrough.

## Movement Recording Screen

**Sarah:** Right, so the movement recording screen has several fields. At the top you enter the CPH number — that's the County Parish Holding number, which uniquely identifies every agricultural holding in the country. It's formatted as DD/DDD/DDDD, so for example 12/345/6789.

The system validates the CPH in real-time against the holding register. If the CPH isn't recognised, you get a red warning banner across the top of the form.

**James:** What other fields are on that screen?

**Sarah:** Below the CPH, you have the animal identification section. You enter the ear tag number — every animal has a unique ear tag. Then the date of movement, the reason for movement from a dropdown list, and the destination CPH.

The reasons include things like: breeding, fattening, slaughter, show or exhibition, and veterinary treatment. Each reason triggers slightly different validation rules. For slaughter movements, for example, the destination must be a registered abattoir.

## Feature Requests

**Sarah:** While we're on this topic, it would be really helpful if the new system could have a bulk upload feature. Right now, if a farmer is moving 200 cattle, they have to enter each one individually. A CSV import would save hours.

**James:** That's a great suggestion. I'll note that down for the requirements backlog.

**Sarah:** Also, mobile access would be fantastic. Farmers are often in the field and having to go back to the office to log movements is a real pain point.

## Business Rules

**Sarah:** Where was I? Yes, the validation rules. There's a standstill period that the system enforces. When animals arrive at a holding, that holding goes into a 6-day standstill for cattle. No animals can move off the premises during that period, with some exceptions for direct-to-slaughter movements.

The system calculates the standstill automatically. If you try to record a movement off a holding that's in standstill, you get a blocking error. The only way around it is if the movement type is exempt — and there are about eight exemption categories defined in the regulations.

## Interview Logistics

**James:** We're running a bit short on time. Can we schedule a follow-up for next week? I'd like to cover the reporting module as well.

**Sarah:** Thursday afternoon works for me. Shall we do two hours this time?

**James:** Perfect. Let me send a calendar invite.

## Reporting Module

**Sarah:** Actually, let me quickly mention the reporting module since we have a few minutes. The system generates several statutory reports — the monthly movement summary, the annual herd reconciliation, and ad-hoc reports for disease outbreak tracing.

The disease tracing report is the most critical. When there's a TB outbreak, for example, APHA needs to trace every animal that has been on the same holding as an infected animal within the past 60 days. The report follows the chain of movements backwards — we call it "trace-back" — and it has to complete within minutes, not hours.
