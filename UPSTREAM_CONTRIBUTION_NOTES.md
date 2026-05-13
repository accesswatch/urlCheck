# Upstream Contribution Notes

This fork has local changes intended to make urlCheck more useful for automated accessibility crawls. The items below separate generally useful improvements from GLOW-specific conveniences so we can decide what to propose back upstream.

## Good upstream candidates

- Recursive same-origin crawl mode with `--crawl` and `--crawl-limit`.
- Silent crawl default: `--crawl` runs invisibly unless `--authenticate` is set. Non-auth invisible scans now use Playwright's bundled headless Chromium instead of system Edge to avoid focus stealing during automation.
- Non-HTML crawl filtering so download links, CSV exports, and media files are skipped during page discovery instead of being opened as browser pages.
- HTTP error reporting: crawler keeps HTTP error URLs in the scan list, and page scans report statuses such as `HTTP 404` under failed scans.
- Generic request headers with repeatable `--header "Name: Value"`.
- Environment-backed request headers with repeatable `--header-env Name=ENV_VAR`, so secrets do not need to appear in shell history.
- Header value redaction in urlCheck logs and command-line diagnostics.

## GLOW-specific convenience

- `--glow-consent-token` and the default `X-GLOW-Automation-Consent: GLOW` header are useful for GLOW testing, but the generic `--header-env` option is the upstream-friendly version of the same capability.

## Validation run locally

- `py -3.13 -m py_compile .\urlCheck.py`
- `py -3.13 .\urlCheck.py https://glow.bits-acb.org --crawl --crawl-limit 3 --header-env X-UrlCheck-Test=URLCHECK_TEST_HEADER --force -o D:\code\urlcheck\reports-smoke-headless` scanned 3 URLs without requiring `--invisible`.

## Open follow-up ideas

- Add a machine-readable crawl summary file that lists scanned, failed, skipped-non-HTML, and HTTP-error URLs separately.
- Consider GUI fields for custom headers if Jamal wants the feature exposed outside CLI workflows.