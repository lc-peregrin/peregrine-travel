# SEO_TARGET_MAP.md — per-page on-page targets for Peregrin

For Claude Code's `seo` teammate. Grounded in peregrin/research/MARKET_RESEARCH.md. DROP THIS IN THE REPO ROOT so Claude Code can read it (it cannot read the Claude Project). Rules for every page:

* Title tag < 60 chars. Meta description < 155 chars. Exactly ONE H1.
* NO EM DASHES anywhere. Never the word "fake" (except warning against faked documents). Core noun "reservation"; "ticket" only in "ticket reservation" / "e-ticket".
* Every guide gets JSON-LD: Article + BreadcrumbList, plus FAQPage if it has an FAQ section.
* Country/topic guides not listed here (the overnight-generated ones) follow the COUNTRY TEMPLATE at the bottom; the `writer` teammate should extend this map for each new guide.

## Core pages

### / (homepage)

* Primary keyword: proof of onward travel · secondary: flight reservation for visa
* Intent: transactional
* Title: `Verifiable Flight Reservations for Visa & Onward Travel`
* Meta: `Get a genuine, verifiable flight reservation in minutes: a real airline booking with a PNR you can verify for visa, immigration, and onward-travel proof.`
* H1: `Get a verifiable flight reservation in minutes`
* Internal links: /blog · /blog/dummy-ticket-visa-application · /blog/flight-reservation-schengen-visa
* Schema: Organization + WebSite

### /blog (guide index)

* Primary keyword: proof of onward travel guides
* Intent: informational hub
* Title: `Proof of Onward Travel Guides by Country | Peregrin`
* Meta: `Practical, up-to-date guides on proof of onward travel, visas, and entry rules by country. Know exactly what to show at check-in and immigration.`
* H1: `Travel and visa guides`
* Internal links: the 3 pillar guides (dummy-ticket, flight-itinerary, digital-nomad)
* Schema: CollectionPage (or Blog) + BreadcrumbList

### /sample-reservation

* Primary keyword: sample flight reservation for visa
* Intent: evaluation
* Title: `See a Sample Flight Reservation (Real PNR) | Peregrin`
* Meta: `See exactly what a Peregrin reservation looks like: a real airline booking reference you can verify, formatted as proof for a visa and airline check-in.`
* H1: `A sample reservation`
* Internal links: / · /blog/dummy-ticket-visa-application
* Schema: WebPage

### /privacy

* Intent: utility (can be indexed; low priority)
* Title: `Privacy Policy | Peregrin`
* Meta: `How Peregrin collects, uses, and protects your personal information when you use our reservation service.`
* H1: `Privacy Policy`
* Schema: WebPage

## Pillar guides (highest commercial value)

### /blog/dummy-ticket-visa-application

* Primary keyword: dummy ticket for visa
* Intent: commercial
* Title: `Dummy Ticket for a Visa: The Legit Way (2026)`
* Meta: `"Dummy ticket" advice is often risky. Here's what embassies actually want and how to show a real, verifiable onward flight without buying one you may not use.`
* H1: `Dummy Ticket for a Visa Application: What It Really Means in 2026`
* Internal links: /blog/flight-itinerary-for-visa-application · /blog/flight-reservation-schengen-visa · /blog/proof-of-onward-travel-thailand
* Schema: Article + FAQPage + BreadcrumbList

### /blog/flight-itinerary-for-visa-application

* Primary keyword: flight itinerary for visa application
* Intent: commercial
* Title: `Flight Itinerary for a Visa Application (2026)`
* Meta: `Visa forms often ask for a flight itinerary. Here's what it means, why not to buy the ticket first, and how to show a verifiable reservation instead.`
* H1: `Flight Itinerary for a Visa Application: The Safe Way in 2026`
* Internal links: /blog/dummy-ticket-visa-application · /blog/flight-reservation-schengen-visa · /blog/proof-of-onward-travel-thailand
* Schema: Article + FAQPage + BreadcrumbList

### /blog/digital-nomad-visa-onward-travel

