# Requirements specification

Priorities use the MoSCoW scale (Must, Should, Could, Won’t). Priorities are provisional until the interview results of Section 2 are entered. Each requirement has an acceptance criterion that can be checked by an automated test, a measurement or an inspection.

## Functional requirements

Table 13 – Functional requirements

| ID | Requirement | Priority | Acceptance criterion |
| --- | --- | --- | --- |
| FR-01 | A guest can register with name, e-mail and password. | Must | Valid data → 201 and a user record with a hashed password; duplicate e-mail → 409; invalid e-mail → 400. |
| FR-02 | A user can log in and log out. | Must | Correct credentials → token; wrong credentials → 401; token expires after the configured time. |
| FR-03 | Access is restricted by role (user, administrator, manager). | Must | Anonymous → 401; user on an administrator endpoint → 403; verified by automated tests for every protected route. |
| FR-04 | A visitor can browse the movie catalogue. | Must | The list shows title, genre, duration and rating from the database. |
| FR-05 | A visitor can view sessions of a movie by date. | Must | Only future sessions are listed, sorted by start time. |
| FR-06 | A user can view the seat map of a session with seat statuses. | Must | Each seat is free, held or booked; statuses match the database at the time of the request. |
| FR-07 | A user can hold up to six seats for a limited time. | Must | Held seats appear as unavailable to others; a hold expires after the configured time (default 10 minutes) and the seat becomes free again. |
| FR-08 | A user can confirm a booking of held seats. | Must | With 100 concurrent requests for the same seat exactly one succeeds and the rest receive 409. |
| FR-09 | A user can pay for a booking through the payment provider (sandbox). | Should | Successful payment → status BOOKED; failed or timed-out payment → hold released. |
| FR-10 | A user can view their tickets and history. | Must | Only the owner (and administrators) can open a ticket; others receive 403. |
| FR-11 | A user can cancel a ticket before the session starts. | Should | Cancelled ticket frees the seat immediately; a cancellation after the cut-off time is rejected. |
| FR-12 | A ticket has a QR code or unique code for entry. | Should | The code is unique and resolves to exactly one ticket. |
| FR-13 | An administrator can create, edit and delete movies. | Must | Changes are visible in the catalogue; deleting a movie with future tickets is rejected. |
| FR-14 | An administrator can manage halls and seat layouts. | Must | A hall has a defined number of rows and seats; layout changes do not affect past sessions. |
| FR-15 | An administrator can create, edit and delete sessions. | Must | A session has movie, hall, start time and price; sessions with tickets cannot be deleted without cancelling them. |
| FR-16 | The system rejects overlapping sessions in the same hall. | Should | Creating an overlapping session returns 409. |
| FR-17 | A manager can view sales and occupancy statistics. | Should | Tickets sold, revenue and occupancy per movie and per period agree with a manual count on test data. |
| FR-18 | The system sends a booking confirmation by e-mail. | Could | A confirmation is sent within one minute of payment in the test environment. |
| FR-19 | A visitor can filter movies by genre and date. | Could | Filters return the expected subset on test data. |
| FR-20 | Ratings and reviews of movies. | Won’t (this release) | Deferred; the earlier prototype includes a rating field, top-rated retrieval and statistics, but a complete user-review workflow is not established. |

## Non-functional requirements

Table 14 – Non-functional requirements

| ID | Category | Requirement | Acceptance criterion |
| --- | --- | --- | --- |
| NFR-01 | Correctness | No seat is sold twice. | 0 duplicate active tickets after a test with 10,000 concurrent booking attempts on a limited set of seats. |
| NFR-02 | Performance | Seat map and booking operations respond quickly under load. | p95 latency below 300 ms for the seat map and below 500 ms for booking at 200 concurrent virtual users on the reference machine (proposed targets, to be calibrated on the baseline). |
| NFR-03 | Security | Credentials and secrets are protected. | Passwords stored with bcrypt or Argon2; secrets read from environment variables; repository scan finds no secrets; OWASP ASVS Level 1 checklist completed [13]. |
| NFR-04 | Reliability | State survives a restart. | Holds and tickets are stored in the database only; after a restart, all holds are still valid until expiry. |
| NFR-05 | Usability | Booking is quick for a first-time user. | Booking completed in at most five steps; mean System Usability Scale score of at least 68 with five test users. |
| NFR-06 | Maintainability | Code is layered and tested. | Handlers, services and repositories are separated; at least 70% statement coverage of the business-logic layer; CI runs tests on every pull request. |
| NFR-07 | Portability | The system can be started on any machine with Docker. | A single command starts the application and the database; documented in the README. |
| NFR-08 | Observability | Operators can see what the system is doing. | Structured logs with request ID; health endpoint returns database status. |
| NFR-09 | Compatibility | Works in current browsers and on phones. | Layout usable at 360 px width in the latest Chrome, Firefox and Safari. |
| NFR-10 | Documentation | The API is documented. | An OpenAPI description covers all public endpoints. |

Numeric targets in NFR-02 and NFR-06 are proposals and will be calibrated on the baseline measurements (V-4).
