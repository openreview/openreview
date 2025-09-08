# Fields

### **Structure of Profiles**

Profiles are an OpenReview object that contains properties. The main three properties that you can expect to interact with are:&#x20;

**`id`**

* **Type:** `str`
* **Description:** The unique identifier of the profile, usually in the form `~First_Last1`.
* **Purpose:** Used as the unique reference to the user across OpenReview (e.g., for assigning roles, authorship, etc.).

`state`&#x20;

* **Type**: `str`&#x20;
* **Description**: The status of the profile
  * `Active` : Profile was moderated and is now active
  * `Active Institutional` : Profile contains an approved institutional email domain
  * `Inactive` : User has not completed Profile Registration (profile is not active)
  * `Rejected` : User needs to correct information in the profile and resubmit it for moderation
  * `Needs Moderation` : Profile is pending moderation

**`content`**

* **Type:** `dict`
* **Description:** The main body of the profile data. It contains structured information about the user.
* **Key Subfields:**
  * `names`: List of name entries. Each entry is a dict with keys like `first`, `last`.
  * `emailsConfirmed`: List of verified email addresses.
  * `emails` : List of all email addresses
  * `preferredEmail`: The primary email to be used for communication.
  * `history`: List of affiliations over time, each with `institution`, `position`, `start`, and `end`.
  * `homepage` , `gscholar`, `dblp`, `github`: Social and academic links.
  * `expertise`: Manually entered expertise areas.
  * `relations`: Manually entered relationships to other users in the OpenReview system.
