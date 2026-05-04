# API Design

API design encompasses various approaches and styles, each tailored to meet specific requirements and preferences. Here are some common API design types:

1. **RESTful APIs (Representational State Transfer)**:

   - **Definition**: RESTful APIs follow the principles of REST, emphasizing stateless communication over HTTP using standard methods (GET, POST, PUT, DELETE) and resource-oriented URIs.
   - **Key Characteristics**:
     - Resources identified by URIs.
     - Standard HTTP methods for CRUD operations.
     - Stateless communication.
     - Use of hypermedia for linking resources.
   - **Advantages**: Scalability, flexibility, simplicity, and ease of caching.
   - **Examples**: Twitter API, GitHub API.

2. **GraphQL APIs**:

   - **Definition**: GraphQL is a query language for APIs and a runtime for executing those queries with your existing data. It provides a more efficient, powerful, and flexible alternative to RESTful APIs.
   - **Key Characteristics**:
     - Clients can request only the data they need.
     - Single endpoint for all queries.
     - Strongly typed schema.
     - Supports real-time updates with subscriptions.
   - **Advantages**: Reduced over-fetching and under-fetching, improved performance, and flexibility for clients.
   - **Examples**: Facebook GraphQL API, GitHub GraphQL API.

3. **SOAP APIs (Simple Object Access Protocol)**:

   - **Definition**: SOAP is a protocol for exchanging structured information in the implementation of web services. It uses XML for message formatting and typically runs over HTTP or SMTP.
   - **Key Characteristics**:
     - XML-based messaging format.
     - Standardized messaging protocol.
     - Supports more complex operations and message structures.
     - Built-in security features.
   - **Advantages**: Built-in error handling, security features, and support for ACID transactions.
   - **Examples**: SOAP APIs used in enterprise systems, banking, and government applications.

4. **gRPC APIs**:

   - **Definition**: gRPC is a high-performance RPC (Remote Procedure Call) framework developed by Google. It uses HTTP/2 for transport and Protocol Buffers for serialization.
   - **Key Characteristics**:
     - Efficient binary serialization with Protocol Buffers.
     - Supports bidirectional streaming and multiplexing.
     - Language-agnostic and platform-independent.
     - Generated client and server code from service definition.
   - **Advantages**: High performance, reduced network overhead, and built-in features like authentication and load balancing.
   - **Examples**: Google Cloud APIs, Istio service mesh.

5. **WebSocket APIs**:

   - **Definition**: WebSocket is a communication protocol that provides full-duplex communication channels over a single TCP connection, allowing for real-time, low-latency data exchange.
   - **Key Characteristics**:
     - Full-duplex communication.
     - Low-latency and real-time capabilities.
     - Supports bi-directional messaging.
     - Ideal for applications requiring continuous data updates.
   - **Advantages**: Real-time communication, reduced latency, and lightweight protocol overhead.
   - **Examples**: Chat applications, real-time gaming, financial trading platforms.

6. **Serverless APIs**:
   - **Definition**: Serverless APIs are backend services that are deployed and managed by cloud providers, allowing developers to focus on writing code without managing servers.
   - **Key Characteristics**:
     - Pay-per-use pricing model.
     - Automatically scaled and managed by cloud provider.
     - Event-driven architecture.
     - Stateless and ephemeral functions.
   - **Advantages**: Reduced operational overhead, auto-scaling, and cost efficiency.
   - **Examples**: AWS Lambda functions exposed via API Gateway, Azure Functions, Google Cloud Functions.

Each API design type has its own advantages, trade-offs, and best use cases. The choice of API design depends on factors such as scalability requirements, data complexity, client needs, and development team preferences.

# API design and Express + node JS compatibility

1. **RESTful APIs**:
   - **Compatibility**: Node.js and Express.js are highly compatible with RESTful API design. Express provides robust routing capabilities, middleware support, and HTTP method handling, making it well-suited for defining RESTful endpoints.
   - **Implementation**: With Express.js, you can define routes for different HTTP methods (GET, POST, PUT, DELETE) and handle requests to specific URIs. Express middleware can be used for tasks like request validation, authentication, and error handling. JSON is commonly used for data serialization in RESTful APIs with Node.js.

2. **GraphQL APIs**:
   - **Compatibility**: Node.js supports GraphQL through various libraries such as Apollo Server and Express-GraphQL. These libraries allow you to create GraphQL endpoints alongside Express.js applications, providing compatibility with Node.js.
   - **Implementation**: You can use Apollo Server or Express-GraphQL to integrate GraphQL into your Express.js application. With GraphQL, you define a schema that represents your data model and specify resolver functions to handle queries and mutations. Express middleware can be used for tasks like authentication and error handling.

3. **SOAP APIs**:
   - **Compatibility**: While SOAP APIs are less common in Node.js and Express.js environments, you can still implement SOAP services using libraries like soap or strong-soap. These libraries enable you to create SOAP endpoints within Express.js applications.
   - **Implementation**: Libraries like soap or strong-soap allow you to define SOAP services in Node.js applications. You can create SOAP endpoints by defining service methods and WSDL (Web Services Description Language) files. Express.js can be used to handle HTTP requests and responses for SOAP services.

