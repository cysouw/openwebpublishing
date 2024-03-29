# The academic web

An ever growing body of academic publications is available online, ideally open access. Most publications today are prepared electronically from the start, and more and more older publications are retro-digitized. And, by their very nature, academic publications are an interconnected network of knowledge. Yet, academia-online does not fully exploit the hypertextuality of the web. It is exceedingly tedious to follow citations to other works, and references to pages or subsections have to be reached by manually scrolling through page-based (PDF) documents. In contrast, by leveraging Open Web technologies it is straightforwardly feasible to prepare academic publications in a truly interconnected way.

The [Open Web Platform](https://www.w3.org/wiki/Open_Web_Platform) is a concept advanced by the World Wide Web Consortium (W3C). It is the collection of open and royalty-free technologies which enables the Web. Basically, this means HTML content with CSS/Javascript layout and various additional technologies like MathML and SVG. With (ever ongoing) advances in HTML/CSS and browser technology, the quality and possibilities of layout are approaching the sophistication of Latex, while additionally being [adaptive/responsive](https://alistapart.com/article/responsive-web-design/). Precise cross-referencing is possible through hyperlinking with hash-formatted [URI Fragments](https://en.wikipedia.org/wiki/URI_fragment). Annotations can be added by using the [Web Annotation Standard](https://www.w3.org/annotation/) of the W3C, for example implemented by [Hypothesis](https://web.hypothes.is). Also note that HTML/CSS is much better suited to accessibility. In contrast, PDF is often unusable for screen readers.

This document proposes some best practices for the preparation of scientific publications on the Open Web with the explicit goal to add to a dialogue about the many pracitical details that this involves. As a practical example, the preparation of a linguistic monograph is detailed using the document preparation framework [Pandoc](https://pandoc.org).

Note that this plea for Open Web Publishing is largely independent of the question of Open Access Publishing. Ideally, scientific publications are open access, so that any links to them easily resolve without paywalls. However, even with restricted access to the content the principles of Open Web Publishing still hold.

# Best practices for academic web-publication

The current de-facto standard of electronic publication is to provide scientific works as page-based PDF. This format is a one-to-one reflection of the traditional book form-factor. Moving to purely web-based publications implies various changes to this model. I propose the following best practices:

- **Prepare publications in HTML+CSS** and use the capacities of modern web-browsers to render the output on the fly. This is a major change to the model of fixed renders, be it in the form of traditional physical typesetting or modern electronic typesetting by word processors or typesetting software like Latex. Many of the subsequent proposals result from adapting publications to this new typesetting paradigm.
- **Combine all parts of a publication into one single HTML file**, including appendices and supplementary texts. In contrast, data tables should be separated in a format suitable for automatic processing of raw data. However, the main text should be a single file that includes everything necessary to display the work. Even rendering large book-sized publications on the fly is technically unproblematic for modern computers with modern browsers. By using single files, publications always remain coherent and can, for example, be easily searched and scrolled through. Ideally, also include all CSS/Javascript inline into the single HTML file. In this way, a publication can easily be shared and archived off-line without losing any formatting.
- **Provide a transparent stable location for to the actual full text**. Stable identifiers like DOIs are fortunately spreading rapidly, but they mostly redirect the user to a landing page with all kind of (more or less useful) information regarding the publication. To allow for direct linking to the content there needs to be a common location for the actual full text. The obvious location should be `STABLEID/index.html` for the actual full text HTML file. However, depending on publisher's preferences this location might be used as a landing page, so an alternative location for the actual fulltext might be needed, e.g. `STABLEID/fulltext.html`.
- **Any updated or changed version of a publication should get a new stable identifier**, so links always resolve to the originally linked-to source. Updating cross-referencing to newer version should not be assumed to be just work automatically, but might need manual inspection. Note that the new identifier means a new stable ID, which still assumes the added `index.html` for linking to the actual content.
- **Add hard-coded paragraph numbering into the text** as an alternative to page numbering. In an adaptive HTML-based publication the concept of a page loses its meaning. For backward compatibility with retro-digitized works it is of course possible to add markers to indicate the original page breaks, but moving forward this model of chunking content is not practical as the page as a unit simply will become superfluous. Instead, the paragraph is an ideal chunk of content for cross-reference. To allow for stable identification of paragraphs, the paragraph numbering has to be hard-coded into the full text on publication.
- **Add transparent IDs to all crucial HTML entities** like headings, paragraphs, tables, example, etc. to allow for detailed cross-referencing using hash-based fragments. For example, by adding an ID like `id=sec4.1` to a heading in the HTML is becomes possible to use a link like `STABLEID/index.html#sec4.1` to directly link to this section. A user following such a link will be directly redirected to that specific heading. The format of the IDs can be freely chosen, as long as the IDs are clearly communicated to readers. However, for consistency I propose the following identifiers for scientific publications:
  - **sec** for any numbered header, adding the actual number in the stable published version, e.g. `sec4.1` 
  - **tbl** for numbered tables
  - **fig** for numbered figures
  - **ex** for numbered examples
  - **eq** for numbered equations
  - **fn** for numbered footnotes
  - **p** for page numbers in retro-digitized publications
  - Finally, for paragraphs the ID should simply be a number, so reference to paragraph number 34 in chapter 5 simply becomes `#5.34`.

# Example

As an example consider the manuscript being prepared for publication at [github.com/cysouw/diathesis](https://github.com/cysouw/diathesis). This manuscript is a testbed for the various practices proposed above. Because it is not yet published in a final form, the stable identifier is of course not yet stable. However, for testing the following location can be used for the full text of the publication: [cysouw.github.io/diathesis/](https://cysouw.github.io/diathesis/index.html). Try adding any fragment to this link to cross-reference a specific part, e.g.

- headings: <https://cysouw.github.io/diathesis/index.html#sec4.2>
- tables: <https://cysouw.github.io/diathesis/index.html#tbl2.1>
- figures: <https://cysouw.github.io/diathesis/index.html#fig1.2>
- examples: <https://cysouw.github.io/diathesis/index.html#ex5.20>
- footnotes: <https://cysouw.github.io/diathesis/index.html#fn3>
- paragraphs: <https://cysouw.github.io/diathesis/index.html#3.7>

Note that the usage of `index.html` has the extra benefit that it can be left out in the links, so the references are even stronger reduced, e.g.:

- examples: <https://cysouw.github.io/diathesis/#ex5.20>
- paragraphs: <https://cysouw.github.io/diathesis/#3.7>

This publication was prepared in Markdown using [Pandoc](https:pandoc.org) for the conversion to HTML, though any other toolchain could just as well be used. To extend the functionality of Pandoc, various filters (Pandoc parlance for extensions) were used:

- [`pandoc-crossref`](https://github.com/lierdakil/pandoc-crossref) for cross-reference to sections, figures and tables.
- [`crossref-adapt`](https://github.com/cysouw/crossref-adapt) for changing the IDs of these cross-references to the format proposed above, so they can transparently be cited.
- [`count-para`](https://github.com/cysouw/count-para) to add numbers to text paragraphs. Refer to a specific paragraph for example as (Cysouw 2021: #2.7). Adding the suffix to a stable link directly redirects the reader to the paragraph, e.g. (cysouw 2021: [#2.7](https://cysouw.github.io/diathesis/#2.7)).
- [`pandoc-ling`](https://github.com/cysouw/pandoc-ling) for the layout, numbering and cross-reference of linguistic examples, using the `ex` identifiers.
- [`citeproc`](https://github.com/jgm/citeproc) for citation and bibliography.

# Cross referencing

To insert cross-references to other Open Web Publications it would be highly desirable for reference managers to insert the proper links. For example, in preparing the above mentioned publication I used Pandoc's [citeproc](https://pandoc.org/MANUAL.html#citation-rendering) to prepare the in-text references and bibliography. Ideally, when I would add a citation like [@cysouw2021: #2.7] in such a work, then the hash-based suffix should ideally be transformed to `STABLEID/index.html#2.7`.
