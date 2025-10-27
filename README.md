# Vehicle Soft-Hold Service

This exercise helps us understand how you design, reason about trade-offs, and write maintainable code. A perfect/complete product is less important than clear decisions, sound architecture, and readable code. Keep it small and focused.

## Up to Senior-level developer Assignment

We sell trucks, vans, machinery, and parts. Multiple buyers often click Reserve for the same vehicle. We need a soft hold that grants the first buyer a 15-minute reservation, shows a countdown, and auto-expires, and emits events (hold.created, hold.released, hold.expired) so other services can react. Showcase a solution with a http response of 409 when trying to put a second hold on a vehicle that already has a hold.

## Requirements
•	Language/Framework: PHP 8.4+, Laravel 11+ (Or any other of your choice).
•	Tooling: Docker
•	Auth: header X-Api-Key (string, required on all write endpoints).
•	Repo UX: Your app should run with at most 3 commands (clone, deps, up). If extra steps are needed, list them clearly in the README.

## What to build
A tiny microservice that can:
1.	Create a hold
2.	Inspect a hold
3.	Release a hold
4.	Auto-expire overdue holds via a schedule
5.	Trigger events via simple dispatcher. Event doesn’t have to do anything, just simulate work by writing to a log/file.

## Data model (minimum)
•	vehicles — id (uuid|int), vin (unique)
•	holds — id (uuid), vehicle_id, buyer_ref, status (ENUM: active|expired|released), expires_at (timestamptz)

## API contracts
### 1) Create hold
`GET /endpoint-url`

Request:
```
{ "vehicle-id": "truck-123", "buyer-ref": "lead-987" }
```

201 - Created
```
{
  “data”: {
     // Details of the response
  }
}
```

409 - Conflict (active hold exists)
```
{
  "error": {
     “message”: “ACTIVE_HOLD_EXISTS”,
     “code”: 409,
     "details ": {
        // Details, when expires, time in seconds
     }
  }
}
```

### 2) Get hold
`GET /endpoint-url`

200 - details of the hold.

### 3) Release hold
`/endpoint-url`

## Background processing
Expiry job: mark overdue active holds as expired and enqueue hold.expired events.

## Evaluation
We value clarity and sound trade-offs over feature completeness; your decisions matter more than a finished product.