* Primary keyword: digital nomad visa proof of onward travel
* Intent: informational + commercial
* Title: `Digital Nomad Visas & Proof of Onward Travel (2026)`
* Meta: `Dozens of countries now offer nomad visas. Here's where proof of onward travel and entry checks fit, and how to cover them without buying a ticket.`
* H1: `Digital Nomad Visas and Proof of Onward Travel in 2026`
* Internal links: /blog/proof-of-onward-travel-thailand · /blog/proof-of-onward-travel-bali-indonesia · /blog/dummy-ticket-visa-application
* Schema: Article + FAQPage + BreadcrumbList

### /blog/flight-reservation-schengen-visa

* Primary keyword: flight reservation for Schengen visa
* Intent: commercial
* Title: `Flight Reservation for a Schengen Visa (2026)`
* Meta: `Schengen embassies advise against buying a ticket before approval. Here's how to show a verifiable flight reservation for your Schengen visa instead.`
* H1: `Flight Reservation for a Schengen Visa: Why You Should Not Buy the Ticket Yet`
* Internal links: /blog/dummy-ticket-visa-application · /blog/flight-itinerary-for-visa-application · /blog/proof-of-onward-travel-thailand
* Schema: Article + FAQPage + BreadcrumbList

## Country / destination guides

### /blog/proof-of-onward-travel-thailand

* Keyword: proof of onward travel Thailand · Intent: informational-commercial
* Title: `Proof of Onward Travel for Thailand (2026)`
* Meta: `Thailand asks visa-exempt visitors for proof of onward travel. Here's what's required, how it's checked, and how to show it without buying a ticket.`
* H1: `Proof of Onward Travel for Thailand: What You Actually Need in 2026`
* Internal links: /blog/dummy-ticket-visa-application · /blog/proof-of-onward-travel-vietnam · /blog/onward-ticket-philippines
* Schema: Article + FAQPage + BreadcrumbList

### /blog/proof-of-onward-travel-vietnam

* Keyword: proof of onward travel Vietnam · Intent: informational-commercial
* Title: `Proof of Onward Travel for Vietnam (2026)`
* Meta: `Vietnam's e-visa is generous, but airlines still check for onward travel at the gate. Here's what you need and how to show it without buying a ticket.`
* H1: `Proof of Onward Travel for Vietnam: What You Actually Need in 2026`
* Internal links: /blog/proof-of-onward-travel-thailand · /blog/dummy-ticket-visa-application · /blog/proof-of-onward-travel-bali-indonesia
* Schema: Article + FAQPage + BreadcrumbList

### /blog/proof-of-onward-travel-bali-indonesia

* Keyword: proof of onward travel Bali · Intent: informational-commercial
* Title: `Proof of Onward Travel for Bali & Indonesia (2026)`
* Meta: `Flying to Bali? Indonesia and the airlines want proof of onward travel. Here's exactly what to show at check-in and immigration, without overspending.`
* H1: `Proof of Onward Travel for Bali and Indonesia: What You Actually Need in 2026`
* Internal links: /blog/proof-of-onward-travel-thailand · /blog/proof-of-onward-travel-vietnam · /blog/dummy-ticket-visa-application
* Schema: Article + FAQPage + BreadcrumbList

### /blog/onward-ticket-philippines

* Keyword: onward ticket Philippines · Intent: informational-commercial
* Title: `Onward Ticket for the Philippines (2026)`
* Meta: `The Philippines rule airlines actually enforce: no onward ticket, no boarding. Here's what counts as proof and how to get it without buying a flight.`
* H1: `Onward Ticket for the Philippines: The Rule Airlines Actually Enforce in 2026`
* Internal links: /blog/proof-of-onward-travel-thailand · /blog/proof-of-onward-travel-vietnam · /blog/dummy-ticket-visa-application
* Schema: Article + FAQPage + BreadcrumbList

### /blog/proof-of-onward-travel-colombia

* Keyword: proof of onward travel Colombia · Intent: informational-commercial
* Title: `Proof of Onward Travel for Colombia (2026)`
* Meta: `Colombia is one of the places airlines really do deny boarding without onward proof. Here's what to show and how to do it without buying a ticket.`
* H1: `Proof of Onward Travel for Colombia: What You Actually Need in 2026`
* Internal links: /blog/proof-of-onward-travel-peru · /blog/proof-of-onward-travel-mexico · /blog/dummy-ticket-visa-application
* Schema: Article + FAQPage + BreadcrumbList

