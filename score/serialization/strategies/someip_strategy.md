# SOME/IP serialization strategy

`SomeIpStrategy` ([`someip_strategy.h`](someip_strategy.h)) is an example
`SerializationStrategy` for the serialization framework. It plugs into
`Serializer<SomeIpStrategy>` and reuses the shared infrastructure — the
`common/visitor` traversal (field walking) and the `SerializeIterator` (storage)
— so it only has to define the SOME/IP *format*. No serialization-library code is
modified.

## What it does

- Byte order is owned by the strategy and applied uniformly to **all** scalar
  types (SOME/IP default: big-endian).
- Supported so far:
  - all fixed-width integers (signed/unsigned) and enums — big-endian;
  - floating point (IEEE-754 binary32/binary64) — big-endian;
  - `bool` — single byte, `0x00`/`0x01`;
  - `LengthPrefixedVector<T, LengthType>` — `[length][elements]`, **compact**
    (actual size, no capacity padding);
  - `amp::static_vector<T, N>` — `[uint32 length][elements]`, **compact**
    (capacity-bounded, which is what makes `GetMaxSize` finite);
  - `amp::pmr::string` — `[uint32 big-endian length][characters]`, **compact**;
  - nested structs (recursively, via the reflection traversal).
- Scalars are written/read **directly** through the iterator (no temporary byte
  buffer, no extra copy); dynamic data is emitted at its actual size.
- `GetMaxSize<T>()` returns the **worst-case** serialized size of a type, computed
  from its capacities (not a concrete value) using the same reflection traversal.
  It is finite whenever every dynamic member is capacity-bounded — exactly the
  bound LoLa needs to size the shared-memory slot.

This is the key contrast to `UDSStrategy`: dynamic data is not padded to a fixed
capacity.

## Why the strategy pattern

Serialization splits into three concerns; only one varies:

- **traversal** — `common/visitor` (generic, shared);
- **storage** — `SerializeIterator` (span or growing container, bounds-checked);
- **format** — the `SerializationStrategy` (the only thing that differs per
  protocol).

So SOME/IP is a self-contained plug-in next to `UDSStrategy`, with no change to
the framework core and nothing to re-qualify.

## Pros

- Format is a plug-in strategy; reuses traversal + storage, only the format is
  written here.
- Compact, big-endian, standard-oriented output — no fixed overlay, no
  compile-time capacity, no padding.
- No change to the library core; one-way dependency, no cyclic dependency.
- Single byte-order source applied to every scalar type; direct-to-iterator
  writes avoid an extra copy.
- De/serialize both covered with `Result`-based error handling; byte-exact
  round-trip unit tests (`unit_test_someip`).
- `GetMaxSize<T>()` gives the worst-case size for slot allocation, for
  capacity-bounded types (tested in `unit_test_someip`).

## Cons / limitations

- The strategy is a duck-typed concept (implicit `Serialize`/`DeSerialize`
  overloads), not an enforced interface — mistakes surface as template errors.
- `GetMaxSize` is only finite when every dynamic member is capacity-bounded
  (e.g. `amp::static_vector`); unbounded members (`LengthPrefixedVector`,
  `amp::pmr::string`) have no finite worst case. SOME/IP over LoLa requires
  bounded types, so this is the expected usage.

## Storage with LoLa

No allocation is involved on the serialization path. LoLa hands us a fixed
shared-memory slot; we serialize **directly into that buffer** via a span
(in-place, bounds-checked, no reallocation). The growing-container case of
`SerializeIterator` does not apply here — the slot is sized up front from
`GetMaxSize<T>()`.

## Location note

`SomeIpStrategy` only *depends on* the framework; it does not have to live inside
the serialization library. It can equally live with its consumer (e.g. the
SOME/IP binding). Only the framework (visitor + iterator + strategy concept) is
the shared/central piece.
