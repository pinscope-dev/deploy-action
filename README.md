# Pinscope deploy

Uploads a firmware build to [Pinscope](https://pinscope.dev) and, if you want,
rolls it out to your devices in waves: a few first, the rest only when those
came back on the new firmware and did not crash.

```yaml
- uses: pinscope-dev/deploy-action@v1
  with:
    token: ${{ secrets.PINSCOPE_TOKEN }}   # an API token that may deploy
    project: 12                           # from the dashboard URL
    files: build/app.bin build/app.elf
    sbom: build/app.spdx                  # optional: idf.py sbom-create, for vulnerability tracking
    rollout: true
    tag: beta                             # optional: only devices tagged beta
    wait: true                            # fail the job if the rollout stops
```

Works for ESP32 (`build/<project>.bin`), Zephyr with MCUboot
(`build/zephyr/zephyr.signed.bin` and `zephyr.elf`), the Pico 2 W (`.uf2`) and
any file for the Linux agent. Without GitHub, the same in any CI:

```sh
curl -fsSL https://pinscope.dev/ci.sh | sh -s -- --token "$PINSCOPE_TOKEN" --project 12 \
  build/app.bin build/app.elf --rollout --wait
```
