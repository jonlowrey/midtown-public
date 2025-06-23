# Midtown - Software Engineer Technical

  

## Problem Statement

  

Our new class offering, **The PIT**, is super popular and has a long waitlist. We want to inform customers via notification when they have been added to the class roster.

  

You have three services to accomplish this.

  

## Available Services

  

### 1. Waitlist Service

  

**Endpoint:** `GET /waitlist/:classID`

  

**Response Body:** `Waitlist`

  

```typescript

interface Waitlist {

  classId: string;

  members: Member[];

}

  

interface Member {

  memberId: string;

}

```

  

### 2. Class Roster Service

  

**Endpoint:** `GET /classRosters`

  

**Response Body:** `ClassRoster[]`

  

```typescript

interface ClassRoster {

  classId: string;

  className: string;

  capacity: number;

  memberIds: string[];

}

```

  

### 3. Notification Service

  

**Endpoint:** `POST /notification/:memberId`

  

**Request Body:** `Notification`

  

```typescript

interface Notification {

  message: string;

}

```

  

## Task

Write a function that uses these APIs to deliver notifications to members when there is a seat open in the class. There are no live endpoints to hit.

  

### Notes

 - There are no live endpoints to hit, so you can make your calls to the paths, eg **('/classRosters')**

 - Feel free to use Fetch, Axios, or the request library of your choosing

 - Please talk us through your reasoning and if you're unsure of something ask questions

## Example Usage

  

```javascript

async function notifyWaitlistMembers(classId) {

// Solution goes here

}

  

// Usage example:

// notifyWaitlistMembers("pit-class-001");

```

  