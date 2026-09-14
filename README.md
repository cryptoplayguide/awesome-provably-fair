# Awesome provably fair [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

> A curated list of standards, randomness sources, reference implementations and maths for **provably fair** systems: games and draws whose outcomes are committed before play and can be recomputed by anyone afterwards.

"Provably fair" means two checkable claims. The outcome was fixed before you acted, because the operator published a commitment to its secret first. And the outcome was computed the published way, because you can rerun the computation from the revealed inputs. It says nothing about whether the odds are good: a provably fair game can still carry a house edge, and this list includes material for pricing that too.

Maintained by [Betkyo Research](https://github.com/betkyo-open-labs). Entries we publish ourselves are marked *(maintainer)*. Contributions of neutral, technical resources are welcome; see [Contributing](#contributing).

## Contents

- [Concepts](#concepts)
- [Standards and primitives](#standards-and-primitives)
- [Public randomness beacons](#public-randomness-beacons)
- [Verifiable randomness on-chain](#verifiable-randomness-on-chain)
- [Implementation pitfalls](#implementation-pitfalls)
- [Verifiers and tools](#verifiers-and-tools)
- [Data and maths of the house edge](#data-and-maths-of-the-house-edge)
- [Contributing](#contributing)

## Concepts

- [Commitment scheme](https://en.wikipedia.org/wiki/Commitment_scheme) - Hiding and binding: the basis of committing to a server seed with its hash before play.
- [Verifiable random function](https://en.wikipedia.org/wiki/Verifiable_random_function) - A random output that comes with a proof anyone can check against a public key.
- [Randomness extractor](https://en.wikipedia.org/wiki/Randomness_extractor) - Turning imperfect entropy into near-uniform bits.

## Standards and primitives

- [RFC 2104: HMAC](https://www.rfc-editor.org/rfc/rfc2104) - The keyed hash most seed-based schemes use as HMAC(serverSeed, clientSeed:nonce:cursor).
- [RFC 6234: US Secure Hash Algorithms](https://www.rfc-editor.org/rfc/rfc6234) - SHA-2 reference code and test vectors.
- [FIPS 180-4: Secure Hash Standard](https://csrc.nist.gov/pubs/fips/180-4/upd1/final) - The normative SHA-256 specification.
- [RFC 9381: Verifiable Random Functions](https://www.rfc-editor.org/rfc/rfc9381) - ECVRF, the standardised VRF construction.
- [SubtleCrypto.sign()](https://developer.mozilla.org/en-US/docs/Web/API/SubtleCrypto/sign) - HMAC in the browser without dependencies, for client-side verifiers.

## Public randomness beacons

- [drand](https://drand.love/) - Distributed, publicly verifiable randomness produced by threshold BLS signatures ([docs](https://docs.drand.love/)).
- [League of Entropy](https://www.cloudflare.com/leagueofentropy/) - The consortium operating the main drand network.
- [NIST Randomness Beacon](https://beacon.nist.gov/home) - Timestamped, signed 512-bit random values published every minute.

## Verifiable randomness on-chain

- [Chainlink VRF](https://docs.chain.link/vrf) - Verifiable random numbers delivered to smart contracts with on-chain proof verification.
- [Randomness in Ethereum proof-of-stake](https://eth2book.info/latest/part2/building_blocks/randomness/) - How RANDAO works and where its bias limits lie.

## Implementation pitfalls

- [Modulo bias](https://en.wikipedia.org/wiki/Fisher%E2%80%93Yates_shuffle) - Why `bytes % n` is not uniform, and how shuffles avoid it (see the section on implementation errors).
- [Double-precision floating point](https://en.wikipedia.org/wiki/Double-precision_floating-point_format) - Converting hash bytes to a number in [0, 1): a verifier must use the same rounding as the server or it will disagree on edge cases.
- Nonce and cursor reuse - Every draw needs a distinct (seed, nonce, cursor) triple; reusing one repeats the outcome.
- Seed rotation - A server seed must be revealed only after it is retired, and a new commitment published before the next round.

## Verifiers and tools

- [provably-fair-verifier](https://github.com/betkyo-open-labs/provably-fair-verifier) *(maintainer)* - Zero-dependency Node CLI and library: HMAC-SHA256 cursor streams, single-rounding byte-to-float, several game mappings. MIT.
- [Provably Fair Round Verifier](https://huggingface.co/spaces/Betkyo/provably-fair-verifier) *(maintainer)* - The same computation in a static web page using Web Crypto; nothing leaves the browser.
- [GitHub topic: provably-fair](https://github.com/topics/provably-fair) - Community implementations to compare against.

## Data and maths of the house edge

- [House edge](https://wizardofodds.com/gambling/house-edge/) - Reference table of house edges across casino games.
- [Hypergeometric distribution](https://en.wikipedia.org/wiki/Hypergeometric_distribution) - The exact hit probabilities behind keno and lottery-style draws.
- [odds-data](https://github.com/betkyo-open-labs/odds-data) *(maintainer)* - Paytables and outcome distributions exported from a game engine's source, CC BY 4.0 ([DOI](https://doi.org/10.5281/zenodo.22725012)).
- [odds-derivations](https://github.com/betkyo-open-labs/odds-derivations) *(maintainer)* - Scripts that derive the returns and charts from those tables.

## Contributing

Open a pull request that adds one resource per line in the form `- [Name](link) - Description.` Resources should be technical and neutral: specifications, papers, open-source code, datasets and explanations. Please do not submit casino promotions, bonus or referral links, or reviews. Disclose any affiliation in the pull request.

## License

[![CC0](https://licensebuttons.net/p/zero/1.0/88x31.png)](https://creativecommons.org/publicdomain/zero/1.0/)

To the extent possible under law, the contributors have waived all copyright and related rights to this list.
