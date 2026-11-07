# bakery

Templates for [SlicedBread](https://slicedbread.dev): self-hosted internal tools that an agent can deploy and operate, with backups that are restore-tested.

Each directory is one template in the format described in SlicedBread's `docs/template-format.md`:

```
umami/
  template.yaml    components, access, secrets, settings, backup, actions, setup
  actions/         scripts that run as short jobs next to the app
  files/           config file templates (if any)
  README.md
```

## How templates reach an install

On every push to `main`, CI checks every template with SlicedBread's own validator, builds one catalog (`catalog.json`: all template files, versions and image digests), signs it with the bakery minisign key, and publishes it as a release. Every SlicedBread install has the public key built in. It starts with the catalog that shipped with it, and picks up a newer one with `sb catalog --refresh` or its daily check. A catalog that is not signed by the key, has been changed, or is older than the one in use is refused. Apps already deployed keep the template version they were created from.

Public key (key ID `D152C8A09D73E5EA`):

```
untrusted comment: minisign public key: D152C8A09D73E5EA
RWTq5XOdoMhS0emtzQd35R9xM8zrXS6lhJO+1zAwwbrjFmsVx6pa5K1D
```

Check a release by hand: `minisign -Vm catalog.json -P RWTq5XOdoMhS0emtzQd35R9xM8zrXS6lhJO+1zAwwbrjFmsVx6pa5K1D`.

## Tiers

Only verified templates are offered for now: each one is deployed and checked on real servers before it is added. Community templates may come later.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).

## Licence

The templates and scripts in this repository are licensed under the [Apache License 2.0](LICENSE). The software they deploy keeps its own licence, named in each template's `upstream` section.
