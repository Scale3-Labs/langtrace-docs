# Langtrace Documentation

Welcome to the official documentation repository for Langtrace. We welcome contributions from everyone. This guide will help you get started with contributing to our documentation.

## Table of Contents

- [Introduction](#introduction)
- [Getting Started](#getting-started)
- [Contributing Guidelines](#contributing-guidelines)
- [Submitting Changes](#submitting-changes)
- [Code of Conduct](#code-of-conduct)
- [License](#license)

## Introduction

This repository contains the source files for the Langtrace documentation. Our goal is to provide clear, accurate, and helpful documentation for all users.

## Getting Started

To contribute to the documentation, follow these steps:

1. **Fork the repository**: Click the "Fork" button at the top right corner of this page to create a copy of this repository under your GitHub account.
2. **Clone your fork**: Clone your forked repository to your local machine.
   ```bash
   git clone https://github.com/your-username/your-fork.git
   ```
3. **Create a new branch**:
   ```bash
   git checkout -b feature-branch
   ```

### Making Changes

1. Make your changes to the docs.
2. Test your changes thoroughly.
3. Commit your changes with a clear and descriptive commit message:

`git commit -m "Description of your changes"`

#### Submitting a Pull Request

1. Push your changes to your fork:

```bash
git push origin feature-branch
```

2. Open a pull request from your fork to the main repository.
3. In the pull request description, provide a clear explanation of your changes and why they should be merged.

### Development

Install the [Mintlify CLI](https://www.npmjs.com/package/mintlify) to preview the documentation changes locally. To install, use the following command

```
npm i -g mintlify
```

Run the following command at the root (where mint.json is)

```
mintlify dev
```

#### Troubleshooting

- Mintlify dev isn't running - Run `mintlify install` it'll re-install dependencies.
- Page loads as a 404 - Make sure you are running in a folder with `mint.json`

### Contributing Guidelines

To ensure consistency and quality in our documentation, please follow these guidelines:

• Markdown Standards: Use Markdown for all documentation files.
• Clarity and Accuracy: Ensure that your contributions are clear, accurate, and well-organized.

Thank you for contributing!
