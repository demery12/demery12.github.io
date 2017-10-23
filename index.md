---
layout: default
---

Hi there! This is a primarily a walkthrough of the work I have done with [Haverford College Digital Scholarship](https://github.com/hcdigitalscholarship). I'll breifly touch on the projects in [my GitHub](https://github.com/demery12) as well. The main motivation for the creation of this is to aid people considering me for a position since it is almost impossible to get a sense of my contributions to Digital Scholarship without guidance; since my laptop is only mildly functional, I did all my work on the public "Wreck Room Computer" and many of the commits under that title are me. I present my code in an honest manner; while I have developed wonderful problem solving skills, and had the oportunity to jump into projects with a good amount of preexisting and messy code, I haven't had the benefit of working on a proper software team with experienced members to guide me. I am eager to continue to improve as a developer!

We will be looking at:
- [Digital Scholarship](#digital-scholarship)
  - [Global Terrorism Research Project](#global-terrorism-research-project)
  - [Bridge](#the-bridge)
  - [Ticha](#ticha)
  - [Beyond Penn's Treaty](#beyond-penns-treaty)
  - [Quakers and Mental Health](#quakers-and-mental-health)
- BODL-Service
- img\_exploration
- CodingChallenges

Let's get started!
# [](#ds)Digital Scholarship

The term Digital Scholarship refers to the use of technology to aid scholarship. Our Digital Scholarship team focuses less on the actual scholarship and more on the technology side, realizing professors' and students' ideas of what technology can do for them. We are admittedly not an experienced software team, but we try our best to be making use of virtual environments, production servers on Droplet and of course Git for version control. As a team we are still learning and as an individual I certainly have a lot to learn as well. But, having played a role on almost all of our active projects, I at least have some experience learning:


## [](#gtrp)[Global Terrorism Research Project](https://github.com/HCDigitalScholarship/global-terrorism-research/tree/e135882407ae773a7d216c4d28876d67d08575bd)

The Global Terrorism Research Project (GTRP) is essentially a database of all public statements made by al Qaeda. Researchers have tagged each of these statements with keywords and the context in which a keyword is being used. The [site was originally on Drupal](https://ds-drupal.haverford.edu/aqsi/) and it frankly [doesn't work that well](https://ds-drupal.haverford.edu/aqsi/search/site). I was originally tasked with beginning to convert the site into a Django site and I would then hand off my progress to a summer worker. I made it further than expected and ended up adding search functionality with Whoosh and filtering functionality. [Here in whoosh\_schema.py](https://github.com/HCDigitalScholarship/global-terrorism-research/blob/e135882407ae773a7d216c4d28876d67d08575bd/gtr_site/management/commands/whoosh_schema.py) I build the appropriate search index based off of the [models](https://github.com/HCDigitalScholarship/global-terrorism-research/blob/e135882407ae773a7d216c4d28876d67d08575bd/gtr_site/models.py). In [views.py](https://github.com/HCDigitalScholarship/global-terrorism-research/blob/e135882407ae773a7d216c4d28876d67d08575bd/gtr_site/views.py) the search functions takes a post request and contructs a query for whoosh. Now I _really_ could have made this a bit more modular as I was repeatedly doing the same thing and that's actually something I'm working on now. There is also some basic HTML in the [templates folder](https://github.com/HCDigitalScholarship/global-terrorism-research/tree/e135882407ae773a7d216c4d28876d67d08575bd/gtr_site/templates/gtr_site) that I wrote, but it is nothing particularly exciting.

All code at [this point](https://github.com/HCDigitalScholarship/global-terrorism-research/tree/e135882407ae773a7d216c4d28876d67d08575bd) in the project has been written by me (or automatically generated Django). I have since passed it on to a summer worker, who took a few steps foward and a few steps back, and now I am working on getting search and filtering to work again.

## [](#bridge)The Bridge 

The Bridge is a classical language project that generates and filters vocabulary list from selected texts. You can check out [a fairly working version](http://bridge.haverford.edu/)! I started working on The Bridge in the Summer of 2016 and it was a _mess_. While I cleaned up some of that mess, I also added to it; this was definitely a big learning experience for me. It's worth noting that I can't directly show you this repository since it is private; the people that generate the data are very protective over it (they may or may not be Belgian monks?!? I was never clear on that point.) I will be able to pull some code snippets, but I want to be as cautious as I can about it. Let's get into the work I did.

When I started working on The Bridge there was a stupidly complicated procedure for updating the database involving hand manipulation of spreadsheets and a couple scripts. I joined all this together so that the spreadsheet could be updated with a single manage.py command (manage.py is what Django uses for various management purposes, I extended it). I then exented the Django admin site so that the updating process is even easier.  The scripts I am referencing here are _somewhat_ revealing of the nature of the data, so I better not show them. They are also not wildly exciting; some csv manipulation, and some calls with Django to update or write data.

I would actually categorize most of my work on The Bridge as bug-finding and fixing. A paticular bug I got snagged on was a mysterious "Too many SQL variables" problem. It turns out that you can't make requests that are "too big" when you are using SQLite, but it is fine for MySQL. So we had to chunk our requests whenever we were using SQLite. I highlight this solution below:

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
Now, it would have been _much_ better to have some constant that we could flip to turn on "SQLite mode"—just some boolean that leads into an if-else sequence... as I said, this was a learning experience.

I also generate some data for a new feature; a count of the number appearance of each word in the text and displaying the first appearance of that word (we actually weren't allowed to display all the appearances, again for data protection reasons). I also added a local definition feature; some words have definitions specific to a text. I check to see if a word/text has local definitions and default to displaying those if it does. We were also generating a lot of duplicate words that we didn't want, which in Python is an easy ```list(set(original_list))``` to fix.

I also spent a lot of time editing main.js, a very large JavaScript file that really should be several smaller JavaScript files. It was large when I got to it, but I added my share of junk to it (including a lot of print statements that include "Dylan" as a marker. Those were my bad. I did eventually delete them.) Within this file, I fixed a bug where the slideout panel was turning invisible instead of sliding back in. This would still be clickable so it would block what you thought you were clicking. I also added the ability to export the vocab list, to a tsv and to a pdf. I also fixed a bug where not enough words were showing up, because some of the data had multiple parts of speech and these were filtered out. I'll highlight this part, I am particularly proud of this one:
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

This was a more recent fix, so it is better commentented and coded, but that's not the reason I'm proud of it. Hopefully I've whined enough that you have an idea about how tricky The Bridge codebase was to navigate. I was able to find an fix this error when the only symptom was a couple words not appearing and that is what I am proud of.

## Ticha

Ticha is a linguistics project that focuses on the Colonial Zapotec Language. You can check out [the site](https://ticha.haverford.edu/en/) it's probably my favorite. I might like it so much because I didn't contribute to this one as much as the previous, so I am less aware of its flaws. I did have one huge part in this project and it was also my crowning achievment in messy code. I called it ticha\_magic as it takes a big sloppy XML file and (in a big sloppy way) turns it into nice clean HTML, filled with hyperlinks and meaningful paragraphs, headers and page breaks. I again cannot directly show you the code because the repository is private; the code boils down to a bunch of conditionals tracking the opening and closing of tags, building up HTML in the process and erroring if there was some bad XML. There were a bunch of corner cases and errors in the XML making this process a lot more complex than it may seem. My bosses were  _really_ impressed with this and in fairness it was really hard. But it is just so messy, I don't feel that good about it. The DS student that has done most of this project somewhat tenatively confronted me about updating ticha\_magic.py and making it a lot neater. I think he was worried about stepping on my toes, which I appreciate, but there was also no one in the world that thought it needed to be updated more. The name stuck though!

## [Beyond Penn's Treaty](https://github.com/HCDigitalScholarship/QI)

[Beyond Penn's Treaty](https://pennstreaty.haverford.edu/) was the first project I really worked on and the first time I made a Django importer. That one took me way longer than it does now. We were trying to use a plugin that wasn't working which it turns out is much more difficult to resolve than just writing your own importer. This was also my first time working with Django and I remember having some trouble setting up relational fields in the database (well really in models with Django).  I'll link the repository and here is [a big commit](https://github.com/HCDigitalScholarship/QI/commit/83f5b5508bc8865e4656a0f4aa242ff08eb90bd0) from me. These are both things you can probably skip, at this point I am just shooting for completeness, but if you do look at that big commit, you can see in admin.py all the importing stuff I did (in a really bad way), and generate.py is another parser I wrote, this one a little tidier and much simpler, also converting XML to HTML, and again more converting in new\_XML\_to\_HTML.py (I didn't fully write this one, but I made a lot of changes/got it properly working).

## [Quakers and Mental Health]

I _thought_ my greatest contribution to this project was getting rid of a carousel but [apparently that change didn't stick](http://qmh.haverford.edu/occu/#row10). I also made an importer, as I will do, and really tidied up the way the Django admin was looking, by changing the way models were displayed and adding some built in django filtering features. For this importer I was mostly able to get things working with the Django-import-export module, but there was one model I had to do a workaround for. This is for the field "Patient Entry" which needs to link to a patient foreign key, but sometimes there were brand new people in the patient entry spread sheet. If I recall correctly, there were also tons of repeated entries with some having more information than others. I used date to distinguish entries for the same person, and then took the one with the most information. The repository is private (probably for copy-righted data reasons), but I can share the code snippet I am referencing:

```
def import_obj(self, obj, data, dry_run):
		if "patient_Info" not in data:
			first_name = data['first_name']
			last_name = data['last_name']
			#print "Here is the data for:",first_name,last_name
			#print data,"\n"
			person_list = Person.objects.filter(first_name=first_name,last_name=last_name)
			if len(person_list) == 0:
				print "Need to make a new person!"
				patient =  RoleType.objects.get(role='Patient')
				new_person = Person(
									first_name=first_name,
									last_name=last_name,
									alias=data['alias'],
									birth=data['rough_year_of_birth'],
									death=data["date_of_death"],
									gender=data['gender'],
									role=patient
									) #one day add birthplace as hometown?
				print "Made new person",first_name,last_name
				new_person.save()
				print new_person.id
				data['patient_Info']=new_person.id
				person_id=new_person.id
			elif len(person_list) == 1:
				print "Yay found a unique pre-existing person"
				person_id=person_list[0].id
				data['patient_Info']=person_id
				# Might need to update this so that it updates more things and only does it if the thing already there is blank
				update_person = Person.objects.get(id=person_id)
				update_person.alias=data['alias']
				update_person.save()
				print "Updated:", update_person
			else:
				print "Names aren't unique, inconvience"
				for name in person_list:
					if name.role.role == 'Patient':
						person_id=name.id
						break
				data['patient_Info']=person_id
				# Might need to update this so that it updates more things and only does it if the thing already there is blank
				update_person = Person.objects.get(id=person_id)
				update_person.alias=data['alias']
				update_person.save()
			if data['day'] != '':
				data['admitdate'] = data['month']+'/'+data['day']+'/'+data['year']
			elif data['month'] != '':
				data['admitdate'] = data['month']+'/'+data['year']
			elif data['year'] != '':
				data['admitdate'] = data['year']

			# check to see if there is already a patient entry for this person
			patient_list = PatientEntry.objects.filter(patient_Info=person_id)
			for entry in patient_list:
				month = ""
				for char in entry.admitdate:
					if char == '/':
						break
					else:
						month+=char
				year = ""
				for char in entry.admitdate[::-1]:
					if char == '/':
						break
					else:
						year = char+year			
				#print entry.admitdate
				#print month,year
				if data["month"]+data["year"] == month+year:
					print "Updating entry!"
					data['id']=patient_list[0].id
					data['status']=patient_list[0].status
					data['exitdate']=patient_list[0].exitdate
					if month != '':
						data['admitdate']=patient_list[0].admitdate
					data['weekly_Rate']=patient_list[0].weekly_Rate
				else:
					print "making a new entry for this person"
			if len(patient_list) == 0:
				print "making a new person"
			print "Modified data:"
			print data
		for field in self.get_fields():
			#print field
			field_name = self.get_field_name(field)
			print field_name
			print obj.id
			self.import_field(field, obj, data)
```






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
