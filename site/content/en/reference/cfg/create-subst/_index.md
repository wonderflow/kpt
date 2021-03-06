---
title: "Create-subst"
linkTitle: "create-subst"
weight: 4
type: docs
description: >
   Create a substitution for one or more fields
---
<!--mdtogo:Short
    Create a substitution for one or more fields
-->

{{< asciinema key="cfg-create-subst" rows="10" preload="1" >}}

Substitutions provide a solution for template-free substitution of field values
built on top of [setters].  They enable substituting values into part of a
field, including combining multiple setters into a single value.

See the [creating substitutions] guide for more info on creating
substitutions.

### Examples
<!--mdtogo:Examples-->
```sh

# Automatically create setters when creating the substitution, inferring
# the setter values.
#
# 1. create a substitution derived from 2 setters.  The user will never
# call the substitution directly, instead it will be computed when the
# setters are used.
kpt cfg create-subst DIR/ image-tag nginx:v1.7.9 \
  --pattern IMAGE_SETTER:TAG_SETTER \
  --value IMAGE_SETTER=image-setter \
  --value TAG_SETTER=tag-setter

# 2. update the substitution value by setting one of the 2 setters it is
# computed from
kpt cfg set . tag v1.8.0

# Manually create setters and substitution.  This is preferred to configure
# the setters with a type, description, set-by, etc.
#
# 1. create the setter for the image name -- set the field so it isn't
# referenced
kpt cfg create-setter DIR/ image nginx --field "none" \
    --set-by "package-default"

# 2. create the setter for the image tag -- set the field so it isn't
# referenced
kpt cfg create-setter DIR/ tag v1.7.9 --field "none" \
    --set-by "package-default"

# 3. create the substitution computed from the image and tag setters
kpt cfg create-subst DIR/ image-tag nginx:v1.7.9 \
  --pattern IMAGE_SETTER:TAG_SETTER \
  --value IMAGE_SETTER=image-setter \
  --value TAG_SETTER=tag-setter

# 4. update the substitution value by setting one of the setters
kpt cfg set . tag v1.8.0
```
<!--mdtogo-->

### Synopsis
<!--mdtogo:Long-->
```
kpt cfg create-subst DIR NAME VALUE --pattern PATTERN --value MARKER=SETTER

DIR
  Path to a package directory

NAME
  The name of the substitution to create.  This is simply the unique key
  which is referenced by fields which have the substitution applied.
  e.g. image-substitution

VALUE
  The current value of the field that will have PATTERN substituted.
  e.g. nginx:1.7.9

PATTERN
  A string containing one or more MARKER substrings which will be
  substituted for setter values.  The pattern may contain multiple
  different MARKERS, the same MARKER multiple times, and non-MARKER
  substrings.
  e.g. IMAGE_SETTER:TAG_SETTER
```
<!--mdtogo-->

[setters]: ../create-setter
[creating substitutions]: ../../../guides/producer/substitutions
