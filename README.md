Recommended Behaviors for Auto-Sharing
======================================

Version: 1.0   
Author(s): [Simeon Simeonov](http://about.me/simeonov) (sim at swoop dot com)  
Contributor(s): [Tantek Çelik](http://tantek.com) (tantek at tantek dot com)


Motivation
----------

To make it easier to share their content, publishers are deploying technologies that automatically add sharing calls to action to various Web page elements. For example, the [Pinterest Pin It Button for Images](http://wordpress.org/support/plugin/pinterest-pin-it-button-for-images/page/2) Wordpress plugin will overlay Pin It buttons on images to enable fast pinning. We refer to these types of technologies broadly as _auto-sharing plugins_ and the user interface elements they introduce as _auto-sharing UI_.

In the absence of meta-data guiding the behavior of auto-sharing plugins there are certain common situations where their behavior may lead to unexpected results. Common examples include auto-sharing UI showing on buttons, background images, headings that happen to be images, etc.

Auto-sharing plugin vendors sometimes allow behavior customization but that puts the onus on the user of the plugin, which is not optimal, especially in the case of other plugins or services introducing UI elements into a Web page. Publishers should not be tasked with managing the interactions of a plethora of plugins and services. What is needed is a simple way for both publishers and third party plugins/services to signal to auto-sharing plugins what should and should not be shared.


Approach
--------

The approach we have taken is to identify the common cases where elements on a Web page should _not_ show auto-sharing UI and attach meta-data to those elements using conventions established by existing Web standards, namely [Accessible Rich Internet Applications (WAI-ARIA) 1.0](http://www.w3.org/TR/wai-aria/). WAI-ARIA 1.0 is a [W3C](http://www.w3.org) Candidate Recommendation, which is W3C terminology for a final draft of a standard. Specifically, we recommend the use of only two roles from the ARIA [roles model](http://www.w3.org/TR/wai-aria/roles): 

- [`button`](http://www.w3.org/TR/wai-aria/roles#button): for content elements that drive actions and should not show auto-sharing UI. An HTML element has the `button` role if it has the `role="button"` attribute/value pair.

- [`presentation`](http://www.w3.org/TR/wai-aria/roles#presentation): for content elements that do not drive actions and also should not show auto-sharing UI. An HTML element has the `presentation` role if it has the `role="presentation"` attribute/value pair.

Because some auto-sharing plugins already support behavior customization via CSS class names, we also provide a backward compatibility mode where the ARIA roles can be expressed as CSS classes:

- `role="button"` with the CSS class `aria-button`

- `role="presentation"` with the CSS class `aria-presentation`


Publisher recommendations
-------------------------

1. Mark up your content using the `role` attribute and the `aria-*` CSS classes.

1. If your auto-sharing plugin supports behavior customization through CSS classes, add `aria-button` and `aria-presentation` to the list of CSS classes that should not activate auto-sharing UI.

### Example

Consider the following HTML snippet for a button.

```html
<div id="btn_next" class="button">
    <img src="http:/example.com/button.jpg" alt="Next">
</div>
```

Following this recommendation, the markup should change to:

```html
<div id="btn_next" class="aria-button button" role="button">
    <img src="http:/example.com/button.jpg" alt="Next">
</div>
```


Auto-sharing plugin vendor recommendations
------------------------------------------

Do not display auto-sharing UI on any HTML element (or its children) if the element has one of:

- The attribute `role` with values `button` or `presentation`

- A CSS class named `aria-button` or `aria-presentation`


Compliant auto-sharing plugins
------------------------------

This section will include a list of auto-sharing plugins that support this recommendation.


Further reading
---------------

For more information on WAI-ARIA roles, see [ARIA and Progressive Enhancement](http://www.alistapart.com/articles/aria-and-progressive-enhancement/).

For information on how using ARIA roles, especially `role="presentation"` helps with accessibility, see [this post](http://john.foliot.ca/aria-hidden/).


Next steps
----------

[Create an issue](https://github.com/swoop-inc/auto-sharing/issues/new) to give us feedback on this document or tell is that your auto-sharing plugin has implemented support for this recommendation.


Copyright
---------

This document is placed into the public domain by the authors. There are no usage, distribution, re-printing, or any other restrictions of any kind with regards to the text or content of this specification.

