---
###
# Internet-Draft Markdown Template
#
# Rename this file from draft-todo-yourname-protocol.md to get started.
# Draft name format is "draft-<yourname>-<workgroup>-<name>.md".
#
# For initial setup, you only need to edit the first block of fields.
# Only "title" needs to be changed; delete "abbrev" if your title is short.
# Any other content can be edited, but be careful not to introduce errors.
# Some fields will be set automatically during setup if they are unchanged.
#
# Don't include "-00" or "-latest" in the filename.
# Labels in the form draft-<yourname>-<workgroup>-<name>-latest are used by
# the tools to refer to the current version; see "docname" for example.
#
# This template uses kramdown-rfc: https://github.com/cabo/kramdown-rfc
# You can replace the entire file if you prefer a different format.
# Change the file extension to match the format (.xml for XML, etc...)
#
###
title: "Javascript Object Exchange Protocol"
abbrev: "JSOX"
category: info

docname: draft-d3x0r-JSOX-latest
submissiontype: independant  # also: "independent", "IAB", or "IRTF"
number:
date:
consensus: true
v: 3
area: AREA
workgroup: WG Working Group
keyword:
 - JSON
 - Javascript
 - Network
 - Protocol
 - 
venue:
  group: WG
  type: Working Group
  mail: WG@example.com
  arch: https://example.com/WG
  github: https://github.com/d3x0r/JSOX-RFC
  latest: https://github.com/d3x0r/JSOX-RFC

author:
 -
    fullname: James Buckeyne
    organization: None
    email: d3ck0r@gmail.com

normative:

informative:


--- abstract

JavaScript Object eXchange (JSOX) is a lightweight, text-based,
language-independent data interchange format.  It was derived from
the ECMAScript Programming Language Standard and JSON which defines 
a small set of formatting rules for the portable representation of 
structured data.


--- middle

# Introduction

JavaScript Object eXchange (JSOX) is a text format for the
serialization of structured data.  It is derived from JSON and
the object literals of JavaScript, as defined in the ECMAScript
Programming Language Standard, Version 7 [ECMA-262].  JSOX
accepts all legal JSON.

JSOX's design goals were to extend and enhance JSON adding more
features for human clarity, while keeping JSON's original goals
for it to be minimal, portable, textual, and a subset of JavaScript.
Additionally, encoding for basic types like TypedArrays, which are
actually a set of 12 classes of array, and support for encoding
cyclic structures.

JSOX values represent seven primitive types (strings, numbers, 
booleans, Dates, BigInt, and TypedArrays(binary data)), two structured
types (objects and arrays), and two place holding values (null and undefined).

A string is a sequence of zero or more Unicode characters [UNICODE].
Note that this citation references the latest version of Unicode
rather than a specific release.  It is not expected that future
changes in the UNICODE specification will impact the syntax of JSON. 
An identifier is a string that does not require quotes.
JSOX allows identifiers to be used for object field names, and for
application type extensions.

An object is a collection of zero or more name/
value pairs, where a name is a string or identifier 
and a value.

An array is an ordered sequence of zero or more values.

JSOX also introduces macros.  A macro is a list of identifiers 
or strings which define the field names of a set of objects.  
Later, a macro reference 


The terms "object" and "array" come from the conventions of
JavaScript.  The term "class" is used in the general sense of

 -
   "a set or category of things having some property or attribute in 
   common and differentiated from others by kind, type, or quality"
   [google search 'class definition' 1]



# Conventions and Definitions

{::boilerplate bcp14-tagged}


# Security Considerations

Generally, there are security issues with scripting languages.  JSOX
is a subset of JavaScript that excludes code expressions.

Since JSOX's syntax deviates from Javascript for class defintions, 
it is, in many cases, longer possible to  use that language's 
"eval()" function to parse JSOX texts; although there is a subset 
of documents, those which do not use typed objects or typed arrays, 
which can still be resolved using "eval()".

