---
layout: default
---

Hi there! This is a walkthrough of [my GitHub](https://github.com/demery12) as well as some of the work I have done with [Haverford College Digital Scholarship](https://github.com/hcdigitalscholarship). The main motivation for the creation of this is to aid people considering me for a position since it is almost impossible to get a sense of my contributions to Digital Scholarship without guidance; since my laptop is only mildly, I did all my work on the public "Wreck Room Computer" and many of the commits under that title are me. If you are here for a different reason, I suspect this would be useful to get a sense about what Digital Scholarship does and also who I am.

We will be looking at:
- [Digital Scholarship](#digital-scholarship)
  - [Global Terrorism Research Project](#global-terrorism-research-project)
  - [Bridge](#the-bridge)
  - [Ticha](#ticha)
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

The Bridge is a classical language project the generates and filters vocabulary list from selected texts. You can check out [a fairly working version](http://bridge.haverford.edu/)! I started working on The Bridge in the Summer of 2016 and it was a _mess_. While I cleaned up some of that mess, I also added to it; this was definitely a big learning experience for me. When I started working on this there was a stupidly complicated procedure for updating the database involving hand manipulation of spreadsheets as well as some scripts. I joined all this together so that the spreadsheet could be updated with a single manage.py command (manage.py is what Django uses for various management purposes, I extended it). I then exented the Django admin site so that the updating process is even easier. The two scripts that do this are [update\_master.py](https://github.com/HCDigitalScholarship/bridge-repo/blob/df7c1e95c50ca762960f151fa5b4d8d1ae3d1f7b/new_bridge/management/commands/update_master.py) and [update\_page.py](https://github.com/HCDigitalScholarship/bridge-repo/blob/df7c1e95c50ca762960f151fa5b4d8d1ae3d1f7b/new_bridge/management/commands/update_page.py) the latter of which makes use of of [text\_import.py](https://github.com/HCDigitalScholarship/bridge-repo/blob/df7c1e95c50ca762960f151fa5b4d8d1ae3d1f7b/new_bridge/management/commands/text_import.py) which was primarly written by another student, but I did have to modify as there were some significant bugs. I would actually categorize most of my work on The Bridge as bug-finding and fixing. The other tangible things I did were in views.py and main.js. Before I draw your atention to specfics of my contributions I'd like to disclaim that I didn't write most of these files and these files exemplify my claim that The Bridge is a mess. One of interesting and particurarly sloppy additions to [views.py](https://github.com/HCDigitalScholarship/bridge-repo/blob/d0b1efa569d8f7e1a2c61e114b0db4038ef6857b/new_bridge/views.py) is all the "comment this out for production" mess. We were running into this mysterious "Too many SQL variables" problem and it was really stumping us. It turns out that you can't make requests that are too big (>100) when you are using SQLite, but it is fine for MySQL. So we had to chunk our requests whenever we were using SQLite. I highlight this solution below:

```
 #COMMENT THIS OUT FOR PRODUCTION
+            #COMMENT vvvvvvvvvvvvvvvvvvvvvv
+            #index = 100
+            vocab_intersection = []
+            #CONSIDER NOT COMMENTINGvvvvvvvvvvvvvvvvvvvvvvvvvv
+            #don't want dups
+            list_word_ids=list(set(list_word_ids))
+            #PROBABLY DON't COMMENT ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
+            #while index<len(list_word_ids):
+            #    vocab_intersection.extend(list(word_appearences.objects.filter(read_texts_filter,
+            #            word__in=list_word_ids[index-100:index],text_name=text).values('word')))
+            #    index += 100
+            #vocab_intersection.extend(list(word_appearences.objects.filter(read_texts_filter,
+            #        word__in=list_word_ids[index-100:index],text_name=text).values('word')))
+            #COMMENT ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
```
Now, it would have been _much_ better to have some constant that we could flip to turn on SQLite mode and then an if statement to do whatever was appropriate. But as I said, this was a learning experience.

I also generate the data in views for a new feature; a count of the number appearance of each word in the text and displaying the first appearance of that word (we actually weren't allowed to display all the appearances for copyright reasons). I also added a local definition feature; some words have definitions specific to a text. I check to see if a word/text has local definitions and default to displaying those if it does. We were also generating a lot of duplicate words that we didn't want, which in Python is an easy ```list(set(original_list)``` to fix.

[main.js](https://github.com/HCDigitalScholarship/bridge-repo/blob/d0b1efa569d8f7e1a2c61e114b0db4038ef6857b/static/main.js) is a very large JavaScript file that really should be several smaller JavaScript files; it was large when I got to it, but I added my share of junk to it (if you look there are a lot of commented out print statements that include "Dylan". Those were my bad.) I also fixed a bug where the slideout panel was turning invisible instead of sliding back in. This would still be clickable so it would block what you thought you were clicking. I also added the ability to export the vocab list, to a tsv and to a pdf. I also fixed a bug where not enough words were showing up, because some of the data had multiple parts of speech. I'll highlight this part, I am particurarly proud of this one:
```
/* Given array of word categories, returns an array of relevant wordTable objects.
 * wordTable objects are retrieved from word category arrays in words_data.*/
function filterWordData(pos_list) {
    var words_data_filtered = [];
    var words_data_keys = Object.getOwnPropertyNames(words_data);
    /* So needed to do is identify 'Adverb, preposition' as 'adverb'
     * and 'preposition'. I made a dictionary (pos_dictionary) with a
     * list of everything it should add when it finds that word.
     * So 'Adverb' will be:
     * 'Adverb' : ['Adverb', 'Adverb, Conjunction', 'Adverb, Preposition'] etc.
     */

    // Intialize a dictionary with all the words we want to add
    var pos_dictionary = {};
    for (var i=0; i<pos_list.length; i++){
	var pos = pos_list[i];
	pos_dictionary[pos]= [];
		
    }

    // Now add (hopefully) all pos from the data appropriately
    for (var i=0; i<words_data_keys.length; i++) {
	var key = words_data_keys[i];
        // A regex that matches 1 or more commas and any number of spaces
        prop_list = key.split(/,+\s*/);
	for (var j=0; j<prop_list.length; j++){
           var prop = prop_list[j];
	   if(pos_dictionary[prop] != undefined){
	      pos_dictionary[prop].push(key);
	      //Always group Adj-1 and Adj-2
              if(prop == 'Adjective_1'){
                 pos_dictionary['Adjective_1'].push('Adjective_2');
		 //pos_dictionary['Adjective_1'].push('Adjective_');
              }
           }
	}
    }

    // pos_dictionary now complete

    var modified = {}; // modified: tracks the things we have already changed so we don't double add
    for (var i=0; i<words_data_keys.length; i++) {
        var key = words_data_keys[i];
        var key_index = pos_list.indexOf(key);
	if (key_index>=0) {
	    for(var j=0; j<pos_dictionary[key].length; j++){
	      var prop = pos_dictionary[key][j];
	      if (modified[prop] == undefined){
	        modified[prop] = true;
                words_data_filtered = words_data_filtered.concat(words_data[prop]);
	      }

	    }
	   // We used to removed items instead of marking them as modified
	   // but then you couldn't properly view the list in console
	   // so I switched it
           // pos_list.splice(key_index,1); //rm that key from pos_list
        }
    }
    console.log(words_data_filtered, 'Words returned by filter');
    console.log(typeof words_data_filtered);
    return words_data_filtered;
}
```

This was a more recent fix, so it is better commentented and coded, but that's not the reason I'm proud of it. If you have been looking at any of the codebase, or just reading this, you should have an idea that The Bridge is a little tricky to navigate. I was able to find an fix this error when the only symptom was a couple words not appearing and that is what I am proud of.


To summarize [here is the diff that best encapsulates my contributions](https://github.com/HCDigitalScholarship/bridge-repo/compare/e8550975f31005be66fc5837b5c0a2884448a34e...d0b1efa569d8f7e1a2c61e114b0db4038ef6857b). There were some other people working on it, but most changes in this timeframe were me. It also might not actually be that useful, but I'll throw it at you anyway.

## [Ticha]()

Ticha is a linguistics project that focuses on the Colonial Zapotec Language. You can check out [the site](https://ticha.haverford.edu/en/) it's probably my favorite. I might like it so much because I didn't contribute to this one as much as the previous, but I did have one huge part in it. It was also my crowning achievment in messy code. I present to you [ticha\_magic.py](https://github.com/HCDigitalScholarship/ticha-django-site/blob/3bc7a54edd393495098cfe9657da7775c076df1d/zapotexts/ticha_magic.py). I wouldn't try to read that, it would be a huge waste of time and a real headache to follow, but it basically is an xml parser that, following a lot of specific rules, converts that XML to HTML. My bosses were actually _really_ impressed with this and in fairness it was really hard. But it is just so messy, I don't feel that good about it. The DS student that has done most of this project somewhat tenatively confronted me about updating ticha\_magic.py and making it a lot neater. I think he was worried about stepping on my toes, which I appreciate, but there was also no one in the world that thought it needed to be updated more. I also thought it was a one in done senario where we run this script on our one ridiculously long XML file and then that's it, but apparently that was not the case. With all that being said, it was still a pretty impressive feat. And it worked!



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
