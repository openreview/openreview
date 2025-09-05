# Objects in OpenReview

OpenReview consists of different objects used to hold or interact with data. This section defines briefly each of the objects and how they funciton within OpenReview.

**Notes**: Primary data objects representing submissions, reviews, comments, and decisions. Each Note contains flexible fields and fine-grained access controls, including `readers`, `writers`, and `signatures`. Notes are created as responses to Invitations, ensuring they adhere to predefined structures and permissions.

**Invitations**: Templates that define the structure, permissions, and rules for creating or editing entities like Notes and Edges. Invitations specify who can perform certain actions (`invitees`), who can read the resulting entities (`readers`), and the expected format of the content. They ensure consistency and enforce workflow rules.

[**Groups**](groups.md): Collections of users (Profiles) that represent roles like authors, reviewers, or progiram chairs. Groups define permissions and access controls within the system. They are essential for managing who can read, write, or sign various entities.

**Edits**: Mechanism for creating or modifying entities like Notes, Groups, and Invitations. An Edit encapsulates the changes and, upon submission, undergoes an inference process to apply these changes to the target entity. This allows for a record of all changes within the system.

[**Profiles**](introduction-to-profiles.md): Represent individual users with unique identifiers (e.g., `~First_Last1`). They store user information such as names, emails, affiliations, and relationships (e.g., co-authors, advisors). Profiles are  linked to different roles and permissions in the system via Groups.







*
