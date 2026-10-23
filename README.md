# Cinema Booking Platform

Diploma project: **Development of a web platform for online booking in cinemas**
(Astana IT University, School of Software Engineering).

## Description
A web platform where moviegoers browse sessions, hold and book seats without double booking,
and cinema staff manage movies, halls, sessions and view statistics.

## Problem, users, expected outcomes
- **Problem:** small cinema operators need a self-controlled booking system that never sells one seat twice and releases unpaid holds.
- **Users:** moviegoers, cinema administrators, cinema managers.
- **Outcomes:** requirements and architecture documentation, a Go/PostgreSQL backend with REST API, web client, tests and load-test results compared with the baseline prototype.

## Team and supervisor
| Student | Role | Account |
|---|---|---|
| Nurbek Shagarbek | Lead Backend Developer | [Shagarbek](https://github.com/Shagarbek) |
| Ismail Zhanatay | Back-end developer | [ikki1769](https://github.com/ikki1769) |
| Erasyl Meirambekov | Front-end developer | [Exzou0](https://github.com/Exzou0) |

Supervisor: Chingis Kharmysov, Assistant Professor.

## Scope and current status
Scope: `docs/Report_dip.pdf`, Section 1.6. Status: analysis and design complete for the approbation period; implementation in progress. Baseline: earlier prototype `Exzou0/Advanced_Final`.

## Selected technologies
Back-end: Go or Node.js, PostgreSQL, REST/JSON (OpenAPI), Docker; front-end: Next.js, TypeScript. Justification: report Section 5.

## Links
- Repository: [\[Click\]](https://github.com/Exzou0/OmniCinema)
- YouTrack: [\[Click\]](https://omnicinema2.youtrack.cloud/projects/DIP)
- Documentation: `docs/`
- Diagrams (editable `.dot` + PNG): `diagrams/`
- UX mockups: `mockups/`

## Repository structure
`src/` source code, `docs/` proposal, literature review, comparisons, requirements, ADRs, `diagrams/`, `mockups/`, `tests/`, `config/` examples.

## Setup, run and test
To be added when runnable code is available. Configuration: copy `config/.env.example` to `.env`.

## Workflow
Branch per YouTrack task (`DIP-5-booking-module`), pull request with task ID and description of checks, review by another team member, tags `pre-defense-1`, `pre-defense-2`, `final-pre-defense`.
