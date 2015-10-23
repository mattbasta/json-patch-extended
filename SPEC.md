# JSON Patch Extended Spec

This document is intended to extend [RFC 6902](https://tools.ietf.org/html/rfc6902).

For the sake of brevity, this document will refer to JSON Patch Extended as "JPE".


## MIME Type

Content containing JPE should be identified with the MIME type `application/json-patch-extended+json`.


## Operations

JPE inherits all operations from RTC 6902.


### `unless`

The `unless` operation tests that a value at the target location is not equal to a specified value.

The operation object MUST contain a `value` member that conveys the value to be compared to the target location's value.

The target location MUST NOT be equal to the `value` value for the operation to be considered successful.

The semantics of "equal" for this operation are identical to those of the `test` operation.

Also, note that ordering of the serialization of object members is not significant.

For example:

```json
{ "op": "unless", "path": "/a/b/c", "value": "bar" }
```


### `exists`

The `exists` operation tests that a value at the target location exists.

The target location MUST contain any value (including `null`) for the operation to be considered successful.

This operation has identical error handling behavior as the `test` operation.

Also, note that ordering of the serialization of object members is not significant.

Examples:

```json
{ "op": "exists", "path": "/a/b/c" }
{ "op": "exists", "path": "/obj/arr/3" }
```


## Security Considerations

This specification does not change the security considerations of its parent specification.


## References

<dl>
  <dt><a href="https://tools.ietf.org/html/rfc6902">RFC 6902</a></dt>
  <dd>Bryan, P., Ed. and M. Nottingham, Ed., "JavaScript Object Notation (JSON) Patch"</dd>
</dl>
