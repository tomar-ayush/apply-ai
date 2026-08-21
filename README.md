# Apply AI

This is the umbrella repository for the Apply AI project. It uses Git submodules to reference the different components of the system, keeping everything accessible from a single location without needing a monolithic repository.

## Components

- **[backend](./backend/)**: The backend service for the resume agent.
- **[frontend](./frontend/)**: The frontend interface for the resume agent.
- **[lambda-pdf](./lambda-pdf/)**: The LaTeX engine Lambda service for PDF generation.
- **[linkedin_note_extension](./linkedin_note_extension/)**: The ApplyAI assistant extension for LinkedIn.

## Setup

To clone this repository and initialize all submodules, run:

```bash
git clone --recurse-submodules <repository-url>
```

If you have already cloned the repository without the submodules, you can fetch them using:

```bash
git submodule update --init --remote
```

> **Note**: The submodules are configured to track their respective `main` branches. You can update all submodules to the latest commits on their `main` branches by running `git submodule update --remote`.
