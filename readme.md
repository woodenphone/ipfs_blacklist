Specification for IPFS blocklist format

# Do not trust this document yet.
This is a work-in-progress by an unqualified hobbyist.


# Overview
This is intended as a guideline for a machine and human readable blocklist for sharing udesirable IPFS hashes, whether it be between site operators, members of the general public, or regulatory bodies.

The general public end-user can then simply add their desired blocklist's URLs to their IPFS implimentation to prevent them from inadvertantly accessing this content.

This specification should provide fields for tags, descriptions, and similar metadata for each prohibited hash to assist in list management.
Support for comments not specific to a hash but to a region of the file should also be provided for ease-of-use purposes.

Basically this is like an adblock-style blocklist for IPFS hashes.


# Specification

Hashes are to be encoded in base32.
Everything should use utf-8 encoding.
If possible, systems should be tolerant of badly formatted lines.
Systems should be tolerant of numerical values being inside strings. (e.g. {some_numeric_value: '12345'} when you want 12345 as an integer)
Escaping should probably be considered but has not been yet.
Any line that starts with a hash character ('#') should be treated as a comment.
An empty line should be ignored and tolerated. Empty in this case means consisting of only whitespace characters.
Try to avoid trusting anything where you can avoid it. Misplaced trust only leads to things breaking.

## Reserved words:
\r\n ends a line

## Predefined JSON fields
Any of these may be either present or absent.
comment: String containing a human-readable comment.
reason: String contining a human-readable reason why this entry is in the list.
tags: Array of strings, each string is one tag.
hash: String containing the hash encoded in base32.
version: Integer version of the entry, increment by one each change. 0 means special custom case, 1 is the first version. Note that this special case for zero is undefined and implimentation-specific. It is expected that software using a value of zero will use a custom field to remember what it signifies.
md5: string contining the MD5 hash of the linked content encoded in base32.
sha1: string contining the SHA1 hash of the linked content encoded in base32
authors: object containing positive integer keys with author information objects as values.
    author keys:
        email: String contining the author's email address.
        name: String containing the full name of the author.
        date: String containing the date of this author's modification in seconds since epoch.
    e.g. authors = {'': {'name':'Jane Smith', 'date':'1547620860', 'email':'jane_smith_1970@example.com',... }


## File format:

<HASH>,{JSON Encoded metadata}\r\n
\#comment\r\n


# List host guidelines
It is suggested to prepend the list file with an identifier entry.
Software should not trust these values if at all possible, but they should be included anyway for ease of maintenance.
Values should be regenerated on list export, but this is optional and should not be relied upon to have occured.
Suggested identification values:

\# list_url: URL_TO_LISTFILE
\# last_modified: SECONDS_SINCE_EPOCH
\# numer_of_lines: NUMBER_OF_LINES
\# generated_by: PROGRAM_USED_TO_GENERATE (If a version number is applicable for the program, put it at the end so it can be easily detected and seperated by code unaware of your program's versioning.)

e.g.
\# list_url: https://example.com/ipfs_blocklist_example_url.txt
\# last_modified: 1547620860
\# numer_of_lines: 0
\# generated_by: Examplecorp IPFS Blocklist Management Studio Professional v127.0.1, 2019-1-16, build 12345abcde


# Client-side guidelines
How end-user software should handle this format.
The basic behavior is to read a line, skip the line if it begins with any of the following ('\r', '\n', '#', ' ', '\t') discard anything after the first comma (','), and decode from base32.

Simplistic implimentation pseudocode:
Read a line from the file.
Check if the first character is in the set ('#', '\r', '\n'), and if so skip this line.
Collect the substring between the start of the line and the first comma (',').
Decode the hash from b32 into whatever internal format your program requires.
Repeat until end of file.


More sophisticated clients may decode the JSON associated with an entry and do something with that information, but this is not required.

# List Generation software guidelines
TODO