#iManage.it

Web based project management tool 


#### Constants

===

**Project Status**

```
{
IN_PROGRESS: 0,
COMPLETED: 1,
ON_HOLD: 2
ABANDONED: 3,
}
```

**Task Status**

```
{
NOT_STARTED: 0,
COMPLETED: 1,
IN_PROGRESS: 2,
WAITING: 3,
CANCELED: 4
}
```

**Feature Request Status**

```
{
NEW: 0,
REJECTED: 1,
ACCEPTED: 2,
DELAYED: 3,
TESTING: 4,
IMPLEMENTED: 5
}
```

**Feature Request Priority**

```
{
LOW: -1,
NORMAL: 0,
HIGH: 1
}
```

#### Data Models

===

**User**

```
{
type: 'UserType',
createdOn: 'unix-time',
createdBy: '{username}',
firstName: '',
lastName: '',
gender: -1|0|1,
notes: '',
dateOfBirth: 'unix-time',
role: '',
email: '',
enabled: true,
contact: {
	address: '',
	state: '',
	country: '',
	zip: '',
	city: '',
	fax: '',
	phone: '',
	mobile: ''
	},
	social: 
	{
		facebook: '',
		twitter: '',
		skype: '',
		jabber: '',
		website: '',
		github: '',
		linkedIn: ''
	}
}
```

**Timeline**

```
{
type: 'TimelineType',
createdOn: 'unix-time',
createdBy: '{username}',
projectId: 0,
notes: ''
}
```


**Project**

```
{
type: 'ProjectType',
createdOn: 'unix-time',
createdBy: '{username}'
deadline: 'unix-time',
completedOn: 'unix-time',
client: ['{client_id}'],
budget: 0,
title: '',
description: '',
team: [],
url: '',
repository: 
	{
		type: 'svn|git'
		username: '', 
		password: '', 
		url: ''
	},
hasDeadline: false,
commitMonitor: false
}
```

**Milestone**

```
{
type: 'MilestoneType',
projectId: '',
name: '',
description: '',
startsOn: '',
endsOn: ''
}
```

**Task**

```
{
type: 'TaskType',
createdOn: 'unix-time',
createdBy: '{username}'
milestoneId: '',
name: '',
description: '',
status: 0,
completedOn: '',
startsOn: '',
endsOn: '',
verified: {
	us: 
	{
		username: '{username}',
		when: 'unix-time'
	},
	client: 
	{
		username: '{username}',
		when: 'unix-time'
	}
},
priority: 0,
team: []
}
```

**Feature Request**

```
{
type: 'FeatureRequestType',
createdOn: 'unix-time',
createdBy: '{username}'
projectId: '',
title: '',
content: '',
priority: 0,
status: 0,
expectedOn: 'unix-time',
implementedOn: 'unix-time'
}
```

**Client**

```
{
type: 'ClientType',
createdOn: 'unix-time',
createdBy: '{username}',
name: '',
companyName: '',
team: [],
contact: {
	address: '',
	state: '',
	ountry: '',
	zip: '',
	city: '',
	fax: '',
	phone: '',
	mobile: '',
	email: ''
	},
social: 
	{
		facebook: '',
		twitter: '',
		skype: '',
		jabber: '',
		website: '',
		github: '',
		linkedIn: ''
	},
notes: ''
}
```

**File**

```
{
type: 'FileType',
createdOn: 'unix-time',
createdBy: '{username}',
projectId: '',
notes: ''
}
```

**Note**

```
{
type: 'NoteType',
createdOn: 'unix-time',
createdBy: '{username}'
projectId: '',
notes: ''
}
```


## HTTP Responses

As defined on [RFC 2616](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html)
- - -


**200 OK**

The request has succeeded

**400 Bad Request**

The request could not be understood by the server due to malformed syntax

**401 Unauthorized**

The request requires user authentication.

**402 Payment Required**

Reserved for future use

**403 Forbidden**

The server understood the request, but is refusing to fulfill it

**404 Not Found**

The server has not found anything matching the Request-URI

**405 Method Not Allowed**

The method specified is not allowed for the resource identified by the Request-URI

**500 Internal Server Error**

The server encountered an unexpected condition which prevented it from fulfilling the request.

