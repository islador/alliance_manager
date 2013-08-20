<h1>Alliance API Authorization &amp; Account Manager</h1>

<h2>Description</h2>

<p>A web based system that allows users to register once for all alliance services. Basic Roles and Access are determined based on IG roles.  </p>

<h3>Features</h3>

<ol>
<li>User Registration
<ul><li>Collects Email, Timezone, Character Name and API’s for new registrants.</li>
<li>Validates the User as part of the alliance via the API.</li>
<li>Validates the User’s Email address as part of registration.</li></ul></li>
<li>Account Manager
<ul><li>A dashboard for viewing and managing all of a User’s Attributes.</li>
<li>Allows a User to update his/her account Attributes.</li></ul></li>
<li>API Authentication
<ul><li>Checks all registered users as often as practical and terminates access should they no longer be in the alliance.</li></ul></li>
<li>Batspam Integration
<ul><li>Adds Phone Number to User Registration and Account Management.</li>
<li>Adds a BatSpam check box to User Registration and Account Management.</li>
<li>Allows Users with requisite roles to send out batspam SMS messages.</li>
<li>Ensures that phone numbers are never easily available to batspam senders or receivers.</li></ul></li>
<li>Voice Coms Integration
<ul><li>Provides API Authenticated voice server access.</li>
<li>Access is terminated if the User is no longer meets API Requirements.</li>
<li>Voice server credentials are added to the Account Manager for the User as a reference.</li>
<li>Credentials are emailed to Users email as well as displayed in their Account Manager.</li></ul></li>
<li>Advanced Roles Management
<ul><li>Adds specialty roles including FC, Capital FC, Diplomat, Logistician, etc.</li>
<li>Adds a Roles management dashboard for CEOs, FCs, Directors, etc.
Forums</li>
<li>Forums that integrate with the Role Management system.</li></ul></li>
<li>Automatic Op Announcements
<ul><li>User configurable op notices</li>
<li>The user may configure for emails or other non-sms op notifications</li>
<li>The user may set notifications for operations hosted by specific people.</li></ul></li>
<li>Alt Blue Requests
<ul><li>Allows a User to register an alt to be Blue’d by the alliance.</li>
<li>Adds Alt APIs to the Account Manager</li>
<li>Requires FULL API for the alt</li>
<li>Sends request to standings manager to add the alt to the blue list.</li>
<li>Sends request to standings manager to remove alt when User leaves/requests it.</li></ul></li>
<li>Notifications Monitor/Relay System
<ul><li>Monitors and logs all Notifications</li>
<li>Relays siege notices via batspam to CEO/Director/FCs</li>
<li>Relays notices to responsible role via batspam</li></ul></li>
<li>Message Fingerprinting
<ul><li>Embeds unique characters/errors/identifiable data into batspams and other notifications to track leaks.</li></ul></li>
</ol>

<h3>Basic System</h3>

<h4>Required Features</h4>

<ul>
<li>User Registration</li>
<li>Account Manager</li>
<li>API Authentication</li>
<li>Advanced roles management</li>
<li>Batspam Integration</li>
</ul>

<h3>Proposed System Architecture</h3>

<h4>Models</h4>

<h5>User</h5>

<ol>
<li>Inherited Models
<ul><li>Has Many Characters</li>
<li>Has One API</li>
<li>Has One Phone Number</li>
<li>Has Many Roles</li></ul></li>
<li>Attributes
<ul><li>Email – A string containing the user’s validated email address</li>
<li>Name – The name of their Main Character</li>
<li>Timezone – The user’s provided Timezone</li>
<li>Corporation Tag – The User’s Corporation Tag, determined by their API.</li>
<li>Main character (determined by main character Boolean) is the user’s display name.</li></ul></li>
</ol>

<h5>Character</h5>

<ol>
<li>Attribute
<ul><li>Main Character Boolean</li>
<li>Has one API</li></ul></li>
</ol>

<h5>API</h5>

<ol>
<li>Attributes
<ul><li>API ID</li>
<li>API Verification Code</li>
<li>API Access Mask</li>
<li>Valid Boolean</li></ul></li>
</ol>

<h5>Phone Number</h5>