4. **gRPC APIs**:
   - **Compatibility**: Node.js supports gRPC through the grpc library, which provides support for building gRPC services and clients. Express.js can be used alongside gRPC for handling HTTP requests and providing additional functionality.
   - **Implementation**: You can use the grpc library to define gRPC services in Node.js applications. Express.js can be used to expose HTTP endpoints for health checks, monitoring, or handling non-gRPC requests. Protocol Buffers (protobuf) are commonly used for defining message formats in gRPC APIs.

5. **WebSocket APIs**:
   - **Compatibility**: Node.js provides built-in support for WebSockets through the ws library, which enables you to create WebSocket servers and clients. Express.js can be used alongside WebSockets for handling HTTP requests and serving WebSocket-based APIs.
   - **Implementation**: You can use the ws library to create WebSocket servers in Node.js applications. Express.js can be used to define HTTP endpoints for initial WebSocket handshake or for serving static resources alongside WebSocket connections. Real-time applications like chat applications or live dashboards can benefit from WebSocket APIs.

6. **Serverless APIs**:
   - **Compatibility**: Node.js is a popular choice for building serverless functions, and Express.js can be used within serverless environments like AWS Lambda or Azure Functions. However, Express.js itself is not necessary for building serverless APIs.
   - **Implementation**: Serverless APIs can be implemented using AWS Lambda, Azure Functions, or Google Cloud Functions with Node.js. Express.js can be used to define the application logic within individual serverless functions, but it's not required. Frameworks like Serverless Framework or AWS SAM can be used for deployment and management of serverless applications.

In summary, Node.js and Express.js provide extensive compatibility and support for implementing various API design types, including RESTful APIs, GraphQL APIs, SOAP APIs, gRPC APIs, WebSocket APIs, and serverless APIs. With the flexibility and scalability offered by Node.js and Express.js, you can choose the API design type that best fits your project requirements and development preferences.

# Express + REST API design patterns

Express.js provides flexibility in designing RESTful APIs, and several design patterns can be employed depending on the complexity and requirements of your application. Here are some common design patterns used for setting up a REST backend with Express.js:

1. **Basic Route Handling**:
   - **Description**: This is the simplest design pattern where you define routes for each API endpoint and handle HTTP methods (GET, POST, PUT, DELETE) accordingly.
   - **Implementation**: Define route handlers for each endpoint using Express Router and handle requests within those handlers.

2. **Controller-Service Pattern**:
   - **Description**: Separates route handling logic (controllers) from business logic (services). Controllers are responsible for handling requests and responses, while services encapsulate business logic.
   - **Implementation**: Define separate controller functions for each route to handle request processing and response generation. Use service functions to encapsulate business logic and perform data manipulation or interaction with external systems.

3. **Middleware Pipeline**:
   - **Description**: Uses middleware functions to modularize request processing logic. Middleware functions can perform pre-processing, authentication, authorization, validation, logging, error handling, and more.
   - **Implementation**: Define middleware functions using Express middleware and attach them to routes or globally to the application using app.use(). Middleware functions are executed in the order they are defined in the middleware pipeline.

4. **Router-based Modularization**:
   - **Description**: Breaks down the API into modular routers, each responsible for handling a specific set of routes or endpoints. This pattern helps organize code and improve maintainability for large APIs.
   - **Implementation**: Define separate Router instances for different sets of routes (e.g., user routes, product routes, auth routes). Mount these routers on the main Express app using app.use().

5. **Model-View-Controller (MVC)**:
   - **Description**: Separates concerns into models, views, and controllers. Models represent data structures and business logic, views handle presentation logic, and controllers handle request processing and response generation.
   - **Implementation**: Define models to represent data structures and business logic. Use controllers to handle route logic and interact with models. Optionally, integrate a view engine like Pug or EJS for server-side rendering if needed.

6. **Repository Pattern**:
   - **Description**: Abstracts data access logic into repository classes or functions. Repositories encapsulate CRUD operations and provide a clean interface for accessing data from different data sources.
   - **Implementation**: Define repository classes or functions to encapsulate data access logic (e.g., querying the database). Use these repositories within route handlers or services to interact with the data layer.

7. **Versioning**:
   - **Description**: Supports multiple versions of the API to accommodate changes and updates without breaking existing clients. Versioning can be done through URL paths, query parameters, headers, or content negotiation.
   - **Implementation**: Define routes for different API versions (e.g., /v1/, /v2/) and handle requests accordingly. Use middleware or routing logic to determine which version of the API to use based on client preferences or constraints.

These design patterns can be used individually or in combination to create scalable, maintainable, and well-structured RESTful APIs with Express.js. The choice of pattern(s) depends on factors such as project size, complexity, team preferences, and future scalability requirements.

---

# REST Maturity Levels (Richardson Maturity Model)

The Richardson Maturity Model, developed by Leonard Richardson, classifies RESTful APIs into four levels of maturity based on how well they adhere to REST principles.

## The Four Levels

### Level 0 — The Swamp of POX (Plain Old XML)

