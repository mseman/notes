---
id: cml6xpbsbt9i2yp60sww9jr
title: HTML
desc: ""
updated: 1700954125677
created: 1700954081387
---

# `<form>` tag

<https://developer.mozilla.org/en-US/docs/Web/HTML/Element/form>

---

## Forms and the back button

> The browser submits forms with full-page refreshes, and expects to get a
> response from the server that tells the browser what to do next. If the
> response is HTML, then the browser will simply render that HTML. However you
> should not do this for a successful form submission.
>
> Have you ever seen those really annoying popups when hitting the back button
> that say "Confirm Form Resubmission"? That's because the browser is trying to
> resubmit the form data to the server that you submitted when you were brought
> to that page the first time. This can be a major problem if the request that
> was submitted was a bank transfer or an airline ticket purchase.
>
> This is a really common problem with an extremely simple solution: don't
> respond with HTML for successful form submissions. Instead, respond with a'
> redirect (using the Location header and a 302 or 303 status code). This is
> commonly referred to as "Post/Redirect/Get" (PRG) pattern.

- Kent C. Dodds, epicweb.dev Full Stack Foundations, Part 4. Data Mutations,
  Intro section, #the-back-button

---

# `<link>` tag

<https://developer.mozilla.org/en-US/docs/Web/HTML/Element/link>

---
