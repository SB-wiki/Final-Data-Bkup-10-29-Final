---
icon: elementor
---

# Content,-Itemset-and-Question-Relationship-Refactoring

### Introduction:

Item set should be a collection of questions where questions will have hasSequenceMember relationship with the member questions. Content should have association relationship with Item set.

### Background & Problem statement:

* Currently content have association relationship with questions. And association relationship does not support index value.
* Since content and question relationship does not have any index value, we are not able to render questions based on index value.
* As per the existing Item set (V3) implementation, Item set is a collection of questions where all the questions are linked to Item set with hasMember relationship which does not support index.

### Design:

* Item set will be a collection of questions where each question will be linked to Item set with **hasSequenceMember** relationship where each relationship will have one index value.
* Instead of linking to each questions, Content should be linked to Itemset with **associatiedTo** relationship.
* Content will have **one-to-one** relationship with Item set.
* Support for Itemset **schema version 2**

Item Set V2 API:Create Item set:POST: itemset/v3/create

Response:

Update Item set:

PATCH: itemset/v3/update/{identifier}

Response:

Read Item set:GET: itemset/v3/read/{identifer}Response:

Retire Item Set:

DELETE: itemset/v3/retire/{identifier}

Response:

Review Item set:POST: itemset/v3/review/{identifier}

Response:

Item set Publish API:

* There will be Item set publish API which will publish the Itemset with a particular **pkgVersion** .
* It will generated a **html template** (previewUrl: it is Itemset metadata) which will hold all the questions linked to item set at the time of Item set publish.
* Currently Item set publish API will be exposed through **Content Publish API** - When content gets publish Item set will also get published.

PDF generator API:

* It will be completely separate and **generic implementation** .
* It will expect HTML template and will **generate pdf file** .
* It will **generate and upload pdf file** to cloud store and return link as response. (need to discuss)

Content Publish API:

* **Item set publish API** will be exposed to **content publish pipeline** .
* As part of content publish, Item set will also be published which will generate Item set **previewUrl** using all the question linked (i.e. set of all the questions linked to Item set, in a sequential way of index) and **tag with Item set as metadata** .
* Using Item set **previewUrl** , **PDF generator** API will generate and return **pdf file URL** link which will be linked to content metadata as **itemSetPreviewUrl** .

***

\[\[category.storage-team]] \[\[category.confluence]]
