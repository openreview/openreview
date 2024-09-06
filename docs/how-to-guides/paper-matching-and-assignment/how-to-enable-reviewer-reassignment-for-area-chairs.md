# How to enable Reviewer Reassignment for Area Chairs

#### Area Chairs: modifying reviewer assignments

If you would like for Area Chairs to be able to modify reviewer assignments, you can enable this by adding a variable in the Group Content for your venue:&#x20;

1. Go to the main conference group, https://openreview.net/group/edit?id=\<insert\_your\_venue\_id>
2. Scroll down to "**Group Content"** and click 'show content'
3. In the editor, add the variable:&#x20;

```
"enable_reviewers_reassignment": {
    "value": true
  }
```

4. Update Content to save the changes

#### Area Chairs: modifying proposed reviewer assignments

If you would like for Area Chairs to be able to modify reviewer assignments before they are deployed, you can enable this by adding a variable in the Group Content for your venue:&#x20;

1. Go to the main conference group, https://openreview.net/group/edit?id=\<insert\_your\_venue\_id>
2. Scroll down to "**Group Content"** and click 'show content'
3. In the editor, add the variable:

```
"reviewers_proposed_assignment_title": { 
    "value": "matcher1" 
}
```

4. Replace "matcher1" with the title of the matching configuration.

![](<../../.gitbook/assets/image (2) (1) (2).png>)
