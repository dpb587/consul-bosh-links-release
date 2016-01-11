An alternative interpretation of [links](https://github.com/cloudfoundry/bosh-notes/blob/master/links.md) focused around more dynamic, consul-based service discovery.

Notable Differences:

 * link properties are accessed via `p('_links.{template_name}.{link_method}.{link_name}.properties.{property_name}')` (remember to add `_links` to your release's job spec if you want access)
 * link properties are defined for providers in the deployment's global properties at `properties._links.{provider_link_name}.{property_name}`
 * rather than `consumes` specifying a network, `provides` specifies it to indicate which IP it advertises
 * uses `consumes_v2` and `provides_v2` in job `spec` files since current BOSH has an old implementation

Create a file which maps releases to your working directories so it can easily lookup the provides/consumes spec metadata...

    $ cat ~/bosh-release-source.yml 
    ---
    openvpn:
      path: "~/Projects/dpb587--openvpn-bosh-release"
      branch: "master"
    ...snip...

Make your deployment manifest through `links-filter` to have it read, parse, and resolve link information...

    _transformers:
      - exec: "~/Projects/dpb587--consul-bosh-links-release/bin/links-filter"


## License

[MIT License](./LICENSE)
