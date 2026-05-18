# TESTS.md

## Manual Verification Steps
1. Build the extension (`npm run build`).
2. Open the extension sidepanel.
3. Open Settings > API Keys & OAuth.
4. Confirm **OpenCode Go** appears in the API Keys list with an input field.
5. Confirm **DeepSeek** appears in the API Keys list with an input field.
6. Enter a test API key for DeepSeek and save.
7. Open the model selector and confirm DeepSeek models (v4-flash, v4-pro) are listed.
8. Enter a test API key for OpenCode Go and save.
9. Open the model selector and confirm OpenCode Go models are listed.

## Automated Checks
- `./check.sh` passes:
  - `biome check --write .` (formatting + linting)
  - `tsc --noEmit` (type checking)
  - `cd site && npm run check` (site checks)

## Edge Cases
- If upstream `pi-ai` fails to build, ensure TypeScript version compatibility.
- If `getProviders()` does not include `deepseek` after rebuild, verify `models.generated.ts` has the provider block.