- Uses HTTP as a transport mechanism only (typically a single endpoint).
- All operations go through one URI (e.g., `/api`) using POST for everything.
- Essentially RPC over HTTP — no use of HTTP methods or resource URIs.
- **Example**:
  ```
  POST /api
  Body: { "action": "getUser", "userId": 123 }
  ```

### Level 1 — Resources

- Introduces the concept of **individual resources** with distinct URIs.
- Still typically uses a single HTTP method (usually POST) for all operations.
- **Example**:
  ```
  POST /users/123
  POST /orders/456
  ```

### Level 2 — HTTP Verbs

- Uses **proper HTTP methods** (GET, POST, PUT, PATCH, DELETE) alongside resource URIs.
- Uses **HTTP status codes** correctly (200, 201, 204, 400, 404, 500, etc.).
- This is what most "RESTful" APIs achieve in practice.
- **Example**:
  ```
  GET    /users/123       → 200 OK
  POST   /users           → 201 Created
  PUT    /users/123       → 200 OK
  DELETE /users/123       → 204 No Content
  ```

### Level 3 — Hypermedia Controls (HATEOAS)

- Responses include **hypermedia links** that tell the client what actions are available next.
- The client discovers available operations dynamically instead of hard-coding URLs.
- Represents the "true" REST as defined by Roy Fielding.
- **Example**:
  ```json
  {
    "id": 123,
    "name": "John Doe",
    "links": [
      { "rel": "self", "href": "/users/123", "method": "GET" },
      { "rel": "update", "href": "/users/123", "method": "PUT" },
      { "rel": "delete", "href": "/users/123", "method": "DELETE" },
      { "rel": "orders", "href": "/users/123/orders", "method": "GET" }
    ]
  }
  ```

## Resource Naming Conventions

- Use **nouns** (not verbs) for resource URIs: `/users`, `/orders`, `/products`
- Use **plural nouns** for collections: `/users` not `/user`
- Use **path parameters** for specific resources: `/users/:id`
- Use **nesting** for relationships: `/users/:id/orders`
- Use **query parameters** for filtering/sorting: `/users?role=admin&sort=name`
- Use **kebab-case** for multi-word resources: `/order-items` not `/orderItems`
- Avoid deep nesting (max 2-3 levels): `/users/:id/orders` ✅, `/users/:id/orders/:oid/items/:iid/reviews` ❌

## When to Use Each HTTP Status Code

| Status Code | Meaning | When to Use |
|---|---|---|
| **200** OK | Success | GET, PUT, PATCH that returns data |
| **201** Created | Resource created | Successful POST that creates a resource |
| **204** No Content | Success, no body | Successful DELETE or PUT with no response body |
| **301** Moved Permanently | Redirect | Resource URI has permanently changed |
| **304** Not Modified | Cache hit | Conditional GET with ETags (see Caching section) |
| **400** Bad Request | Client error | Validation failure, malformed request body |
| **401** Unauthorized | Not authenticated | Missing or invalid authentication token |
| **403** Forbidden | Not authorized | Authenticated but lacks permission |
| **404** Not Found | Resource doesn't exist | Invalid resource URI or ID |
| **409** Conflict | State conflict | Duplicate resource, version conflict |
| **422** Unprocessable Entity | Semantic error | Request is syntactically valid but semantically wrong |
| **429** Too Many Requests | Rate limited | Client exceeded rate limit |
| **500** Internal Server Error | Server error | Unhandled exception or server failure |
| **503** Service Unavailable | Temporarily down | Server overloaded or under maintenance |

---

# Pagination

Pagination is essential for APIs that return large datasets. Two main strategies exist: offset-based and cursor-based.

## Offset-Based Pagination

- Uses `skip` and `limit` (or `page` and `pageSize`) parameters.
- Simple to implement and understand.
- **Problem**: Inconsistent results when data is inserted/deleted between requests (items can be skipped or duplicated).

### MongoDB Implementation

```javascript
// GET /api/posts?page=2&limit=10
router.get('/posts', async (req, res) => {
  const page = Math.max(1, parseInt(req.query.page) || 1);
  const limit = Math.min(100, Math.max(1, parseInt(req.query.limit) || 10));
  const skip = (page - 1) * limit;

  const [posts, total] = await Promise.all([
    Post.find().sort({ createdAt: -1 }).skip(skip).limit(limit),
    Post.countDocuments()
  ]);

  res.json({
    data: posts,
    pagination: {
      currentPage: page,
      pageSize: limit,
      totalItems: total,
      totalPages: Math.ceil(total / limit),
      hasNextPage: page * limit < total,
      hasPrevPage: page > 1
    }
  });
});
```

## Cursor-Based Pagination

- Uses a **cursor** (typically the `_id` or a timestamp of the last item) to fetch the next batch.
- More efficient for large datasets — avoids `skip()` which scans and discards documents.
- Stable results even when new data is inserted concurrently.

### MongoDB Implementation

