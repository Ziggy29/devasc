# 1.0 Software Development and Design (15%)
## 1.1 Compare data formats (XML, JSON, and YAML)
### Why use Plain-Test Data Formats?
- Structured for computers
- Annotated for humans
- Open and extensible
- Self-describing
- Platform Agnostic/Lifespan

### XML (Extensible Markup Language)
- Tags: `<Tag></Tag>`
- Attributes: `Name = ""`
- Element: `<price>29.99</price>`
- Not as common with modern APIs
- Extremely verbose. More difficult to read and write.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<People>
    <Person Id="1">
        <FirstName>Benjamin</FirstName>
        <LastName>Finkel</LastName>
        <Email>ben.finkel@cbtnuggets.com</Email>
    </Person>

    <Person Id="2">
        <FirstName>Jane</FirstName>
        <LastName>Doe</LastName>
        <Email>jane.doe@cbtnuggets.com</Email>
    </Person>

    <Person Id="3">
        <FirstName>Pat</FirstName>
        <LastName>Smith</LastName>
        <Email>pat.smith@cbtnuggets.com</Email>
    </Person>
            
</People>
```
### JSON (JavaScript Object Notation)
- Great for serialized data – communication between APIs
- Fairly easy to read. A little more difficult to write correctly.
- Uses {} to denote objects, [] to denote arrays, etc. More “programmatical” in nature.
- JSON Example:

```json
{
    "people": [
        {
            "Id": "1",
            "FirstName": "Benjamin",
            "LastName": "Finkel",
            "Email": "ben.finkel@cbtnuggets.com"
        },
        {
            "Id": "2",
            "FirstName": "Jane",
            "LastName": "Doe",
            "Email": "jane.doe@cbtnuggets.com"
        },
        {
            "Id": "3",
            "FirstName": "Pat",
            "LastName": "Smith",
            "Email": "pat.smith@cbtnuggets.com"
        }
    ]
}
```
- `console.log(myObj[1].LastName)` = "Doe"

### YAML (YAML Ain't Markup Language)
- Great for configuration files
- Uses indentation to define data structure, similar to how Python structures code.
- Purposefully made to be easy for humans to read/write
- Allows commenting

YAML Example:
```yaml
---
people:
-  Id: 1
   Code: 524.37
   FirstName: Benjamin
   LastName: Finkel
   Email: ben.finkel@cbtnuggets.com
   Active: true
- Id: 2
  Code: 813.09
  FirstName: Jane
  LastName: Doe
  Email: jane.doe@cbtnuggets.com
  Active: true
- Id: 3
  Code: 102.9
  FirstName: Pat
  LastName: Smith
  Email: pat.smith@cbtnuggets.com
  Active: false
...
--- #This is the start of a new document in YAML
item1: item1value
item2: item2value
```
## 1.2 Describe parsing of common data format to Python data structures

### Parse YAML in Python
```python
import yaml
from yaml import load, load_all

stream = open('sample.yaml','r')
documents = load_all(stream, Loader=yaml.FullLoader)

print(type(documents))

for doc in documents:
    print(type(doc))

    print(doc['people'][1]['LastName'])
```

### Parse JSON in Python
```python
import json

jsonstr = """{"people":[{"Id":"1","FirstName":"Benjamin","LastName":"Finkel",
    "Email":"ben.finkel@cbtnuggets.com"},{"Id":"2","FirstName":"Jane","LastName":"Doe",
    "Email":"jane.doe@cbtnuggets.com"},{"Id":"3","FirstName":"Pat","LastName":"Smith",
    "Email":"pat.smith@cbtnuggets.com"}]}"""

jsonobj = json.loads(jsonstr)

print(jsonobj['people'][1])

jsonobj = json.load(open('sample.json'))

print(jsonobj['people'])
```

### Parse XML in Python
```python
from lxml import etree as ET

#Get the XML file data
stream = open('sample.xml','r')

#Parse the data into an ElementTree object
xml = ET.parse(stream)

#Get the 'root' Element object from the ElementTree
root = xml.getroot()

#Iterate through each child of the root Element
for e in root:
    #Print the stringified version of the element
    print(ET.tostring(e))
    print("")
    
    #Print the 'Id' attribute of the Element object
    print(e.get("Id"))
```
Output:
```
b'<Person Id="1">\n        <FirstName>Benjamin</FirstName>\n        <LastName>Finkel</LastName>\n        <Email>ben.finkel@cbtnuggets.com</Email>\n    </Person>\n\n    '

1
b'<Person Id="2">\n        <FirstName>Jane</FirstName>\n        <LastName>Doe</LastName>\n        <Email>jane.doe@cbtnuggets.com</Email>\n    </Person>\n\n    '

