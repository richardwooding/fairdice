# fairdice

Commit-reveal dice (and fair randomness) between untrusting peers — no
trusted server, no dealer. Useful for peer-to-peer games where either side
could otherwise cheat the roll.

A roll is a three-message exchange:

1. The roller generates random bytes and broadcasts a commitment
   `SHA-256(deterministic-CBOR(reveal))`.
2. The opponent broadcasts its own random contribution.
3. The roller reveals; everyone (spectators too) checks the reveal against
   the commitment and derives the dice from the combined randomness.

The roller can't pick the outcome (it's bound before seeing the opponent's
contribution) and the opponent can't either (any contribution yields uniform
dice against the unknown-but-fixed commitment). A reveal that doesn't match
its commitment is caught by every participant.

```go
reveal, commit, _ := fairdice.NewRoll()      // roller: broadcast commit
rb, _ := fairdice.NewResponse()              // opponent: broadcast rb
// roller broadcasts reveal; everyone verifies + derives:
if fairdice.Verify(commit, reveal) {
    d1, d2 := fairdice.Dice(reveal, rb)      // uniform 1..6 each, deterministic
}
```

`Opening` derives an unequal opening pair (the backgammon convention).
Dice are uniform via rejection sampling over an expandable hash stream.

## Install

```sh
go get github.com/richardwooding/fairdice
```

Extracted from [kibitz](https://github.com/richardwooding/kibitz), where it
drives provably-fair backgammon dice.

## License

MIT
