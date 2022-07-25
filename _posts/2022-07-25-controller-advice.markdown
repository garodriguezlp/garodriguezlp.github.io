---
layout: post
title:  "Controller Advices Are Not Only Meant For Exception Handling"
date:   2022-07-25 00:00:00 +0000
categories: spring mvc
---

I bet most of you thought Spring's
[`@ControllerAdvice`](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/annotation/ControllerAdvice.html)
components where meant only for exception handling. But today, I just find out that they can be used for two purposes: the
definition of
[`@InitBinder`](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/annotation/InitBinder.html)
[`@ModelAttribute`](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/annotation/ModelAttribute.html)
annotated methods.

So, for those of you that has no clue about these two Spring MVC features, let me share what I recently discovered.

# `@InitBinder`

[Data binding](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#validation) is the Spring MVC
mechanism that magically allows us to bind user input to the domain model of our applications. And that's what we typically use
in our controllers, like this:

```java
@PostMapping("/user")
public void createUser(@Valid User user, BindingResult result) {
    if (result.hasErrors()) {
        // handle errors
    }
}
```

But, it turns out that sometimes we need to tweak some rules of the binding process, for example, avoid binding fields named
`password`, like this:

```java
@InitBinder
public void initBinder(WebDataBinder binder) {
    binder.setDisallowedFields("password");
}
```

So, even though the definition of these customizations works perfectly fine on the controller classes, doing it that way leaves no
room for reusability, and there's in when Controller Advices come to the rescue.


# `@ModelAttribute`

Chances are there are scenarios where we need to have an initialized model before handling some MVC requests in our Controller
classes, and that's exactly what we can achieve with this annotation in the following fashion:

```java
@ModelAttribute
public void populateUser(Model model) {
    model.addAttribute("user", new User());
}
```

In the same sense as with `@InitBinder`s, if needing to reuse any of this model initialization methods, we could place them inside
Controller Advices.

Ok, that's it for today, short and sweet, and I hope it helps. Thank for reading, and have a nice day!
