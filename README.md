# json-patch-extended

An extended version of JSON Patch (RFC 6902). JSON Patch Extended is strictly a superset of JSON patch.

You can read the contents of the latest version of spec [in SPEC.md](SPEC.md).

This specification is licensed under [the BSD license](LICENSE.md).


## Justification

JSON Patch is an incredibly useful way of describing and managing changes between JSON objects. It lacks, however, a number of features that prevent it from facilitating use cases in some types of distributed systems. Testing whether a member exists on an object, for instance, is not possible with vanilla JSON Patch.

JSON Patch on its own is designed to allow the modification of an existing JSON document. The use cases that it targets are API-centric. That is, it's designed to serve APIs that are REST-based. It does a great job of this. However, for systems where JSON Patch is used to manage changes to free-form JSON-formatted objects, there are significant limitations. For example:

- Atomically creating values only if they do not already exist is not possible with JSON Patch. Because the `test` operation only support testing a target location for equality and `undefined` is not a part of JSON, you cannot safely test whether an object does or does not exist.
- Performing atomic changes to values may require a substantial amount of overhead to write JSON Patch queries in a way that fits the available operations. For example, performing an update only if a value is not the expected value is not possible with a single command; instead, a system must first perform a query to determine a value, then create a command to assert equality against the fetched value and update it.
- Along the same lines as the previous example, dealing with a schemaless or otherwise free-form blob of JSON requires a substantial amount of overhead. Setting a value safely may require extra fetches and assertions because object keys and array indices have an overlap in the [JSON-Pointer spec](https://tools.ietf.org/html/rfc6901). Additionally, these assertions may be very large if the value being tested is very large (e.g., an array or object with a large number of members), potentially making simple commands very resource-intensive.
