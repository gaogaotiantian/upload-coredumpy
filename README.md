# upload-coredumpy
github action to upload coredumpy dumps

## Usage

```yml
- uses: gaogaotiantian/upload-coredumpy@v0.1
  with:
    # Artifact Name
    name:

    # File, directory or wildcard pattern of coredumpy dumps to upload
    path:

    # Days to keep the artifact
    # Optional. Default is 7
    retension-days:
```

## Example
```yml
jobs:
  build:
    env:
      # Set up an env var for storing data
      COREDUMPY_DUMP_DIR: ${{ github.workspace }}/coredumpy_data
    steps:
      # ...
      - name: Do test
        # or you can do coredumpy.patch_unittest(directory=os.getenv("COREDUMPY_DUMP_DIR"))
        # for `unittest`
        run: pytest --enable-coredumpy --coredumpy-dir "${{ env.COREDUMPY_DUMP_DIR }}"
      - name: Upload coredumpy artifacts
        uses: gaogaotiantian/upload-coredumpy@v0.1
        # Only do this step if the test failed
        if: failure()
        with:
          # Make sure this is unique, you may have different variable in your matrix
          name: coredumpy_data_${{ matrix.os }}_${{ matrix.python-version }}
          path: ${{ env.COREDUMPY_DUMP_DIR }}
```

You will have a vscode URI starting with `vscode://gaogaotiantian.coredumpy-vscode/load-github-artifact`
in your github action log as well as your workflow summary. Open that in your browser to load the dump.