<ol>
<li>Attributes
<ul><li>A string of digits for use with bulk SMS Service</li>
<li>Is encrypted with reversible encryption.</li>
<li>Valid Boolean</li></ul></li>
</ol>

<h5>Role</h5>

<ul>
<li>An Attribute hash with several possible values: CEO, FC, Standings Manager, etc.</p></li>
</ul>

<h3>Role Interactions/Definitions</h3>

<h5>Administrator</h5>

<ul>
<li>CRUD all other roles</li>
</ul>

<h5>[corp] CEO</h5>

<ul>
<li>CRUD all user role’s within its corp</li>
<li>Batspam send access within its corp</li>
<li>Is Unique Role</li>
</ul>

<h5>[corp] Director</h5>

<ul>
<li>CRUD all non-CEO role’s within its corp</li>
<li>Batspam send access within its corp</li>
</ul>

<h5>[corp] User</h5>

<ul>
<li>No rights</li>
</ul>

<h5>Head Fleet Commander</h5>

<ul>
<li>CRUD over all FCs including Division Heads</li>
<li>Batspam send access</li>
<li>Op declaration access</li>
<li>Is Unique Role</li>
</ul>

<h5>General Fleet Commander</h5>

<ul>
<li>General Batspam access</li>
<li>General op declaration access</li>
</ul>

<h5>Division Head Fleet Commander</h5>

<ul>
<li>CRUD over other FCs within the division</li>
<li>Batspam send access to division members</li>
<li>Division Op declaration access</li>
<li>Is Unique Role</li>
</ul>

<h5>Division FC</h5>

<ul>
<li>Batspam send access to division members</li>
<li>Division op declaration access</li>
</ul>

<h3>API Validation Rules</h3>

<ul>
<li>An API is invalid if it returns an error while being validated</li>
<li>An API is invalid if it resolves to an out of alliance character</li>
</ul>

<h3>CEO API Requirements</h3>

<h5>CEO API’s must include</h5>

<ul>
<li>Corporation Kill Log</li>
<li>Corporation Wallet Journal</li>
<li>Corporation Wallet Transactions</li>
<li>Corporation Starbase List</li>
<li>Corporation Starbase Detail</li>
<li>Corporation Notifications</li>
<li>Asset List</li>
<li>Contact List</li>
<li>Industry Jobs</li>
<li>Market Orders</li>
<li>Account Balances</li>
</ul>

<h3>Use Cases</h3>

<h4>First Time Registration</h4>

<ol>
<li>User registers with service, provides Name, Email, API, Phone and Timezone</li>
<li>Name, Timezone and Email are stored as attributes of the User</li>
<li>API is stored as attributes of the User’s API Object.</li>
<li>User’s API is validated, determining access rights and User’s Corporation Tag attribute.</li>
<li>User’s Phone Number is stored as an attribute of the User’s Phone Number Object.</li>
<li>User’s Phone Number is Encrypted.
7.User’s Email is validated by sending a Validation Email. The user clicks the link to validate.</li>
<li>User’s Phone Number is validated by sending a Validation SMS.</li>
<li>User confirms SMS receipt by typing in the Validation Code contained in the SMS.</li>
<li>User’s Phone Number’s valid attribute is updated.</li>
</ol>

<h4>User Logs in</h4>

<ol>
<li>User navigates to web page and is directed to login window</li>
<li>User submits account credentials</li>
<li>Account credentials are verified by system</li>
<li>User is directed to Account Manager</li>
</ol>

<h4>User is Given Fleet Commander Role</h4>

<ol>
<li>Administrator/Division Head/Head FC logs in.</li>
<li>Administrator selects a user on the Roles Management Dashboard</li>
<li>Administrator assigns that user the Fleet Commander role.</li>
</ol>

<h4>A User’s API becomes Invalid</h4>

<ol>
<li>API Authentication is conducted by a chron job</li>
<li>User’s API is found to be invalid</li>
<li>User’s API’s Valid Boolean is flipped to invalid</li>
<li>User loses all access</li>
</ol>

<h4>User’s API identifies him/her as the CEO</h4>

<ol>
<li>User’s API is authenticated by a chron job</li>
<li>User is found to be a member of an alliance corporation</li>
<li>User is assigned to that corporation</li>
<li>User is found to be the CEO of the corporation</li>
<li>User is assigned the CEO role.</li>
</ol>

