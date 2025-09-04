# Requirement Gathering Case Study: Scheduled Order Delivery

## Business Problem

The business wants to introduce a **Scheduled Order Delivery** feature where customers can pre-schedule their orders to be delivered at a specific date and time. The goal is to improve customer convenience, better manage delivery partner capacity, and increase user satisfaction.

---

## General Clarification Questions

* Who are the primary users of this feature?
* Will this option be available for all products or only selected categories?
* How should this option appear in the user interface?
* Should scheduling be optional or mandatory for certain product types?
* Will this feature be rolled out across all regions or specific markets first?

---

## Functional Requirement Clarifications

* Should users be able to edit or cancel their scheduled delivery after placing the order?
* What is the maximum number of days in advance a delivery can be scheduled?
* Will the delivery slot availability depend on delivery partner capacity?
* Should users be able to choose both date and time, or only time windows (e.g., 10 AM–1 PM)?
* What happens if the product is out of stock on the scheduled date?
* Should the payment be charged immediately or closer to the delivery date?

---

## Non-Functional Requirement Clarifications

* Should the scheduling system support peak load handling (e.g., festival seasons)?
* What is the acceptable system response time when fetching available slots?
* Do we need notifications/reminders for scheduled deliveries?
* Should there be fallback mechanisms if delivery partners fail to fulfill the order?
* Is there a requirement for high availability (e.g., 99.9% uptime) for this scheduling service?

---

## Edge Case Clarifications

* What happens if a delivery slot becomes unavailable after the user schedules it?
* How should the system handle overlapping delivery requests from multiple users at the same address?
* What if the user changes their delivery address after scheduling?
* How should the system behave if there are severe delays due to unforeseen events (e.g., weather, strikes)?
* Should the system auto-reschedule or cancel if delivery partners cannot meet the requested slot?

---

## Outcome of Requirement Gathering

By asking structured clarification questions across general understanding, functional needs, non-functional requirements, and edge cases, the ambiguities around the Scheduled Order Delivery feature were minimized. This analysis ensures that the solution is **user-friendly, technically feasible, and scalable**. The requirement gathering process lays a strong foundation for design, development, and smoother stakeholder alignment moving forward.
