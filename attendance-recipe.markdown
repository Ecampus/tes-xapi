

# Attendance Recipe 0.0.1 

This recipe defines the structure and terms used in Statements intended to record the experience of 
attending something. For comments or suggestions on this recipe please email support+recipes@tincanapi.com.

This recipe version is 0.0.1.

## What Are You Attending?

This recipe defines a number of Activity Types of event that can be used, but the object could be *anything* people could attend. Where a type of event is listed in this recipe, you should use the identifier listed below. If the type of event is not listed in the table below, please refer to [The Registry](https://registry.tincanapi.com/) for other Activity types or [sign up](https://registry.tincanapi.com/#signUp) to create a new Activity Type if the event type you need is not listed. 

The Activity Types defined in this recipe are listed below:

activity type         | IRI
----------------------|-----
meeting               | [http://adlnet.gov/expapi/activities/meeting](http://adlnet.gov/expapi/activities/meeting)
game                  | [http://activitystrea.ms/schema/1.0/game](http://activitystrea.ms/schema/1.0/game)
place                 | [http://activitystrea.ms/schema/1.0/place](http://activitystrea.ms/schema/1.0/place)
conference            | [http://id.tincanapi.com/activitytype/conference](http://id.tincanapi.com/activitytype/conference)
conference session    | [http://id.tincanapi.com/activitytype/conference-session](http://id.tincanapi.com/activitytype/conference-session)
tutor session         | [http://id.tincanapi.com/activitytype/tutor-session](http://id.tincanapi.com/activitytype/tutor-session)
event                 | [http://activitystrea.ms/schema/1.0/event](http://activitystrea.ms/schema/1.0/event)

The "event" Activity Type should only be used when the type of event is **unspecified**. For example a mobile 
application might ask learners to select the event type from a drop down of options; 
```http://activitystrea.ms/schema/1.0/event``` could be used to represent an 'other' option in this dropdown. 

Here's an example Activity object representing a **meeting**:

```js
"object": {
   "id": "http://www.example.com/attendance/34534",
   "definition": {
       "name": {
           "en-GB": "example meeting",
           "en-US": "example meeting"
       },
       "description": {
           "en-GB": "An example meeting that happened on a specific occasion with certain people present.",
           "en-US": "An example meeting that happened on a specific occasion with certain people present."
       },
       "type": "http://adlnet.gov/expapi/activities/meeting",
   },
   "objectType": "Activity"
}
```

## Simple Attendance

This recipe defines two levels of attendance tracking: simple and detailed. Use Simple Attendance tracking when you only wants to record that some people attended an event. Use Detailed Attendance tracking to record more detail about attendance at the event. Recipe adopters are encouraged to implement either Simple Attendance **or** Simple Attendance *and* Detailed Attendance. Detailed attendance is not intended to be used on its own. 

Simple Attendance is described in this section. Detailed Attendance is described below. 

Simple Attendance is used if you want to record something like this:

    Jeff, Kylie and Tim attended the meeting

You can use a single statement using the [attended](http://adlnet.gov/expapi/verbs/attended) verb to do this.

### Required Properties

- **actor**: a **group** of attendees attending the event;
- **verb**: [http://adlnet.gov/expapi/verbs/attended](http://adlnet.gov/expapi/verbs/attended)
- **object**: what is being attended;
- **timestamp**: always represents the start of the event.

### Optional Properties

- **result.duration**: can optionally be used to record the length of the event. 
- **result.response**: can optionally be used to record a plain text summary of the end of the event. 

### Example

```js
{
    "actor": {
       "name": "Example Group",
        "account" : {
            "homePage" : "https://example.com",
            "name" : "7ce61a81-c95b-4e07-8355-266b53f29a7f"
       },
       "member": [
           {
               "name": "Jeff Sampson",
               "account": {
                   "homePage": "https://www.example.com",
                   "name": "07f2c03a-8d2e-4d85-80c0-fd584a500bde"
               },
               "objectType": "Agent"
           },
           {
               "name": "Kylie Holroyd",
               "account": {
                   "homePage": "https://www.example.com",
                   "name": "f4dbfd19-f437-4920-b206-046684d02f23"
               },
               "objectType": "Agent"
           },
           {
               "name": "Tim McKeefe",
               "account": {
                   "homePage": "https://www.example.com",
                   "name": "a75afa2d-2899-40fc-8a29-8a1d027a2d4c"
               },
               "objectType": "Agent"
           },
       ],
       "objectType": "Group"
    },
    "verb": {
        "id": "http://adlnet.gov/expapi/verbs/attended",
        "display": {
            "en-US": "attended"
        }
    },
    "object": {
       "id": "http://www.example.com/attendance/34534",
       "definition": {
           "name": {
               "en-GB": "example meeting",
               "en-US": "example meeting"
           },
           "description": {
               "en-GB": "An example meeting that happened on a specific occasion with certain people present.",
               "en-US": "An example meeting that happened on a specific occasion with certain people present."
           },
           "type": "http://adlnet.gov/expapi/activities/meeting"
       },
       "objectType": "Activity"
    },
    "result": {
       "success": true,
       "completion": true,
       "response": "We agreed on some example actions.",
       "duration": "PT1H0M0S"
    },
    "context": {
        "contextActivities": {
            "category": [
                {
                    "id": "http://id.tincanapi.com/recipe/attendance/base/0_0_1",
                    "definition": {
                        "type": "http://id.tincanapi.com/activitytype/recipe"
                    },
                    "objectType": "Activity"
                }
            ]
        }
    }
}
```

### Notes

Activity Providers can issue **multiple statements** with the same timestamp and Activity Id to refer to the same event, updating the duration with each statement. Then the statement with the largest result.duration is considered to be the final duration and definitive statement about the event.

The **statement with the longest result.duration is considered to be the definitive statement**. The result.response of that statement is also considered to be the *final* plain text summary of the event. Other statement's result.response properties are considered to be *drafts*.

Activity Providers should *not* issue 'attended' statements with the same Activity Id and *different* timestamps. The statements would combine to create conflicting data about the start time of the event.

**Consider using Detailed Attendance tracking if this approach to duration and start time tracking is insufficient.**

## Detailed Attendance

Detailed Attendance tracking is used to provide more detail.

Here's a table of all the statements covered by Detailed Attendance tracking (see "Statements" below for details):

Actor                                     | Verb                                                             | Object                 | required 
------------------------------------------|------------------------------------------------------------------|------------------------|--------------------
Agent who scheduled the object            | scheduled: [ref](http://activitystrea.ms/schema/1.0/schedule)    | what is being attended | optional
Agent who opened object                   | opened: [ref](http://activitystrea.ms/schema/1.0/open)           | as above               | required
Attendee who registered for the object    | registered: [ref](http://adlnet.gov/expapi/verbs/registered)     | as above               | optional
Attendee who unregistered from the object | unregistered: [ref](http://id.tincanapi.com/verb/unregistered)   | as above               | optional    
Attendee who joined the object            | joined: [ref](http://activitystrea.ms/schema/1.0/join)           | as above               | required
Attendee who left the object              | left: [ref](http://activitystrea.ms/schema/1.0/leave)            | as above               | optional
Agent who adjourned the object            | adjourned: [ref](http://id.tincanapi.com/verb/adjourned)         | as above               | optional
Agent who resumed the object              | resumed: [ref](http://adlnet.gov/expapi/verbs/resumed)           | as above               | optional      
Agent who closed the object               | closed: [ref](http://activitystrea.ms/schema/1.0/close)          | as above               | required

Some attendance things are beyond the scope of this recipe:

- **detailed attendee management** such as invitations, RSVPs etc; creation of events and registration are in scope.
- **commentary** on what is being attended, e.g. statements using the commented verb.
- **sharing agenda and minutes** before and after what is to be or was attended.

These will be covered by additional recipes as required. 

### Event Work Flow

Not many assumptions about work flow are made. We are just trying to record what actually happened; who was listed as being a possible attendee, who attended and when, who came and went, etc. 
    
The bare minimum:

    Jeff opened the meeting
    Jeff closed the meeting

And with some attendees joining the meeting:

    Jeff opened the meeting
    Kylie joined the meeting
    Ken joined the meeting
    
... some time later ...
    
    Jeff closed the meeting
    
Or, perhaps with some timing:
    
    Jeff opened the meeting
    Kylie joined the meeting
    Ken joined the meeting
    Kylie left the meeting
    Jeff adjourned the meeting

... some time later ...
    
    Kylie joined the meeting
    Jeff resumed the meeting
    
... some time later ...
    
    Jeff closed the meeting
    
Or with registration:

    Kylie registered for the meeting
    Ken registered for the meeting
    Jeff opened the meeting
    Kylie joined the meeting
    Ken joined the meeting
    Jeff closed the meeting

Events should have timestamp properties that indicate a logical series.

- Events must be opened before they can be adjourned or closed;
- Events must be adjourned before they can be resumed;
- Attendees can join and leave events at any time;
- Events can be opened after being closed (the event was closed, but was started again for some reason).
- Some events might be adjourned and never resume (perhaps the intention was to resume the event but it not resumed for some reason);
- Adjourned events can be closed and closed events can be adjourned to indicate that the intention is now to resume the event later.

Although missing statements are not recommended (e.g. an event that has no 'opened' statement), statement consumers should design for this possibility. 

### Statements

#### Scheduled (optional)

    Ken scheduled the meeting

For example, an admin might do this by adding a meeting to a calendar, for instance, or by scheduling a training session.

**Verb**: [http://activitystrea.ms/schema/1.0/schedule](http://activitystrea.ms/schema/1.0/schedule) 

---

#### Registered & Unregistered (optional)

The actor property of the Registered and Unregistered statements is the *person who registered for or unregistered from the event*. It's analogous to a person (the actor) filling his or her own name on a form to register on unregister for a meeting, conference or whatever it may be. In this case Kylie (the actor) is registering to attend the meeting:

    Kylie Attendee registered for the meeting.

Optionally, you can send **multiple register and unregister statements** for specific attendees **over time**. This allows you to **alter** the list of who *might* be attending over time.

The list of people who registered as *possibly* attending is *distinct from* the eventual list of people who *actually* attended - see "Joined & Left" below.

*If the people who will be attending are not being decided in advance, registered and unregistered statements are unnecessary.*

More complex scenarios - invites, RSVPS, etc - will be covered by additional recipes as required.

##### Registered (optional)

    Kylie registered for the meeting

A person can **register** as planning to attend -- this adds the person to the list of people who *might* attend.

**Verb**: [http://adlnet.gov/expapi/verbs/registered](http://adlnet.gov/expapi/verbs/registered)

##### Unregistered (optional)

    Tim unregistered from the meeting

A person can **unregister** as planning to attend -- this removes the person from the list of people who *might* attend.

**Verb**: [http://id.tincanapi.com/verb/unregistered](http://id.tincanapi.com/verb/unregistered)

---

#### Opened (optional)

    Jeff opened the meeting

Optionally, **open** the meeting. You do this when you start proceedings. You only open the meeting once.

(If more than one opened statement is received, the most recent one is considered to be be the definitive one.)

**Verb**: [http://activitystrea.ms/schema/1.0/open](http://activitystrea.ms/schema/1.0/open) 

---

#### Joined & Left

    Kylie joined the meeting
    Kylie left the meeting
    Kylie joined the meeting

You can send *multiple* **joined** and **left** statements over time for attendees. This allows you to record comings and goings. This would be analogous to some sort of "sign in" and "sign out" paperwork.

##### Joined (required)

    Kylie joined the meeting

State who joined what is being attended.

**Verb**: [http://activitystrea.ms/schema/1.0/join](http://activitystrea.ms/schema/1.0/join)

##### Left (optional)

    Kylie left the meeting

State who left what is being attended.

**Verb**: [http://activitystrea.ms/schema/1.0/leave](http://activitystrea.ms/schema/1.0/leave) 

---

#### Adjourned & Resumed (optional)

    Jeff adjourned the meeting
    Jeff resumed the meeting

Optionally, you can **time attendance** for the meeting as a whole. A timer would start when the meeting is *opened*. You can sned as many adjourned and resumed statements as you wish. This is analogous to starting a timer and pausing it.

##### Adjourned (optional)

    Jeff adjourned the meeting

Adjourn the meeting, with the the intention of resuming it later.

**Verb**: [adjourned - IRI REQUIRED](IRI REQUIRED)

##### Resumed (optional)

    Jeff adjourned the meeting

Resume what is being attended.

**Verb**: [http://adlnet.gov/expapi/verbs/resumed](http://adlnet.gov/expapi/verbs/resumed)  

---

#### Closed (required)

    Jeff closed the meeting

Once what is being attended is over, you can **close** it.

**Verb**: [http://activitystrea.ms/schema/1.0/close](http://activitystrea.ms/schema/1.0/close) 

##### Closed Statement Properties

**result**

This is where you record the "result" of the attendance. For example, the result of a meeting:

```js
"result": {
   "success": true,
   "completion": true,
   "response": "We agreed on some example actions.",
   "duration": "PT1H0M0S"
}
```

## Properties that Apply to All Statements
The properties in this section apply to all statements in both Simple and Detailed attendance tracking. 

### object.id (required)

Attendance at a particular meeting, tutorial session, or whatever it may be, would involve a series of statements related to that meeting, tutorial session or whatever it may be. For example, a statement might say:

    Kylie joined the meeting
    
or

    Jeff closed the meeting

All statements like this about a particular meeting, tutorial session, or whatever it may be, all **refer to a common Activity Id**. This is extremely important as systems reading statements will regard statements using different 
Activity Ids as distinct events, even if all other data points are identical. 

E.g.

```js
...
"object": {
   "id": "http://www.example.com/attendance/34534", // all statements about this particular meeting use this id
   "definition": {
       "name": {
           "en-GB": "example meeting",
           "en-US": "example meeting"
       },
       "description": {
           "en-GB": "An example meeting that happened on a specific occasion with certain people present.",
           "en-US": "An example meeting that happened on a specific occasion with certain people present."
       },
       "type": "http://adlnet.gov/expapi/activities/meeting"
   },
   "objectType": "Activity"
}
...
```

### context.category (required)

**All** statements include the **Recipe Id** in the "category" context activity list. (See [spec](https://github.com/adlnet/xAPI-Spec/blob/master/xAPI.md#4162-contextactivities-property)) and [here](http://tincanapi.com/recipeshow-it-works/) and [here](https://registry.tincanapi.com/#uri/activityType/289).

```js
"context": {
   ...
   "contextActivities": {
      ...
        "category": [
            {
                "id": "http://id.tincanapi.com/recipe/attendance/base/0_0_1",
                "definition": {
                    "type": "http://id.tincanapi.com/activitytype/recipe"
                },
                "objectType": "Activity"
            }
        ]
       ]
   }
...
```

### authority, context.instructor, context.extensions.http://id.tincanapi.com/extension/observer (optional)

The "authority" can be any "agent" (e.g. user, group, system). Usually the instructor will be the person 
(or system) recording attendance.

In this example *Jeff* is the instructor:

```js
"context": {
    ...
        "instructor":
        {
            "name": "Jeff Sampson",
            "account": {
                "homePage": "http://www.example.com",
                "name": "c531277-b57b-4c15-8d91-d292c5b2b8f7"
            },
            "objectType": "Agent"
        },
    ...
```

If the instructor is the person recording the attendance, the instructor may *also* be the **authority** for 
the statement. 

Sometimes the instructor is *not* the person recording attendance. (For example, you might 
have an instructor or group of instructors running the session and somebody else taking attendance.)

To record this you will need to add a **context.observer** agent (person, group or system). This is the 
agent *observing* what is being attended and recording when it opened, who joined, who left, when it 
closed and so on.

```js
"context": {
    ...
    "extensions": { 
         "http://id.tincanapi.com/extension/observer": {
            "name": "Ken Admin",
            "account": {
                "homePage": "http://www.example.com",
                "name": "83220327-7608-412e-a4e9-f0f2d6a933ce"
            },
            "objectType": "Agent"
        }
     }
```

### context.team (optional)

Optionally, statements can include a Group for the attendees being observed, as the context "**team**" property. (see [spec](https://github.com/adlnet/xAPI-Spec/blob/master/xAPI.md#416-context)).

(This can include all the attendees or just refer to group by ID or name if there are a lot of attendees?)

```js
"context": {
    ...
       "team":
        {
            "name": "Example Group",
            "account" : {
               "homePage" : "https://example.com",
               "name" : "7ce61a81-c95b-4e07-8355-266b53f29a7f"
            },
            "member": [ 
              {
                  "name": "Jeff Sampson",
                  "account": {
                      "homePage": "https://www.example.com",
                      "name": "07f2c03a-8d2e-4d85-80c0-fd584a500bde"
                  },
                  "objectType": "Agent"
              },
              {
                  "name": "Dee Holroyd",
                  "account": {
                      "homePage": "https://www.example.com",
                      "name": "f4dbfd19-f437-4920-b206-046684d02f23"
                  },
                  "objectType": "Agent"
              },
              {
                  "name": "David McKeefe",
                  "account": {
                      "homePage": "https://www.example.com",
                      "name": "a75afa2d-2899-40fc-8a29-8a1d027a2d4c"
                  },
                  "objectType": "Agent"
              },
            ],
            "objectType": "Group"
        },
    ...
```

### Example Statement

Here's a complete example of an entire statement. In this case an "opened" statement:

```js
{
    "id": "6690e6c9-3ef0-4ed3-8b37-7f3964730bee",
    "actor": {
        "name": "Jeff Sampson",
        "account" : {
            "homePage" : "http://example.com",
            "name" : "3f5fc6e0-8562-46af-b623-83a212a28b5d"
        },
       "objectType": "Agent"
    },
    "verb": {
        "id": "http://activitystrea.ms/schema/1.0/open",
        "display": {
            "en-GB": "opened",
            "en-US": "opened"
        }
    },
    "context": {
        "contextActivities": {
            "parent": [
                {
                    "id": "http://www.example.com/someactivity", 
                    "objectType": "Activity"
                }
            ],
            "category": [
                {
                    "id": "IRI REQUIRED of attendance recipe",
                    "objectType": "Activity",
                    "definition": {
                        "name": {
                            "en": "A recipe used for attendance"
                        },
                        "description": {
                            "en": "A recipe used for attendance"
                        },
                        "type": "http://example.com/expapi/profiles/attendance"
                    }
                }
            ]
        },
        "instructor":
        {
            "name": "Jeff Sampson",
            "account": {
                "homePage": "http://www.example.com",
                "name": "c531277-b57b-4c15-8d91-d292c5b2b8f7"
            },
            "objectType": "Agent"
        },
        "team":
        {
            "name": "Example Group",
            "account" : {
               "homePage" : "https://example.com",
               "name" : "7ce61a81-c95b-4e07-8355-266b53f29a7f"
            },
            "member": [ 
              {
                  "name": "Jeff Sampson",
                  "account": {
                      "homePage": "https://www.example.com",
                      "name": "07f2c03a-8d2e-4d85-80c0-fd584a500bde"
                  },
                  "objectType": "Agent"
              },
              {
                  "name": "Dee Holroyd",
                  "account": {
                      "homePage": "https://www.example.com",
                      "name": "f4dbfd19-f437-4920-b206-046684d02f23"
                  },
                  "objectType": "Agent"
              },
              {
                  "name": "David McKeefe",
                  "account": {
                      "homePage": "https://www.example.com",
                      "name": "a75afa2d-2899-40fc-8a29-8a1d027a2d4c"
                  },
                  "objectType": "Agent"
              },
            ],
            "objectType": "Group"
        },
       "extensions": { 
        // here's where additional data about the activity can go
        // e.g. where oauth is not used, the observer is the person using the app to record attendance 
        // see "Who Or What is Actually Recording Attendance"
        "http://id.tincanapi.com/extension/observer": {
            "name": "Ken Admin",
            "account": {
                "homePage": "http://www.example.com",
                "name": "83220327-7608-412e-a4e9-f0f2d6a933ce"
            },
            "objectType": "Agent"
        }
       },
    },
    "timestamp": "2013-05-18T05:32:34.804Z",
    "authority": {
        "account": {
            "homePage": "http://cloud.scorm.com/", 
            "name": "anonymous"
        },
        "objectType": "Agent"
    },
    "version": "1.0.0",
    "object": {
        "id": "http://www.example.com/meetings/occurances/34534",
        "definition": {
            "name": {
                "en-GB": "example meeting",
                "en-US": "example meeting"
            },
            "description": {
                "en-GB": "An example meeting that happened on a specific occasion with certain people present.",
                "en-US": "An example meeting that happened on a specific occasion with certain people present."
            },
            "type": "http://adlnet.gov/expapi/activities/meeting",
        },
        "objectType": "Activity"
    }
}
```

