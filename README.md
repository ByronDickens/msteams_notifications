Microsoft Teams Notifications
=========

This role sends notifications to Microsoft Teams.


Role Variables
--------------

```yaml

# This role accepts the following standard input variables:
notification_subject: (Mapped to msteams_title)
notification_body: (Mapped to msteams_text)
notification_automation_result: (Use to switch the color of the card, which is green by default. If this variable contains the value `succeeded` then green will be used, if it contains `failed` then red will be used) 

# This variable should contain the webhook url to a MS Teams channel. E.g. https://mycompany.webhook.office.com/GUID/IncomingWebhook/GUID
msteams_webhook: 
# This variable is used as a destination link for a button in the message, it should be populated with a link to the Ansible Automation Platform web page    
msteams_aap_url: https://www.redhat.com/en/technologies/management/ansible
# This variable is used for the title of the notification card.
msteams_title: Automation Activity Notification
# This variable is used for the text body of the notification card.
msteams_text: |
      An **automation activity** has generated this notification. The card colour can be used to indicate that the automation activity completed successfully (Green)  or not (Red).
      
      Many fields support markdown formatting such as a table: 

      | Column 1 | Column 2 | Column 3 |
      | --- | --- | ---|
      | aaa | bbb | ccc |
# This variable is used for the colour access of the notification card.
msteams_color:  # Color accent of card in hex - E81123=Red  10E662=Green By default this is empty to allow the value to be chosen based on the notification_automation_result variable value
# This variable is used to embed actions on the notification card including the button to go to Ansible Automation Platform web page.
msteams_actions:
      - "@type": OpenUri
        name: AAP Web Page
        targets:
        - os: default
          uri:  "{{ msteams_aap_url  | default(none) or omit }}"     
# This variable is used to provide richer content in the notification message as sections. S

msteams_sections:
  - title: This is the **section's title** property
        activity_title: This is the section's **activityTitle** property
        activity_subtitle: This is the section's **activitySubtitle** property
        activity_text: This is the section's **activityText** property. 
        text: |
            This is the section's text property. 

              | column 1 | column 2 | column 3 |
              | --- | --- | ---|
              | aaa | bbb | ccc |
        facts:
        - name: A fact name
          value:  A fact value
        actions:
        - "@type": ActionCard
          name: Comment
          inputs:
          - "@type": TextInput
            id: comment
            is_multiline: true
            title: Input's title property
          actions:
          - "@type": HttpPOST
            name: Save
            target: http://...
        - "@type": ActionCard
          name: Due Date
          inputs:
          - "@type": DateInput
            id: dueDate
            title: Input's title property
          actions:
          - "@type": HttpPOST
            name: Save
            target: http://...
        - "@type": HttpPOST
          name: Action's name prop.
          target: http://...
        - "@type": OpenUri
          name: Action's name prop
          targets:
          - os: default
            uri: http://...
      - start_group: true
        title: This is the title of a **second section**
        text: This second section is visually separated from the first one by setting its
          **startGroup** property to true.
```

Dependencies
------------

This role depends on the community.general Ansible Collection

Example Playbook
----------------

```yaml
- name: Send Microsoft Teams Notifications
  hosts: localhost
  connection: local
  gather_facts: no
  tasks:
    - name: Apply MS Teams notifications role
      include_role:
        name: itblaked.msteams_notifications
```

License
-------

BSD

Author Information
------------------

Blake Douglas, blake@redhat.com
