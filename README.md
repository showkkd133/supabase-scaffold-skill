# supabase-scaffold-skill

A Claude Code skill that generates full-stack Supabase project scaffolding from natural language data model descriptions.

## What it does

Given a natural language description of your data model (e.g., "Users can create projects, each project has multiple tasks with status tracking"), this skill generates:

- **SQL Migrations** with Row Level Security (RLS) policies
- **TypeScript type definitions** in Supabase Database format (`Row`, `Insert`, `Update`)
- **Next.js App Router API routes** (GET / POST / PUT / DELETE with pagination)
- **Zod validation schemas** (create, update, query params)
- **Auth configuration** (email/password, OAuth, Magic Link)
- **React form components** (powered by react-hook-form + zod)
- **Relationship support** (foreign keys, many-to-many junction tables)

## RLS Best Practices

The skill includes three battle-tested RLS patterns:

| Pattern | Use Case | Strategy |
|---------|----------|----------|
| **Owner-based** | Users own their data | `auth.uid() = user_id` |
| **Role-based** | Admin / Editor / Viewer | JWT role claim or `user_roles` table |
| **Org-based** | Multi-tenant organizations | Membership check via `org_members` table |

## Trigger Phrases

Use any of these in Claude Code to activate the skill:

- "supabase scaffold"
- "generate data model"
- "create supabase table"
- "supabase migration"

## Installation

### As a Claude Code Skill

Copy the skill file to your Claude skills directory:

```bash
mkdir -p ~/.claude/skills/supabase-scaffold
cp skills/supabase-scaffold/SKILL.md ~/.claude/skills/supabase-scaffold/SKILL.md
```

### As a Claude Code Plugin

Add this repository as a plugin in your Claude Code configuration.

## Example Usage

Tell Claude Code something like:

> "I need a supabase scaffold for a blog platform. Users can create posts with title, content, and status (draft/published/archived). Posts can have multiple tags (many-to-many). Each post can have comments from authenticated users."

The skill will:

1. Extract entities: `posts`, `tags`, `posts_tags`, `comments`
2. Determine relationships and ownership model
3. Generate SQL migrations with RLS
4. Generate TypeScript types, Zod schemas, API routes, and form components
5. Run a quality checklist to catch common pitfalls

## Common Pitfalls Covered

The skill actively warns about:

- Forgetting to enable RLS on new tables
- Leaking `service_role` key to the client
- Missing DELETE policies
- Using `getSession()` instead of `getUser()` on the server
- Junction tables without RLS
- Timestamps without timezone (`timestamp` vs `timestamptz`)
- Float precision issues for currency (use `numeric` instead)

## Tech Stack

- [Supabase](https://supabase.com) (PostgreSQL + Auth + Storage)
- [Next.js](https://nextjs.org) App Router
- [TypeScript](https://www.typescriptlang.org)
- [Zod](https://zod.dev) validation
- [react-hook-form](https://react-hook-form.com)

## License

MIT - see [LICENSE](./LICENSE)