```javascript
// GET /api/posts?cursor=507f1f77bcf86cd799439011&limit=10
router.get('/posts', async (req, res) => {
  const limit = Math.min(100, Math.max(1, parseInt(req.query.limit) || 10));
  const cursor = req.query.cursor;

  const query = cursor ? { _id: { $lt: cursor } } : {};

  const posts = await Post.find(query)
    .sort({ _id: -1 })
    .limit(limit + 1); // fetch one extra to check if there's a next page

  const hasNextPage = posts.length > limit;
  if (hasNextPage) posts.pop(); // remove the extra item

  res.json({
    data: posts,
    pagination: {
      nextCursor: hasNextPage ? posts[posts.length - 1]._id : null,
      hasNextPage
    }
  });
});
```

## Concurrent Insertion Problem with Offset Pagination

If new documents are inserted while a client is paginating with offsets:
- **Page 1**: Client fetches items 1–10.
- **New insert**: A new item is added at position 1 (e.g., newest first).
- **Page 2**: Client fetches with `skip(10)` — the item that was #10 is now #11, so it gets skipped entirely.
- **Result**: The client never sees that item.

Cursor-based pagination avoids this because it always fetches items **after** a specific reference point, regardless of what was inserted before it.

---

# API Versioning

API versioning allows you to evolve your API without breaking existing clients. Three main strategies:

## 1. URI Path Versioning

The version is part of the URL path. Most common and most visible.

```javascript
// Express implementation
const v1Router = express.Router();
const v2Router = express.Router();

v1Router.get('/users', (req, res) => {
  res.json({ version: 'v1', data: users });
});

v2Router.get('/users', (req, res) => {
  // v2 returns a different response shape
  res.json({ version: 'v2', data: users.map(u => ({ ...u, fullName: `${u.first} ${u.last}` })) });
});

app.use('/api/v1', v1Router);
app.use('/api/v2', v2Router);
```

**Pros**: Easy to understand, easy to route, cacheable.
**Cons**: URL pollution, harder to sunset old versions.

## 2. Header Versioning (Custom Header or Accept Header)

The version is sent in a request header.

```javascript
// Express middleware for header-based versioning
function versionMiddleware(req, res, next) {
  const version = req.headers['api-version'] || req.headers['accept']?.match(/version=(\d+)/)?.[1] || '1';
  req.apiVersion = parseInt(version);
  next();
}

app.use(versionMiddleware);

app.get('/api/users', (req, res) => {
  if (req.apiVersion === 2) {
    return res.json({ version: 'v2', data: usersV2 });
  }
  res.json({ version: 'v1', data: usersV1 });
});
```

**Pros**: Clean URLs, flexible.
**Cons**: Not visible in URL, harder to test in browser, can't be bookmarked.

## 3. Query Parameter Versioning

The version is passed as a query parameter.

```javascript
// GET /api/users?version=2
app.get('/api/users', (req, res) => {
  const version = parseInt(req.query.version) || 1;

  if (version === 2) {
    return res.json({ version: 'v2', data: usersV2 });
  }
  res.json({ version: 'v1', data: usersV1 });
});
```

**Pros**: Simple, easy to test.
**Cons**: Easy to forget, clutters query string, optional parameter can cause confusion.

## Comparison

| Strategy | Visibility | Cacheability | Clean URLs | Ease of Use |
|---|---|---|---|---|
| URI Path | ✅ High | ✅ Yes | ❌ No | ✅ Easy |
| Header | ❌ Hidden | ⚠️ Varies | ✅ Yes | ⚠️ Medium |
| Query Param | ⚠️ Medium | ⚠️ Varies | ❌ No | ✅ Easy |

**Recommendation**: URI path versioning is the most widely adopted (used by GitHub, Stripe, Twitter). Use it unless you have a specific reason not to.

---

# Idempotency

## What is Idempotency?

An API operation is **idempotent** if making the same request multiple times produces the same result as making it once. This is critical for reliability — network failures, timeouts, and retries are common in distributed systems.

| HTTP Method | Idempotent? | Why |
|---|---|---|
| GET | ✅ Yes | Reading data doesn't change state |
| PUT | ✅ Yes | Replaces the entire resource — same input = same result |
| DELETE | ✅ Yes | Deleting the same resource twice = resource is still deleted |
| PATCH | ⚠️ Depends | Can be idempotent if setting absolute values, not if incrementing |
| **POST** | ❌ **No** | Creates a new resource each time — duplicates possible |

## Why POST Specifically Needs Idempotency Keys

POST is used for creating resources and triggering actions (payments, emails, etc.). If a client sends a POST and the network drops the response, the client doesn't know if it succeeded. Retrying without idempotency protection could create **duplicate records**, **double charges**, or **duplicate notifications**.

An **idempotency key** is a unique identifier (typically a UUID) sent by the client. The server stores the result of the first request and returns the cached result for any duplicate key.

## Implementing Idempotency Keys with Redis in Express

