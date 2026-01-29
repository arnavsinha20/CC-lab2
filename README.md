
# CC Fest Monolith (FastAPI)

Monolithic FastAPI app for a fest portal with SQLite persistence and Tailwind-rendered Jinja templates. Includes Locust scripts to demo load/perf behavior and an intentional crash path to illustrate blast radius.

## Features
- User auth (register/login) with SQLite
- Event listing and registration
- My Events + checkout total calculation
- Intentional crash path to show monolith-wide failure
- Locust load tests for /events, /my-events, /checkout

## Setup
1) Install deps:
```bash
pip install -r requirements.txt
```
2) Seed events (creates fest.db if missing):
```bash
python insert_events.py
```

## Run the app
```bash
uvicorn main:app --reload --port 8000
```
Then open http://localhost:8000/login

## Key routes
- GET /login, POST /login
- GET/POST /register
- GET /events?user=<username>
- GET /register_event/{event_id}?user=<username> (intentional crash at 404)
- GET /my-events?user=<username>
- GET /checkout (sums all event fees)
- GET /docs for OpenAPI UI

## Load testing with Locust
```bash
locust -f locust/events_locustfile.py
# or
locust -f locust/myevents_locustfile.py
locust -f locust/checkout_locustfile.py
```
Access the Locust UI at http://localhost:8089 and set host to http://localhost:8000.

## Project structure (top-level)
- main.py            FastAPI app + routes/templates
- database.py        SQLite connection helper
- insert_events.py   Seeds events table
- checkout/          Checkout logic
- locust/            Load-test scripts
- templates/         Jinja + Tailwind UI
- requirements.txt   Dependencies

## Notes
- The crash link in Events uses event_id=404 to demonstrate monolith failure → rendered via templates/error.html
- Database file fest.db is generated locally; regenerate via insert_events.py when needed
