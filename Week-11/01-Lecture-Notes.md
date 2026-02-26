WEEK 11 — Reliability, Downtime & Deployment Strategies
Why Systems Fail and How DevOps Engineers Reduce User Impact

Week 11 Purpose (Read This First)

By now, you understand that:

- deployments are change events

- production environments are fragile

- configuration matters

This week answers the next critical question:

What happens to users when things go wrong — and how do DevOps engineers minimize damage?

You will learn:

i. what downtime really is

ii. common causes of service outages

iii. why “restart fixes everything” is dangerous

iv. how deployment strategies reduce risk

v. why rollback thinking is mandatory

This week moves students from deployment awareness to operational responsibility.


1. What Is Reliability? (Correct Definition)

Reliability is not:

- “the app is running”

- “the container started”

- “no errors right now”

Reliability means Users can consistently access the service as expected, even during change or failure.

A system that works only when nothing changes is unreliable.

DevOps Thinking

Systems are reliable when they continue working despite change.


2. What Is Downtime? (From a User’s Perspective)

Downtime is any period when users cannot use the service.

This includes:

- service not responding

- partial outages

- slow responses

- broken features

From users’ point of view:

“If it doesn’t work, it’s down.”


3. Common Causes of Downtime During Deployment

Internalize these causes of downtime:

i. Stopping a service before starting the new one

ii. Port conflicts

iii. Crashed containers after restart

iv. Misconfigured environment variables

v. Missing dependencies

vi. Human error during manual steps

Most outages are self-inflicted.


4. The Danger of “Just Restart It”

Restarting services:

- causes immediate downtime

- may hide deeper problems

- breaks user sessions

- can trigger cascading failures

DevOps engineers restart only with intent, not panic.


5. Introduction to Deployment Strategies (Conceptual)

We do not automate yet.
We think first.

a. Recreate Strategy (What You’ve Been Doing)

Process:

Step i. Stop old version

Step ii. Start new version

Pros:

- simple

Cons:

- guaranteed downtime

- risky

- unacceptable in production

This is what beginners do.

b. Rolling Update (Conceptual)

Process:

Step i. update instances one at a time

Observe: service remains partially available

Pros:

- reduced downtime

Cons:

- harder to manage manually

- inconsistent state during rollout

c. Blue-Green Deployment (Conceptual)

Process:

Step i. run old and new versions side by side

Step ii. switch traffic when ready

Pros:

- near-zero downtime

- easy rollback

Cons:

- requires duplication

- harder without orchestration


DevOps Thinking

Deployment strategy is about risk management, not speed.


6. Rollback Is Not Optional

Every deployment must answer: “What do we do if this fails?”

Rollback options include:

i. restarting old container

ii. redeploying previous image

iii. switching traffic back

If rollback is unclear, deployment is unsafe.


7. Observability During Deployment

DevOps engineers monitor:

i. logs

ii. service availability

iii. user-facing behavior

They do not deploy blindly.

Key question during deployment:

“Are users still being served?”