# User Research & Survey Analysis Report (DIP-15)

**Project Name:** OmniCinema  
**Task ID:** DIP-15 (User Interviews and Requirements Validation)  

---

## 1. Executive Summary

This report documents the findings from the initial user research phase conducted for the **OmniCinema** booking platform. To establish empirical grounding for our functional requirements (DIP-20) and technical decisions (such as transactional TTL holds and QR ticketing), a target survey was conducted collecting feedback from potential cinema-goers.

The primary objectives were:
1. Identifying pain points in existing aggregator solutions (e.g., Ticketon, Kino.kz).
2. Empirically determining the optimal Seat Hold Time-To-Live (TTL) duration to avoid race conditions and double bookings.
3. Validating user demand for self-service ticket cancellations, QR-based electronic passes, and interactive hall layouts.

---

## 2. Research Methodology & Dataset Overview

* **Data Collection Method:** Online structured survey and user interviews.
* **Dataset File:** `docs/user_research/Cinema_ticket_booking_survey.csv`
* **Sample Size ($N$):** 9 responses (Initial cohort for Pre-Defense milestone; expansion planned for post-defense pilot testing DIP-16).
* **Target Audience:** Active cinema visitors purchasing tickets online.

---

## 3. Key Findings & Data Metrics

### 3.1 Visitor Behavior & Purchasing Habits (Q1–Q5)
* **Frequency:** 66.7% of respondents visit cinemas 1–2 times every 3 months, with 22.2% high-frequency visitors (6–10 times per quarter).
* **Group Size:** 66.7% buy **2 tickets per transaction** (couples/pairs), while 33.3% buy for groups of 3–5 people.
* **Booking Window:** 55.6% purchase tickets **on the same day** as the session, making system responsiveness and real-time seat availability critical.
* **Preferred Platforms:** Aggregator services (e.g., Ticketon, Kino.kz) are preferred over individual cinema chain apps due to session comparison convenience.

### 3.2 Pain Points & Market Friction (Q6–Q7)
When evaluating negative experiences with existing booking solutions, users reported:
1. **Race Conditions / Double Booking Issues:** Experiencing situations where two people were allocated the same seat or where seat holds dropped during payment processing.
2. **Hidden Commissions:** Unexpected service fees added at the final checkout step.
3. **Rigid Cancellation Policies:** Inability to cancel or modify bookings without visiting the physical box office.

### 3.3 Seat Selection & Hold TTL Justification (Q8–Q9)
* **Seat Preferences:** High demand for central seats (middle rows) and aisle seats.
* **Hold TTL (Time-To-Live) Preference:**
  * **2 Minutes:** 44.4% (4 respondents)
  * **5 Minutes:** 22.2% (2 respondents)
  * **10 Minutes:** 22.2% (2 respondents)
  * **15 Minutes:** 11.1% (1 respondent)

 **Architectural Decision Derivation (DIP-5 / DIP-21):**  
 Based on user feedback, setting a hold timeout that is too short (e.g., 2 minutes) creates transaction anxiety during payment gateway redirect, while a long timeout (10–15 minutes) leads to artificial seat scarcity.  
 **Conclusion:** The system will implement a **2 to 5-minute dynamic hold (TTL)** with a prominent countdown timer on the frontend UI.

### 3.4 Desired Features & Value Additions (Q10–Q12)
1. **Electronic QR Tickets:** Instant generation of offline-accessible QR codes for turnstile scanning.
2. **Self-Service Cancellation:** Automatic refund/cancellation window up to 30–60 minutes before session start.
3. **Transparent Pricing:** Clear itemized pricing without hidden processing fees.

---

## 4. Mapping Research to Technical Requirements

| User Feedback / Pain Point | Derived System Requirement | YouTrack Task Reference |
| :--- | :--- | :--- |
| "Double booking occurs on popular sessions" | Atomic database transactions with Redis/PostgreSQL seat locking (`hold_id`) | **DIP-5** (Backend) |
| "Need enough time to enter payment details" | 2–5 min Hold TTL with real-time countdown timer | **DIP-5** / **DIP-8** |
| "Want paperless entry" | Auto-generated QR codes upon payment confirmation | **DIP-8** / **DIP-10** |
| "Inability to cancel tickets easily" | Automated cancellation endpoint and state transition (`BOOKED` $\rightarrow$ `CANCELLED`) | **DIP-5** / **DIP-6** |

---

## 5. Conclusion & Next Steps

The user research successfully validates the core product hypothesis for OmniCinema. The empirical findings directly inform our database schema (DIP-4), transactional reservation logic (DIP-5), and UI/UX mockups (DIP-22).

**Immediate Next Steps:**
1. Maintain this report and raw CSV in `docs/user_research/` for traceability.
2. Link this document to YouTrack task **DIP-15**.
3. Conduct pilot SUS (System Usability Scale) testing on the working prototype during **DIP-16** post-defense.