```javascript
const { v4: uuidv4 } = require('uuid');
const Redis = require('ioredis');
const redis = new Redis();

const IDEMPOTENCY_TTL = 86400; // 24 hours

async function idempotencyMiddleware(req, res, next) {
  // Only apply to POST requests
  if (req.method !== 'POST') return next();

  const idempotencyKey = req.headers['idempotency-key'];
  if (!idempotencyKey) return next(); // No key = no protection

  const cacheKey = `idempotency:${idempotencyKey}`;
  const cached = await redis.get(cacheKey);

  if (cached) {
    const { statusCode, body } = JSON.parse(cached);
    return res.status(statusCode).json(body);
  }

  // Override res.json to capture the response
  const originalJson = res.json.bind(res);
  res.json = async (body) => {
    await redis.set(cacheKey, JSON.stringify({
      statusCode: res.statusCode,
      body
    }), 'EX', IDEMPOTENCY_TTL);
    return originalJson(body);
  };

  next();
}

app.use(idempotencyMiddleware);
```

## Making a Payment API Retry-Safe

```javascript
// Client sends: POST /api/payments
// Headers: { "Idempotency-Key": "pay_abc123-unique-uuid" }

router.post('/payments', async (req, res) => {
  const { amount, currency, customerId, paymentMethod } = req.body;

  // 1. Check if payment already exists with this idempotency key
  const existingPayment = await Payment.findOne({
    idempotencyKey: req.headers['idempotency-key']
  });

  if (existingPayment) {
    return res.status(200).json({
      message: 'Payment already processed',
      payment: existingPayment
    });
  }

  // 2. Process the payment
  const payment = await Payment.create({
    amount,
    currency,
    customerId,
    paymentMethod,
    idempotencyKey: req.headers['idempotency-key'],
    status: 'pending'
  });

  try {
    // 3. Charge via payment gateway
    const charge = await paymentGateway.charge({ amount, currency, paymentMethod });

    payment.status = 'succeeded';
    payment.gatewayId = charge.id;
    await payment.save();

    res.status(201).json({ payment });
  } catch (err) {
    payment.status = 'failed';
    payment.error = err.message;
    await payment.save();

    res.status(402).json({ error: 'Payment failed', details: err.message });
  }
});
```

---

# Error Response Standards (RFC 7807)

## RFC 7807 — Problem Details for HTTP APIs

RFC 7807 defines a standard format for error responses, so clients can consistently parse and handle errors across different APIs.

### Standard Fields

| Field | Type | Required | Description |
|---|---|---|---|
| `type` | string (URI) | Yes | A URI reference that identifies the problem type |
| `title` | string | Yes | A short, human-readable summary |
| `status` | integer | Yes | The HTTP status code |
| `detail` | string | No | A human-readable explanation specific to this occurrence |
| `instance` | string (URI) | No | A URI reference for the specific occurrence |

### Express Implementation

```javascript
// Error class following RFC 7807
class ApiError extends Error {
  constructor(status, title, detail, type) {
    super(detail || title);
    this.status = status;
    this.title = title;
    this.detail = detail;
    this.type = type || 'about:blank';
    this.instance = null; // set per-request
  }

  toJSON() {
    return {
      type: this.type,
      title: this.title,
      status: this.status,
      detail: this.detail,
      ...(this.instance && { instance: this.instance })
    };
  }
}

// Global error handler middleware
function errorHandler(err, req, res, next) {
  if (err instanceof ApiError) {
    err.instance = req.originalUrl;
    return res.status(err.status).set('Content-Type', 'application/problem+json').json(err.toJSON());
  }

  // Fallback for unexpected errors
  res.status(500).set('Content-Type', 'application/problem+json').json({
    type: 'about:blank',
    title: 'Internal Server Error',
    status: 500,
    detail: 'An unexpected error occurred',
    instance: req.originalUrl
  });
}

app.use(errorHandler);

// Usage in routes
router.get('/users/:id', async (req, res, next) => {
  const user = await User.findById(req.params.id);
  if (!user) {
    return next(new ApiError(404, 'User Not Found', `No user with id ${req.params.id} exists`));
  }
  res.json(user);
});
```

### Response Example

```json
{
  "type": "https://api.example.com/errors/insufficient-funds",
  "title": "Insufficient Funds",
  "status": 422,
  "detail": "Account balance is $10.00 but the transaction requires $25.00",
  "instance": "/api/payments/txn_abc123"
}
```

## Formatting Validation Errors Consistently

Extend the RFC 7807 format with a `errors` array for field-level validation:

```javascript
class ValidationError extends ApiError {
  constructor(errors) {
    super(400, 'Validation Error', 'One or more fields failed validation');
    this.type = 'https://api.example.com/errors/validation';
    this.errors = errors; // array of { field, message, code }
  }

  toJSON() {
    return {
      ...super.toJSON(),
      errors: this.errors
    };
  }
}

// Usage
throw new ValidationError([
  { field: 'email', message: 'Must be a valid email address', code: 'invalid_format' },
  { field: 'age', message: 'Must be at least 18', code: 'min_value' }
]);
```

### Validation Error Response Example

```json
{
  "type": "https://api.example.com/errors/validation",
  "title": "Validation Error",
  "status": 400,
  "detail": "One or more fields failed validation",
  "instance": "/api/users",
  "errors": [
    { "field": "email", "message": "Must be a valid email address", "code": "invalid_format" },
    { "field": "age", "message": "Must be at least 18", "code": "min_value" }
  ]
}
```

---

# Request Validation with Zod

## What is Zod?

Zod is a TypeScript-first schema validation library. It's lightweight, has zero dependencies, and works well with Express for validating request bodies, params, and query strings.

