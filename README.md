# IMSpector

IMSpector is a Python CLI for IMS/SIP smartphone conformance and differential testing.

It helps you:
- validate test suites defined in YAML
- run expected-response checks against one or more devices
- compare behavior between devices to catch interoperability differences
- switch transport backends without changing test logic

## Why IMSpector

- **Interface-first architecture**: transport and message I/O are cleanly separated from test execution.
- **YAML-driven workflows**: test suites and runtime settings stay versioned and reproducible.
- **Real + stub backends**: use `stub` for offline deterministic runs or `sip-proxy-http` for live traffic.
- **Device discovery support**: discover devices from SIP REGISTER flows when using `sip-proxy-http`.

## Quick start

1. Create and activate a Python 3.10+ virtual environment.
2. Install dependencies:

   ```bash
   pip install -r requirements.txt
   ```

3. Validate a test suite:

   ```bash
   python main.py validate --test-config examples/test_suite_register.yaml
   ```

4. List available devices:

   ```bash
   python main.py list-devices --runtime-config examples/runtime_stub.yaml
   ```

5. Run standard checks:

   ```bash
   python main.py run-standard --test-config examples/test_suite_register.yaml --runtime-config examples/runtime_stub.yaml --device pixel_8
   ```

6. Run differential comparison:

   ```bash
   python main.py compare --test-config examples/test_suite_register.yaml --runtime-config examples/runtime_stub.yaml --device pixel_8 --device galaxy_s24
   ```

## Commands

- `validate`: validate test suite YAML.
- `list-devices`: show configured and discovered devices.
- `run-standard`: run expected-response checks for selected devices.
- `compare`: run cross-device differential checks.
- `live-edit-list`: list active `ims_proxy` live-edit rules.
- `live-edit-clear`: clear active `ims_proxy` live-edit rules.

## Exit codes

- `0`: success
- `2`: configuration validation error
- `10`: standard run finished with at least one failed case
- `11`: differential run found differences

## Documentation

- `/docs/user-manual.md` — end-to-end usage guide
- `/docs/test-config.md` — test suite schema
- `/docs/runtime-config.md` — runtime schema and transport config
- `/docs/discovery-methods.md` — device discovery methods
- `/docs/http-sip-proxy.md` — SIP proxy integration details
- `/docs/test-program-architecture.md` — architecture overview

## Project structure

- `/ims_tester/cli.py` — command definitions and execution flow
- `/ims_tester/engine.py` — standard and differential test runners
- `/ims_tester/adapters.py` — pluggable transports and discovery adapters
- `/ims_tester/config.py` — YAML loading + validation
- `/ims_tester/models.py` — domain and report models
- `/examples/` — runnable sample configurations
