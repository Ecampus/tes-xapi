

## Attendance Recipe

### What Are You Attending?

We're using a "meeting" as the object in the example statements below, but the object could be *anything* people could attend.

There are a few [activity types](https://registry.tincanapi.com/#home/activityTypes) in the registry people could attend.

Here are some **examples**:

activity type         | uri
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

> **TODO** based on NOTE from Andrew: Do we need opening and closing in a bare minimum version. It'd be nice for systems that don't do detailed tracking to be able to say in 1 statement that Joe, Bob and Sue had a meeting lasting 1 hour covering X topic.

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
	}
}
```

## Detailed Attendance

Sometimes you may want to record things in more detail.

### Summary

Attendance at a particular meeting, tutorial session, or whatever it may be, would involve a series of statements related to that meeting, tutorial session or whatever it may be. For example, a statement might say:

	Kylie joined the meeting
	
or

	Jeff closed the meeting

All statements like this about a particular meeting, tutorial session, or whatever it may be, would all **refer to a common object id**. E.g.

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

Here's a table of all the statements this recipe covers (see "Discussion" below for details):

Actor                       | Verb                                                             | Object                
----------------------------|------------------------------------------------------------------|------------------------
Agent who initialised       | initialize: [ref](http://adlnet.gov/expapi/verbs/initialized)    | what is being attended 
Agent who opened object     | opened: [ref](http://activitystrea.ms/schema/1.0/open)           | as above               
Attendee who registered     | registered: [ref](http://adlnet.gov/expapi/verbs/registered)     | as above               
Attendee who unregistered   | unregistered: [ref](http://id.tincanapi.com/verb/unregistered)   | as above               
Agent who started timer     | started: [ref](http://activitystrea.ms/schema/1.0/start)         | as above               
Attendee who joined         | joined: [ref](http://activitystrea.ms/schema/1.0/join)           | as above               
Attendee who left           | left: [ref](http://activitystrea.ms/schema/1.0/leave)            | as above               
Agent who paused timer      | paused: [ref](http://id.tincanapi.com/verb/paused)               | as above               
Agent who resumed timer     | resume: [ref](http://adlnet.gov/expapi/verbs/resumed)            | as above               
Agent who commented         | comment: [ref](http://adlnet.gov/expapi/verbs/commented)         | as above               
Agent who closed attendance | closed: [ref](http://activitystrea.ms/schema/1.0/close)          | as above   

The Tincan [registry](https://registry.tincanapi.com/#home/verbs) provides the complete list of xAPI verbs from which these were derived.

Other verbs are beyond the scope of this recipe: 

- **scheduling and invitations** with verbs such as assigned, Scheduled, acknowledged, rsvped yes, RSVPed maybe, rsvped no, invited, received, opened, confirmed, and so on;
- **commentary** on what is being attended, e.g. statements using the commented verb;
- **sharing agenda and minutes** before and after what is to be or was attended;
- **results** of attendance.

> NOTE: link to other recipes here.

### Discussion

#### Who Will Be Attending?

Optionally, **register** and **unregister** people to add them or remove from the list of people who *might* attend.

	Kylie registered for the meeting
	Tim unregistered from the meeting

You can send **multiple register and unregister statements** for specific attendees **over time**. This allows you to **alter** the list of who will or might be attending over time.

Note: the list of people who registered as possibly attending is *distinct from* the eventual list of people who *actually* attended - see "Who Actually Attended" below.

*If the people who will be attending are not being decided in advance at all, registered and unregistered statements are unnecessary.*

#### Opening

Optionally, **open** the meeting.

	Jeff opened the meeting.

#### Who Actually Attended

You can record who *actually* attended or didn't attend.

	Kylie joined the meeting
	Kylie left the meeting
	Kylie joined the meeting

You can send *multiple* **joined** and **left** statements over time for attendees. This allows you to record comings and goings. This would be analogous to some sort of "sign in" and "sign out" paperwork.

#### Timing Attendance as Whole

Optionally, you can **time attendance** for the meeting as a whole.

	Jeff started the meeting

You can **pause** timing of the attendance:

	Jeff paused the meeting

And **resume** timing:

	Jeff resumed the meeting
	
You can **pause** and **resume** as many times as you wish.

#### Closing

Once what is being attended is over, you can **close** it.

	Jeff closed the meeting

### Very Few Workflow Assumptions

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
	Jeff started the meeting
	Kylie joined the meeting
	Ken joined the meeting
	Kylie left the meeting
	Jeff paused the meeting

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

### Result -- flagged for removal

If the meeting has a "result" then you can record that in a **closed** statement.

```js
"result": {
   "extensions": {
       "http://example.com/profiles/meetings/resultextensions/minuteslocation": "X:\\meetings\\minutes\\examplemeeting.one"
   },
   "success": true,
   "completion": true,
   "response": "We agreed on some example actions.",
   "duration": "PT1H0M0S"
}
```

### Example Statement

#### Preliminary Notes

##### Authority & context.instructor

The "authority" can be any "agent" (e.g. user, group, system).

Usually the instructor will be the person (or system) recording attendance.

In this example *Jeff* is the instructor.

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

Often the instructor will *also* be the authority.

Sometimes, you may find the instructor is not the person logged in and recording attendance. (You might have an instructor or group of instructors running the session and somebody else taking attendance for example.)

To note this you will need to add an context.observer agent to the context the statement.

```js
"context": {
	...
	"extensions": { 
	           "observer iri required": {
	           	"name": "Ken Admin", 
	           	"id" : "83220327-7608-412e-a4e9-f0f2d6a933ce"
	           }
	       },
```

> NOTE: unsure about the word "observer", but we'll need something for this situation.

##### context.category 

**All** statements include the **Recipe ID** in the "category" context activity list. (See [spec](https://github.com/adlnet/xAPI-Spec/blob/master/xAPI.md#4162-contextactivities-property)) and [here](http://tincanapi.com/recipeshow-it-works/) and [here](https://registry.tincanapi.com/#uri/activityType/289).

```js
"context": {
   "registration": "ec531277-b57b-4c15-8d91-d292c5b2b8f7", // session, instance id, etc 
   "contextActivities": {
	  ...
       "category": [ // this is where the Recipe ID goes
           {
               "id": "IRI of attendance recipe",
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

##### context.registration

See [spec](https://github.com/adlnet/xAPI-Spec/blob/master/xAPI.md#4161-registration-property). 

Relates to the instance of the attendance.

##### context.team (optional)

Optionally, statements include a Group for the attendees being observed, as the context "**team**" property. (see [spec](https://github.com/adlnet/xAPI-Spec/blob/master/xAPI.md#416-context)).

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

#### Example

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
        "registration": "ec531277-b57b-4c15-8d91-d292c5b2b8f7", // session id, instance id ... etc
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
                    "id": "IRI of attendance recipe",
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
           "observer iri required": {
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
                "room iri required": {
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

