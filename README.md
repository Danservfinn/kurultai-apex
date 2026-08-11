# kurultai-apex

The front page of [kurult.ai](https://kurult.ai). One static page: identity, a hand-stamped ledger of floored cumulative facts, and a directory of the estate's public surfaces.

## Invariants

- No build system, no framework, no client JavaScript, no webfonts. The committed files are the artifact.
- Every number on the page is a floored cumulative fact with a visible "as of" stamp. Never publish a live counter, a rate, or a current-state claim.
- No legal-entity claims ("LLC") until the NC certificate exists.
- `llms.txt` lists durable public surfaces only — never gated hostnames.
- Design test for any edit: the page must remain correct if never touched again.

## Deploy

Cloudflare Pages, direct upload of the repo root:

```sh
CLOUDFLARE_API_TOKEN=$(cat ~/.kublai/secrets/cloudflare-pages-api-token) \
  npx wrangler@latest pages deploy . --project-name kurultai-apex
```

Break-glass (no CLI): Cloudflare dashboard → Workers & Pages → kurultai-apex → Create deployment → drag this folder in.

Rollback: redeploy any prior deployment from the Pages dashboard (one click).

## Verify after deploy

```sh
curl -sI https://kurult.ai/            # 200 text/html, server: cloudflare
curl -sI https://kurult.ai/llms.txt    # 200 text/plain
curl -sI https://www.kurult.ai/        # 301 -> https://kurult.ai/
dig +short kurult.ai MX                # unchanged — mail records are never touched
```
