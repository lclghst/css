# C2 CSS Style Guide

![CSS](http://i.imgur.com/Q3cUg29.gif)

### Table of contents

1. [Why](#why)
2. [How](#how)
3. [Example](#example)
4. [Example 2](#example-2)
5. [Modifiers](#modifiers)
6. [Benefits](#benefits)
7. [Javascript](#javascript)
8. [Animation](#animation)
9. [Why BEM?](#why-bem)
10. [tl;dr](#tldr)
11. [Future](#future)
12. [Further reading](#further-reading)

## Why

![just no](http://i.imgur.com/6bwGUFH.jpg)

We need to change our approach to CSS. 

This isn't a ruleset that you must follow 100% and salute before you go to bed but rather a guide to how we can make our CSS:

* Clearer code
* Easier maintenance
* Write less code
* Separation of markup and style
* Faster development
* Faster loading pages

## How

Using a method called [BEM].

BEM stands for Block Element Modifier and it is more simple than it sounds.

## Example

I will pick on myself here with some examples of CSS I have written for previous projects.

In [Mackillop](https://www.mackillop.org.au/) we have a simple sub-pages section.

![Mackillop sub-pages example](https://github.com/lclghst/css/blob/master/img/example1.png)

Quite simple when we break it down:

A subpage consists of image on left, description on right.

Here is the css selector for the description `body.landing-page div.sub-pages>div.row` `div.description` AND `body.landing-page div.left div.description`.

The HTML structure looks like

```html
<div class="subpages">
	<div class="row">
		<div class="thumb"><a><img /></a></div>
		<div class="description">
			<h3/>
			<p/>
			<a class="read-more" />
		</div>
	</div>
</div>
```

And our css selectors look like

```css
body.landing-page div.sub-pages{}
body.landing-page div.sub-pages > div.row{}
body.landing-page div.sub-pages > div.row h3{}
body.landing-page div.sub-pages > div.row h3:hover{}
body.landing-page div.sub-pages > div.row p{}
body.landing-page div.sub-pages > div.row div.description{}
.read-more{}
.read-more:hover{}
body.landing-page div.sub-pages .thumb{}
body.landing-page div.left div.description p{}
body.landing-page div.left div.description{}
body.landing-page .thumb{}
```

So how can we apply our [BEM] guidelines to this?

1. We do not want any references to HTML in our css for specificity reasons (I'll go into more detail later)

   eg. ~~body~~`.landing-page `~~div~~`.sub-pages>`~~div~~`.row `~~div~~`.description`

2. The direct child selector `>` is probably overkill in 99% of cases. Why not just omit it here, along with `div.row`?

  This way we are keeping our CSS separate from Bootstrap.

   eg. ~~body~~`.landing-page `~~div~~`.sub-pages`~~> div.row div~~`.description`

3. What if we want this subpage module on a different template page? At the moment it only works on the landing page.

   eg. ~~body .landing-page div~~`.sub-pages`~~> div.row div~~`.description`

4. Taking this one step further, we could put the description into its own class.

   eg. `.sub-pages__description`

This technique is [BEM].

Let's apply it to our sub-pages module, so our HTML looks more like

```html
<div>
	<div class=”subpage”>
		<div class=”subpage__thumb”><a><img /></a></div>
		<div>
			<h3 class=”subpage__title”/>
			<p class=”subpage__description”/>
			<a class=”read-more” />
		</div>
	</div>
</div>
```

And our CSS selectors look like

```css
.subpage{}
  .subpage__thumb{}
  .subpage__description{}
  .subpage__title{}
    .subpage__title:hover{}
.read-more{}
```

Wow, that's a lot less code already. Now if someone was to go back and make any style updates, it is a lot easier for them to understand exactly what will be affected by their changes.

Also we can now safely put the subpages module on any other page we like, without having to rewrite our code.

To further this example, now that we have uncoupled the style from the markup, we can even change how the modules HTML is structured and not worry about having to change our selectors.

   eg. We have to change the subpages into list elements

```html
<ul>
	<li class=”subpage”>
		<div class=”subpage__thumb”><a><img /></a></div>
		<div>
			<h3 class=”subpage__title”/>
			<p class=”subpage__description”/>
			<a class=”read-more” />
		</div>
	</li>
</ul>
```

## Example 2

Another example from [Mackillop](https://www.mackillop.org.au/), this time the menu.

```html
<div class="custom-nav">
    <div class="row">
        <div class="col-xs-12">
            <div class="menu-bar">
                <ul class="custom-nav">
                    <li class="selected"><a>Home</a></li>
                    <li><a>MacKillop</a></li>
                    <li><a>Our Services</a></li>
                    <li><a>How You Can Help</a></li>
                    <li><a>News and Media</a></li>
                    <li><a>Careers</a></li>
                    <li><a>Contact Us</a></li>
                    <li class="search">
                        <input type="text" placeholder="Search">
                        <img class="close" src="/assets/images/close-icon.png" style="display: none;">
                        <div class="autocomplete" style="display: none;"></div>
                    </li>
                </ul>
            </div>        
        </div>
    </div>
</div>
```

```css
div.custom-nav
    div.custom-nav div.row
    div.custom-nav div.row > div
    div.custom-nav div.row div.menu-bar
div.custom-nav ul.custom-nav
    div.custom-nav ul.custom-nav > li
        div.custom-nav ul.custom-nav > li > a
            div.custom-nav ul.custom-nav > li.selected > a,
            div.custom-nav ul.custom-nav > li:hover > a
            div.custom-nav ul.custom-nav > li:first-child > a
    div.custom-nav ul.custom-nav > li.search
        div.custom-nav ul.custom-nav > li.search input
        div.custom-nav ul.custom-nav > li.search img
        div.custom-nav ul.custom-nav > li.search input::-webkit-input-placeholder
        div.custom-nav ul.custom-nav > li.search input:-moz-placeholder
        div.custom-nav ul.custom-nav > li.search input::-moz-placeholder
        div.custom-nav ul.custom-nav > li.search input:-ms-input-placeholder
        div.custom-nav ul.custom-nav > li.search input:focus
div.custom-nav ul.custom-nav div.autocomplete
    div.custom-nav ul.custom-nav div.autocomplete span
    div.custom-nav ul.custom-nav div.autocomplete span:hover
```

Then something like this... Remember [BEM] isn't a strict ruleset, just a loose guide to writing easier CSS.

You can't really go wrong with trying it; it will still be better than what we are doing now.

```html
<div class="menu-wrap__outer">
    <div class="row menu-wrap">
        <div class="col-xs-12 menu-wrap__inner">
            <div class="menu__bar">
                <ul class="menu">
                    <li class="menu__item menu__item--selected"><a class="menu__link">Home</a></li>
                    <li class="menu__item"><a class="menu__link">MacKillop</a></li>
                    <li class="menu__item"><a class="menu__link">Our Services</a></li>
                    <li class="menu__item"><a class="menu__link">How You Can Help</a></li>
                    <li class="menu__item"><a class="menu__link">News and Media</a></li>
                    <li class="menu__item"><a class="menu__link">Careers</a></li>
                    <li class="menu__item"><a class="menu__link">Contact Us</a></li>
                    <li class="search">
                        <input class="search__input" type="text" placeholder="Search">
                        <img class="search__close" src="/assets/images/close-icon.png" style="display: none;">
                        <div class="search__autocomplete" style="display: none;"></div>
                    </li>
                </ul>
            </div>        
        </div>
    </div>
</div>
```

```css
.menu-wrap__outer
    .menu-wrap
        .menu-wrap__inner
.menu__bar
.menu
    .menu__item
        .menu__link
            .menu__item--selected .menu__link,
            .menu__item:hover .menu__link
            .menu__item:first-child .menu__link
.search
    .search__input
    .search__close
        .search__input::-webkit-input-placeholder
        .search__input:-moz-placeholder
        .search__input::-moz-placeholder
        .search__input:-ms-input-placeholder
        .search__input:focus
.search__autocomplete
    .search__autocomplete span
        .search__autocomplete span:hover
```

Like I said before, not a 100% perfect silver bullet but you get the gist.

## Modifiers

![yes i am dog](http://i.imgur.com/SOIqChP.jpg?1)

Ok that's fine for basic classes but what about warnings, success, hover, active states etc?

This is where the M of [BEM] comes into play.

Here we have a simple nav menu

```html
<nav>
  <ul class="menu">
    <li><a class="active">Home</a></li>
    <li><a>About</a></li>
    <li><a>Contact</a></li>
  </ul>
</nav>
```



```css
ul.menu {}
ul.menu > li {}
ul.menu > li > a {}
ul.menu > li > a.active {}
```

Now let's wave our magic [BEM] wand over the code to get

```html
<nav>
  <ul class="menu">
    <li class="menu__item"><a class="menu__link active">Home</a></li>
    <li class="menu__item"><a class="menu__link">About</a></li>
    <li class="menu__item"><a class="menu__link">Contact</a></li>
  </ul>
</nav>
```

```css
.menu {}
  .menu__item {}
  .menu__link {}
    .menu__link.active {}
```

Hmmm... that `.menu__link.active` doesn't look right. What happens if we want to apply an active state to a button or input? We either specify a long `.form button.submit.active` sort of selector, or we might run the risk of inheriting an unwanted active state from another rule.

```css
.menu {}
  .menu__item {}
  .menu__link {}
    .menu__link--active {}
```

Instead, let's create a Modifier class called `.menu__link--active`. This way we can apply it to our menu links and know it wont get inherited by another active state on some other element.

## Benefits

![having a good day](http://i.imgur.com/9RPDCTC.gif)

From our example above, our CSS has become:

##### Modular

   We can put the subpage module anywhere else in the website and not have to write any more CSS for it.

##### Flexible

   If we have to make changes to the HTML structure, we can just add whatever we need to, without changing the selector or class name.

##### Faster

   Believe it or not, our CSS and web pages will load faster now. Not only because we have reduced the length of the class names but because we have reduced how specific our selectors are.

   CSS is a funny beast and it actually reads a CSS selector from right to left. Crazy huh?

   Take for example `body.landing-page div.sub-pages > div.row div.description{}`
   First it looks for all elements with the `.description` class, then only the `<div>` with `.description`, then only the `<div>` with `.description` that are inside a `.row` parent, then only the `<div>` with `.description` that are inside a `.row` parent that is a `<div>` etc etc

   So you can see it is a lot more efficient (and quicker) for our browsers to only have to match the single class.

##### Self documenting

   Almost. It now takes less time for our poor stressed front-end developers to look at our BEM-ified CSS and understand what goes where and what effect changing such and such class will do throughout the site.

## Javascript

![Chris at work](http://i.imgur.com/rZDKwoj.gif)

Now we have uncoupled our CSS from our HTML, it is time to do the same with our Javascript.

There is no problem binding a Javascript event to a css class, it only becomes a problem when we use a class that is being used for style.

The problem being that at the moment you can accidentally break our Javascript by changing a class name, or HTML element.

What we can do instead is create a new class name that has no CSS styles associated with it, preferably prefixed with `js` so we can be even more clear in what the class is for.

For example

```html
<button class=”subscribe__submit btn”>Submit</button>
```

and our js
```javascript
$('.subscribe_submit').click(function ()
{
	$(this).parent().find('ul').slideToggle('fast', function () { return false; });
});
```

We apply our new `js` only class `js-toggleParent`.

```html
<button class=”subscribe__submit btn js-toggleParent”>Submit</button>
```

Update our Javascript code

```javascript
$('.js-toggleParent').click(function ()
{
	$(this).parent().find('ul').slideToggle('fast', function () { return false; });
});
```

Now we can change styles with ease. Also it is quite clear that there is a Javascript event attached to this element and the name tells us what it does.

Yes it is long, yes it is ugly but it makes it a whole lot easier for someone to look at your code and know what is going on and for someone else to make any changes without breaking the styles or scripts.

## Animation

![amirite](http://i.imgur.com/At6RaHk.png)

A few performance gains we can easily make.

Try not to use jQuery `$.animate()`. 

Instead of moving elements with `Top` `Right` `Bottom` `Left` use `translate` and `transform`.
This applies also to using `position: absolute`. Positioning with `transform` avoids choppy FPS and poor animation performance.

## Why BEM?

![more memes](http://i0.wp.com/www.developermemes.com/wp-content/uploads/2013/07/Web-Developer-What-I-Actually-Do-Meme.jpg)

There are many different approaches to writing CSS. 

For example

* [OOCSS](https://github.com/stubbornella/oocss/wiki) (Object Oriented CSS)
* [BEM] You should know this one by now 😉
* [SMACSS](https://smacss.com/) (Scalable and Modular Architecture for CSS)
* [ACSS](http://acss.io/) (Atomic CSS)
* [SUIT CSS](https://suitcss.github.io/)

There is no silver-bullet for writing **the best** CSS.

Some of the above examples are more for huge enterprise corporations, or they rely on different tools/workflow.

For me, BEM 'just werks'. It's a tiny bit of effort for a lot of gain.

## tl;dr

![didn't read lol](http://i.imgur.com/Na81R.gif)

This guide is to help us all write better CSS. But it is just that - a guide.

By putting a little more effort into our class names and separating our CSS from our HTML and Javascript, we can make developing and maintaining our front-end code a lot nicer.

The essence of [BEM]: Block__Element--Modifier

If you don't like double__underscores or double--hyphens then use something else. 

As long as the CSS you are writing is:

* Uncoupled from the HTML. (no `div.class`, `ul.something`.)
* Uncoupled from the Javascript. (no `$("div.logo").click({});` Bind to a `js` prefixed class.)
* Low specificity. (Flat css classes. Keep cascading to an absolute minimum. no `body.class .menu .item .link >  span`.) 

## Future

![meme](https://memeexplorer.com/cache/846.jpg)

These things are being used in the real world right now. None of these things are new or should even be considered 'cutting edge'.

Some things that I think could be worth looking in to, eventually

##### Preprocessors

* [SASS](http://sass-lang.com/)
* [LESS](http://lesscss.org/)
* [Stylus](https://learnboost.github.io/stylus/)
* [PostCSS](https://github.com/postcss/postcss)

##### Build tools

These sorts of tools work in Visual Studio as well!

* [nodejs](https://nodejs.org/) & [NPM](https://www.npmjs.com/)
* [gulp](http://gulpjs.com/)
* [Grunt](http://gruntjs.com/)

## Further reading

For more BEM information:

* https://en.bem.info/
* http://getbem.com/
* http://www.smashingmagazine.com/2012/04/a-new-front-end-methodology-bem/
* https://css-tricks.com/bem-101/
* http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/

[BEM]: https://en.bem.info/
