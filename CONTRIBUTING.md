# Contributing to rbot

Thank you for your interest in contributing. This document covers the process for reporting issues, proposing changes, and submitting pull requests.

---

## Table of Contents

- [Contributing to rbot](#contributing-to-rbot)
  - [Table of Contents](#table-of-contents)
  - [Code of Conduct](#code-of-conduct)
  - [Getting Started](#getting-started)
  - [How to Contribute](#how-to-contribute)
    - [Reporting Bugs](#reporting-bugs)
    - [Suggesting Features](#suggesting-features)
    - [Submitting Pull Requests](#submitting-pull-requests)
  - [Development Workflow](#development-workflow)
  - [Coding Standards](#coding-standards)
  - [Commit Messages](#commit-messages)
  - [Questions?](#questions)

---

## Code of Conduct

All contributors are expected to interact respectfully. Please be constructive and professional in all discussions.

---

## Getting Started

1. Fork the repository and clone your fork:
   ```bash
   git clone https://github.com/rlxai/rbot.git
   cd rbot
   ```

2. Set up the environment using Docker (recommended):
   ```bash
   docker build -f docker/Dockerfile.gazebo -t rlai-bot:dev .
   ```

   Or natively on Ubuntu 24.04 with ROS 2 Jazzy:
   ```bash
   bash scripts/install_deps.sh
   bash scripts/build.sh
   ```

3. Create a feature branch:
   ```bash
   git checkout -b feat/your-feature-name
   ```

---

## How to Contribute

### Reporting Bugs

Open a [GitHub Issue](https://github.com/rlxai/rbot/issues) and include:

- ROS 2 distribution and Gazebo version (`ros2 --version`, `gz sim --version`)
- Whether you are running natively or in Docker
- Steps to reproduce the problem
- Expected vs actual behaviour
- Relevant log output or screenshots

### Suggesting Features

Open a GitHub Issue with the label `enhancement`. Describe:

- The problem you are trying to solve
- Your proposed solution
- Any alternatives you considered

### Submitting Pull Requests

1. Ensure your branch is up to date with `main`:
   ```bash
   git fetch upstream
   git rebase upstream/main
   ```

2. Run the build and lint checks locally before opening a PR:
   ```bash
   bash scripts/build.sh
   python3 -m pytest tests -q
   cd src && ament_flake8 --max-line-length=100
   ```

   The repository-level tests validate Gazebo models, worlds, and launch argument
   forwarding. CI runs them separately because `colcon test` only discovers tests
   registered by ROS packages.

3. Open a pull request against `main`. In the PR description:
   - Reference any related issues (`Closes #42`)
   - Describe what changed and why
   - Note any breaking changes

4. A maintainer will review your PR. Please respond to feedback in a timely manner.

---

## Development Workflow

| Branch | Purpose |
|---|---|
| `main` | Stable, always builds |
| `develop` | Integration branch for active work |
| `feat/<name>` | New features |
| `fix/<name>` | Bug fixes |
| `docs/<name>` | Documentation-only changes |

PRs should target `main` (or `develop` for large features).

---

## Coding Standards

**C++ packages** (`rlai_description`, `rlai_gazebo`, `rlai_control`, etc.)
- Follow the [ROS 2 C++ style guide](https://docs.ros.org/en/jazzy/Contributing/Code-Style-Language-Versions.html)
- `CMAKE_BUILD_TYPE=Release` is enforced by `colcon.meta`

**Python packages** (`rlai_bringup`, `rlai_navigation`, `rlai_mapping`, etc.)
- PEP 8, max line length 100 (enforced by CI via `ament_flake8`)
- Use `rclpy` logging (`self.get_logger()`) rather than `print()`

**SDF / URDF / Xacro**
- Validate with `check_urdf` before submitting changes to robot description files
- New Gazebo worlds must be SDF 1.11 (Harmonic-native)

---

## Commit Messages

Use the [Conventional Commits](https://www.conventionalcommits.org/) format:

```
<type>(<scope>): <short summary>

[optional body]

[optional footer]
```

Common types: `feat`, `fix`, `docs`, `refactor`, `test`, `ci`, `chore`

Examples:
```
feat(rlai_navigation): add MPPI tuning for narrow corridor scenarios
fix(rlai_gazebo): correct spawn position in small_warehouse world
docs(architecture): add system-level overview diagram
```

---

## Questions?

Open a [GitHub Discussion](https://github.com/rlxai/rbot/discussions) or reach out at **contact@robolabs.ai**.
