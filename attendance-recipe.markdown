

# Attendance Recipe

## What Are You Attending?

We're using a "meeting" as the object in the example statements below, but the object could be *anything* people could attend.

There are a few [activity types](https://registry.tincanapi.com/#home/activityTypes) in the registry people could attend.

Here are some **examples**:

activity type         | IRI
----------------------|-----
meeting               | [http://adlnet.gov/expapi/activities/meeting](http://adlnet.gov/expapi/activities/meeting)
game                  | [http://activitystrea.ms/schema/1.0/game](http://activitystrea.ms/schema/1.0/game)
place                 | [http://activitystrea.ms/schema/1.0/place](http://activitystrea.ms/schema/1.0/place)
Conference            | [http://id.tincanapi.com/activitytype/conference](http://id.tincanapi.com/activitytype/conference)
Conference Session    | [http://id.tincanapi.com/activitytype/conference-session](http://id.tincanapi.com/activitytype/conference-session)
tutor session         | [http://id.tincanapi.com/activitytype/tutor-session](http://id.tincanapi.com/activitytype/tutor-session)

... and so on. 

If you can't find one that covers your needs in the registry, you can **propose your own**. (Just [sign up](https://registry.tincanapi.com/#signUp) for the registry and make a suggestion.)

Here's an example of a **meeting**:

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

If what is being attended is **unspecified**, you can use the "event" activity type. (Note that this is a generic term and discouraged for other cases.)

## Simple Attendance

You might just want to record something like this:

	Jeff, Kylie and Tim attended the meeting

You can use a single statement using the [attended](http://adlnet.gov/expapi/verbs/attended) verb to do this. E.g.

```js
{
	"actor": {
	   "name": "Example Group",
	    "account" : {
	        "homePage" : "https://example.com",
	        "name" : "7ce61a81-c95b-4e07-8355-266b53f29a7f"
	   },
	   "member": [ // attendees
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
    "verb": { // attended verb
        "id": "http://adlnet.gov/expapi/verbs/attended",
        "display": {
            "en-US": "attended"
        }
    },
	"object": { // could be any suitable activity type, in this case meeting
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
	   "duration": "PT1H0M0S" // the duration of the meeting
	}
}
```

Sometimes a simple, "single statement" attendance will do. But in other cases you might want more detail.

## Detailed Attendance

Here's a table of all the statements this recipe covers (see "Statements" below for details):

Actor                                     | Verb                                                             | Object                 | required 
------------------------------------------|------------------------------------------------------------------|------------------------|--------------------
Agent who initialised the object          | initialized: [ref](http://adlnet.gov/expapi/verbs/initialized)   | what is being attended | optional
Agent who opened object                   | opened: [ref](http://activitystrea.ms/schema/1.0/open)           | as above               | required
Attendee who registered for the object    | registered: [ref](http://adlnet.gov/expapi/verbs/registered)     | as above               | optional
Attendee who unregistered from the object | unregistered: [ref](http://id.tincanapi.com/verb/unregistered)   | as above               | optional    
Attendee who joined the object            | joined: [ref](http://activitystrea.ms/schema/1.0/join)           | as above               | required
Attendee who left the object              | left: [ref](http://activitystrea.ms/schema/1.0/leave)            | as above               | optional
Agent who adjourned the object            | adjourned: IRI REQUIRED                                          | as above               | optional
Agent who resumed the object              | resumed: [ref](http://adlnet.gov/expapi/verbs/resumed)           | as above               | optional      
Agent who closed the object               | closed: [ref](http://activitystrea.ms/schema/1.0/close)          | as above               | required

The Tincan [registry](https://registry.tincanapi.com/#home/verbs) provides the complete list of xAPI verbs from which these were derived.

Other verbs are beyond the scope of this recipe: 

- **scheduling and invitations** with verbs such as assigned, Scheduled, acknowledged, rsvped yes, RSVPed maybe, rsvped no, invited, received, opened, confirmed, and so on;
- **commentary** on what is being attended, e.g. statements using the commented verb.

Other aspects beyond the scope of this recipe:

- **sharing agenda and minutes** before and after what is to be or was attended.

### Discussion

Not many assumptions about work flow are made, we are just trying to record what actually happened - who was listed as being a possible attendee, who attended and when, who came and went, etc.
	
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

### Statements

#### Initialized (optional)

	Ken initialized the meeting

For example, an admin might do this by adding a meeting to a calendar, for instance, or by scheduling a training session.

**Verb**: [http://adlnet.gov/expapi/verbs/initialized](http://adlnet.gov/expapi/verbs/initialized) 

---

#### Registered & Unregistered (optional)

Optionally, you can send **multiple register and unregister statements** for specific attendees **over time**. This allows you to **alter** the list of who *might* be attending over time.

The list of people who registered as *possibly* attending is *distinct from* the eventual list of people who *actually* attended - see "Joined & Left" below.

*If the people who will be attending are not being decided in advance at all, registered and unregistered statements are unnecessary.*

##### Registered (optional)

	Kylie registered for the meeting

You can **register** people -- to add them to the list of people who *might* attend.

**Verb**: [http://adlnet.gov/expapi/verbs/registered](http://adlnet.gov/expapi/verbs/registered)

##### Unregistered (optional)

	Tim unregistered from the meeting

You can **unregister** people -- to remove from the list of people who *might* attend.

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

**Verb**: [adjourned - IRI REQUIRED](IRI REQUIRED), [http://adlnet.gov/expapi/verbs/resumed](http://adlnet.gov/expapi/verbs/resumed)  

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

### Properties that Apply to All Statements

#### authority, context.instructor, context.extensions.\<observer - IRI REQUIRED\> (optional)

The "authority" can be any "agent" (e.g. user, group, system). Usually the instructor will be the person (or system) recording attendance.

In this example *Jeff* is the instructor:

```js
"context": {
	...
        "instructor": // can be a single agent or a group of agents
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

Often the instructor will *also* be the **authority**.

Sometimes, you may find the instructor is *not* the person logged in and recording attendance. (For example, you might have an instructor or group of instructors running the session and somebody else taking attendance.)

To note this you will need to add a **context.observer** agent (person, group or system). This is the agent *observing* what is being attended and recording when it opened, who joined, who left, when it closed and so on.

```js
"context": {
	...
	"extensions": { 
	           "observer IRI REQUIRED": {
	           	"name": "Ken Admin", 
	           	"id" : "83220327-7608-412e-a4e9-f0f2d6a933ce"
	           }
	       },
```

#### object.id (required)

Attendance at a particular meeting, tutorial session, or whatever it may be, would involve a series of statements related to that meeting, tutorial session or whatever it may be. For example, a statement might say:

	Kylie joined the meeting
	
or

	Jeff closed the meeting

All statements like this about a particular meeting, tutorial session, or whatever it may be, would all **refer to a common Activity Id**. E.g.

```js
...
"object": { // could be any suitable activity type, in this case meeting
   "id": "http://www.example.com/attendance/34534", // all statements about this particular meeting refer to this meeting
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

#### context.category (required)

**All** statements include the **Recipe ID** in the "category" context activity list. (See [spec](https://github.com/adlnet/xAPI-Spec/blob/master/xAPI.md#4162-contextactivities-property)) and [here](http://tincanapi.com/recipeshow-it-works/) and [here](https://registry.tincanapi.com/#uri/activityType/289).

```js
"context": {
   ...
   "contextActivities": {
	  ...
       "category": [ // this is where the Recipe ID goes
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
...
```

#### context.team (optional)

Optionally, statements can include a Group for the attendees being observed, as the context "**team**" property. (see [spec](https://github.com/adlnet/xAPI-Spec/blob/master/xAPI.md#416-context)).

(This can include all the attendees or just refer to group by ID or name if there are a lot of attendees?)

```js
"context": {
	...
	   "team": // the attendees
        {
			"name": "Example Group",
			"account" : {
			   "homePage" : "https://example.com",
			   "name" : "7ce61a81-c95b-4e07-8355-266b53f29a7f" // id of group
			},
			"member": [ 
			  {
			      "name": "Jeff Sampson", // also the instructor
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
    "id": "6690e6c9-3ef0-4ed3-8b37-7f3964730bee", // id of the statement
	"actor": {
	    "name": "Jeff Sampson", // the person starting the "meeting"
	    "account" : {
	        "homePage" : "http://example.com",
	        "name" : "3f5fc6e0-8562-46af-b623-83a212a28b5d" // user id at example.com
	    },
	   "objectType": "Agent"
	},
    "verb": {
        "id": "http://activitystrea.ms/schema/1.0/open", // opened verb
        "display": {
            "en-GB": "opened",
            "en-US": "opened"
        }
    },
    "context": {
        "contextActivities": {
            "parent": [
                {
                	// the "meeting" is part of some wider activity such as a training event 
                    "id": "http://www.example.com/someactivity", 
                    "objectType": "Activity"
                }
            ],
            "category": [ // this is where the Recipe ID goes
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
        "instructor": // can be a single agent or a group of agents
        {
            "name": "Jeff Sampson",
            "account": {
                "homePage": "http://www.example.com",
                "name": "c531277-b57b-4c15-8d91-d292c5b2b8f7"
            },
            "objectType": "Agent"
        },
        "team": // the attendees
        {
			"name": "Example Group",
			"account" : {
			   "homePage" : "https://example.com",
			   "name" : "7ce61a81-c95b-4e07-8355-266b53f29a7f" // id of group
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
           "observer IRI REQUIRED": {
           	"name": "Ken Admin", 
           	"id" : "83220327-7608-412e-a4e9-f0f2d6a933ce"
           }
       },
    },
    "timestamp": "2013-05-18T05:32:34.804Z", // ISO 8601 timestamp
    "authority": {
        "account": {
        		// will end up being the credentials used to send the statement to
        		// the LRS, use OAuth if possible.
            "homePage": "http://cloud.scorm.com/", 
            "name": "anonymous"
        },
        "objectType": "Agent"
    },
    "version": "1.0.0",
    "object": {
        "id": "http://www.example.com/meetings/occurances/34534",
        "definition": {
            "extensions": { 
            	  // here's where additional info about what is being attended can go, 
                "room IRI REQUIRED": {
                	"name": "Kilby", 
                	"id" : "http://example.com/rooms/342"
                }
            },
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

