---
is: "[[note]]"
of: "[[security]]"
of: "[[hacking web services]]"
---
# Notes
This looks at how browsers and html interact to provide web security, and approaches to attack them.

# Permissions-Policy
A website can define a permissions policy. Permissions are browser features, such as allowing access to location, cameras, or clipboard. By setting a Permissions-Policy, the browser knows to pop up a message asking if the site can have those permissions. This applies not only to the site, but to any third-party sites it includes.

This increases the security of a site. If an attacker finds a XSS vulnerability, they can't use it to access browser features not defined in the policy. The browser will automatically block those requests.

There are two places to apply a policy

## Permissions-Policy header
A website can send a header stating which permissions are allowable. Note, Mozilla is in the middle of changing the header name from ```Feature-Policy``` to ```Permissions-Policy```.

[Feature-Policy - HTTP | MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Feature-Policy)

## Iframe allow property
Permissions for 3rd party websites can be attached to an iframe. By default, all permissions could flow to those sites if the header allows it. But Google is changing Chrome security so that certain permissions (location, camera, microphone) are completely blocked in iframes unless the permission is added to the iframe, to reduce the risk of drive-by attacks.

[Deprecating Permissions in Cross-Origin Iframes - The Chromium Projects](https://dev.chromium.org/Home/chromium-security/deprecating-permissions-in-cross-origin-iframes)
