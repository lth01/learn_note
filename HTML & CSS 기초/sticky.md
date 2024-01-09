---
Date: 2024-01-09
Title: Position Sticky
reference: https://developer.mozilla.org/ko/docs/Web/CSS/position
---
%% https://www.w3.org/TR/CSS22/visufx.html#propdef-overflow %%
- overflow body에서 안되는 이유


UAs must apply the 'overflow' property set on the root element to the viewport. When the root element is an HTML "HTML" element or an XHTML "html" element, and that element has an HTML "BODY" element or an XHTML "body" element as a child, user agents must instead apply the 'overflow' property from the first such child element to the viewport, if the value on the root element is 'visible'. The 'visible' value when used for the viewport must be interpreted as 'auto'. The element from which the value is propagated must have a used value for 'overflow' of 'visible'.
