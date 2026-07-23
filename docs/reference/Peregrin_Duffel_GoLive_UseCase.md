# Peregrin — Duffel Go-Live Use Case Description

**What Peregrin does:** Peregrin provides travellers with genuine flight reservations to satisfy visa and immigration "proof of onward/return travel" requirements. Customers purchase a reservation; we create a real order via Duffel's Hold Order feature, which holds price and space for the fare's standard guarantee window. If the customer doesn't need to actually fly (the common case — the reservation exists solely to satisfy a documentation check), the hold lapses naturally per the fare's own terms. Customers who do want to fly can pay to confirm before the hold expires.

**Volume:** Starting at low volume (dozens of bookings/month), scaling with demand.

**Routes/carriers:** Primarily Southeast Asian routes to start (Thailand, Philippines, Indonesia, Malaysia, Singapore), expanding based on customer demand.

**Compliance notes:** This is a disclosed, above-board use of Duffel's documented Hold Order & Pay Later feature — customers are informed the reservation may lapse, and no ticket is issued unless paid for.
