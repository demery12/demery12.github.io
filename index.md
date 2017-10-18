---
layout: default
---

Hi there! This is a walkthrough of [my GitHub](https://github.com/demery12) as well as some of the work I have done with [Haverford College Digital Scholarship](https://github.com/hcdigitalscholarship). The main motivation for the creation of this is to aid people considering me for a position since it is almost impossible to get a sense of my contributions to Digital Scholarship without guidance. If you are here for a different reason, I suspect this would be useful to get a sense about what Digital Scholarship does and also who I am.

We will be looking at:
- [Digital Scholarship](#digital-scholarship)
  - [Global Terrorism Research Project](#global-terrorism-research-project)
  - [Bridge](#the-bridge)
  - Ticha
  - Beyond Penn's Treaty
  - Quakers and Mental Health
- BODL-Service
- img\_exploration
- CodingChallenges

Let's get started!
# [](#ds)Digital Scholarship

The term Digital Scholarship refers to the use of technology to aid scholarship. Our Digital Scholarship team focuses less on the actual scholarship and more on the technology side, realizing professors' and students' ideas of what technology can do for them. We are admittedly not an experienced software team, but we try our best to be making use of virtual environments, production servers on Droplet and of course Git for version control. I have played a central role on almost all of our active projects:


## [](#gtrp)[Global Terrorism Research Project](https://github.com/HCDigitalScholarship/global-terrorism-research/tree/e135882407ae773a7d216c4d28876d67d08575bd)

The Global Terrorism Research Project (GTRP) is essentially a database of all public statements made by al Qaeda. Researchers have tagged each of these statements with keywords and the context in which a keyword is being used. The site was originally on Drupal and it frankly [doesn't work that well](https://ds-drupal.haverford.edu/aqsi/). I was originally tasked with starting to convert the site into a Django site and I would then hand off my progress to  summer worker. I made it further than expected and ended up adding search functionality with whoosh and filtering functionality. [Here in whoosh\_schema.py](https://github.com/HCDigitalScholarship/global-terrorism-research/blob/e135882407ae773a7d216c4d28876d67d08575bd/gtr_site/management/commands/whoosh_schema.py) I build the appropriate search index based off of the [models](https://github.com/HCDigitalScholarship/global-terrorism-research/blob/e135882407ae773a7d216c4d28876d67d08575bd/gtr_site/models.py). In [views.py](https://github.com/HCDigitalScholarship/global-terrorism-research/blob/e135882407ae773a7d216c4d28876d67d08575bd/gtr_site/views.py) the search functions takes a post request and contructs a query for whoosh. Now I _really_ could have made this a bit more modular as I was repeatedly doing the same thing and that's actually something I'm working on now. There is also some basic HTML in the [templates folder](https://github.com/HCDigitalScholarship/global-terrorism-research/tree/e135882407ae773a7d216c4d28876d67d08575bd/gtr_site/templates/gtr_site) that I wrote, but it is nothing particularly.

All code at [this point](https://github.com/HCDigitalScholarship/global-terrorism-research/tree/e135882407ae773a7d216c4d28876d67d08575bd) in the project has been written by me (or automatically generated Django).

## [](#bridge)[The Bridge](https://github.com/HCDigitalScholarship/bridge-repo/tree/df7c1e95c50ca762960f151fa5b4d8d1ae3d1f7b) 

The Bridge is a classical language project the generates and filters vocabulary list. You can check out [a fairly working version](http://bridge.haverford.edu/)! I started working on The Bridge in the Summer of 2016 and it was a _mess_. While I cleaned up some of that mess, I also added to it; this was definitely a big learning experience for me. When I started working on this there was a stupidly complicated procedure for updating the database involving hand manipulation of spreadsheets as well as some scripts. I joined all this together so that the spreadsheet could be updated with a single manage.py command (manage.py is what Django uses for various management purposes, I extended it). I then exented the Django admin site so that the updating process is even easier. The two scripts that do this are [update\_master.py](https://github.com/HCDigitalScholarship/bridge-repo/blob/df7c1e95c50ca762960f151fa5b4d8d1ae3d1f7b/new_bridge/management/commands/update_master.py) and [update\_page.py](https://github.com/HCDigitalScholarship/bridge-repo/blob/df7c1e95c50ca762960f151fa5b4d8d1ae3d1f7b/new_bridge/management/commands/update_page.py) the latter of which makes use of of [text\_import.py](https://github.com/HCDigitalScholarship/bridge-repo/blob/df7c1e95c50ca762960f151fa5b4d8d1ae3d1f7b/new_bridge/management/commands/text_import.py) which was primarly written by another student, but I did have to modify as there were some significant bugs.


```

#### [](#header-4)Header 4

*   This is an unordered list following a header.
*   This is an unordered list following a header.
*   This is an unordered list following a header.

##### [](#header-5)Header 5

1.  This is an ordered list following a header.
2.  This is an ordered list following a header.
3.  This is an ordered list following a header.

###### [](#header-6)Header 6

| head1        | head two          | three |
|:-------------|:------------------|:------|
| ok           | good swedish fish | nice  |
| out of stock | good and plenty   | nice  |
| ok           | good `oreos`      | hmm   |
| ok           | good `zoute` drop | yumm  |

### There's a horizontal rule below this.

* * *

### Here is an unordered list:

*   Item foo
*   Item bar
*   Item baz
*   Item zip

### And an ordered list:

1.  Item one
1.  Item two
1.  Item three
1.  Item four

### And a nested list:

- level 1 item
  - level 2 item
  - level 2 item
    - level 3 item
    - level 3 item
- level 1 item
  - level 2 item
  - level 2 item
  - level 2 item
- level 1 item
  - level 2 item
  - level 2 item
- level 1 item

### Small image

![](https://assets-cdn.github.com/images/icons/emoji/octocat.png)

### Large image

![](https://guides.github.com/activities/hello-world/branching.png)


### Definition lists can be used with HTML syntax.

<dl>
<dt>Name</dt>
<dd>Godzilla</dd>
<dt>Born</dt>
<dd>1952</dd>
<dt>Birthplace</dt>
<dd>Japan</dd>
<dt>Color</dt>
<dd>Green</dd>
</dl>

```
Long, single-line code blocks should not wrap. They should horizontally scroll if they are too long. This line should be long enough to demonstrate this.
```

```
The final element.
```
