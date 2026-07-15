name: Bug report
description: Report a bug in NJORD Engine
title: "[Bug]: "
labels: ["bug", "needs-triage"]
body:
  - type: checkboxes
    id: checks
    attributes:
      label: Before you start
      options:
        - label: I have searched existing issues and this is not a duplicate
          required: true

  - type: textarea
    id: description
    attributes:
      label: What happened?
      description: A clear and concise description of the bug. Include what you saw vs. what you expected if it's visual.
      placeholder: The dither pass flickers when the camera moves along the Y axis...
    validations:
      required: true

  - type: textarea
    id: reproduction
    attributes:
      label: How to reproduce
      description: Steps and/or a minimal code example. The smaller the repro, the faster the fix.
      placeholder: |
        1. Spawn a mesh with ToonMaterial
        2. Move the camera with ...
        3. Observe ...

```rust
        // minimal example
```
    validations:
      required: true

  - type: textarea
    id: expected
    attributes:
      label: Expected behavior
    validations:
      required: true

  - type: input
    id: version
    attributes:
      label: NJORD Engine version
      description: Release tag or commit hash (`git rev-parse --short HEAD`). Note your Bevy version too if you've overridden the pinned one.
      placeholder: v0.3.1 / a1b2c3d
    validations:
      required: true

  - type: input
    id: rust-version
    attributes:
      label: Rust version
      description: Output of `rustc --version`
      placeholder: rustc 1.88.0 (stable)

  - type: dropdown
    id: os
    attributes:
      label: Operating system
      multiple: true
      options:
        - Windows
        - Linux
        - macOS
    validations:
      required: true

  - type: input
    id: gpu
    attributes:
      label: GPU, driver and backend
      description: Important for rendering/shader bugs
      placeholder: RTX 4070, driver 560.xx, Vulkan

  - type: textarea
    id: logs
    attributes:
      label: Logs and backtrace
      description: Run with `RUST_BACKTRACE=1` and paste relevant output. Formatted as code automatically.
      render: text

  - type: textarea
    id: context
    attributes:
      label: Additional context
      description: Screenshots, video, related issues, workarounds.
