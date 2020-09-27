+++
title = "Email `IE` grandpa of messaging"
date = "2020-09-25T05:24:51Z"
tags = ["email"]
draft = false
author = "admin"
emoji= true
+++

We all have been getting email, sending email, some simple ones, some fancy html ones... Recently I tried to create responsive email for work and here's my journey of creating an email.

First I tried to use [MJML](https://mjml.io/) tool, I couldn't change email's main content width... mjml takes 600px width by default and our design has 800px, also had to change css styles, didn't dive much into docs and example, i tried being hackies and after failing, I thought, whatever I know html/css3 pretty well, I will do it without using any tool, the pride of me awaken ;)

Well nothing big, converted design to html template and send to my email, the gmail... ohooo finished, perfect... 

##### - Rise of inline css

oh no, gmail doesnt support class, dammit, I will take back that perfect :stuck_out_tongue_winking_eye: 

`Inspect element` -> gmail adds extra character to class so style was not working

looking around google searches and stackoverflow, I figure out we need to add inline styles that element... so I used [https://htmlemail.io/inline/](https://htmlemail.io/inline/) for adding styles to that element according the class (generated inline css).

##### - Rise of fixed width

Started looking good in desktop, guessed its responsive so should be good in mobile view too, but noo, email does not want to be responsive, 

Checked different sites for resources to make it responsive, all suggested to use media query, but media-query does not work in inline css :( neither its working inside `<style><style>` coz class is being mummified with extra random characters added (at least in Gmail).

thought this could be solution
```
[class^="view-online"] {
    ....
}

[id*="block"] {

}
```

it didnt work as expected so, final solution was to give fixed width div and make it `display: inline-block` like:
```
<div styles:"width:800px">
    <div styles:"max-width:250px; width:100%; display:inline-block;">
    <div styles:"max-width:550px; ; width:100%; display:inline-block;">
</div>
```
now it does act responsive in mobile :D

Next, 
#### A block with headline over image background

well `float` doesn't work in email neither do `position: absolute`... complex huh, gradient css or svg does not load oops

pufff, Lots of `divs tables` inside `div's`


#### Guess who's back - `<Table>` - back again 

![alt "guess whose back"](https://media2.giphy.com/media/e5nosGeL5NvgY/giphy.gif)

Gmail started looking good but people uses variety of email clients, yahoo, aol, outlook, its Ok in web browser but desktop app, it was worst... 

All `div's` had to be converted to `table - td`, 

Desktop app doesn't load image by default and background-image in css was out of concern so `bgColor` of table with base color for fall back

#### Fonts

Well I must say, I can only distinguish between serif and san-serif but not all fonts with san-serif attributes so I usually get confused if fonts changed were working or not :stuck_out_tongue_winking_eye:

I used `<link>` to add open-sans which seems didn't work, so added

```
font-family: Helvetica, Arial, sans-serif;
```

Normally font family added in parent should propagate to its child elements, well this is `email` and it doesn't happen.


- fonts don't get propagate

To fix this, I added 'font' class to each element with text and let inline css be generated.

Finally all started working well, unless iPhone came to rescue with font issue (satirical) , it was showing Arial font for some text and Helvetica for some

The fix: Text must be inside `<p>` tag 

Now, most of the issue's are solved, email sent successfully, outlook desktop application show Okish email, and am off for happy weekend, yay

if something I should have tried or missed, do comment, Thank you for reading