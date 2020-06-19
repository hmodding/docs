# Slugs

Slugs are strings that can be used in URLs. Slugs must fulfill all of the following rules:

* Slugs must be at least one character and at most 64 characters long.
* Slugs may only contain letters, numbers, dots \("."\), dashes \("-"\) and underscores \("\_"\).
* All letters must be in lower case and from the basic latin alphabet.

### Examples

{% hint style="success" %}
Good examples:

* _`my-mod`_
* _`version-1.3.4`_
* _`test-6.4.1_RC-3`_
{% endhint %}

{% hint style="danger" %}
Bad examples:

* _`be$t-mod`_\(dollar signs are not allowed\)
* _`café-mod`_\("é" is not a letter of the latin alphabet\)
* _`<script>alert('xss')</script>`_ \("&lt;&gt;\(\)'/" are not allowed\)
* _`my mod 2`_ \(whitespaces are not allowed\)
* _`a-very-very-very-very-very-very-very-very-very-very-very-very-very-long-name`_ \(longer than 64 characters\)
{% endhint %}

