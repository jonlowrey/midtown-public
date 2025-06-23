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








  
  
  
  

## Example Solution

  

```javascript

async function notifyWaitlistMembers(classId) {

  try {

    // Get current class roster to check available seats

    const classRosters = await fetch('/classRosters').then(res => res.json());

    const targetClass = classRosters.find(roster => roster.classId === classId);

    if (!targetClass) {

      console.error(`Class with ID ${classId} not found`);

      return;

    }

    // Calculate available seats

    const currentEnrollment = targetClass.memberIds.length;

    const availableSeats = targetClass.capacity - currentEnrollment;

    if (availableSeats <= 0) {

      console.log(`No available seats in ${targetClass.className}`);

      return;

    }

    // Get waitlist for this class

    const waitlist = await fetch(`/waitlist/${classId}`).then(res => res.json());

    if (waitlist.members.length === 0) {

      console.log(`No members on waitlist for ${targetClass.className}`);

      return;

    }

    // Notify members up to the number of available seats

    const membersToNotify = waitlist.members.slice(0, availableSeats);

    const notificationPromises = membersToNotify.map(async (member) => {

      const notification = {

        message: `Great news! A seat has opened up in ${targetClass.className}. You can now enroll!`

      };

      try {

        await fetch(`/notification/${member.memberId}`, {

          method: 'POST',

          headers: {

            'Content-Type': 'application/json',

          },

          body: JSON.stringify(notification)

        });

        console.log(`Notification sent to member ${member.memberId} for ${targetClass.className}`);

      } catch (error) {

        console.error(`Failed to notify member ${member.memberId}:`, error);

      }

    });

    await Promise.all(notificationPromises);

  } catch (error) {

    console.error('Error in notifyWaitlistMembers:', error);

  }

}

  

// Usage example:

// notifyWaitlistMembers("pit-class-001");

```

  

### Key Design Decisions

  

1. **Error Handling**: Comprehensive try-catch blocks to handle API failures gracefully

2. **Seat Calculation**: Checks available capacity before sending notifications

3. **Batch Processing**: Uses `Promise.all()` to send multiple notifications concurrently

4. **Logging**: Provides feedback on operations for debugging and monitoring

5. **Boundary Checks**: Validates class exists and waitlist has members before proceeding
