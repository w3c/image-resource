# Image Resource

## Background

The `ImageResource` dictionary represents an image to be used as part of a web application. The dictionary includes
additional information about the image (besiceds the src) that allows the user agent to choose the best image to show
 in an environment when a developer provides multiple `ImageResource`s. It is currently defined in the [Manifest spec](https://www.w3.org/TR/appmanifest/#imageresource-and-its-members).

The `ImageResouce` is generic enough that it is being used in other specs as well (such as
[Background Fetch](https://wicg.github.io/background-fetch/#dom-backgroundfetchuioptions-icons)). This explainer will
cover how to extract `ImageResource` into its own spec, and migrate specs using it to the new definitions.

## Web IDL

The new definition of `ImageResource` will only keep the generic members.

```webidl
dictionary ImageResource {
  required USVString src;
  DOMString sizes;
  USVString type;  
}
```

## Scope

The spec will include the IDL and the algorithms to parse the fields. However, the spec will not include:

* Fetching the image

  There are application-specific fields needed to create an appropriate request, such as `client`,
  `service-worker mode`, among others.

* Choosing an appropriate `ImageResource`

  This is also application specific, where different applications operate under different constraints. Furthermore,
  this should ultimately be a user agent decision.

## Migration

`ImageResource` is defined as a dictionary, and is therefore not web-exposed. Moving things around will not cause
any interoperability issues. 

### Manifest

The Manifest spec also defines manifest-specific fields, which wouldn't make sense in other contexts. The Manifest spec
will define its own type by extending `ImageResource` and including the new members it needs. A few things will have
to be reworded in the spec as well.

```webidl
dictionary ManifestImageResource : ImageResource {
  USVString platform;
  USVString purpose;
}
```

### Background Fetch

Background Fetch can use the new `ImageResource` directly.
