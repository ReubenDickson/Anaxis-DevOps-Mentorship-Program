WEEK 12 — Scaling & Availability Challenges
Why “Just Run Another Container” Quickly Becomes a Problem

Week 12 Purpose (Read This First)

In the real world, applications do not fail only because of bugs.
They fail because:

- traffic increases

- users grow

- load becomes unpredictable

- one instance cannot handle demand

This week answers the question: How do systems behave when demand grows — and why manual scaling collapses?

You will attempt to scale applications without orchestration, so you can clearly see:

i. what works

ii. what breaks

iii. what becomes unmanageable

This discomfort is intentional.


1. What Is Scaling? (Correct Definition)

Scaling means adjusting system capacity to meet demand.

There are two types:

a. Vertical Scaling

- you add more CPU/RAM to one server

- this method has physical limits

- it requires downtime

b. Horizontal Scaling

- you add more instances

- this method distributes load

- It is preferred in modern systems

DevOps engineers almost always scale horizontally.


DevOps Thinking

Scale is not about power — it’s about distribution.


2. Single Instance Is a Single Point of Failure

When you run one container:

- if it crashes, service goes down

- if it is overloaded, users suffer

- if it restarts, downtime occurs

This is not hypothetical.
This is reality.


3. First Attempt at Scaling (Naïve Approach)

Let’s try to “scale” manually.

Run multiple containers:
``` bash
docker run -d -p 8081:80 my-app
docker run -d -p 8082:80 my-app
docker run -d -p 8083:80 my-app
```

You now have three instances, three ports and three access points.

But users expect:

One service, not three URLs.

This is the first major problem.


4. The Load Distribution Problem

Questions immediately arise:

i. How do users choose which instance?

ii. What happens if one instance dies?

iii. How is traffic balanced?

iv. How do we add or remove instances safely?

Manual answers means bookmarks, scripts and guessing.

These do not scale.


5. Manual Load Balancing (Conceptual)

In production, traffic must be:

i. evenly distributed

ii. automatically rerouted on failure

iii. invisible to users

But without orchestration:

- load balancing is fragile

- configuration is manual

- failure handling is slow

This is where systems start to feel brittle.


6. Availability vs Scaling (Important Distinction)

Scaling handles load while Availability handles failure.

You can scale and still be unavailable if:

i. instances are not monitored

ii. traffic is not redirected

iii. failures are not detected

DevOps engineers design for both.


7. The Operational Burden of Manual Scaling

As instances grow:

- ports must be tracked

- configs must be consistent

- logs multiply

- monitoring becomes harder

- human error increases

This is the cost of growth.

DevOps Thinking

Scaling without control increases risk faster than capacity.