**501 Not Implemented**

The server does not support the functionality required to fulfill the request.

**503 Service Unavailable**

The server is currently unable to handle the request due to a temporary overloading or maintenance of the server


##HTTP Methods used

```
GET /projects - Retrieves a list of project

GET /projects/12 - Retrieves a specific project

POST /projects - Creates a new project

PUT /projects/12 - Updates project #12

PATCH /projects/12 - Partially updates project #12

DELETE /projects/12 - Deletes project #12
```

**Query String Parameters**

Data paging for requests that return array of results uses _bookmark_ and _limit_ that defines size of the data page. By default it is limited to 20 rows and you can use values from 1 to 100 rows.

```
GET /timeline?bookmark={1..n}&limit={1..100}
```

You can search within iManage.it storage by adding _q_ parameter into your query string, for example:

```
GET /projects?q="My Project"
```

You can also perform sorting on results to be returned by adding _sort_ field into your query string:

```
GET /projects?sort="-createdOn"
```

For more information on sorting and pagination, please refer to [IBM Cloudant Search Indexes](https://cloudant.com/for-developers/search/) documentation. 


##API Responses

Becide HTTP standard responses, there are also two types of API responses server will reply on requests  received.

Successful operation response. _result_ field can be of any data type

```
{
"type": "result",
"result": {}
}
```

Responses that return list of rows, _result_ would be be in the following form:

```
{
"type": "result",
"result": {
		"bookmark": ""
		"totalRows": 0..n,
		"rows": [...]
	}
}
```

Failed operation response. _result_ field usually is the error message returned by server

```
{
"type": "error",
"result": {}
}
```


##API endpoints

### Search

Searching within entire application

```
GET /search?query={...}
```

In current version of the API Search API is disabled

- - - 

###  Timeline

**Retreive timeline records**

Getting list of records

```
GET /timeline
```

Response

```
{
type: 'result',
result: {}
}
```

Retreive single timeline record by _id_

```
GET /timeline/{id}
```

Response

```
{
type: 'result',
result: {...}
}
```

**Create new timeline record**

```
POST /timeline
```

Request body. If _projectId_ field is _null_ it will create a timeline post not related to a project. Those timeline posts visible to everyone within the application.

```
{
createdOn: 'unix-time',
createdBy: '{username}',
projectId: 0,
notes: ''
}
```

Response

```
{
type: 'result',
result: ...
}
```

**Deleting timeline records**

Delete timeline record by _id_

```
DELETE /timeline/{id}
```

- - - 


### Clients

Client within iManage.it is a person or a company that can have one or more projects in management. All updates on tasks, notes, files and other related data from the project visible to to a Client.

**Retreive Clients**

Get all clients registered within iManage.it. Will return an array of Client Type objects, or empty array if no clients defined.

```
GET /clients
```

Response

```
{
type: 'result',
result: {...}
}
```

**Create new Client profile**


```
POST /clients
```
Request body

```
{
name: '',
companyName: '',
team: [],
contact: {
	address: '',
	state: '',
	ountry: '',
	zip: '',
	city: '',
	fax: '',
	phone: '',
	mobile: '',
	email: ''
	},
social: 
	{
		facebook: '',
		twitter: '',
		skype: '',
		jabber: '',
		website: '',
		github: '',
		linkedIn: ''
	},
notes: ''
}
```

Response

```
{
type: 'result',
result: ...
}
```

**Get client profile by _client_id_**

```
GET /clients/{client_id}
```

Get assigned users for the client profile. Each client can have none or many users

```
GET /clients/team
```

### Projects

**Retreive projects**

```
GET /projects
```

Returns array of objects of Project type

```
[]
```

**Create new project**

```
POST /projects
```

Request body

```
{
deadline: 'unix-time',
completedOn: 'unix-time',
client: ['{client_id}'],
budget: 0,
title: '',
team: [],
description: '',
url: '',
repository: 
	{
		type: 'svn|git'
		username: '', 
		password: '', 
		url: ''
	},
hasDeadline: false,
commitMonitor: false
}
```

Result

```
{
type: 'result'
result: []
}
```


### Pins


Get all pins

```
GET /pins
```

Get all pins for the selected _username_

```
/pins/{username}
```
