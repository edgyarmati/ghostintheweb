# TASKS.md

## Task 1: Update upstream `pi-ai` to add DeepSeek provider
- [ ] 1.1 Add `"deepseek"` to `KnownProvider` in `../pi-mono/packages/ai/src/types.ts`
- [ ] 1.2 Add DeepSeek models (`deepseek-v4-flash`, `deepseek-v4-pro`) to `../pi-mono/packages/ai/src/models.generated.ts`
- [ ] 1.3 Rebuild `pi-ai` (`cd ../pi-mono/packages/ai && npm run build`)

## Task 2: Update Sitegeist UI to expose providers
- [ ] 2.1 Remove `"opencode-go"` from `HIDDEN_PROVIDERS` in `src/dialogs/ApiKeysOAuthTab.ts`
- [ ] 2.2 Add `deepseek: "deepseek-v4-flash"` to `DEFAULT_MODELS` in `src/sidepanel.ts`

## Task 3: Verification
- [ ] 3.1 Run `./check.sh` in sitegeist root
- [ ] 3.2 Fix any type errors or lint warnings

## Task 4: Changelog and Commit
- [ ] 4.1 Add entries to `CHANGELOG.md` under `[Unreleased]`
- [ ] 4.2 Stage and commit with conventional commit format