# IANA Considerations

   The MIME media type for JSOX text is application/jsox.

   Type name:  application

   Subtype name:  jsox

   Required parameters:  n/a

   Optional parameters:  n/a

   Encoding considerations:  binary

   Security considerations:  See [RFC!??!], Section 12.

   Interoperability considerations:  Described in [RFC!??!]

   Published specification:  [RFC!??!]

   Applications that use this media type:
      JSOX has been used to exchange data between applications written
      in all of these programming languages: C, C++, JavaScript.


   Additional information:
      Magic number(s): n/a
      File extension(s): .jsox
      Macintosh file type code(s): TEXT

   Person & email address to contact for further information:
      IESG
      <>

   Intended usage:  COMMON

   Restrictions on usage:  none

   Author:
      J Decker
      <d3ck0r@gmail.com>

   Change controller:
      IESG
      <>

   Note:  No "charset" parameter is defined for this registration.
      Adding one really has no effect on compliant recipients.

# Specifications of JSOX

This document describes JSOX and registers the media type "application/jsox".

Reference implementation at https://github.com/d3x0r/jsox has 
a README.md which also contributes to the definition.

All of the specifications of JSOX syntax agree on the syntactic
elements of the language.


## JSOX Grammar

A JSOX text is a sequence of tokens.  The set of tokens includes six
structural characters, identifiers, strings, numbers, and six literal 
names.

A JSOX text is a serialized value, or class specification. 


   JSOX-text = ws value ws

These are the six structural characters:

   begin-array     = ws %x5B ws  ; [ left square bracket

   begin-object    = ws %x7B ws  ; { left curly bracket

   end-array       = ws %x5D ws  ; ] right square bracket

   end-object      = ws %x7D ws  ; } right curly bracket

   name-separator  = ws %x3A ws  ; : colon

   value-separator = ws %x2C ws  ; , comma

Insignificant whitespace is allowed before or after any of the six
structural characters.  Whitespace characters explicitly end a previous token
and any following non-whitespace characters will begin a new token.  All 
whitespace is optional, except where syntax requirements impose requirement;
although some required whitespace can also be resolved using quotes around
identifiers, a stream of numbers requires whitespace separation.


   ws = *(
           %x20 /              ; Space
           %xA0 /              ; Non-breaking Space
           %x09 /              ; Horizontal tab
           %x0A /              ; Line feed or New line
           %x07ec /            ; 2028 LS (Line separator)
           %x07ed /            ; 2029 PS (paragraph separator)
           %x0D )              ; Carriage return

Identifiers are either a string or any sequence of characters that do not 
include structural characters or whitespace, and also does not begin with 
a digit, minus, plus, or decimal-point. JSOX keywords OPTIONAL are quoted, but
recommended practice to stringification is to quote them.

Stringifiers MUST quote any identifiers that contain any JSOX structural 
character or whitespace.

