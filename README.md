# React Router Best Practices

A specialized collection of performance optimization guidelines and architectural patterns for React Router v7, formatted as "skills" for AI coding agents.

## Overview

This repository hosts the **react-router-best-practices** skill, a comprehensive set of 40+ rules designed to help developers and AI agents build high-performance React Router v7 applications. These rules cover critical areas such as waterfall elimination, bundle size optimization, and server-side data streaming.

## Skills

### react-router-best-practices

This skill provides actionable guidance on:

- **Eliminating Waterfalls:** Utilizing React Router v7 Loaders and `Promise.all` to fetch data in parallel.
- **Streaming & Defer:** Using `defer` to stream slow data without blocking the UI.
- **Bundle Optimization:** Implementing `React.lazy` and `Suspense` for code splitting.
- **Server-Side Performance:** Non-blocking side effects and efficient data serialization in Loaders/Actions.
- **Rendering Optimization:** Reducing re-renders and optimizing DOM interactions.

**Location:** `skills/react-router-best-practices`

## Usage

This repository is structured to be consumed by AI agents to enhance their coding capabilities. The rules are compiled into `AGENTS.md` files which serve as a knowledge base for the agent.

## Credits

Special thanks to the following projects for their inspiration and foundation:

- **[vercel-labs/agent-skills](https://github.com/vercel-labs/agent-skills)** for the original Agent Skills framework and content structure.
- **[remix-run/react-router](https://github.com/remix-run/react-router)** for the powerful routing library that makes these patterns possible.

## License

MIT
