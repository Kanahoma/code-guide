# Markup

Markup should not define the look and feel of the content on the page beyond the most basic structural concepts such as headers, paragraphs, and lists.

**Reducing Markup** Whenever possible, avoid superfluous parent elements when writing HTML. Many times this requires iteration and refactoring, but produces less HTML.

Websites should be written using the least amount of markup that accomplishes the goal.

## Structure

Our focus is on writing clean and semantic markup. Semantic markup refers to the use of HTML markup to reinforce the meaning of the information in web pages, rather than just defining its presentation. This approach results in markup that can be processed not only by traditional web browsers but also by many other user agents.

Semantic elements, such as `<header>`, `<nav>`, `<footer>`, or `<article>`, have clearly defined meanings for both the browser and the developer. These elements do a much better job of explaining the content within them compared to generic elements like `<span>` or `<div>`. However, this doesn't mean that we never use `<div>` elements in our markup. We just believe in using the appropriate semantic element for each job.

## Schema.org Markup

In order to enhance how search engines comprehend information and deliver the best search results, Google, Bing, Yandex, and Yahoo! collaborated to create [Schema.org](https://schema.org/). Search engines receive structured data they can use to improve the way pages display in search results by adding Schema.org data to HTML.

For instance, Schema.org and rich snippets are responsible if you've ever looked up a restaurant and seen star ratings in the search results.

The purpose of schema.org data is to explain to search engines what your data actually means rather than just what it says.

In order to ensure that our clients have the best search visibility possible, we use Schema.org data when appropriate and fair.

There are two ways to provide Schema.org data: “microdata” markup added to a page’s structure, or a JSON-LD format. Google recommends developers adopt JSON-LD, which isolates the data intended for search engines from the markup intended for user agents. Although the JSON-LD spec allows for linking to data in an external file, search engines only parse JSON-LD data when it appears within a `<script type="application/ld+json">` tag.

To ensure that Schema.org markup is valid, it should be tested using the [Google Structured Data Testing Tool](https://search.google.com/structured-data/testing-tool/u/0/).

## Accessibility

All our projects must at the very least meet [WCAG 2.1 Level A Standards](https://www.w3.org/WAI/standards-guidelines/wcag/).

### Accessible Forms

Forms can be a challenge for accessibility, but by following some key guidelines, we can ensure that they meet accessibility standards. One important step is to use the    `<label>` element to associate a label with each form field. This can make the form more readable for screen readers and other assistive technologies.

Another way to improve accessibility is to group form elements logically using the `<fieldset>` element. This can be particularly helpful for users who rely on screen readers or who have cognitive disabilities.

Additionally, it is important to ensure that all interactive elements in the form are keyboard navigable. This can make the form easier to use for people with vision or mobility disabilities, as well as those who are not able to use a mouse. The tab order should be based on a logical source order of elements, and if changes to the order are necessary, it may indicate that the markup or layout should be revised to improve accessibility. In this case, it is best to have a conversation with the designer to discuss possible updates.

---
> Last Modified: {docsify-updated}
