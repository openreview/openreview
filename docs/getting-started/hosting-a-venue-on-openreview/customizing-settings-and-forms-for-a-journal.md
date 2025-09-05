# Customizing settings and forms for a Journal

These are the settings that you can change for your journal:

Note: AE - Action Editor; EIC - Editor-in-Chief

### Journal Settings

* **website\_urls**
  * Definition: URLs to relevant pages of the website
  * Possible values: dictionary in the format {page\_name : url}
  * Default value: None
* **editors\_email**
  * Definition: List of emails of the editors-in-chief of the journal
  * Possible values: list of strings
  * Default value: None
* **issn**
  * Definition: ISSN of the journal
  * Possible values: string
  * Default value: "XXXX-XXXX"
* **has\_publication\_chairs**
  * Definition:
  * Possible values:
  * Default value: False

### Submission Settings

* **submission\_public**
  * Definition: Whether submissions can be seen by the general public (anyone with the link)
  * Possible values: True, False
  * Default value: True
* **author\_anonymity**
  * Definition: True if authors are anonymous.
  * Possible values: True, False
  * Default value: True
* **assignment\_delay**
  * Definition: Delays the notification to the reviewer in case the AE make a mistake and decides to remove the assignment (in seconds)
  * Possible values: integer
  * Default value: 5
* **expertise\_model**
  * Definition:
  * Possible values:
  * Default value: "specter+mfr"
* **submission\_name**
  * Definition: The name of the submission invitation in the system
  * Possible values: string
  * Default value: "Submission"
* **submission\_length**
  * Definition: The type of the submission
  * Possible values: List of strings from this set:&#x20;
  * Default value: \[ "Regular submission (submissions may be any length)" ]
* **submission\_license**
  * Definition:
  * Possible values:&#x20;
  * Default value: "CC BY-SA 4.0"
* **release\_submission\_after\_acceptance**
  * Definition:
  * Possible values: True, False
  * Default value: True
* **eic\_submission\_notification**
  * Definition:
  * Possible values: True, False
  * Default value: False
* **skip\_camera\_ready\_revision**
  * Definition:
  * Possible values: True, False
  * Default value: False

### **Review Settings**

* **skip\_ac\_recommendations**
  * Definition: Set as True to skip the phase where authors recommend AEs for their paper.
  * Possible values: True, False
  * Default value: False
* **number\_of\_reviewers**
  * Definition: Number of reviewers needed to trigger the next step.
  * Possible values: integer
  * Default value: 3
* **reviewers\_max\_papers**
  * Definition: Maximum number of papers that can be assigned to a given reviewer per year
  * Possible values: integer
  * Default value: 6
* **action\_editors\_max\_papers**
  * Definition:
  * Possible values: integer
  * Default value: 12
* **archived\_action\_editors**
  * Definition: If True it allows AEs to keep current assignments, however AEs are moved to the archival group so they aren't given new assignments.
  * Possible values: True, False
  * Default value:  True
* **archived\_reviewers**
  * Definition:
  * Possible values:
  * Default value: False
* **expert\_reviewers**
  * Definition: If True creates the Expert\_Reviewers group. Related to certifications.
  * Possible values: True, False
  * Default value: True
* **skip\_reviewer\_responsibility\_acknowledgement**
  * Definition:
  * Possible values: True, False
  * Default value: False
* **skip\_reviewer\_assignment\_acknowledgement**
  * Definition:
  * Possible values: True, False
  * Default value: False
* **show\_conflict\_details**
  * Definition:
  * Possible values: True, False
  * Default value: False

### Duration Settings

* **ae\_recommendation\_period**
  * Definition: Number of weeks authors are able to recommend AEs. (soft deadline)
  * Possible values: integer
  * Default value: 1
* **under\_review\_approval\_period**
  * Definition: Number of weeks the AE has to approve a submission for review.
  * Possible values: integer
  * Default value: 1
* **reviewer\_assignment\_period**
  * Definition: Number of weeks the AE has to assign reviewers.
  * Possible values: integer
  * Default value: 1
* **review\_period**
  * Definition: Length of the review period (in weeks)
  * Possible values: integer
  * Default value: 2
* **discussion\_period**
  * Definition: Length of the discussion period between reviewers (in weeks)
  * Possible values: integer
  * Default value: 2
* **recommendation\_period**
  * Definition: Length of the reviewer recommendation period (in weeks)
  * Possible values: integer
  * Default value: 2
* **decision\_period**
  * Definition: Length of the decision period for AEs (in weeks)
  * Possible values: integer
  * Default value: 1
* **camera\_ready\_period**&#x20;
  * Definition: Length of the camera ready revision period for authors (in weeks)
  * Possible values: integer
  * Default value: 4
* **camera\_ready\_verification\_period**
  * Definition: Length of the camera ready verification period (in weeks)
  * Possible values: integer
  * Default value: 1

### Form customizations:

See [here](https://github.com/openreview/openreview-py/blob/6afa3b9c6152aaab5993e276e619d71e445c58a2/openreview/journal/invitation.py#L893) to see the default fields of the different forms. (Search for the set\_\[Form Name]\_invitation function).

* **submission\_additional\_fields**
  * Definition: Additional fields/customization for the submission form
  * Possible values: JSON
  * Default value: None.&#x20;
* **review\_additional\_fields**
  * Definition: Additional fields/ customization for the additional review form
  * Possible values: JSON
  * Default value: None.&#x20;
* **official\_recommendation\_additional\_fields**
  * Definition: Additional fields/ customization for the official recommendation form
  * Possible values: JSON
  * Default value: None.&#x20;
* **decision\_additional\_fields**
  * Definition: Additional fields/ customization for the decision form
  * Possible values: JSON
  * Default value: None.&#x20;

### **Certifications**

* **certifications:**
  * Definition:
  * Possible values: string\[]
  * Default value:&#x20;
* **eic\_certifications:**
  * Definition:
  * Possible values: string\[]
  * Default value:&#x20;
* **event\_certifications**
  * Definition:
  * Possible values: string\[]
  * Default value:

These settings are formatted into JSON and updated in the Journal Request Form. An example JSON is below:&#x20;

```
'submission_public': True,
'author_anonymity': True,
'assignment_delay': 5,
'submission_name': 'Submission',
'certifications': [
    'Featured Certification',
    'Reproducibility Certification',
    'Survey Certification'
],
'eic_certifications': [
    'Outstanding Certification'
],                            
'submission_length': [
    'Regular submission (no more than 12 pages of main content)', 
    'Long submission (more than 12 pages of main content)'
],
'issn': '2835-8856',
'website_urls': {
    'editorial_board': 'https://jmlr.org/tmlr/editorial-board.html',
    'evaluation_criteria': 'https://jmlr.org/tmlr/editorial-policies.html#evaluation',
    'reviewer_guide': 'https://jmlr.org/tmlr/reviewer-guide.html',
    'editorial_policies': 'https://jmlr.org/tmlr/editorial-policies.html',
    'faq': 'https://jmlr.org/tmlr/contact.html'                     
},
'editors_email': 'tmlr-editors@jmlr.org',
'skip_ac_recommendation': False,
'number_of_reviewers': 3,
'reviewers_max_papers': 6,
'ae_recommendation_period': 1,
'under_review_approval_period': 1,
'reviewer_assignment_period': 1,
'review_period': 2,
'discussion_period' : 2,
'recommendation_period': 2,
'decision_period': 1,
'camera_ready_period': 4,
'camera_ready_verification_period': 1,
'archived_action_editors': True,
'expert_reviewers': True,
```
