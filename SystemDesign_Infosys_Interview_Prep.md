# System Design (Basics) — Infosys Interview Preparation

> Note: Infosys fresher/DSE-SP interviews rarely ask deep, large-scale system design (like "design Twitter" from a FAANG interview). They focus on **fundamental concepts, trade-offs, and basic architecture reasoning** — especially if you have a project on your resume. This guide is scoped accordingly.

## 🗺️ Question Roadmap

**Basics**
1. 🔴 What is System Design? Why does it matter?
2. 🔴 Monolithic vs Microservices Architecture
3. 🔴 Vertical Scaling vs Horizontal Scaling
4. 🟠 What is Load Balancing?
    
**Core Concepts**
5. 🟠 Caching — what it is, where it's used
6. 🟠 SQL vs NoSQL — when to choose which
7. 🟡 What is an API Gateway?
8. 🟡 CAP Theorem (basic understanding)

**Practical / Resume-based**
9. 🟠 How would you design a URL Shortener? (classic beginner design question)
10. 🟡 Client-Server Architecture basics

---

## A. 🔴 MUST KNOW

### Q1. What is System Design? Why does it matter?

**Answer:**
> "System design is the process of defining the architecture, components, and data flow of a software system to meet functional and non-functional requirements like scalability, reliability, and performance."

**Interview Tip:** Mention there are two levels: **High-Level Design (HLD)** — overall architecture, components, and how they interact; and **Low-Level Design (LLD)** — class diagrams, database schema, individual module logic. At fresher level, expect mostly conceptual/HLD-style questions.
**Follow-up:** What's the difference between HLD and LLD?

---

### Q2. Monolithic vs Microservices Architecture

| Aspect | Monolithic | Microservices |
|---|---|---|
| Structure | Single codebase, single deployable unit | Multiple independent, small services |
| Scaling | Scale the entire app together | Scale individual services independently |
| Development | Simpler to start, harder to maintain as it grows | More complex setup, easier to maintain long-term |
| Deployment | One deployment for the whole app | Each service deployed independently |
| Failure impact | One bug can bring down the entire app | A failure in one service doesn't necessarily crash others |
| Communication | Function calls within the same process | Network calls (REST/gRPC) between services |

**Example (real-world):** An e-commerce app as a monolith has Order, Payment, and Inventory all in one codebase. As microservices, each becomes its own independently deployable service with its own database, communicating via APIs.

**Interview Tip:** Say: "Monolithic is simpler and faster to build initially, which is why startups often start there. Microservices shine at scale, when different teams need to work independently and different parts of the system have very different scaling needs."
**Common mistake:** Saying microservices are "always better" — mention the added complexity (network latency, distributed debugging, data consistency across services) as a genuine trade-off.
**Follow-up:** How do microservices communicate with each other? (REST APIs, message queues like Kafka/RabbitMQ, gRPC.)

---

### Q3. Vertical Scaling vs Horizontal Scaling

| Aspect | Vertical Scaling (Scale Up) | Horizontal Scaling (Scale Out) |
|---|---|---|
| Method | Add more power (CPU/RAM) to an existing server | Add more servers/machines |
| Limit | Has a hardware ceiling | Practically unlimited (add more machines) |
| Downtime | Often requires downtime to upgrade | Can be done with zero downtime |
| Complexity | Simple | Needs load balancing, data synchronization |
| Cost pattern | Gets expensive quickly at high-end hardware | More cost-effective at large scale |

**Interview Tip:** Say: "Vertical scaling is like upgrading a single worker to be stronger; horizontal scaling is like hiring more workers." Simple analogy interviewers appreciate.
**Follow-up:** Which does a distributed database use? (Horizontal scaling — e.g., adding more nodes/shards.)

---

## B. 🟠 VERY IMPORTANT

### Q4. Load Balancing

> "A load balancer distributes incoming network traffic across multiple servers, so no single server gets overwhelmed. It improves availability, reliability, and performance."

**Common algorithms (mention briefly):**
- **Round Robin** — requests distributed sequentially across servers
- **Least Connections** — sends traffic to the server with the fewest active connections
- **IP Hash** — routes based on client IP (useful for session persistence)

**Interview Tip:** Mention it also enables **high availability** — if one server goes down, the load balancer redirects traffic to healthy ones.
**Follow-up:** Where would you place a load balancer in a typical web architecture? (Between the client and the application servers.)

---

### Q5. Caching

> "Caching stores frequently accessed data in a fast-access layer (like memory) so future requests for the same data can be served quickly, without hitting the slower database every time."

**Where it's used:**
- **Browser caching** — static assets (images, CSS, JS)
- **CDN** — caches content geographically closer to users
- **Application-level caching** — e.g., Redis/Memcached storing frequently queried DB results

**Example (conceptual):**
```python
cache = {}

def get_user(user_id):
    if user_id in cache:
        return cache[user_id]        # cache hit - fast
    user = query_database(user_id)   # cache miss - slow
    cache[user_id] = user
    return user
```

**Interview Tip:** Mention **cache invalidation** as a known hard problem — "one of the two hard problems in computer science" is a fun, memorable line if it fits naturally. Also mention **TTL (Time To Live)** as a common invalidation strategy.
**Follow-up:** What happens if cached data becomes stale? (You need an invalidation/expiry strategy — TTL, write-through, or explicit invalidation on update.)

