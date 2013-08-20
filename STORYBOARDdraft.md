# Alliance API Authorization &amp; Account Manager
## Description
A web based system that allows users to register once for all alliance services. Basic Roles and Access are determined based on IG roles.  


### Features
1. User Registration
 - Collects Email, Timezone, Character Name and API’s for new registrants.
 - Validates the User as part of the alliance via the API.
 - Validates the User’s Email address as part of registration.
2. Account Manager
 - A dashboard for viewing and managing all of a User’s Attributes.
 - Allows a User to update his/her account Attributes.
3. API Authentication
 - Checks all registered users as often as practical and terminates access should they no longer be in the alliance.
4. Batspam Integration
 - Adds Phone Number to User Registration and Account Management.
 - Adds a BatSpam check box to User Registration and Account Management.
 - Allows Users with requisite roles to send out batspam SMS messages.
 - Ensures that phone numbers are never easily available to batspam senders or receivers.
5. Voice Coms Integration
 - Provides API Authenticated voice server access.
 - Access is terminated if the User is no longer meets API Requirements.
 - Voice server credentials are added to the Account Manager for the User as a reference.
 - Credentials are emailed to Users email as well as displayed in their Account Manager.
6. Advanced Roles Management
 - Adds specialty roles including FC, Capital FC, Diplomat, Logistician, etc.
 - Adds a Roles management dashboard for CEOs, FCs, Directors, etc.
Forums
 - Forums that integrate with the Role Management system.
7. Automatic Op Announcements
 - User configurable op notices
 - The user may configure for emails or other non-sms op notifications
 - The user may set notifications for operations hosted by specific people.
8. Alt Blue Requests
 - Allows a User to register an alt to be Blue’d by the alliance.
 - Adds Alt APIs to the Account Manager
 - Requires FULL API for the alt
 - Sends request to standings manager to add the alt to the blue list.
 - Sends request to standings manager to remove alt when User leaves/requests it.
9. Notifications Monitor/Relay System
 - Monitors and logs all Notifications
 - Relays siege notices via batspam to CEO/Director/FCs
 - Relays notices to responsible role via batspam
10. Message Fingerprinting
 - Embeds unique characters/errors/identifiable data into batspams and other notifications to track leaks.


### Basic System
#### Required Features
 - User Registration
 - Account Manager
 - API Authentication
 - Advanced roles management
 - Batspam Integration

### Proposed System Architecture
#### Models

##### User
1. Inherited Models
 - Has Many Characters
 - Has One API
 - Has One Phone Number
 - Has Many Roles
2. Attributes
 - Email – A string containing the user’s validated email address
 - Name – The name of their Main Character
 - Timezone – The user’s provided Timezone
 - Corporation Tag – The User’s Corporation Tag, determined by their API.
 - Main character (determined by main character Boolean) is the user’s display name.

##### Character
1. Attribute
 - Main Character Boolean
 - Has one API

##### API
1. Attributes
 - API ID
 - API Verification Code
 - API Access Mask
 - Valid Boolean

##### Phone Number
1. Attributes
 - A string of digits for use with bulk SMS Service
 - Is encrypted with reversible encryption.
 - Valid Boolean

##### Role
 - An Attribute hash with several possible values: CEO, FC, Standings Manager, etc.</p>

### Role Interactions/Definitions
##### Administrator
 - CRUD all other roles
##### [corp] CEO
 - CRUD all user role’s within its corp
 - Batspam send access within its corp
 - Is Unique Role
##### [corp] Director
 - CRUD all non-CEO role’s within its corp
 - Batspam send access within its corp
##### [corp] User
 - No rights
##### Head Fleet Commander
 - CRUD over all FCs including Division Heads
 - Batspam send access
 - Op declaration access
 - Is Unique Role
##### General Fleet Commander
 - General Batspam access
 - General op declaration access
##### Division Head Fleet Commander
 - CRUD over other FCs within the division
 - Batspam send access to division members
 - Division Op declaration access
 - Is Unique Role
##### Division FC
 - Batspam send access to division members
 - Division op declaration access

### API Validation Rules
 - An API is invalid if it returns an error while being validated
 - An API is invalid if it resolves to an out of alliance character

### CEO API Requirements
##### CEO API’s must include
 - Corporation Kill Log
 - Corporation Wallet Journal
 - Corporation Wallet Transactions
 - Corporation Starbase List
 - Corporation Starbase Detail
 - Corporation Notifications
 - Asset List
 - Contact List
 - Industry Jobs
 - Market Orders
 - Account Balances

### Use Case
#### First Time Registration
1. User registers with service, provides Name, Email, API, Phone and Timezone
2. Name, Timezone and Email are stored as attributes of the User
3. API is stored as attributes of the User’s API Object.
4. User’s API is validated, determining access rights and User’s Corporation Tag attribute.
5. User’s Phone Number is stored as an attribute of the User’s Phone Number Object.
6. User’s Phone Number is Encrypted.
7.User’s Email is validated by sending a Validation Email. The user clicks the link to validate.
8. User’s Phone Number is validated by sending a Validation SMS.
9. User confirms SMS receipt by typing in the Validation Code contained in the SMS.
10. User’s Phone Number’s valid attribute is updated.

#### User Logs in
1. User navigates to web page and is directed to login window
2. User submits account credentials
3. Account credentials are verified by system
4. User is directed to Account Manager

#### User is Given Fleet Commander Role
1. Administrator/Division Head/Head FC logs in.
2. Administrator selects a user on the Roles Management Dashboard
3. Administrator assigns that user the Fleet Commander role.

#### A User’s API becomes Invalid
1. API Authentication is conducted by a chron job
2. User’s API is found to be invalid
3. User’s API’s Valid Boolean is flipped to invalid
4. User loses all access

#### User’s API identifies him/her as the CEO
1. User’s API is authenticated by a chron job
2. User is found to be a member of an alliance corporation
3. User is assigned to that corporation
4. User is found to be the CEO of the corporation
5. User is assigned the CEO role.

