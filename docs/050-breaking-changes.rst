********************************
Solidity v0.5.0 Breaking Changes
********************************

Solidity version 0.5.0 introduces many breaking changes.
This section shows how to update your code regarding some of the changes.
For the full list please check `the release changelog <https://github.com/ethereum/solidity/releases>`_.

The functions ``.call()`` (and family), ``keccak256()``, ``sha256()``, ``ripemd160()`` now accept only a single ``bytes``.
Change every ``.call()`` to a ``.call("")`` and every ``.call(signature, a, b, c)`` to use ``.call(abi.encodeWithSignature(signature, a, b, c))`` (the last one only works for value types).
Change every ``keccak256(a, b, c)`` to ``keccak256(abi.encodePacked(a, b, c))``.

Explicit function visibility is now mandatory.
Add ``public`` to every function and ``external`` to every fallback or interface function that does not specify its visibility already.

Explicit data location for all variables of struct, array or mapping types (including function parameters) is now mandatory.
For example, change ``uint[] x = m_x`` to ``uint[] storage x = m_x``.
Note that ``external`` functions require parameters with a data location of ``calldata``.

Variables of contract type do not have access to ``address`` members.
Therefore, it is now necessary to explicitly convert values of contract type to addresses before using an ``address`` member.
Example: if ``c`` is a contract, change ``c.transfer(...)`` to ``address(c).transfer(...)``.

Conversions between ``bytesX`` and ``uintY`` of different size are now disallowed.
For example, it is now possible to convert a ``bytes4`` (4 bytes) to a ``uint64`` (8 bytes). This can be achieved by first converting the ``bytes4`` variable to ``bytes8`` and then to ``uint64``.

C99-style scoping rules are now enforced.
Declare function variables before using them in the same or deeper scope.

The ``throw`` statement is now disallowed.
Change ``throw()`` to ``revert()`` or use ``require``/``assert`` with a condition.