2
b'<Person Id="3">\n        <FirstName>Pat</FirstName>\n        <LastName>Smith</LastName>\n        <Email>pat.smith@cbtnuggets.com</Email>\n    </Person>\n            \n'

3
```
## 1.6 Identify the advantages of common design patterns (MVC and Observer)

### Why Patterns?
- People have been programming for a long time. And it’s very common for programmers to run into the same problems that other people have run into.
- So it’s beneficial to learn common design patterns so that you can use proven solutions instead of banging your head against the wall trying to solve design issues on your own.

### Singleton Design
A pattern that restricts classes to only being instantiated once during the execution of the program
- Database connection strings
- Configuration values
- Constants/state values

### Observer Design
The simplest way I can think of to describe the observer pattern is: What if you had an editable table of data on a screen, and to the left of that data you had a chart based on that data. Every time you update the data in the table you’d expect the graph to change right? That’s exactly the kind of problem that the Observer pattern is a solution for.

It quite literally “observes” an object that other objects rely on. And when that object’s state changes, the observer says “Hey, you over there, this object you rely on just changed.” Then those other objects can be update based on the object’s new state.

### Model-View-Controller

This is a pattern used in many languages – it is for the use of applications with a GUI.

- Model: Your data structures and potentially business logic. Though it may just be used to get data from a database.
- View: What your user sees
- Controller: The glue between your model and your view. It gets data from the model to be shown to the user, and sends data from the view to the model layer to be saved to databases etc.

One of the benefits of this pattern is the separation of responsibilities into logical layers. This makes it easier to organize, understand, and test your code.

<img src="mvc.png" alt="drawing" width="500"/>

## 1.7 Explain the advantages of version control

### Understanding Version Control Systems and Git

- Version Control Systems (VCS) maintain a history of changes to files for reference or rollback
- Centralised VCS rely on a master database to maintain all data, history and file revisions. 
    - This has a number of problems, mainly if the master database fails, all the work is lost. 
- Git is a distributed VCS. Every person working on the repo, have a full copy of the project
- Distributed VCSs allows for <b>multiple full clones</b> of the entire historical record

### Creating you First Repository
A repository is a collection of files that Git tracks changes for
- One directory with its subdirectories
- The .git subdirectory makes it a repo
- Can be created from scratch or 'cloned'

## 1.8 Utilize common version control operations with Git

### Git Clone
    git clone [url]

- <b>Example:</b> `git clone git@github.com:FooBartn/WxTeamsSharp.git`
- <b>Purpose:</b> retrieve an entire repository from a hosted location via URL

### Git Add/Remove
    git add [file]

- <b>Example:</b> `git add parsejson.py`
- <b>Purpose:</b> Add a file as it looks now to your next commit. Also known as Staging. Using `git add`. will add/stage all new and modified files. `git add -A` will stage all new, modified, and deleted files

### Git Commit
    git commit -m "[descriptive message]" 

- <b>Purpose:</b> Commit your staged content as a new commit snapshot
```
git commit -amend
```
- Replaces the previous commit

### Git Push/Pull
    git push [alias] [branch]

- Example: `git push origin master` <– Push the local master branch to remote named origin
- Purpose: Transmit local branch commits to the remote repository branch

```
git pull
```
- Purpose: Fetch and merge any commits from the tracking remote branch

### Git Branch

    git branch

<b>Purpose:</b>  List your branches. a * will appear next to the currently active branch

    git branch [branch-name]

<b>Purpose:</b>  Create a new branch at the current commit

    git checkout [branch]

<b>Purpose:</b>  Switch to another branch and check it out into your working directory

    git checkout -b [branch]

<b>Purpose:</b>  Create another branch, switch to it, and check it out into your working directory

### Git Merge and handling conflicts

    git merge [branch]

<b>Purpose:</b> Merge the specified branch’s history into the current one
- Target branch: The branch the changes are being pulled from
- Receiving Branch: The branch the changes are being pulled into
```
git status
```
<b>Purpose:</b> One of the primary purposes is to list out any any files affected by merge conflicts.

## <i>For more detail on resolving merge conflicts see <a href="https://www.git-tower.com/learn/git/ebook/en/command-line/advanced-topics/merge-conflicts">Git-Tower: Merge Conflicts</a> </i>


### Git Reset
    git reset
- Removes commits resetting the Head

<img src="git/git-reset.png" alt="drawing" width="500"/>

### Git Revert
    git revert
<img src="git/git-revert.png" alt="drawing" width="500"/>

- Creates reverse commits, preserving all history

### Git Log
    git log
- The `git log` command is used to view the changes made on the repository
- `git log --oneline` more condesed version

### Git Diff
    git diff
<b>Purpose:</b> Compare staged commits with `git diff [branch1] [branch2]`

    git diff --staged

<b>Purpose:</b> Diff of what is staged but not yet commited

## <i> Note: Check out the Atlassian: <a href = "https://www.atlassian.com/git/tutorials/saving-changes/git-diff"> Git-Diff Tutorial</a> to see how to read and understand the output from git diff!</i>

<br>

# 2.0 Understanding and Using APIs (20%)

<b>API (Application Programming Interface):</b> A way for a computer to communicate directly with another computer program. 
<br>

## 2.1 Construct a REST API request to accomplish a task given API documentation
### HTTP Verbs / Actions

- **POST**: Create
- **GET**: Read
- **PUT**: Update
- **PATCH**: Update
- **DELETE**: Delete

<img src="url-diagram.png" alt="drawing" width="500"/>

## 2.3 Identify the constraints when consuming APIs

<b>RESTful API:</b> An API that follows a specific set of constraints
<ul>
    <li>Uniform Interface</li>
    <li>Client Server</li>
    <li>Stateless</li>
    <li>Layered</li>
    <li>Cacheable</li>
    <li>Code on Demand</li>
</ul>

<b>Uniform Interface:</b> Follow a consistent and decoupled interface for API calls.
- Ideally only one URI for any given 'resource'.
- `/movies/{movieId}`
- `/movies/{movieId}/update`
- `/movies/{movieId}/delete`

<b>Client Server:</b> There must be a decoupled client and server in the implementation architecture

<b>Stateless:</b> Each API call must contain everything needed to perform the requested operation
- The server cannot remember the client from request to request.

**Code on Demand**: Provides flexibility to a client by allowing it to download code.

## 2.4 Explain common HTTP response codes associated with REST APIs
- **1xx (100-199)**: Informational Responses
- **2xx (200-299)**: Sccessful Responses
    - **200** OK: All looks good
    - **201** Created: New resource created
- **3xx (300-399)**: Redirects
- **4xx (400-499)**: Client Errors
    - **400** Bad Request: Request was invalid
    - **401** Unauthorized: Authentication missing or incorrect
    - **403** Forbidden Request: was understood but not allowed
    - **404** Not Found: Resource not found
- **5xx (500-599)**:
    - **500** Internal Server Error: Something wrong with the server
    - **503** Service Unavailable: Server is unable to complete request

## 2.5 Troubleshoot a problem given the HTTP response code, request and API documentation

Let's check out the [Webex Teams API: Basics](https://developer.webex.com/docs/api/basics)

Scroll down to the API Errors section. You should see a table like this:

| Code | Status       | Description                                                                                               |
|------|--------------|-----------------------------------------------------------------------------------------------------------|
| 200  | OK           | Successful Request                                                                                        |
| 204  | No Content   | Successful request without body content                                                                   |
| 400  | Bad Request  | The request was invalid or cannot be otherwise served. An accompanying error message will explain further |
| 401  | Unauthorized | Authentication credentials were missing or incorrect                                                      |

Most APIs will have information like this to let you know what went wrong and how to handle it.

> Note: If you see an error message in the 5xx range, you're all outta luck. Those are server issues and the only thing you can do is send a message to the owners of that API and hope they fix it.

## 2.6 Identify the parts of an HTTP response (response code, headers, body)
<img src="request-response.png" alt="drawing" width="500"/>

| Header      | Example Value | Purpose       |
| :---        |    :----:     |          :--- |
| Content-Type      | application/json         | Specify the format of the data in the body.  |
| Accept   | application/json          | Specify the requested format for the returned data, such as JSON.     |
| Authorization   | Bearer aBcDeFgHiJklMnOor0pQrS5z          | Provide credentials, such as a generated token that can expire, to authorize a request.      |
| Date/Time Stamp   | Thu Jan,4 17:02:23 CST 2018          | Date and time of a request or response message.     |

## 2.7 Utilize common API authentication mechanisms: basic, custom token, and API keys
### API Keys
You register for a service, agree to all the terms, and they give you an API key that is strictly for your account. You then put that in the header of every request you make. I.e. if ICanHazDadJoke did require an API key, the request might look like this:

``` python
import requests

