"# alphanumeric directive" 


The goal of this directive is to discard non alphanumeric keystrokes when users are typing and to remove non-alphanumeric characters when users paste or drag/drop text in an input box. So the directive needs to handle 3 events: keydown, paste and drop.

First, let’s take a look at the keyboard event listener. To handle keyboard events, we need to be cautious that keycodes are different in Mac and Windows. Also we need to consider that number keys have different keycodes in the main keyboard and the number pad. The code snippet below is adopted from StackOverflow answers with some improvements.
  
All infrastructure has been established and we use a selector alphanumeric to bind this directive to the host element. The code snippet below shows an example usage.
  
<input type="text" alphanumeric  maxlength="3">

Simple enough and we meet customers’ needs. Of course, server side validation is always a MUST.
