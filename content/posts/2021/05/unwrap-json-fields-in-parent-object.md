---
layout: :theme/post
title: "Unwrap JSON fields in parent object"
date: "2021-05-21"
tags: "jackson"
---

In this post I'll be showing how we can unwrap the fields of a class in its parent.

By serializing into JSON the following Java class

```java
public class Album {

    private String title;
    private int year;
    private Band band;

}
```

we will end up with this JSON

```json
{
    "title": "This album rocks!",
    "year": 2021,
    "band": {
        "bandName": "The Incredible Magnificents",
        "bandMembers": 33
    }
}
```

In case we want to pull the band fields up a level inside the **Album** object, we can annotate the **Band** field as **@JsonUnwrapped**

```java
@JsonUnwrapped
private Band band;
```

And by doing that, we get the contents of the band object inside the album one.

```json
{
    "title": "This album is unwrapped!",
    "year": 2021,
    "bandName": "The Incredible Magnificents",
    "bandMembers": 33
}
```