url = 'https://icanhazdadjoke.com/search'
headers = {
    'Accept': 'application/json',
    'Authorization': 'Bearer 235235235xyz'
}
params = {
    'term': 'dog',
    'limit': '5'
}
response = requests.get(url, headers=headers, params=params)
joke = response.json()
print(joke)
```
### Basic Authentication
Basic authentication is the simplest. And the least secure. You send the username and password over plain text. So if it's done, it really should be done using Transport Layer Security (TLS) -- i.e. it should use https, not http. In Python it looks like this:

``` python
requests.get('https://api.github.com/user', auth=HTTPBasicAuth('user', 'pass'))
```

### Digest Authentication
Digest Authentication is very similar to Basic, except that it encrypts the data with a hash instead of sending it in plain text. So even though TLS is always preferred, in this case it's not as risky.

``` python
requests.get('https://api.github.com/user', auth=HTTPDigestAuth('user', 'pass'))
```

### OAuth
OAuth is the most common form of authentication if you're authenticating on behalf of other users. Think of when you use a Facebook account to authenticate with a non-Facebook service. When you sign up, you've probably seen a page that says "This application would like to access your account with these privileges: Read your username, Read your email, etc". That's the Oauth workflow at work!

To see more about how to use Oauth with Python, check out [requests-oauthlib](https://requests-oauthlib.readthedocs.io/en/latest/oauth2_workflow.html)

### Token Authentication
Token-based authentication is a protocol that generates encrypted security tokens. It enables users to verify their identity to websites, which then generates a unique encrypted authentication token. 

1. **Request:** The user logs in to a service using their login credentials, which issues an access request to a server or protected resource.
2. **Verification:** The server verifies the login information to determine that the user should have access. This involves checking the password entered against the username provided.
3. **Token submission:** The server generates a secure, signed authentication token for the user for a specific period of time.
4. **Storage:** The token is transmitted back to the user’s browser, which stores it for access to future website visits. When the user moves on to access a new website, the authentication token is decoded and verified. If there is a match, the user will be allowed to proceed.
5. **Expiration:** The token will remain active until the user logs out or closes the server.

# 3.0 Cisco Platforms and Development (15%)
## 3.6 Describe the device level APIs and dynamic interfaces for IOS XE and NX-OS



## 3.7 Identify the appropriate DevNet resource for a given scenario (Sandbox, Code Exchange, support, forums, Learning Labs, and API documentation)
### [Sandbox](https://developer.cisco.com/site/sandbox/)
Engineers, developers, network admins, architects, or anyone else who desires to develop/test against Cisco’s APIs, controllers, network gear, collaboration suite and more for free!

- Run your code against live infrastructure 24x7
- Free access to a variety of labs; choose from virtualized environments, simulators, and real hardware
- Select always on labs or reservation-based labs based on your need

### [Code Exchange](https://developer.cisco.com/exchange/)
A single, curated, online catalog for Cisco customers to find code, products, and services offered from across the Cisco ecosystem.

- Online, curated set of source code repositories related to all Cisco technology areas on public GitHub.
- Hundreds of code repositories, created and maintained by Cisco engineering teams, community contributors, ecosystem partners, technology and open source communities, and individual developers.

### [Support](https://developer.cisco.com/site/support/)
Need help on DevNet? Have a technology-specific question? We provide different ways of support to get your questions answered!

- Knowledge Base
- Community Forum
- Chat with DevNet
- Case-Based Ticket

### [Forums](https://community.cisco.com/t5/for-developers/ct-p/4409j-developer-home)
Get connected with other Cisco technology experts in DevNet.

### [Learning Labs](https://developer.cisco.com/learning/)
Dive deeper into Cisco and Cisco Partner technologies with DevNet Learning Labs, including Enterprise Networks, Data Center, Collaboration, Cloud, SDN, and IoT. Whether you’re getting started or need a programming refresher, the Learning Labs get you started with tutorials covering REST APIs, Python, JavaScript, and other engineering technologies and concepts.

## 3.8 Apply concepts of model driven programmability (YANG, RESTCONF, and NETCONF) in a Cisco environment
### What are Data Models?
A data model is a well understood and agreed upon method to describe "something". As an example, consider this simple "data model" for a person.

Person
- ***Gender***- male, female, other.
- ***Height*** - Feet & Inches, or Meters.
- ***Weight*** - Pounds or Kilos.
- ***Hair Color*** - brown, blond, black, red, other.
- ***Eye Color*** - brown, blue, green, hazel, other.

Using the generic data model, you can describe an individual in a way that is easy for others to understand.

<img src="standard_device_interface.jpeg" alt="drawing" width="500"/>

### YANG
YANG is a language for describing data models. Although it can describe any data model. An example is shown below.

```
module ietf-interfaces {
  import ietf-yang-types {
    prefix yang;
  }
  container interfaces {
    list interface {
      key "name";
      leaf name {
        type string;
      }
      leaf enabled {
        type boolean;
        default "true";
    }
}
```

Some aspects of the language to note: 
- Every data model is a `module`, a self-contained top-level hierarchy of nodes.
- Data types can be `imported` from another YANG module or defined within a module.
- It uses `containers` to group related nodes.
- It uses `lists` to identify nodes that are stored in sequence.
- A `leaf` represents each individual attribute of a node.
- Every leaf must have an associated `type`.

### NETCONF
NETCONF (Network Configuration) is the primary transport protocol that is used with YANG data models today. It defines how a manager (client) and agent (server) communicate in a standard fashion.

<img src="netconf_protocol_stack1.jpg" alt="drawing" width="700"/>


### NETCONF and RESTCONF

Check out these learning labs:

- [Intro NETCONF](https://developer.cisco.com/learning/lab/intro-netconf/step/1)
- [Intro RESTCONF](https://developer.cisco.com/learning/labs/intro-restconf/step/1)

# 5.0 Infrastructure and Automation (20%)
## 5.1 Describe the value of model driven programmability for infrastructure automation
### Why Network Automation
- Faster
    - Network Automation makes us agile
    - It removes the need to spend countless hours deploying VLANS, ACLs and other configurations manually
- Standardization
    - We know exactly what the global configurations will be across the network
    - If you have several engineers configuring the same thing, each one will have a different way of going about it, which makes it even more difficult to troubleshoot.
    - By creating a single automation script, we can eliminate this issue with all the engineers being clear on the function of the one script rather than each other's configurations.
- Known outcomes
    - The more devices you have to configure, the more human errors you are prone to. 
- Monitoring
    - Network automation enables easier troubleshooting and identify problems within the network quicker, rather than scrolling through thousands of lines of CLI code.

### What can you automate?

1. **Monitoring**
    - You can get specific data when you need it
    - There is no need to weed through lines of output to find the error or statistic you are looking for.

2. **Provisioning**
    - Update OS
    - Assign IP addresses
    - Create user credentials

3. **Migrating**
    - Moving files and data safely across the network to the correct configured location

4. **Configuration Management**
    - The configuration of devices can be automated
    - We can assign a desired state, the policy is stated that the device should remain in this state and can be mnitored.
    - If the device drifts from this state, it can be acted on before problems arise
    - Policies can be implemented, to prevent bad practices when configuring. If an engineer develops a script that compromises the network in some way, a policy will prevent them from doing so. 

5. **Troubleshooting**
    - Parse through outputs and check for any anomalies to detect an issue.
    - When the issue is detected it canm be acted on early. 

## 5.2 Compare controller-level to device-level management
The main difference is in a controller-level management, a computer communicates with the networking device, whereas in device-level management, a human would communicate with the device through the IOS. 

<img src="dev-cont.png" alt="drawing" width="700"/>

| Device-level                                                               | Controller-level                                                                                                                            |   |   |   |
|--------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------|---|---|---|
| Devices are configured one-by-one via SSH or console connection                | Devices are centrally managed and monitored from the controller GUI or other applications using REST API.                                |   |   |   |
| Devices are manually configured via console connection before deployment       | The admin communicates their intent to the controller, which changes those intentions into configurations on the managed network devices |   |   |   |
| Configurations and policies are managed per-device                             | Configurations and policies are centrally managed                                                                                        |   |   |   |
| New network deployments can take a long time due to the manual labour required |                                                                                                                                          |   |   |   |
| Errors and failures are more likely due to increased manual effort             |                                                                                                                                          |   |   |   |                                                                                |   |   |   |

## 5.3 Describe the use and roles of network simulation and test tools (such as VIRL and pyATS)
### VIRL
Cisco Virtual Internet Routing Lab (VIRL) is a software tool Cisco developed to build and run network simulations without the need for physical hardware.

Cisco CML/VIRL provides several interfaces: 
- CLI interface
- Powerful web user interface
- REST API for automation and integration
- Multiple virtual instances of operating systems and solutions—including Cisco SD-WAN


## 5.6 Describe the capabilities of automation tools such as Ansible, Puppet, Chef, and Cisco NSO
### Ansible
1. Automation and Orchestration
    - Automation deals with ***WHAT*** is that task
    - Orchestration deals with ***WHEN*** we want to execute that task
2. Ansible is native to Python
3. It is ***free*** and open-source
4. Ansible is ***Agentless***, meaning no process or software needs to be running for you to work with Ansible
5. You can use ***Modules*** to interact with a specific OS

## 5.10 Interpret the results of a RESTCONF or NETCONF query