# Java and Groovy Style Guide

## Package Names:
Package names are all lowercase, with consecutive words simply concatenated together (no underscores). For example, com.example.deepspace, not com.example.deepSpace or com.example.deep_space.

## Class Names:
Class names are written in UpperCamelCase. Class names are typically nouns or noun phrases.This means that the name of the class should be something that would be the subject of a verb.
Test classes are named starting with the name of the class they are testing, and ending with Test. For example, HashTest or HashIntegrationTest.

## Method Names:
Method names are written in lowerCamelCase. Method names are typically verbs or verb phrases. For example, sendMessage or stop. Underscores may appear in JUnit test method names to separate logical components of the name. One typical pattern is test<MethodUnderTest>_<state>, for example testPop_emptyStack.

## Constant Names:
Constant names use CONSTANT_CASE: all uppercase letters, with words separated by underscores (Example).

## Non-constant Field Names:
Non-constant field names (static or otherwise) are written in lowerCamelCase. These names are typically nouns or noun phrases. For example, computedValues or index.

## Parameter Names:
Parameter names are written in lowerCamelCase. One-character parameter names in public methods should be avoided.

## Local Variable Names:
Local variable names are written in lowerCamelCase. Even when final and immutable, local variables are not considered to be constants, and should not be styled as constants.

## Camel Case:
Sometimes there is more than one reasonable way to convert an English phrase into camel case, such as when acronyms or unusual constructs like "IPv6" or "iOS" are present. To improve predictability, Google Style specifies the following (nearly) deterministic scheme.
Beginning with the prose form of the name:
- Convert the phrase to plain ASCII and remove any apostrophes. For example, "Müller's algorithm" might become "Muellers algorithm".
- Divide this result into words, splitting on spaces and any remaining punctuation (typically hyphens).
**Recommended**: if any word already has a conventional camel-case appearance in common usage, split this into its constituent parts (e.g., "AdWords" becomes "ad words"). Note that a word such as "iOS" is not really in camel case per se; it defies any convention, so this recommendation does not apply.
- Now lowercase everything (including acronyms), then uppercase only the first character of:
  ... each word, to yield upper camel case, or
  ... each word except the first, to yield lower camel case
- Finally, join all the words into a single identifier.
Note that the casing of the original words is almost entirely disregarded. Examples:


| Prose form                | Correct           | Incorrect         |
|---------------------------|-------------------|-------------------|
| "XML HTTP request"        | XmlHttpRequest    | XMLHTTPRequest    |
| "New customer ID"         | newCustomerId     | newCustomerID     |
| "Inner stopwatch"         | innerStopwatch    | innerStopWatch    |
| "supports  IPv6 on iOS?"  | supportsIpv6OnIos | supportsIPv6OnIOS | 
|  "YouTube importer"       | YouTubeImporter   |                   |


***Note***: Some words are ambiguously hyphenated in the English language: for example "nonempty" and "non-empty" are both correct, so the method names checkNonempty and checkNonEmpty are likewise both correct.

