---
layout: default
title: Semantic Versioning
has_children: false
nav_order: 3
---

This Style Guide uses the [terminology convention](./conventions/docs/terminologies/keywords) when using the phrases must, must not, should, should not, and may. All examples given are non-normative and serve only to illustrate the normative language of the style guide.

# How to version your APIs
API versioning is quite important because clients can be making requests to exisiting APIs and if you make certain changes you could break their existing calls. So versioning is one way to indicate that you are making a breaking change to your implementation.

Your API version should follow the semantic versioning conventions as describe below.

## Semantic Versioning
Semantic versioning is using 3 digit to describe 3 different types of changes. Let's look at the example below.

`1.5.47`
- The first digit, `1`, is the most significant digit. It will dictate wether or not you've made breaking changes. We refer to this digit as the major version.
- The second digit, `5`, is refer as the minor version.
- The third digit, `47`, is for anything else for example patches.

### Major Version
Version when you make **incompatible** API changes, you shouldn't have too many of them if you're doing things sensibly and trying to keep the amount of change that is of a breaking nature to a minimum. There are going to be a lot of requests for additional functionality and that's what the minor version is about.

#### Examples of action that will create a major version 
1. An API now requires a new **mandatory** input else the request will throw a 422 error.
2. Removing or Renaming an attribute returned (youAwesomeName --> name)

### Minor Version
Version when you add functionality in a backwards compatible manner. This version is used when you want to do things like add a new operation on your APIs. It is a **non-breaking action** because anyone who's got an existing operation can still call it and they just ignore the new one.

#### Examples of action that will create a major version 
1. New method added. For example the API previously had only a GET object by id method but now the API has a listing method too.
2. Returning new attributes

### Patch Version
Version when you make backwards compatible bug fixes and documentation updates, like if there were a typo in the documentation or in the success message returned by the API.

#### Examples of action that will create a major version 
1. Typo on the success message. (Pokemon created *successsfully* --> Pokemon created *successfully*)
2. Backwards compatible bug fixes (Regex expression improved, code refactoring)


## Client side version

The client shouldn't be updating the link of the version every time a patch or a minor version is added. Instead we want to only let him know when a breaking change is made, thus, a new major version.

In order to achieve this, we want to have the major version number the url of the API path.
```
/v1/pokemon
/v2/pokemon
```
**not**
{: .text-red-300 }
```
/v1.15/pokemon
/v2.16.19/pokemon
```

In other words, a path using versioning must follow the following regular expression: `/^\/v[1-9]\/.*/`

## Specifications
Specifications based on [Semantic Versioning 2.0.0](https://semver.org/#semantic-versioning-specification-semver)

1. Software using Semantic Versioning must declare a public API. This API could be declared in the code itself or exist strictly in documentation. However it is done, it should be precise and comprehensive.
   
2. A normal version number must take the form X.Y.Z where X, Y, and Z are non-negative integers, and must not contain leading zeroes. X is the major version, Y is the minor version, and Z is the patch version. Each element must increase numerically. For instance: 1.9.0 -> 1.10.0 -> 1.11.0.
   
3. Once a versioned package has been released, the contents of that version must not be modified. Any modifications must be released as a new version.

4. Major version zero (0.y.z) is for initial development. Anything may change at any time. The public API should not be considered stable.

5. Version 1.0.0 defines the public API. The way in which the version number is incremented after this release is dependent on this public API and how it changes.

6. Patch version Z (x.y.Z  x > 0) must be incremented if only backwards compatible bug fixes are introduced. A bug fix is defined as an internal change that fixes incorrect behavior.

7. Minor version Y (x.Y.z  x > 0) must be incremented if new, backwards compatible functionality is introduced to the public API. It must be incremented if any public API functionality is marked as deprecated. It may be incremented if substantial new functionality or improvements are introduced within the private code. It MAY include patch level changes. Patch version must be reset to 0 when minor version is incremented.

8. Major version X (X.y.z  X > 0) must be incremented if any backwards incompatible changes are introduced to the public API. It may also include minor and patch level changes. Patch and minor version must be reset to 0 when major version is incremented.

9.  A pre-release version may be denoted by appending a hyphen and a series of dot separated identifiers immediately following the patch version. Identifiers must comprise only ASCII alphanumerics and hyphens [0-9A-Za-z-]. Identifiers must not be empty. Numeric identifiers must not include leading zeroes. Pre-release versions have a lower precedence than the associated normal version. A pre-release version indicates that the version is unstable and might not satisfy the intended compatibility requirements as denoted by its associated normal version. Examples: 1.0.0-alpha, 1.0.0-alpha.1, 1.0.0-0.3.7, 1.0.0-x.7.z.92, 1.0.0-x-y-z.–.

10. Build metadata may be denoted by appending a plus sign and a series of dot separated identifiers immediately following the patch or pre-release version. Identifiers must comprise only ASCII alphanumerics and hyphens [0-9A-Za-z-]. Identifiers must not be empty. Build metadata must be ignored when determining version precedence. Thus two versions that differ only in the build metadata, have the same precedence. Examples: 1.0.0-alpha+001, 1.0.0+20130313144700, 1.0.0-beta+exp.sha.5114f85, 1.0.0+21AF26D3—-117B344092BD.

11. Precedence refers to how versions are compared to each other when ordered.
    1.  Precedence must be calculated by separating the version into major, minor, patch and pre-release identifiers in that order (Build metadata does not figure into precedence).
    2.  Precedence is determined by the first difference when comparing each of these identifiers from left to right as follows: Major, minor, and patch versions are always compared numerically.
        Example: 1.0.0 < 2.0.0 < 2.1.0 < 2.1.1.
    3.  When major, minor, and patch are equal, a pre-release version has lower precedence than a normal version:
        Example: 1.0.0-alpha < 1.0.0.
    4.  Precedence for two pre-release versions with the same major, minor, and patch version must be determined by comparing each dot separated identifier from left to right until a difference is found as follows:
    5.  Identifiers consisting of only digits are compared numerically.
    6.  Identifiers with letters or hyphens are compared lexically in ASCII sort order.
    7.  Numeric identifiers always have lower precedence than non-numeric identifiers.
    8.  A larger set of pre-release fields has a higher precedence than a smaller set, if all of the preceding identifiers are equal.
        Example: 1.0.0-alpha < 1.0.0-alpha.1 < 1.0.0-alpha.beta < 1.0.0-beta < 1.0.0-beta.2 < 1.0.0-beta.11 < 1.0.0-rc.1 < 1.0.0.