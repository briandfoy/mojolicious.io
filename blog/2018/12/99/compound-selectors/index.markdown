---
title: 'Complex CSS Selectors'
tags:
    - css
    - selectors
    - dom
author: brian d foy
images:
  banner:
    src: '/static/1280px-Dogsled.jpg'
    alt: 'Sled dogs waiting to run'
    data:
      attribution: |-
        <a rel="nofollow" class="external text" href="https://www.flickr.com/photos/feuilllu/101083313/in/photolist-9W5xK-SWEcv6-qmMqBb-9W66e-umaBj-7Gkg2a-bnmWDf-jZPHK7-bAgNFi-bnmW3J-Hd8y3-dYNuYc-Hd9o9-22MW7qW-6qZWkL-7yzScF-24W5o6o-bBNUMi-5QPxcD-boz5qm-VmkEd7-bBeNyD-5ZuaNe-eFvSS-5ZubhH-hroLWM-dyiEjD-7x18L-6qVKL2-5BtJpc-Tat77P-V2rCfA-HdcdK-bpyFMR-qmUynZ-bBtZU6-zYsqAQ-bBNVia-RSZkN5-Hd8zq-bBtY8M-4okvTd-bBeMZz-EnynD-feMXov-qUSFPR-boTZGE-9mfjyh-9e8KsD-kspqik">Image</a> by <a href="https://www.flickr.com/photos/feuilllu/">Pierre Metivier</a> <a href="https://creativecommons.org/licenses/by-sa/2.0" title="Creative Commons Attribution-Share Alike 2.0">CC BY-SA 2.0</a>
data:
  bio: briandfoy
  description: 'use complex descriptions to target and extract data'
---

## Use complex selectors to target and extract data

---

You may be familiar with Cascading Style Sheets (CSS) for making your web pages look beautiful (but not mine, really). You can add meta data to tags and then address those tags:

	<img id="bender" class="robot" src="..." />
	<img id="fry" class="human" src="..." />
	<img id="leela" class="mutant" src="..." />

CSS can address these items by their ID or class to apply styles to them. This addressing is a "selector":

	img#fry   { border: 1px; }

	img.robot { margin: 20px; }

The HTML can be a bit more complicated. Perhaps those interesting tags are in a list:

	<ul class="employees">
	<li><img id="bender" class="robot" src="..." />
	<li><img id="fry" class="human" src="..." />
	<li><img id="leela" class="mutant" src="..." />
	</ul>

You'd like to affect only those images in that list. You can specify that as an ancestry in the selector:

	ul.employees img.human { border: 1px; }

	ul.employees img.robot { margin: 20px; }

But you can do much more with these. With [Mojo::DOM](https://mojolicious.org/perldoc/Mojo/DOM), which supports CSS Selectors [Level 3](https://www.w3.org/TR/2018/PR-selectors-3-20180911/) (and some stuff from [Level 4](https://www.w3.org/TR/selectors-4/)), you can use the same addressing to extract data.
