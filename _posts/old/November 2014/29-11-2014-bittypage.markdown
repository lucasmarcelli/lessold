---
layout: old-post
title: "Generating the Kitty Page"
subtitle: "That was an interesting exercise."
category: coding
project: this-website
---

[I really like my cat.](/kitty)

I wanted to make a page where I could drop photos of my cat and they would automagically populate the page, but since I'm using [Jekyll](http://jekyllrb.com/), which only serves static content, this proved to be a bit more complicated than I expected.

My initial idea was to simply to loop through a folder of cleverly named photos (1.jpg, 2.jpg etc) using a liquid for loop, like so:

{% raw %}
`{% for i in (1..5) %}` <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`<img src="/img/bitty/{{ i }}.jpg">` <br>
`{% endfor %}`
{% endraw %}

This is actually a pretty elegant solution, but it has some problems. For one, every time I would want put a new photo in, I'd  have to increase the counter manually each time. This would also not allow me to include my genius little blurb underneath each photo.

I knew that Jekyll wouldn't give me anything like this out of the box, so this would have to be done through some sort of plugin. I *also* knew that I couldn't be the only person who had ever wanted something like this, a suspicion that a quick Google search [confirmed](https://github.com/sillylogger/jekyll-directory/blob/master/_plugins/directory_tag.rb).

This plugin generates a directory list according to the sames rules as `ls` on unix systems. I realized that by naming my pictures what I want the blurb to be, I could append it underneath using the `slug` attribute it creates for me. However, the files would be listed in alphabetical order. I wanted Bitty's photos displayed in chronological order, with the newest at the top.

It was almost perfect! I just added one little line of code:

`slug.gsub!(/\d+/, '')`

This removed any numbers from `slug`, which means I could prepend numbers to my filenames to make them showup in the order I wanted. I chose to prepend them with 3 digits, effectivley giving me 899 photos on the page before I have to figure out a better way. This resulted in a pretty funny looking folder:

<p class="img-text">
	<img src="/assets/img/posts/old/img/2014Nov/result.png" title="So much kitty">
	I wonder how long it'll be until I need to use 4 digits...
</p>

Once I did that, all that was left was simply writing a for loop like so:

{% raw %}
`<div class="img-center">` <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`{% directory path: img/bitty reverse: true %}` <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`<img src="{{ file.url }}"` <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`alt="{{ file.name }}"` <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`datetime="{{ file.date | date_to_xmlschema }}"/>` <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`<p>{{ file.slug }}</p>` <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`{% enddirectory %}` <br>
`</div>` <br>
{% endraw %}

With the `img-center` class looking like so:

`.img-center {` <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`text-align: center;` <br>
`}` <br>
`.img-center img {` <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`max-width: 100%;` <br>
`}` <br>
`.img-center p {` <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`font-size: 85%;` <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`margin-top: 5px;` <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`font-style: italic;` <br>
`}`

And then it worked like magic!