## Zod Validation Middleware for Express

```javascript
const { z } = require('zod');

// Generic validation middleware factory
function validate(schema) {
  return (req, res, next) => {
    const result = schema.safeParse({
      body: req.body,
      query: req.query,
      params: req.params
    });

    if (!result.success) {
      const errors = result.error.issues.map(issue => ({
        field: issue.path.join('.'),
        message: issue.message,
        code: issue.code
      }));

      return res.status(400).json({
        type: 'https://api.example.com/errors/validation',
        title: 'Validation Error',
        status: 400,
        detail: 'One or more fields failed validation',
        errors
      });
    }

    // Replace with parsed (and transformed) values
    req.body = result.data.body;
    req.query = result.data.query;
    req.params = result.data.params;
    next();
  };
}

// Define schemas
const createUserSchema = z.object({
  body: z.object({
    name: z.string().min(2).max(100),
    email: z.string().email(),
    age: z.number().int().min(18).max(120),
    role: z.enum(['user', 'admin']).default('user')
  }),
  query: z.object({}).optional(),
  params: z.object({}).optional()
});

// Usage in routes
router.post('/users', validate(createUserSchema), async (req, res) => {
  const user = await User.create(req.body);
  res.status(201).json(user);
});
```

## Sanitizing Input Against XSS and NoSQL Injection

### XSS Sanitization

```javascript
const xss = require('xss');

// Middleware to sanitize all string fields in request body
function sanitizeXSS(req, res, next) {
  if (req.body) {
    const sanitize = (obj) => {
      for (const key in obj) {
        if (typeof obj[key] === 'string') {
          obj[key] = xss(obj[key]);
        } else if (typeof obj[key] === 'object' && obj[key] !== null) {
          sanitize(obj[key]);
        }
      }
    };
    sanitize(req.body);
  }
  next();
}

app.use(express.json());
app.use(sanitizeXSS);
```

### NoSQL Injection Prevention

MongoDB operators like `$gt`, `$ne`, `$regex` in user input can bypass queries. Sanitize by stripping keys that start with `$`.

```javascript
const mongoSanitize = require('express-mongo-sanitize');

// Strips any keys that start with '$' or contain '.'
app.use(mongoSanitize());

// Or manually:
function sanitizeMongo(req, res, next) {
  const sanitize = (obj) => {
    for (const key in obj) {
      if (key.startsWith('$') || key.includes('.')) {
        delete obj[key];
      } else if (typeof obj[key] === 'object' && obj[key] !== null) {
        sanitize(obj[key]);
      }
    }
  };
  if (req.body) sanitize(req.body);
  if (req.query) sanitize(req.query);
  if (req.params) sanitize(req.params);
  next();
}
```

**Example attack without sanitization**:
```json
// POST /api/login
{ "username": "admin", "password": { "$ne": "" } }
// This would match any user with username "admin" regardless of password!
```

---

# Caching Strategies

Caching reduces server load, speeds up responses, and saves bandwidth. Two key strategies for Express APIs:

## ETags and Conditional Requests

An **ETag** (Entity Tag) is a response header containing a hash or version identifier for a resource. The client can send this back in subsequent requests so the server can check if the data has changed.

### How It Works

1. Server responds with `ETag: "abc123"` header.
2. Client sends `If-None-Match: "abc123"` on next request.
3. If data hasn't changed → server returns `304 Not Modified` (no body, saves bandwidth).
4. If data changed → server returns `200 OK` with new data and new ETag.

### Express Implementation

```javascript
const crypto = require('crypto');

function generateETag(data) {
  return crypto.createHash('md5').update(JSON.stringify(data)).digest('hex');
}

router.get('/products/:id', async (req, res) => {
  const product = await Product.findById(req.params.id);
  if (!product) return res.status(404).json({ error: 'Not found' });

  const etag = generateETag(product);

  // Check if client has current version
  if (req.headers['if-none-match'] === etag) {
    return res.status(304).end();
  }

  res.set('ETag', etag).json(product);
});
```

> **Tip**: Express has built-in ETag support via `app.set('etag', true)` (enabled by default), but custom ETags give you more control.

## Redis Caching Layer Middleware

A generic middleware that caches API responses in Redis with configurable TTL.

```javascript
const Redis = require('ioredis');
const redis = new Redis();

function cacheMiddleware(ttlSeconds = 300) {
  return async (req, res, next) => {
    // Only cache GET requests
    if (req.method !== 'GET') return next();

    const cacheKey = `cache:${req.originalUrl}`;

    try {
      const cached = await redis.get(cacheKey);
      if (cached) {
        return res.set('X-Cache', 'HIT').json(JSON.parse(cached));
      }
    } catch (err) {
      console.error('Redis cache error:', err);
      // Don't fail the request if Redis is down — just skip cache
    }

    // Override res.json to store in cache
    const originalJson = res.json.bind(res);
    res.json = async (body) => {
      try {
        await redis.set(cacheKey, JSON.stringify(body), 'EX', ttlSeconds);
      } catch (err) {
        console.error('Redis cache write error:', err);
      }
      res.set('X-Cache', 'MISS');
      return originalJson(body);
    };

    next();
  };
}

// Usage
router.get('/products', cacheMiddleware(600), async (req, res) => {
  const products = await Product.find();
  res.json(products);
});
```

