# Frontend Technology Guidelines

This is the **default technology stack and convention for frontend projects**.

The objective is to keep projects consistent, maintainable, and easy for engineers to move between.

These are **not strict rules**. Teams may deviate when there is a clear technical reason, but the decision should be documented.

## Core Stack

| Concern              | Default Choice                                   | Docs                          |
| -------------------- | ------------------------------------------------ | ----------------------------- |
| Language & Framework | TypeScript + React                               | https://react.dev             |
| Project Structure    | Feature-Sliced Design                            | https://feature-sliced.design |
| Package Manager      | pnpm                                             | https://pnpm.io               |
| Build/Dev Tool       | Vite                                             | https://vite.dev              |
| Styling              | Tailwind CSS                                     | https://tailwindcss.com       |
| Component Library    | shadcn/ui                                        | https://ui.shadcn.com         |
| Routing              | React Router                                     | https://reactrouter.com       |
| HTTP Client          | Axios / Fetch                                    | https://axios-http.com        |
| State Management     | Redux Toolkit — only for shared or complex state | https://redux.js.org          |
| Linting              | ESLint                                           | https://eslint.org            |

## Project Structure

Use **[Feature-Sliced Design](https://feature-sliced.design)** as the default guideline for organising frontend applications.

Typical structure:

```text
src/
├── app/
├── pages/
├── widgets/
├── features/
├── entities/
└── shared/
```

The main objective is to organise code around **features and business domains** rather than large generic technical folders.

Prefer:

```text
features/
  create-task/

entities/
  task/

shared/
  ui/
  api/
```

instead of allowing folders such as:

```text
components/
hooks/
services/
utils/
```

to become large collections of unrelated code.

Generic folders are acceptable inside a specific feature or domain.

Example:

```text
features/
└── create-task/
    ├── ui/
    ├── model/
    ├── api/
    └── lib/
```

For smaller projects, it is acceptable to use a simplified subset of FSD.

Do not introduce layers that provide no meaningful value.

## State Management

Keep state as close as possible to where it is used.

```text
Local UI State      → React State
Shared / Global     → Redux Toolkit
```

Use React state such as `useState` or `useReducer` for local component state.

Use **[Redux Toolkit](https://redux.js.org)** when state needs to be shared across multiple features or represents application-level state.

Examples:

* Global application settings
* Cross-feature workflow state
* Shared application configuration
* Complex state used across multiple parts of the application

Avoid putting everything into Redux by default.

## API Calls

Use **[Axios](https://axios-http.com)** or native `fetch` for backend API communication.

API calls should generally be kept behind an API layer instead of being scattered directly throughout UI components.

Example:

```text
shared/
└── api/
    ├── client.ts
    └── endpoints/
```

Feature-specific API calls may also live within their corresponding feature.

```text
features/
└── create-task/
    └── api/
```

Advanced server-state libraries such as **[RTK Query](https://redux-toolkit.js.org/rtk-query/overview)** or **[TanStack Query](https://tanstack.com/query/latest)** may be introduced when the project requires:

* API caching
* Automatic refetching
* Cache invalidation
* Optimistic updates
* More complex asynchronous state management

They are **not part of the default stack**.

## UI Components

Use **[shadcn/ui](https://ui.shadcn.com)** as the default starting point for reusable UI components.

Application-independent reusable components should generally live under:

```text
shared/ui/
```

Examples:

```text
shared/ui/button
shared/ui/dialog
shared/ui/table
shared/ui/input
```

Feature-specific components should stay within the corresponding feature or entity.

## Good to Have

The following tools are recommended when the project benefits from them, but they are **not mandatory defaults**.

| Concern                 | Tool                  | Docs                              |
| ----------------------- | --------------------- | --------------------------------- |
| Form Management         | React Hook Form       | https://react-hook-form.com       |
| Runtime Validation      | Zod                   | https://zod.dev                   |
| Component Documentation | Storybook             | https://storybook.js.org          |
| Unit Testing            | Vitest                | https://vitest.dev                |
| Component Testing       | React Testing Library | https://testing-library.com/react |
| E2E Testing             | Playwright            | https://playwright.dev            |
| Formatting              | Prettier              | https://prettier.io               |

These tools should be introduced when they solve a clear project need rather than being added automatically.

## General Principles

Prefer:

* Consistent project structure
* Feature/domain-oriented organisation
* Simple solutions over unnecessary abstractions
* Local state over premature global state
* Reusable UI components where appropriate
* Strong TypeScript typing
* Adding libraries only when they solve a clear problem

Consistency between projects is more important than enforcing every library listed above.