---

### Q6. SQL vs NoSQL — When to Choose Which

| Aspect | SQL (Relational) | NoSQL (Non-relational) |
|---|---|---|
| Schema | Fixed, predefined schema | Flexible/dynamic schema |
| Data structure | Tables with rows/columns | Document, key-value, graph, or column-family |
| Scaling | Typically vertical (though modern SQL DBs support horizontal too) | Designed for horizontal scaling |
| Consistency | Strong consistency (ACID) | Often eventual consistency (varies by DB) |
| Use case | Structured data with relationships — banking, e-commerce orders | Large-scale, unstructured/semi-structured data — social media feeds, logs, catalogs |
| Examples | MySQL, PostgreSQL | MongoDB, Cassandra, Redis |

**Interview Tip:** Say: "I'd choose SQL when data is structured and relationships between entities matter, and strong consistency is important — like financial transactions. I'd choose NoSQL when I need flexible schema, high write throughput, and horizontal scalability — like storing user activity logs."
**Follow-up:** Have you used a NoSQL database in any project? — **verify this from your actual project before saying it in the interview.**

---

## C. 🟡 GOOD TO KNOW

### Q7. API Gateway

> "An API Gateway is a single entry point that sits in front of multiple backend services (especially in a microservices architecture), handling things like routing, authentication, rate limiting, and logging, so individual services don't have to implement these separately."

**Interview Tip:** Mention it simplifies the client side too — the client talks to one gateway instead of knowing about every individual microservice.

---

### Q8. CAP Theorem (Basic Understanding)

> "CAP theorem states that a distributed system can only guarantee two out of three properties at the same time: Consistency, Availability, and Partition Tolerance."

| Property | Meaning |
|---|---|
| Consistency | Every read gets the most recent write |
| Availability | Every request gets a response (success or failure), no downtime |
| Partition Tolerance | System keeps working even if network communication between nodes fails |

**Interview Tip:** Since network partitions are unavoidable in real distributed systems, the real-world trade-off is usually between **Consistency and Availability** (CP vs AP systems). This is a deeper topic — a solid, confident one-paragraph answer is enough at fresher level; don't over-elaborate unless pushed.

---

### Q9. Designing a URL Shortener (Classic Beginner Design Question)

This is a common, approachable system design question even for freshers. Structure your answer:

**1. Requirements:**
- Given a long URL, generate a short unique URL
- Redirect short URL to original long URL
- (Optional) Track click analytics

**2. Basic approach:**
- Store a mapping: `short_code → long_url` in a database
- Generate `short_code` using a technique like base62 encoding of an auto-incrementing ID, or a hash of the URL

**3. High-level flow:**
```
User submits long URL
   → Server generates short_code
   → Store {short_code: long_url} in DB
   → Return short URL (e.g., short.ly/abc123)

User visits short URL
   → Server looks up short_code in DB
   → Redirects (HTTP 301/302) to original long_url
```

**4. Things to mention if asked to go deeper:**
- Use a **cache** (like Redis) in front of the database for fast lookups on popular short URLs
- Handle **collision** — what if two long URLs generate the same short code?
- **Database choice** — a simple key-value store (NoSQL) works well since access pattern is mostly by short_code lookup

**Interview Tip:** You don't need to know a "perfect" answer — interviewers mainly want to see structured thinking: requirements → approach → data flow → possible optimizations. Speak through it step by step rather than jumping straight to a solution.
**Follow-up:** How would you handle a very high volume of requests? (Caching + horizontal scaling of the redirect service + load balancer.)

---

### Q10. Client-Server Architecture Basics

> "In client-server architecture, the client (e.g., browser, mobile app) sends requests, and the server processes them and sends back responses. The client typically handles the UI/presentation, while the server handles business logic and data storage."

**Interview Tip:** Mention this is the foundation almost every web app (including Django/React projects) is built on — connect it back to your resume project if relevant. **Verify this from your actual project before saying it in the interview.**

---

## ⭐ Top Questions to Memorize

1. Monolithic vs Microservices — trade-offs, not just definitions
2. Vertical vs Horizontal Scaling with the "worker" analogy
3. What is Load Balancing and why it's needed
4. What is Caching and where it's used (browser, CDN, app-level)
5. SQL vs NoSQL — when to choose which, with a use-case example each
6. What is an API Gateway
7. CAP Theorem — one confident paragraph
8. URL Shortener design — full structured walkthrough
9. Client-Server architecture basics
10. HLD vs LLD

## 🎯 Interview Preparation Checklist

- [ ] Can explain monolithic vs microservices with genuine trade-offs, not just a one-sided answer
- [ ] Can explain vertical vs horizontal scaling with the analogy
- [ ] Can walk through the URL shortener design end-to-end without hesitation
- [ ] Can explain SQL vs NoSQL with one clear use case for each
- [ ] Can explain caching and mention at least one real invalidation strategy (TTL)
- [ ] Comfortable connecting system design concepts (load balancer, caching, API) to my own resume project — **verify this from your actual project before saying it in the interview**
