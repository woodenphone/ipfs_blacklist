Specification for IPFS blocklist


# Overview
This is intended as a guideline for a machine and human readable blocklist for sharing udesirable IPFS hashes, whether it be between site operators, members of the general public, or regulatory bodies.

The general public end-user can then simply add their desired blocklist's URLs to their IPFS implimentation to prevent them from inadvertantly accessing this content.

This specification should provide fields for tags, descriptions, and similar metadata for each prohibited hash to assist in list management.
Support for comments not specific to a hash but to a region of the file should also be provided for ease-of-use purposes.




# Specification

Hashes are to be encoded in base32.

## Reserved words:
\r\n ends a line


## File format:

<HASH>,{JSON Encoded metadata}\r\n
\#comment\r\n
