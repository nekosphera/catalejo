# Catalejo

**Federated DCAT catalogue for EDC data spaces.**

A catalejo is a spyglass: an instrument for seeing what is far away. This one
lets a data space participant see the catalogues of the others from where it
stands, as one DCAT graph it can query, and refuses metadata that does not
conform to the profile the space agreed on.

It does three things and nothing else:

- **Federates.** Reads the catalogue of every connector it is given — assets,
  policy definitions and contract definitions — and keeps an RDF graph in step
  with them.
- **Writes only what changed.** The graph is compared with the catalogue the
  run wants, and only the difference is written. An unchanged catalogue writes
  nothing, which is what keeps a triple store from growing without bound.
- **Validates.** Checks a metadata record against a DCAT-AP profile and its
  SHACL shapes, and answers which constraint failed and where.

It does not do identity, contract negotiation, transfer or policy enforcement.
Those belong to the connector.

## Quickstart

```bash
export FUSEKI_ADMIN_PASSWORD=change-me
export CONNECTOR1_NAME=connector-1
export CONNECTOR1_URL=https://your-connector.example.org
export CONNECTOR1_CLIENT_SECRET=…
export KEYCLOAK_URL=https://auth.example.org

docker compose up -d
```

The catalogue is then a SPARQL endpoint on `127.0.0.1:3030`:

```sparql
SELECT ?title ?publisher ?accessURL WHERE {
  GRAPH ?g {
    ?dataset a dcat:Dataset ;
             dct:title ?title ;
             dct:publisher ?publisher ;
             dcat:distribution/dcat:accessURL ?accessURL .
  }
}
```

To validate a record without running anything else:

```bash
pip install -r requirements.txt
PYTHONPATH=src python3 -m catalejo.cli \
  vocabularies/dcat-ap/1.0.0/shapes.ttl my-dataset.json
```

Exit code 0 when it conforms, 1 when it does not, so it can gate a publication
pipeline.

## Proving it

`./smoke.sh` stands the whole thing up against a stub data space, federates,
queries the graph and checks that a conforming record is accepted and a
non-conforming one refused. It needs docker, python3, curl and jq, and leaves
nothing behind.

It exists because a quickstart nobody has run is a README, not a quickstart.

## What it speaks

- **Dataspace Protocol** — through the connectors it reads; the EDC management
  API v3 is what it calls.
- **DCAT** — `dcat:Dataset`, `dct:*`, one `dcat:keyword` per keyword, and a
  `dcat:Distribution` carrying `dcat:accessURL` for the means of access.
- **SHACL** — the profile in `vocabularies/dcat-ap/` decides conformance. The
  required properties are read from the shapes, never restated beside them.
- **ODRL** — policy metadata is carried into the catalogue, so an offering can
  be found together with the terms it is offered under.

`myds:deliveryMode` is an extension of the profile this was extracted from,
not part of DCAT-AP. Replace `vocabularies/dcat-ap/` with your own profile and
the validator follows it: it reads whatever shapes it is given.

## Observability

`observability/` carries an nginx configuration that records the state
transitions of the protocol — cataloguing, contract negotiation and transfer —
as one JSON object per line, classified by state machine, with the status kept
as the success or the failure. It is optional and it needs no credential for
anybody's management API: the requests are recorded where they already pass.

## Known limits, stated rather than discovered

- **The Fuseki image is community-maintained.** There is no Apache-published
  Fuseki image: the project ships a Dockerfile you build yourself. The compose
  pins by digest the image this catalogue is operated with, whose last
  publication was July 2024. Building from Apache's own `jena-fuseki-docker`
  and publishing the result by digest is the better answer and is not done
  here yet.
- **The federator needs an OIDC provider** to obtain a token for each
  connector. A connector that does not require one is not supported today.
- **Federation is polled**, on an interval. There is no subscription.
- The unit tests live upstream, in the repository this is exported from. What
  ships here is the smoke test, which is the one that proves the bundle rather
  than its parts.

## Licence

Apache License 2.0. Third-party components keep their own licences; see
`THIRD_PARTY_NOTICES.md`.
