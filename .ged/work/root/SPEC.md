# SPEC.md: Add DeepSeek and OpenCode Go Provider Options

## Goal
Expose DeepSeek and OpenCode Go as API-key-based provider options in the Sitegeist browser extension.

## Background
- OpenCode Go already exists as a `KnownProvider` in the upstream `@mariozechner/pi-ai` package but is hidden from Sitegeist's UI via `HIDDEN_PROVIDERS`.
- DeepSeek does not yet exist as a first-class `KnownProvider` in `pi-ai`. DeepSeek models are only available through aggregator providers (OpenRouter, Fireworks, etc.).
- The user confirmed DeepSeek v4-flash and v4-pro are the desired models, accessible via `api.deepseek.com`.

## Architecture
- **Upstream (`../pi-mono/packages/ai`)**: Provider registry (`types.ts`), model registry (`models.generated.ts`), streaming implementation (`openai-completions.ts`).
- **Sitegeist (`src/`)**: UI hiding logic (`ApiKeysOAuthTab.ts`), default model mapping (`sidepanel.ts`), generic API key storage (`ProviderKeysStore`).

## Changes Required

### 1. Upstream `pi-ai` package (`../pi-mono/packages/ai`)
- Add `"deepseek"` to `KnownProvider` union in `src/types.ts`.
- Add DeepSeek model entries (`deepseek-v4-flash`, `deepseek-v4-pro`) to `src/models.generated.ts` with:
  - `api`: `"openai-completions"` (DeepSeek API is OpenAI-compatible)
  - `baseUrl`: `"https://api.deepseek.com"`
  - `reasoning`: `true` (both models support thinking mode)
  - Costs and context windows per official pricing docs
- The `openai-completions.ts` provider already has `baseUrl.includes("deepseek.com")` detection, so no streaming changes are needed.
- Rebuild `pi-ai` (`npm run build`).

### 2. Sitegeist Extension
- **Unhide OpenCode Go**: Remove `"opencode-go"` from `HIDDEN_PROVIDERS` in `src/dialogs/ApiKeysOAuthTab.ts`.
- **Unhide DeepSeek**: Remove `"deepseek"` from `HIDDEN_PROVIDERS` (it will be added by `getProviders()` once upstream is rebuilt).
- **Default model mapping**: Add `deepseek: "deepseek-v4-flash"` to `DEFAULT_MODELS` in `src/sidepanel.ts`. (`opencode-go` already has a default.)

## Acceptance Criteria
- [ ] DeepSeek appears in the API Keys settings tab with a key input field.
- [ ] OpenCode Go appears in the API Keys settings tab with a key input field.
- [ ] Both providers show up in the model selector after entering an API key.
- [ ] `check.sh` (biome + tsc) passes without errors.

## Files Modified
| File | Action |
|------|--------|
| `../pi-mono/packages/ai/src/types.ts` | Add `"deepseek"` to `KnownProvider` |
| `../pi-mono/packages/ai/src/models.generated.ts` | Add DeepSeek models |
| `../pi-mono/packages/ai/dist/*` | Rebuild artifacts |
| `src/dialogs/ApiKeysOAuthTab.ts` | Remove `opencode-go` and `deepseek` from `HIDDEN_PROVIDERS` |
| `src/sidepanel.ts` | Add `deepseek` default model |
| `CHANGELOG.md` | Record changes under `[Unreleased]` |