Stringifiers SHOULD quote these.
     matches keywords ( [ "true","false","null","NaN","Infinity","undefined"], 
     matches this regex  /((\n|\r|\t)|[ \{\}\(\)\<\>\!\+\-\*\/\.\:\, ])/

    identifier = ws [ string / not(0-9.+-)  *(not ws and not structure character ) ] ws

Javascript seems to stringify the values before using them as the object key.
It may be that there's an implementation that 'true' and `true` is not the string "true".

When dealing with a JSOX stream, whitespace may be significant to separate
values, for example the stream '1 2 3 4 5' which are 5 simple numbers.

# Values

   A JSOX value MUST be an object, typed-object, array, typed-array, number,
   string, or one of the following six literal names:

      false null true Infinity NaN undefined

   The literal names MUST exactly match tbe case.  No other literal names are
   allowed.

      value = false / null / true / object / array / number / string
            / Infinity / NaN / undefined

      false = %x66.61.6c.73.65   ; false

      null  = %x6e.75.6c.6c      ; null

      true  = %x74.72.75.65      ; true

      Infinity = [ minus ] %x49.6e.66.69.6e.69.7f.79  ; [-]Infinity

      number = *[ plus | minus ] int [ radix ] [ time-symbol ] [ frac ] [ exp ] [ bigint-suffix ]

      NaN = %x4e.61.4e                      ; NaN

      undefined = %x75.6e.64.65.66.69.6e.65.64   ; undefined


# Objects and Typed Objects

   An object structure is represented as a pair of curly brackets
   surrounding zero or more name/value pairs (or members).  A name is a
   string.  A single colon comes after each name, separating the name
   from the value.  A single comma separates a value from a following
   name.  The names within an object SHOULD be unique.

      object = begin-object [ member *( value-separator member ) ]
               end-object

      typed-object = identifier begin-object [ string, *(string)] end-object

      typed-object-reference = identifier ws begin-object [ value, *(value)] end-object

      member = string name-separator value

      identifier = non-digit non-ws-character* 

   An object whose names are all unique is interoperable in the sense
   that all software implementations receiving that object will agree on
   the name-value mappings.  When the names within an object are not
   unique, the behavior of software that receives such an object is
   unpredictable.  Many implementations report the last name/value pair
   only.  Other implementations report an error or fail to parse the
   object, and some implementations report all of the name/value pairs,
   including duplicates.

   JSOX parsing libraries MAY differ as to whether or
   not they make the ordering of object members visible to calling
   software.  Implementations whose behavior does not depend on member
   ordering will be interoperable in the sense that they will not be
   affected by these differences.

   Typed Objects have a type definition, defined as a top-level object;
   the definition specifies the fields for the object, so later references
   of that object type only need specify values.  In Javascript, all
   objects that are the same type, share the same prototype.  In C,
   the information about the class of object is retained, but is only
   represented for parser consumers to resolve.   

# Arrays and Typed Arrays

   An array structure is represented as square brackets surrounding zero
   or more values (or elements).  Elements are separated by commas.

   array = begin-array [ value *( value-separator value ) ] end-array

   There is no requirement that the values in an array be of the same
   type.

   ECMA Script defined (in version 6?) typed arrays, which are packed,
   binary types of data.  These can be encoded in JSOX using the same
   sort of definition as a Typed Object, but, instead there are several
   hard coded array types.   The element of a typed array buffer is
   a base64 encoded identifier.  The base64 data should be decoded to 
   the raw bytes before parser output.

      ab - array buffer, untyped data of some byte length.
      u8 - unsigned 8 bit integer array (Uint8Array),
      uc8 (Uint8ClampedArray), s8(Int8ARray), 
      u16, s16, 
      u32, s32,
      (u64, s64?),  (Uint64Array, Int64Array, not yet implemented in the real world)
      f32, f64,

   And finally, an internal (to the parser), array type 'ref'.  Reference
   arrays contain strings and integer numbers, which define the path in the
   current object or array to the value to use.  A 'ref' type array may NOT
   occur as a primitive value.

# Base 64 Encoding
	uses 
		ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789$_
	accepts '=' as a nul terminator.
   The following characters are accepted at the final encoding, but most
	will require quoting.  With the above defined sequence quotes may be
   omitted on binary typed array information.

		'$':62
		'+':62
		'-':62
		'.':62

		'_':63
		'/':63
		',':63

   

# Numbers

   Generally, numbers are any string of charcters which may be passed
   to the Number() constructor in ES6.  JSOX breaks that by also 
   allowing 'T','Z',':','+', and '-' to switch to a ES6 Date type,
   and numbers suffixed with an 'n' which uses BigInt constructor
   to result with the number's value.

   The representation of numbers is similar to that used in most
   programming languages.  A number is represented in base 10 using
   decimal digits.  It contains an integer component that may be
   prefixed with an optional minus sign, which may be followed by a
   fraction part and/or an exponent part.  

   In JSON, Leading zeros are not allowed.  Leading zeros are allowed,
   although will often change the value's interpretation.  Radix numbers
   like 0x123 have a leading zero, followed by a format character ( one of
   'x','o',or 'b' in any case, which inidicate hexidecimal, octal, and binary
   respectively).  A leading zero on an integer changes the meaning to
   be octal, and while parsing, any character not in 0-7 emits an error.

   A number begins with [0-9], '+', and '.'.  Once a number starts, only
   a limited number of characters are allowed and they are all less
   then 127.  

   Underscore('_') may be used in a number. It is treated as a Non-Breaking-Zero-
   Width-space (NBZWSP).  That is the character is not stored, it and 
   does not modify the number.  Many languages support this now; although
   most have very strict rules about allowed placement.  A leading 
   underscore puts this value in the realm of Identifiers.

   A fraction part is a decimal point followed by one or more digits.

   An exponent part begins with the letter E in upper or lower case,
   which may be followed by a plus or minus sign.  The E and optional
   sign are followed by one or more digits.

      number = [ plus minus ] int [ frac ] [ exp ]

      radix-number = [ plus minus ] %x30 [radix] int 

      date = [ plus minus ] [ int [ time-symbol ] ]*DIGIT

      bigint = [ minus / plus ] int bigint-suffix

      decimal-point = %x2E       ; .

      digit1-9 = %x31-39         ; 1-9

      e = %x65 / %x45            ; e E

      exp = e [ minus / plus ] 1*DIGIT

      frac = decimal-point 1*DIGIT

      int = zero / ( digit1-9 *DIGIT )

      minus = %x2D               ; -

      plus = %x2B                ; +

      zero = %x30                ; 0

      radix = [ %58 ] [ %78 ] [ %4f ] [ %6f ] [ %42 ] [ %62 ] ; [ X ] [x] [O] [o] [B] [b] 

      time-symbol = [ %54 ] [ %5a ] [ %2b ] [ %2d ] [ %3a ]   ; [ T ] [Z] [+] [-] [:]

      bigint-suffix = %6e        ; n

      underscore = %5f

   Dates decoded which are not ISO 8601 compatible may result in 'Invalid Date';
   especially given the vagueness of date value specification.

   With the addition fo BigInt types in (?), the native ECMA Script is
   an integer number suffixed with 'n'.

   This specification allows implementations to set limits on the range
   and precision of numbers accepted.  Since software that implements
   IEEE 754-2008 binary64 (double precision) numbers [IEEE754] is
   generally available and widely used, good interoperability can be
   achieved by implementations that expect no more precision or range
   than these provide, in the sense that implementations will
   approximate JSOX numbers within the expected precision.  A JSOX
   number such as 1E400 or 3.141592653589793238462643383279 may indicate
   potential interoperability problems, since it suggests that the
   software that created it expects receiving software to have greater
   capabilities for numeric magnitude and precision than is widely
   available.

   Note that when such software is used, numbers that are integers and
   are in the range [-(2**53)+1, (2**53)-1] are interoperable in the
   sense that implementations will agree exactly on their numeric
   values.


# Identifiers

   Identifiers are not themselves values that result, but can be used
   for the field or defining typed objects.

   Unquoted identifier strings must not have ':', ']', '[', '{', '}', ',', 
   any quote or whitespace.  They must also not begin with characters that begin
   a number.

# Strings

   The representation of strings is similar to conventions used in ECMA Script.
   A string begins and ends with
   quotation marks.  All Unicode characters may be placed within the
   quotation marks, except for the characters that must be escaped:
   the quotation mark used to open the string, and reverse solidus.

   Any character may be escaped.  If the character is in the Basic
   Multilingual Plane (U+0000 through U+FFFF), then it may be
   represented as a six-character sequence: a reverse solidus, followed
   by the lowercase letter u, followed by four hexadecimal digits that
   encode the character's code point.  The hexadecimal letters A though
   F can be upper or lower case.  So, for example, a string containing
   only a single reverse solidus character may be represented as
   "\u005C".
       Escaping character that are 1 byte may be done with \x or \[0-2]
       for hex or octal conversion.

       or \u{HEXDIGITS}
       or \u0034

   Alternatively, there are two-character sequence escape
   representations of some popular characters.  So, for example, a
   string containing only a single reverse solidus character may be
   represented more compactly as "\\".


   To escape an extended character that is not in the Basic Multilingual
   Plane, the character is represented as a 12-character sequence,
   encoding the UTF-16 surrogate pair.  So, for example, a string
   containing only the G clef character (U+1D11E) may be represented as
   "\uD834\uDD1E".

      string = quotation-mark *char quotation-mark

      char = unescaped /
          escape (
              %x22 /          ; "    quotation mark  U+0022
              %x27 /          ; '    quotation mark  U+0027
              %x60 /          ; `    quotation mark  U+0060
              %x5C /          ; \    reverse solidus U+005C
              %x2F /          ; /    solidus         U+002F
              %x62 /          ; b    backspace       U+0008
              %x66 /          ; f    form feed       U+000C
              %x6E /          ; n    line feed       U+000A
              %x72 /          ; r    carriage return U+000D
              %x74 /          ; t    tab             U+0009
              %x78 2HEXDIG )  ; xXX                  
              %x30-%x32 2OCTDIG )  ; 0               
              %x75 4HEXDIG )  ; uXXXX                U+XXXX

      escape = %x5C              ; \

      quotation-mark = %x22 /     ; "
                       %x29 /     ; '
                       %x60 /     ; `

      unescaped = %x20-21 / %x23-5B / %x5D-10FFFF


# String and Character Issues

##  Character Encoding

   JSOX text SHALL be encoded in UTF-8, UTF-16, or UTF-32.  The default
   encoding is UTF-8, and JSOX texts that are encoded in UTF-8 are
   interoperable in the sense that they will be read successfully by the
   maximum number of implementations; there are many implementations
   that cannot successfully read texts in other encodings (such as
   UTF-16 and UTF-32).

   Implementations MUST NOT add a byte order mark to the beginning of a
   JSOX text.  In the interests of interoperability, implementations
   that parse JSOX texts MAY ignore the presence of a byte order mark
   rather than treating it as an error.

##  Unicode Characters

   When all the strings represented in a JSOX text are composed entirely
   of Unicode characters [UNICODE] (however escaped), then that JSOX
   text is interoperable in the sense that all software implementations
   that parse it will agree on the contents of names and of string
   values in objects and arrays.

   However, the ABNF in this specification allows member names and
   string values to contain bit sequences that cannot encode Unicode
   characters; for example, "\uDEAD" (a single unpaired UTF-16
   surrogate).  Instances of this have been observed, for example, when
   a library truncates a UTF-16 string without checking whether the
   truncation split a surrogate pair.  The behavior of software that
   receives JSOX texts containing such values is unpredictable; for
   example, implementations might return different values for the length
   of a string value or even suffer fatal runtime exceptions.

##  String Comparison

   Software implementations are typically required to test names of
   object members for equality.  Implementations that transform the
   textual representation into sequences of Unicode code units and then
   perform the comparison numerically, code unit by code unit, are
   interoperable in the sense that implementations will agree in all
   cases on equality or inequality of two strings.  For example,
   implementations that compare strings with escaped characters
   unconverted may incorrectly find that "a\\b" and "a\u005Cb" are not
   equal.


# Parsers

   A JSOX parser transforms a JSOX text into another representation.  A
   JSOX parser MUST accept all texts that conform to the JSOX grammar.
   A JSOX parser MAY accept non-JSOX forms or extensions.

   An implementation may set limits on the size of texts that it
   accepts.  An implementation may set limits on the maximum depth of
   nesting.  An implementation may set limits on the range and precision
   of numbers.  An implementation may set limits on the length and
   character contents of strings.

# Generators

   A JSOX generator produces JSOX text.  The resulting text MUST
   strictly conform to the JSOX grammar.


# Examples

   This is a JSOX object:

      {
        Image: {
            tag: A0013,
            Width:  800,
            Height: 600,
            Title:  "View from 15th Floor",
            "Thumbnail": {
                "Url":    "http://www.example.com/image/481989943",
                "Height": 125,
                "Width":  100
            },
            "Animated" : false,
            "IDs": [116, 943, 234, 38793]
          }
      }

   Its Image member is an object whose Thumbnail member is an object and
   whose IDs member is an array of numbers.

   This is a JSOX array containing two objects:

      [
        {
           "precision": "zip",
           "ident":     123594985n,
           "Latitude":  37.7668,
           "Longitude": -122.3959,
           created:     2018-09-11T03:43:53.345-07:00,
           binary:      u8[U2VjcmV0],   /* Secret */
           Address:     "",
           City:        "SAN FRANCISCO",
           State:       "CA",
           Zip:         "94107",
           Country:     "US"
        },
        {
           precision:   "zip",
           "ident":     123594986n,
           Latitude:    37.371991,
           "Longitude": -122.026020,
           created:     2018-09-11T10:43:52.437Z,
           "binary":    u8[SGVsbG8sIFdvcmxkIQ==],   // Hello, World!
           "Address":   "",
           City:        "SUNNYVALE",
           "State":     "CA",
           Zip:         "94085",
           "Country":   "US"
        }
      ]

   This is an example of typed Objects

      locale{ precision,ident,Latitude,Longitude,
              created,binary,Address,City,State,Zip,Country}
      [ locale{ "zip",123594985n,37.7668,-122.3959,                     
                2018-09-11T03:43:53.345-07:00,
                u8[U2VjcmV0],
                "","SAN FRANCISCO","CA","94107","US"},
        locale{ "zip",123594986n,37.371991,-122.026020,                     
                2018-09-11T10:43:52.437Z,
                u8[SGVsbG8sIFdvcmxkIQ==],
                "","SUNNYVALE","CA","94107","US"}
      ]

      locale{ "zip",123594988n,36.1699,-115.1398,
              2018-09-11T13:23:23.636Z,
              u8[SGVsbG8sIFdvcmxkIQ==],
              "","LAS VEGAS","NV","89109","US"}

   This is an example of a reference

      { 
         company : { name : "Example.com",
                  employees : [ { name:"bob" },{name:"tom"} ],
                  manager : ref["company","employees",0]
                   }
      }

      // output.company.manager === output.company.employees[0]



   Here are three small JSOX texts containing only values:

   "Hello world!"

   42

   123n

   true

   Infinity

   

# Contributors

   RFC 4627 was written by Douglas Crockford.  This document was
   constructed by making a relatively small number of changes to that
   document; thus, the vast majority of the text here is his.

# References

##  Normative References

   [IEEE754]  IEEE, "IEEE Standard for Floating-Point Arithmetic", IEEE
              Standard 754, August 2008,
              <http://grouper.ieee.org/groups/754/>.

   [RFC2119]  Bradner, S., "Key words for use in RFCs to Indicate
              Requirement Levels", BCP 14, RFC 2119, March 1997.

   [RFC5234]  Crocker, D. and P. Overell, "Augmented BNF for Syntax
              Specifications: ABNF", STD 68, RFC 5234, January 2008.

   [UNICODE]  The Unicode Consortium, "The Unicode Standard",
              <http://www.unicode.org/versions/latest/>.

## Informative References

   [ECMA-262] Ecma International, "ECMAScript Language Specification
              Edition 5.1", Standard ECMA-262, June 2011,
              <http://www.ecma-international.org/publications/standards/
              Ecma-262.htm>.

   [ECMA-404] Ecma International, "The JSON Data Interchange Format",
              Standard ECMA-404, October 2013,
              <http://www.ecma-international.org/publications/standards/
              Ecma-404.htm>.

   [Err3607]  RFC Errata, Errata ID 3607, RFC 3607,
              <http://www.rfc-editor.org>.

   [Err607]   RFC Errata, Errata ID 607, RFC 607,
              <http://www.rfc-editor.org>.

   [RFC4627]  Crockford, D., "The application/json Media Type for
              JavaScript Object Notation (JSON)", RFC 4627, July 2006.

   [RFC7159]  T. Bray, Ed., "The JavaScript Object Notation (JSON)
              Data Interchange Format", RFC 7159, March 2014.

   [RFC7493]  T. Bray, Ed., "The I-JSON Message Format", RFC 7493, 
              March 2015


--- back

# Acknowledgments
{:numbered="false"}

There's noone to blame except myself, and the prior arts of Douglaus Crockform, and Tim Bray.

