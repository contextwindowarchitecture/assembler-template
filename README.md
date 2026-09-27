# cwa-assembler-template

A template for a [Context Window Architecture](https://contextwindowarchitecture.io) (CWA) assembler in a new language. It holds what the Python reference assembler and the TypeScript assembler share, and what every further port needs on day one: the working rules, a porting guide, the license, CI, and three bootstrap scripts that vendor the published contract, run the conformance cases against a port written in any language, and check the report the website imports.

It holds no assembler code. A port is built from the published spec alone, test-first, and the spec is copied in by the first command below.

## Quick start

```sh
# 1. Create the port from this template (PORTING.md, step 1), then vendor the contract from a website checkout:
python3 scripts/vendor_contract.py --website ../website
python3 scripts/vendor_contract.py --verify

# 2. Read, in this order:
#    vendor/cwa/conformance/README.md, vendor/cwa/contract/requirements.json, vendor/cwa/schema/

# 3. Build the port test-first, in the order PORTING.md gives, and run the cases through a small adapter:
python3 scripts/conformance.py --command "<adapter>" --name <package> --version 0.0.1 --language <Language>
python3 scripts/check_report.py --allow-failures
```

## What is here

| File | Purpose |
| --- | --- |
| `AGENTS.md` | The standing rules for agents and contributors, with placeholders for the language's commands; `CLAUDE.md` imports it |
| `PORTING.md` | The guide: what a port is, the files it reads, the guard tests, the build order, the adapter protocol, how the website picks the port up, and the portability checklist |
| `LICENSE`, `NOTICE` | Apache-2.0, the license of the specification; a port's LICENSE must equal the vendored one |
| `cliff.toml` | git-cliff configuration, so the changelog is generated from the Conventional Commit history |
| `.github/workflows/ci.yml` | CI: the vendored contract against the website commit it pins, and the committed report against the contract; add the language's test job |
| `scripts/vendor_contract.py` | Copies the contract from a website checkout into `vendor/cwa/`, pinned by SHA-256 in `vendor/cwa.lock.json`; checks drift; verifies the lock |
| `scripts/conformance.py` | Runs every case and rejection through an adapter command and writes `conformance-report.json` |
| `scripts/check_report.py` | Checks a report, whichever runner wrote it, against the schema, the lock and the case directories |

The scripts need Python 3.10 or newer and the standard library; the `jsonschema` package is optional and adds schema validation. A port may replace them with native tools that do the same.

## The implementations so far

| Language | Repository | Package |
| --- | --- | --- |
| Python | `contextwindowarchitecture/assembler-python` | the reference assembler |
| TypeScript | `contextwindowarchitecture/assembler-typescript` | `@contextwindowarchitecture/assembler` |

A port never reads them. Each was built from the vendored contract alone, and the two behaviors on which they disagreed while both passed every case became spec fixes with a case each. That independence is the point of another port.

## License

Apache License 2.0, the same as the specification: see [LICENSE](LICENSE) and [NOTICE](NOTICE).
