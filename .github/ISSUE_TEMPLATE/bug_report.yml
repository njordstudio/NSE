name: Bug report
description: Report a bug in NJORD Engine
title: "[Bug]: "
labels: ["bug"]
body:
  - type: textarea
    id: description
    attributes:
      label: What happened?
      description: What you saw vs. what you expected.
    validations:
      required: true

  - type: textarea
    id: reproduction
    attributes:
      label: How to reproduce
      description: Steps or a minimal code example.
    validations:
      required: true

  - type: input
    id: version
    attributes:
      label: Version / commit
      placeholder: v0.3.1 / a1b2c3d
    validations:
      required: true

  - type: input
    id: environment
    attributes:
      label: OS, GPU and driver
      placeholder: Windows 11, RTX 4070, 560.xx

  - type: textarea
    id: logs
    attributes:
      label: Logs / backtrace
      description: Run with `RUST_BACKTRACE=1` and paste relevant output.
      render: text
