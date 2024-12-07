---
theme: seriph
title: Event-Driven Architecture with Google Pub/Sub
info: |
  ## Event-Driven Architecture
  Presented by: Munir Khakhi
class: text-center
---

# Event-Driven Architecture with Google Pub/Sub

**Presented by:**  
**Munir Khakhi**

<div @click="$slidev.nav.next" class="mt-12 py-1" hover:bg="white op-10">
  Press Space for the next slide <carbon:arrow-right />
</div>

---
transition: fade
---

# Introduction

- **What is Event-Driven Architecture (EDA)?**
- **Benefits of EDA**:  
  - Scalability  
  - Resilience  
  - Flexibility
- **Why Google Pub/Sub?**
  - Reliability  
  - Scalability  
  - Security

---
layout: image
image: ./images/eda.gif
backgroundSize: 35em 80%
backgroundPosition: center
---

<div style="position: absolute; bottom: 1em; width: 100%; text-align: left; font-size: 0.8em; color: gray;">
  Credit: <a href="https://www.linkedin.com/in/rocky-bhatia-a4801010/" target="_blank">Rocky Bhatia</a>
</div>


---
transition: slide-left
---


# What is Event-Driven Architecture?

Event-Driven Architecture (EDA) is a software design pattern in which components communicate through events.

**Key Characteristics**:  
- Loose coupling between services  
- High responsiveness  
- Real-time communication

**Common Use Cases**:  
- Payment processing  
- Real-time notifications  
- Microservices communication

---
transition: slide-left
---

# Benefits of EDA

**Scalability**  
Systems can easily scale by adding more consumers or producers.  

**Resilience**  
Events are persisted and can be retried in case of failures.  

**Flexibility**  
Components can be replaced or updated independently.

---
transition: slide-left
---

# Why Google Pub/Sub?

Google Pub/Sub is a fully managed messaging service that enables asynchronous communication.

**Key Features**:  
- **Reliability**: Durable message storage and guaranteed delivery.  
- **Scalability**: Handles millions of messages per second.  
- **Security**: Encrypted data and access control.

---
transition: slide-left
---

# Deep Dive into Google Pub/Sub

**Message Delivery Semantics**  

1. **At-least-once delivery**:  
   - Messages may be delivered multiple times to ensure reliability.  

2. **At-most-once delivery**:  
   - Messages are delivered only once, without retries.

---
layout: center
---

# Code Example: Publish Message in Node.js

```ts {monaco-run}
async function publishMessage(message) {
  try {
    const messageId = await pubSubClient.topic(topicName).publishMessage({
      data: Buffer.from(message),
    });
    console.log(`Message published with ID: ${messageId}`);
  } catch (error) {
    console.error(`Error publishing message: ${error.message}`);
  }
}
```

- **Step 1**: Initialize `pubSubClient` with your credentials.  
- **Step 2**: Create or use an existing topic.  
- **Step 3**: Publish a message using `publishMessage`.  

---
layout: center
---

# Code Explanation: Laravel Example

Learn how to configure and use Google Pub/Sub in a Laravel application.

### Configuration

```php {monaco}
'pubsub' => [
    'driver' => 'pubsub',
    'queue' => 'test',
    'project_id' => env('PUBSUB_PROJECT_ID', 'pubsub-eda'),
    'retries' => 5,
    'request_timeout' => 60,
    'keyFilePath' => env('PUBSUB_KEY_FILE', storage_path('pubsub-eda.json')),
    // Subscriber will be key and publisher will be value here
    'subscribers' => [
        'test-sub' => 'test'
    ],
    'plain_handlers' => [
        'plain_sub' => App\Jobs\PlainClass::class // This one for non-Laravel format messages
    ],
],
```

---

## Dispatching Jobs

```php {monaco}
dispatch(new ProcessMessage($message))
    ->onQueue('test')
    ->onConnection('pubsub');
```

<style>
/* Style to make code blocks full-width */
pre {
  width: 100% !important;
  text-align: left;
  margin: 0 auto;
}
</style>


---
transition: slide-up
---

# Practical Example: Pub/Sub Workflow

- **Producer**: Sends messages to a Pub/Sub topic.
- **Topic**: Acts as a messaging hub.  
- **Subscriber**: Receives and processes the messages.

---

# Error Handling & Retry Mechanisms

- Configure **retry policies** to handle transient errors.
- Use **dead-letter topics** for undeliverable messages.
- Monitor message flow using Google Cloud's **Stackdriver**.

---
layout: center
class: text-center
---

# Q&A Session

Got questions? Let’s discuss!