# Redis Notes (Backend Developer Friendly)

## 1. What is Redis?

Redis is an in-memory data structure store used as:
- Cache
- Database
- Message broker
- Queue
- Session store

Redis stores data in RAM, which makes it extremely fast compared to traditional databases that read from disk.

# Real-World Analogy

Imagine a shopkeeper:
- Main register/book = Database (slow but permanent)
- Whiteboard near counter = Redis (fast temporary memory)

Instead of checking the large register repeatedly for tea price, the shopkeeper checks the whiteboard instantly.

Redis works the same way:
- Frequently needed data is kept in memory
- Faster access
- Reduces load on primary database

# 2. Why Redis is Used

Redis is mainly used to:
- Reduce database load
- Improve response time
- Handle high traffic
- Store temporary data
- Share data between servers

# 3. Redis is NOT a Replacement for Main Database

Primary databases:
- Store permanent data
- Ensure durability
- Support complex queries

Redis:
- Optimized for speed
- Mostly temporary/fast-access data
- Used as supporting layer

# Typical Architecture

Client Request
      ↓
Backend Server
      ↓
Check Redis Cache
      ↓
If Found → Return Fast
If Not Found →
      ↓
Query Main Database
      ↓
Store Result in Redis
      ↓
Return Response

# 4. Key Features of Redis

- In-Memory Storage
- Extremely Fast
- Key-Value Store
- TTL Support
- Persistence Optional
- Pub/Sub Messaging
- Rich Data Structures

# 5. Common Redis Use Cases

## A. Caching

Problem:
Database queries are expensive and slow.

Solution:
Store frequently accessed data in Redis.

Example:

```javascript
const cachedUser = await redis.get("user:101");

if (cachedUser) {
   return JSON.parse(cachedUser);
}

const user = await db.users.findById(101);

await redis.set("user:101", JSON.stringify(user));

return user;
```

Benefits:
- Faster API response
- Reduced DB queries
- Better scalability

# 6. Session Storage

Redis is commonly used to store user sessions.

Benefits:
- Shared across servers
- Fast access
- Temporary storage
- TTL support

Example:

```javascript
await redis.set(
   `session:${sessionId}`,
   JSON.stringify(userData)
);
```

# 7. OTP & Temporary Data

Redis is excellent for:
- OTPs
- Password reset tokens
- Email verification codes

Example:

```javascript
await redis.set("otp:9999", "483921", {
   EX: 300
});
```

EX: 300 means expire after 300 seconds.

# 8. Rate Limiting

Used to prevent abuse/spam.

Example:

```javascript
const key = `rate:${ip}`;

const requests = await redis.incr(key);

if (requests === 1) {
   await redis.expire(key, 60);
}

if (requests > 100) {
   throw new Error("Rate limit exceeded");
}
```

# 9. Job Queues

Redis lists can manage background jobs.

Examples:
- Sending emails
- Notifications
- Image processing

Example:

```javascript
await redis.lpush(
   "emailQueue",
   JSON.stringify(emailData)
);
```

Worker:

```javascript
const job = await redis.rpop("emailQueue");
```

# 10. Redis Data Structures

## String
SET name "Vipin"
GET name

## List
LPUSH queue job1
RPUSH queue job2

## Hash
HSET user:1 name Vipin age 28

## Set
SADD tags javascript redis nodejs

## Sorted Set
ZADD leaderboard 100 vipin

# 11. Persistence in Redis

Methods:
- RDB Snapshots
- AOF Logs

# 12. When Should You Use Redis?

Use Redis when you need:
- High-speed reads
- Temporary storage
- Caching
- Sessions
- Queues
- Rate limiting

# 13. When NOT to Use Redis

Avoid Redis when:
- Need permanent critical storage only
- Large relational queries
- Data exceeds RAM limits

# 14. Redis vs Traditional Database

Redis:
- RAM-based
- Extremely fast
- Best for caching/temp data

Traditional DB:
- Disk-based
- Slower
- Best for permanent storage

# 15. Popular Redis Alternatives

- KeyDB
- DragonflyDB
- Valkey

# 16. Important Backend Engineering Insight

Do NOT use Redis just because big companies use it.

Use it only when:
- Performance bottlenecks exist
- Read-heavy traffic exists
- Temporary storage is required

# 17. Modern Backend Stack

Frontend
   ↓
Backend API
   ↓
Redis Cache Layer
   ↓
Main Database

# 18. Interview-Level Understanding

Redis trades memory cost for speed.

RAM is expensive but extremely fast.

# 19. Beginner Mistakes

- Using Redis as primary DB
- Caching everything blindly
- No cache invalidation strategy
- Storing huge objects unnecessarily

# 20. Cache Invalidation

Problem:
Database updated but cache still has old data.

Solutions:
- Delete cache after update
- Use TTL
- Cache versioning

# 21. Redis Commands Cheat Sheet

```bash
SET key value
GET key

DEL key

EXPIRE key 60

INCR counter

LPUSH queue item
RPOP queue

HSET user name Vipin
HGET user name

TTL key
```

# 22. Final Summary

Redis is:
- Fast
- Lightweight
- In-memory

Best used for:
- Caching
- Sessions
- OTPs
- Queues
- Rate limiting

It complements your main database rather than replacing it.

# One-Line Definition

Redis is a high-performance in-memory data store mainly used for caching and temporary fast-access data in scalable applications.