### Cache Invalidation

```javascript
// Invalidate cache when data changes
router.post('/products', async (req, res) => {
  const product = await Product.create(req.body);
  await redis.del('cache:/api/products'); // Invalidate list cache
  res.status(201).json(product);
});

router.put('/products/:id', async (req, res) => {
  const product = await Product.findByIdAndUpdate(req.params.id, req.body, { new: true });
  await redis.del(`cache:/api/products/${req.params.id}`); // Invalidate item cache
  await redis.del('cache:/api/products');                   // Invalidate list cache
  res.json(product);
});
```

---

# File Upload

## File Upload with Multer, Validation, and S3 Streaming

### Basic Multer Setup with Validation

```javascript
const multer = require('multer');
const path = require('path');

// Configure storage
const storage = multer.memoryStorage(); // Store in memory for S3 streaming

// File filter for validation
const fileFilter = (req, file, cb) => {
  const allowedTypes = ['image/jpeg', 'image/png', 'image/webp', 'application/pdf'];
  if (allowedTypes.includes(file.mimetype)) {
    cb(null, true);
  } else {
    cb(new Error(`File type ${file.mimetype} is not allowed`), false);
  }
};

const upload = multer({
  storage,
  fileFilter,
  limits: {
    fileSize: 5 * 1024 * 1024, // 5MB max
    files: 5                    // Max 5 files at once
  }
});
```

### Streaming to AWS S3

```javascript
const { S3Client, PutObjectCommand } = require('@aws-sdk/client-s3');
const { v4: uuidv4 } = require('uuid');

const s3Client = new S3Client({ region: process.env.AWS_REGION });

async function uploadToS3(file) {
  const key = `uploads/${uuidv4()}${path.extname(file.originalname)}`;

  await s3Client.send(new PutObjectCommand({
    Bucket: process.env.S3_BUCKET,
    Key: key,
    Body: file.buffer,
    ContentType: file.mimetype,
    ContentDisposition: 'inline'
  }));

  return {
    key,
    url: `https://${process.env.S3_BUCKET}.s3.${process.env.AWS_REGION}.amazonaws.com/${key}`,
    originalName: file.originalname,
    size: file.size,
    mimeType: file.mimetype
  };
}

// Route: single file upload
router.post('/upload', upload.single('file'), async (req, res) => {
  if (!req.file) {
    return res.status(400).json({ error: 'No file provided' });
  }

  const result = await uploadToS3(req.file);
  res.status(201).json(result);
});

// Route: multiple file upload
router.post('/upload/multiple', upload.array('files', 5), async (req, res) => {
  if (!req.files || req.files.length === 0) {
    return res.status(400).json({ error: 'No files provided' });
  }

  const results = await Promise.all(req.files.map(uploadToS3));
  res.status(201).json(results);
});

// Error handling for multer
app.use((err, req, res, next) => {
  if (err instanceof multer.MulterError) {
    if (err.code === 'LIMIT_FILE_SIZE') {
      return res.status(400).json({ error: 'File too large. Max size is 5MB.' });
    }
    if (err.code === 'LIMIT_FILE_COUNT') {
      return res.status(400).json({ error: 'Too many files. Max is 5.' });
    }
    return res.status(400).json({ error: err.message });
  }
  next(err);
});
```

---

# Webhooks

## What are Webhooks?

Webhooks are HTTP callbacks — when an event occurs in one system, it sends an HTTP POST request to a configured URL in another system. They enable real-time, event-driven communication between services (e.g., Stripe notifying your app of a payment).

## Complete Webhook System

### Webhook Sender (Your API notifying external consumers)

```javascript
const crypto = require('crypto');
const axios = require('axios');

// Webhook model
// { url, secret, events: ['order.created', 'order.updated'], active: true }

class WebhookSender {
  // Generate HMAC signature for payload verification
  static generateSignature(payload, secret) {
    return crypto
      .createHmac('sha256', secret)
      .update(JSON.stringify(payload))
      .digest('hex');
  }

  // Send webhook with retry logic
  static async send(webhookUrl, secret, event, payload, maxRetries = 3) {
    const body = {
      id: crypto.randomUUID(),
      event,
      timestamp: new Date().toISOString(),
      data: payload
    };

    const signature = this.generateSignature(body, secret);

    for (let attempt = 1; attempt <= maxRetries; attempt++) {
      try {
        const response = await axios.post(webhookUrl, body, {
          headers: {
            'Content-Type': 'application/json',
            'X-Webhook-Signature': signature,
            'X-Webhook-Event': event,
            'X-Webhook-Delivery': body.id
          },
          timeout: 10000 // 10 second timeout
        });

        if (response.status >= 200 && response.status < 300) {
          await WebhookLog.create({ webhookId: body.id, event, status: 'delivered', attempt });
          return;
        }
      } catch (err) {
        console.error(`Webhook delivery attempt ${attempt} failed:`, err.message);

        if (attempt < maxRetries) {
          // Exponential backoff: 1s, 4s, 9s
          await new Promise(resolve => setTimeout(resolve, attempt * attempt * 1000));
        } else {
          await WebhookLog.create({ webhookId: body.id, event, status: 'failed', attempt, error: err.message });
        }
      }
    }
  }

