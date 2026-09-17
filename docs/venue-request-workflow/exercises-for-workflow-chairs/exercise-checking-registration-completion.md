# Exercise: Checking Registration Completion

#### Objectives

Learn to use the OpenReview API to:

1. Retrieve members of a group
2. Check whether reviewers have accepted the invitation
3. Check whether reviewers have completed registration
4. Sentd a message to users who haven't completed registration



**Setup:**  Complete [Prerequisites](prerequisites.md)

### 1. Retrieve&#x20;

Using the API, retrieve the notes for Reviewer Registration. The invitation is typically in the format `<YOUR_VENUE_ID>/Reviewers/-/Registration`

**Resources:** [Working with notes ](../../how-to-guides/data-retrieval-and-modification/how-to-get-all-notes-for-submissions-reviews-rebuttals-etc.md)

The signature of each note is a list containing the profile ID of the user who submitted the note. We  use these signatures to identify who has completed the registration.&#x20;

<pre class="language-python"><code class="lang-python">##assuming a registration_notes object
<strong>users_registration_completed = [r.signatures[0] for r in registration_notes]
</strong></code></pre>

Users may have multiple profile IDs, so it is important to normalize profile IDs when  note signatures. To do so, first retrieve the profiles for members of the `users_registration_completed`  list from Step 1 above.

**Resources:** See [this exercise](exercise-getting-profile-information.md) and [Introduction to Profiles](../../getting-started/objects-in-openreview/introduction-to-profiles.md)

```python
profiles = client.get_profiles(users_registration_completed)
normalized_profiles = [p.id for p in profiles]
```

**Check your work:**  You should have a list of profile ids of the users

### 2. Retrieve Reviewer Group

Next, retrieve the members of the Reviewer group for your venue. This group shows all reviewers that have accepted the invitation to review for your venue and hence received the registration task. Normalize the reviewer profiles as well.

Use the function `client.get_group()` . Typically the reviewer group ID is  `<YOUR_VENUE_ID>/Reviewers` .&#x20;

```python
reviewer_group = client.get_group('GROUP_ID').members
```

**Resources:** See [Introduction to Groups](../../getting-started/objects-in-openreview/groups.md)

### 3. Compare Registration Signatures to Reviewer Group



Finally, identify the users in `reviewer_group` who are not in `normalized_profiles` to make a list of users who have not completed their registration:

```python
registration_not_completed = [profile for profile in reviewer_group if not profile in normalized_profiles]
```

### 4. Send message to reviewers who have not completed registration

Use `post_message`  to send an email to the users who have not completed registration.&#x20;

**Resources**: [How to send emails with python client](../../how-to-guides/communication/how-to-send-messages-with-the-python-client.md)
