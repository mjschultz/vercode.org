## `vercode ░:▝:█!🉐`

vercode is compatible with semver, but less restrictive.

### Specification

1. Versions MUST BE sorted in codepoint order.
2. A vercode `<series>` is increased when there is a breaking API change.
3. A vercode `<series>:<feature>` adds features to an API but does not break the existing API.
4. A vercode `<series>:<feature>:<fix>` fixes the software in an API compatible with that feature level.
5. A vercode MUST BE a fully specified `<series>:<feature>:<fix>`.
6. A vercode MAY HAVE a prerelease, signified by "!" to denote danger.
7. A vercode MAY HAVE a build id, signified by "Δ" to denote a different build of the same `<vercode>`.
8. A `<unicode identifier>` is any unicode character except "!", "Δ", or ":" as those are reserved separators.

### (Modified) Backus–Naur Form Grammar for vercode versioning

A <unicode identifier> is any unicode character except "!", "Δ", or ":" as those are reserved separators.

```markdown
<vercode> ::= <vercode base>
            | <vercode base> "!" <pre-release>
            | <vercode base> "Δ" <build>
            | <vercode base> "!" <pre-release> "Δ" <build>

<vercode base> ::= <series> ":"" <feature> ":"" <fix>

<series> :: = <unicode identifier>

<feature> :: = <unicode identifier>

<patch> :: = <unicode identifier>

<pre-release> ::= <unicode identifier>
                | <unicode identifier> <pre-release>

<build> ::= <unicode identifier>
          | <unicode identifier> <build>

<unicode identifier> ::= <any unicode codepoint - "!" - ":" - "Δ">
```

### Generate Unicode Identifiters

Since there are multitudes of Unicode characters, we provide scripts to generate the character sets locally.

#### Python 3

```python
from itertools import count
from unicodedata import category, name

RESERVED_CHARACTERS = {"!", ":", "Δ"}

def clean_name(c):
    try:
        uname = name(c)
    except ValueError as e:
        assert str(e) == 'no such name'
        uname = str(e)
    return uname


for i in count():
    try:
        c = chr(i)
    except ValueError:
        break
    if c in RESERVED_CHARACTERS:
        continue
    print(f'{i:>7} {hex(i):>8} {category(c)} {clean_name(c)}')
```
