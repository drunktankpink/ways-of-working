
Report on absent tag values for selected tags
```yaml
policies:

  - name: ec2-tag-compliance
    resource: ec2
    comment: |
      Report on total count of non-compliant instances
    filters:
      - or:
        - "tag:SERVICE_NAME": absent
        - "tag:TAG_NAME": absent
```

Report on non-autoscaling instances without selected tags. Stop after 2 days and terminate after 5
```yaml
policies:

  - name: ec2-tag-compliance-mark
    resource: ec2
    comment: |
      Find all non-asg instances that are non-conformant to tagging policy. Tag for stoppage in 1 day.
    filters:
      - "tag:aws:autoscaling:groupname": absent
      - "tag:c7n_status": absent
      - or:
        - "tag:SERVICE_NAME": absent
        - "tag:TAG_NAME": absent
    actions:
      - type: mark-for-op
        op: stop
        days: 1

  - name: ec2-tag-compliance-unmark
    resource: ec2
    comment: |
      Any instances previous marked as non-conformant, that are now compliant should be unmarked.
    filters:
      - "tag:c7n_status": not-null
      - "tag:SERVICE_NAME": not-null
      - "tag:TAG_NAME": not-null
    actions:
      - unmark
      - start

  - name: ec2-tag-compliance-stop
    resource: ec2
    comment: |
      Stop all non-asg instances previously marked for stoppage by todays date, and schedule for termination in 2 days. Verify they are still non-conformant to tagging policies.
    filters:
      - "tag:aws:autoscaling:groupname": absent
      - type: marked-for-op
        op: stop
      - or:
        - "tag:SERVICE_NAME": absent
        - "tag:TAG_NAME": absent
    actions:
      - stop
      - mark-for-op
        op: terminate
        days: 3

  - name: ec2-tag-compliance-terminate
    resource: ec2
    comment: |
      Terminate all stopped instances marked for termination.
    filters:
      - "tag:aws:autoscaling:groupname": absent
      - type: marked-for-op
        op: terminate
      - or:
        - "tag:SERVICE_NAME": absent
        - "tag:TAG_NAME": absent
    actions:
      - type: terminate
        force: true

  - name: ec2-tag-compliance-nag-stop
    resource: ec2
    comment: |
      Stop all instances marked for termination every hour, starting 1 day before their termination.
    filters:
      - "tag:aws:autoscaling:groupname": absent
      - type: marked-for-op
        op: terminate
        skew: 1
      - or:
        - "tag:SERVICE_NAME": absent
        - "tag:TAG_NAME": absent
    actions:
      - stop
```