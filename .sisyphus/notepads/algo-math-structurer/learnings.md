# Learnings - algo-math-structurer

## Task 1: Directory Structure & Manifests
- Claude Cowork plugins are 100% markdown + JSON — no code, no build steps
- plugin.json lives at `.claude-plugin/plugin.json` (NOT at root)
- .mcp.json lives at plugin root
- Skills go in `skills/{skill-name}/SKILL.md`
- Commands go in `commands/{command-name}.md`
- Name must be kebab-case, match directory name
- plugin.json required fields: name, version, description, author
- Standalone plugins use empty `mcpServers: {}` in .mcp.json

## Task 2: README.md
- README lives at plugin root: `algo-math-structurer/README.md`
- Required sections: Description, Skills, Commands, Setup Instructions, Usage Examples, Limitations, Customization Tips
- 83 lines written, all 7 sections verified present
- Validation evidence saved to `.sisyphus/evidence/task-2-readme-validation.txt`
