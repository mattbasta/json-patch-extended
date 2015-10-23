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


### `absent`

The `absent` operation tests that no value exists at the target location.

The target location MUST NOT contain any value (including `null`) for the operation to be considered successful. If any ancestor value described by the `path` value is absent, the operation should short-circuit and be considered successful.

This operation has identical error handling behavior as the `test` operation.

Examples:

```json
{ "op": "absent", "path": "/a/b/c" }
{ "op": "absent", "path": "/obj/arr/3" }
```


### `type`

The `type` operation tests that a value at the target loation is of a pre-defined type.

The operation object MUST contain a `type` member that contains an array of type names that are considered acceptable for the value at the target location. The array may not be empty.

The target location MUST contain a value for the operation to be considered successful. The value at the target location must have a type that matches one of the types listed in the operation object's `type` member.

The acceptable values of the array at the `type` member on the operation object are as follows:

<dl>
  <dt>object</dt>
  <dd>Represents an object value. Equivalent to `typeof x === 'object' && !Array.isArray(x)` in JavaScript.</dd>
  <dt>array</dt>
  <dd>Represents an array value. Equivalent to `Array.isArray(x)` in JavaScript.</dd>
  <dt>string</dt>
  <dd>Represents a string. Equivalent to `typeof x === 'string'` in JavaScript.</dd>
  <dt>number</dt>
  <dd>Represents a number. Equivalent to `typeof x === 'number'` in JavaScript.</dd>
  <dt>boolean</dt>
  <dd>Represents a boolean. Equivalent to `typeof x === 'boolean'` in JavaScript.</dd>
  <dt>null</dt>
  <dd>Represents the `null` object. Equivalent to `x === null` in JavaScript.</dd>
</dl>

This operation has identical error handling behavior as the `test` operation.

Examples:

```json
{ "op": "type", "type": [ "string", "null" ], "path": "/a/b/c" }
{ "op": "type", "type": [ "number" ], "path": "/a/b/num" }
```


## Security Considerations

This specification does not change the security considerations of its parent specification.


## References

<dl>
  <dt><a href="https://tools.ietf.org/html/rfc6902">RFC 6902</a></dt>
  <dd>Bryan, P., Ed. and M. Nottingham, Ed., "JavaScript Object Notation (JSON) Patch"</dd>
</dl>
