# Praxis Developer – Git, GitHub, and Vercel Protocol

This is the standard protocol for bootstrapping a new Praxis project with GitHub and Vercel. Agent0 must assume this protocol has already been executed and must not re-initialize git or scaffold in a different directory.

## Naming

Canonical names (example):
- Local project directory: `/a0/usr/projects/praxis_a0`
- GitHub repo: `Praxis.A0`

Avoid typos or alternate directories.

## Git + GitHub setup (manual)

1. Create GitHub repo:
   - Name: `Praxis.A0`
   - Empty (no README, no .gitignore, no license).

2. On the server (or dev machine):

```bash
cd /a0/usr/projects
mkdir praxis_a0
cd praxis_a0

git init

cat > .gitignore << 'EOF'
.a0proj/
node_modules/
.next/
dist/
out/
.env
.env.local
EOF

git add .gitignore
git commit -m "chore: add base gitignore"

git remote add origin https://github.com/<ORG_OR_USER>/Praxis.A0.git
git branch -M master
git push -u origin master
```

## Next.js scaffold (manual, not by Agent0)

In `/a0/usr/projects/praxis_a0`:

```bash
npx create-next-app@latest . \
  --ts \
  --use-npm \
  --eslint \
  --tailwind \
  --src-dir false \
  --app \
  --import-alias "@/*"
```

Then:

```bash
git add .
git commit -m "feat: scaffold Praxis Developer MVP app"
git push
```

From this point on:
- The GitHub repo is live with a Next.js 14 + TS + Tailwind scaffold.
- Vercel can import the project directly from GitHub.

## Vercel project (UI)

In Vercel:
- Create new project from the `Praxis.A0` GitHub repo.
- Root directory: `./`
- Framework: Next.js (auto)
- Deploy.

Agent0 must assume:
- Git is initialized and connected.
- The Next.js scaffold already exists at the repo root.
- Vercel is already linked to the GitHub repo.
