---
layout: post
title:  "Two CSS animations"
category: compsci
tags: web-dev, css
summary: "A short guide to simple CSS animations."
---

Recently I've been getting really into web development. I'm taking a [React course on Udemy][1] at the moment, and building a website for [Filmmakers Without Cameras][2] using it. I think it's pretty cool, and I'm enjoying getting more comfortable with Javascript. Something I didn't know about before I took this on though is that you could animate HTML using pure css. And it looks pretty cute too.

So in this little tutorial I'm going to go into how I made the [two effects you might have seen on my Twitter][5], because they are just *so simple* and full of uwu.

## Pop Out Shadow Effect

![Large title gif](/assets/id/{{ page.id }}/largetitle.gif)

This is what we're working towards. Ideally the title would already have a class, say `.title` that contains only the title. Mine is additionally wrapped inside a `<div>` tag with class `.diagonalBox` that manages the tilt of the text.

Note that my classes are in camel case, and not the standard dashed format used in standard html & css because this website is built with react and `diagonal-box` is not valid javascript syntax.

At any rate, my not-quite-html code for this heading looks like this:

```html
<div className = { styles.container } >
    <div className = { styles.diagonalBox }>
      <h1 className = { styles.title }>A very cute sample title</h1>
    </div>
  </div>
```

Or in standard html:

```html
<div class = "container">
      <div class = "diagonalBox">
        <h1 class = "title"> Filmmakers Without Cameras </h1>
      </div>
    </div>
```

Now we're set to inspect the actual magic css. The `.container` class isn't really relevant to the animation so I'll leave it out. The diagonal box is really simple:

```css
.diagonalBox {
  transform: skewY( $text-tilt-degrees );
}
```

The `.title` class then defines the font, colour, and that kind of thing. If you were to put the animation in this class, it would either

- happen once when you load the page, or
- happen over and over, if you set it as `infinite`.

Notice how this animation is triggered by my mouse hovering over the title though. Something I didn't realise is that all standard html elements are hoverable, just like links are.

```css
h1.title:hover{
  animation: popOut 1s ease-in-out;
}
```

Here `popOut` is what we called the animation, `1s` is the duration of the animation and `ease-in-out` is the animation timing function. It defines the speed curve of an animation.

[Read more about animation timing functions.][6]

Now we need to define what the animation actually does. CSS has an inbuilt rule called `keyframes` specifically for this.

```css
@keyframes popOut {
  0% {
    text-shadow:            5px 5px 15px #633e95;
  }
  50% {
    text-shadow:            30px 30px 15px #633e95;
  }
  100% {
    text-shadow:            5px 5px 15px #633e95;
  }
}
```

The first two numbers of the `text-shadow` property specify the extent of the shadow, the third number specifies the blur and the colour is the colour of the shadow.

And that's it, you have a little on-hover animation for a title.

## Neon Blink Link

![Neon effect gif](/assets/id/{{ page.id }}/neon2.gif)

For these, you simply need the anchor tag to have the class `.neon` (or whatever you want to call it), so it looks something like this:

```html
<a class = "neon" href = "/about"> About </a>
```

The css for this one is a bit longer though. There's two parts: first you define what the neon class does itself — how neon text should look, and then you specify the behaviour on hover.

The `blink` animation simply changes the opacity of the text in an interval. I've left this one as scss because it's easier to then see what's going on with the colours especially, but it can be easily converted into css by simply replacing the `$...` variables with their corresponding values.

```scss
@import url('https://fonts.googleapis.com/css2?family=Pacifico&display=swap');

$color-neon-blue:          #2695ff;
$color-neon-blue-50:       rgba(38, 149, 255, 0.5);

$font-family:             'Pacifico', 'Leckerli One', cursive;
$text-primary-color:       lighten( $color-neon-blue, 50% );
$shadow-color:             $color-neon-blue;
$shadow-color-50:          $color-neon-blue-50;

.neon {
  font-family:            'Pacifico';
  color:                  lighten( $color-neon-blue, 50% );
  text-shadow:
    .5px .5px 2px gray,
    0 0 5px $shadow-color,
    0 0 10px $shadow-color-50,
    0 0 15px $shadow-color,
    0 0 20px $shadow-color-50,
    0 0 40px $shadow-color,
    0 0 100px $shadow-color,
    0 0 200px $shadow-color,
    0 0 300px $shadow-color,
    0 0 500px $shadow-color;
}

a.neon:hover {
  text-decoration: none;
  color:    $text-primary-color;
  text-shadow:
    .5px .5px 2px gray,
    0 0 5px $shadow-color,
    0 0 10px $shadow-color,
    0 0 15px $shadow-color,
    0 0 20px $shadow-color,
    0 0 40px $shadow-color,
    0 0 100px $shadow-color,
    0 0 200px $shadow-color,
    0 0 300px $shadow-color,
    0 0 500px $shadow-color;
  animation: blink 5s;
}

@keyframes blink {
  20% {
    opacity: 1;
  }
  22% {
    opacity: 0.2;
  }
  23% {
    opacity: 1;
  }
  25% {
    opacity: 0.1;
  }
  26% {
    opacity: 1;
  }
}
```

## Want More?

If you are interested in web animations, [Josh Cormeau's blog][4] is the place to go. He recently wrote [this tutorial][3] on the effect he calls `Boop`(adorable), which is not pure css, but is delightful ✨

---

##### All links
- [Udemy - React course][1]
- [Twitter - Filmmakers Without Cameras][2]
- [Josh Cormeau -- Boop Tutorial][3]
- [Josh Cormeau's Blog][4]
- [Twitter - A video of these animatons in action][5]
- [W3Schools - Animation Timing Functions][6]

[1]: https://www.udemy.com/course/react-the-complete-guide-incl-redux/ "Udemy - React"
[2]: https://twitter.com/FWCmagazine "Twitter - Filmmakers Without Cameras"
[3]: https://www.joshwcomeau.com/react/boop/ "Josh Cormeau - Boop tutorial"
[4]: https://www.joshwcomeau.com/react/boop/ "Josh Cormeau's blog"
[5]: https://twitter.com/vvveracruz/status/1336406094097362949?s=20 "A little video of some animations I made"
[6]: https://www.w3schools.com/CSSref/css3_pr_animation-timing-function.asp "W3Schools - Animation timing functions"