### /blog/proof-of-onward-travel-mexico

* Keyword: proof of onward travel Mexico · Intent: informational-commercial
* Title: `Proof of Onward Travel for Mexico (2026)`
* Meta: `Mexico and the airlines can ask for onward travel on arrival. Here's what's required, who gets asked, and how to show it without buying a ticket.`
* H1: `Proof of Onward Travel for Mexico: What You Actually Need in 2026`
* Internal links: /blog/proof-of-onward-travel-costa-rica · /blog/proof-of-onward-travel-colombia · /blog/dummy-ticket-visa-application
* Schema: Article + FAQPage + BreadcrumbList

### /blog/proof-of-onward-travel-costa-rica

* Keyword: proof of onward travel Costa Rica · Intent: informational-commercial
* Title: `Proof of Onward Travel for Costa Rica (2026)`
* Meta: `Costa Rica is famous for onward-travel checks at check-in. Here's what officers and airlines want and how to show it without buying a ticket you won't use.`
* H1: `Proof of Onward Travel for Costa Rica: What You Actually Need in 2026`
* Internal links: /blog/proof-of-onward-travel-mexico · /blog/proof-of-onward-travel-peru · /blog/dummy-ticket-visa-application
* Schema: Article + FAQPage + BreadcrumbList

### /blog/proof-of-onward-travel-japan

* Keyword: proof of onward travel Japan · Intent: informational-commercial
* Title: `Proof of Onward Travel for Japan (2026)`
* Meta: `Most visitors enter Japan visa-free, but airlines and immigration can still ask for a return or onward booking. Here's how to be ready without overspending.`
* H1: `Proof of Onward Travel for Japan: What You Actually Need in 2026`
* Internal links: /blog/proof-of-onward-travel-thailand · /blog/proof-of-onward-travel-vietnam · /blog/dummy-ticket-visa-application
* Schema: Article + FAQPage + BreadcrumbList

### /blog/proof-of-onward-travel-peru

* Keyword: proof of onward travel Peru · Intent: informational-commercial
* Title: `Proof of Onward Travel for Peru (2026)`
* Meta: `Peru and the airlines can ask for proof of onward travel. Here's what's required, where you'll be asked, and how to show it without buying a ticket.`
* H1: `Proof of Onward Travel for Peru: What You Actually Need in 2026`
* Internal links: /blog/proof-of-onward-travel-colombia · /blog/proof-of-onward-travel-costa-rica · /blog/dummy-ticket-visa-application
* Schema: Article + FAQPage + BreadcrumbList

### /blog/onward-ticket-turkey

* Keyword: onward ticket Turkey · Intent: informational-commercial
* Title: `Onward Ticket for Turkey: What Airlines Check (2026)`
* Meta: `Turkey's e-visa is easy, but airlines can still ask for an onward ticket at check-in. Here's what counts and how to show it without buying a flight.`
* H1: `Onward Ticket for Turkey: What Airlines Actually Check in 2026`
* Internal links: /blog/flight-reservation-schengen-visa · /blog/dummy-ticket-visa-application · /blog/proof-of-onward-travel-thailand
* Schema: Article + FAQPage + BreadcrumbList

## COUNTRY TEMPLATE (for overnight-generated guides — writer teammate, extend this map)

* Slug: `proof-of-onward-travel-<country>` (or `onward-ticket-<country>` where that phrasing has more search)
* Keyword: `proof of onward travel <Country>` (or `onward ticket <Country>`)
* Title: `Proof of Onward Travel for <Country> (2026)` (keep < 60 chars)
* Meta (make it UNIQUE per country, < 155 chars, mention the country + a specific local angle so metas don't duplicate): `<Country> can ask for proof of onward travel at check-in and immigration. Here's what's required and how to show it without buying a ticket.`
* H1: `Proof of Onward Travel for <Country>: What You Actually Need in 2026`
* Internal links: the dummy-ticket pillar + 2 nearby/relevant country guides
* Schema: Article + FAQPage + BreadcrumbList