  // Dispatch to all registered webhooks for an event
  static async dispatch(event, payload) {
    const webhooks = await Webhook.find({ events: event, active: true });
    await Promise.allSettled(
      webhooks.map(wh => this.send(wh.url, wh.secret, event, payload))
    );
  }
}

// Usage in your API
router.post('/orders', async (req, res) => {
  const order = await Order.create(req.body);
  res.status(201).json(order);

  // Fire webhook asynchronously (don't block response)
  WebhookSender.dispatch('order.created', order);
});
```

### Webhook Receiver (Your app receiving external webhooks, e.g., from Stripe)

```javascript
// Verify webhook signature to ensure authenticity
function verifyWebhookSignature(secret) {
  return (req, res, next) => {
    const signature = req.headers['x-webhook-signature'];
    if (!signature) {
      return res.status(401).json({ error: 'Missing webhook signature' });
    }

    const expectedSignature = crypto
      .createHmac('sha256', secret)
      .update(JSON.stringify(req.body))
      .digest('hex');

    const isValid = crypto.timingSafeEqual(
      Buffer.from(signature),
      Buffer.from(expectedSignature)
    );

    if (!isValid) {
      return res.status(401).json({ error: 'Invalid webhook signature' });
    }

    next();
  };
}

// Webhook receiver endpoint
router.post(
  '/webhooks/stripe',
  express.json({ type: 'application/json' }),
  verifyWebhookSignature(process.env.STRIPE_WEBHOOK_SECRET),
  async (req, res) => {
    const { event, data } = req.body;

    // Process idempotently — use the webhook delivery ID to avoid double-processing
    const deliveryId = req.headers['x-webhook-delivery'];
    const alreadyProcessed = await WebhookLog.findOne({ deliveryId });
    if (alreadyProcessed) {
      return res.status(200).json({ message: 'Already processed' });
    }

    // Handle event types
    switch (event) {
      case 'payment.succeeded':
        await handlePaymentSuccess(data);
        break;
      case 'payment.failed':
        await handlePaymentFailure(data);
        break;
      default:
        console.log(`Unhandled webhook event: ${event}`);
    }

    await WebhookLog.create({ deliveryId, event, status: 'processed' });

    // Always respond 200 quickly to acknowledge receipt
    res.status(200).json({ received: true });
  }
);
```

## Webhook Event Payload Design

Best practices for designing webhook payloads:

```json
{
  "id": "evt_abc123",
  "event": "order.created",
  "timestamp": "2026-03-24T10:00:00.000Z",
  "version": "1.0",
  "data": {
    "id": "ord_456",
    "customer": "cus_789",
    "total": 99.99,
    "currency": "USD",
    "items": [
      { "product": "prod_001", "quantity": 2, "price": 49.99 }
    ]
  }
}
```

**Key principles**:
- Include a unique **event ID** for idempotent processing.
- Include a **timestamp** for ordering and debugging.
- Use **dot-notation event names** (e.g., `order.created`, `payment.failed`).
- Include the **full resource** in `data` (not just an ID) to avoid extra API calls.
- **Version** your webhook payloads so subscribers can handle changes gracefully.

---

# Quick Reference: API Design Interview One-Liners

| Topic | One-Liner |
|---|---|
| **Richardson Maturity** | Level 0 = RPC, Level 1 = Resources, Level 2 = HTTP Verbs, Level 3 = HATEOAS |
| **Offset Pagination** | Simple but breaks under concurrent inserts — items can be skipped or duplicated |
| **Cursor Pagination** | Uses last item's ID as pointer — stable and efficient for large datasets |
| **URI Versioning** | Most common; version in path (`/v1/users`) — used by Stripe, GitHub |
| **Idempotency Key** | Client-generated UUID sent in header; server caches + replays the response |
| **RFC 7807** | Standard error format: `{ type, title, status, detail, instance }` |
| **Zod** | TypeScript-first schema validation; zero deps; `safeParse()` for non-throwing validation |
| **NoSQL Injection** | Attacker passes `{ "$ne": "" }` as input — use `express-mongo-sanitize` |
| **ETag** | Hash of response data; client sends `If-None-Match` → server returns 304 if unchanged |
| **Redis Cache** | Cache GET responses with TTL; invalidate on writes; add `X-Cache: HIT/MISS` header |
| **Multer** | Express middleware for multipart/form-data; use `memoryStorage()` for S3 streaming |
| **Webhook Security** | HMAC-SHA256 signature + `timingSafeEqual()` to verify authenticity |
| **Webhook Retry** | Exponential backoff (1s, 4s, 9s); max 3 attempts; log delivery status |
| **HMAC Signing** | `crypto.createHmac('sha256', secret).update(payload).digest('hex')` |
| **Rate Limiting** | Tiered by API key plan; use Redis sliding window; return `429` + `Retry-After` header |