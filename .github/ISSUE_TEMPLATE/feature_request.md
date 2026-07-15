---
name: Feature request
description: Suggest an idea for NJORD Engine
title: "[Feature]: "
labels: ["enhancement"]
---
body:
  - type: textarea
    id: problem
    attributes:
      label: Problem / motivation
      description: What are you trying to do that doesn't work today?
    validations:
      required: true

  - type: textarea
    id: solution
    attributes:
      label: Proposed solution
      description: What you want to happen, e.g. API sketch or workflow.
    validations:
      required: true

  - type: textarea
    id: alternatives
    attributes:
      label: Alternatives and context
      description: Workarounds you've tried, related issues, references.
