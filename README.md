"# alphanumeric directive" 


The goal of this directive is to discard non alphanumeric keystrokes when users are typing and to remove non-alphanumeric characters when users paste or drag/drop text in an input box. So the directive needs to handle 3 events: keydown, paste and drop.

First, let’s take a look at the keyboard event listener. To handle keyboard events, we need to be cautious that keycodes are different in Mac and Windows. Also we need to consider that number keys have different keycodes in the main keyboard and the number pad. The code snippet below is adopted from StackOverflow answers with some improvements.

private alphaNumerics="0123456789abcdefghijklmnopqrstuvwzyz ";

 @HostListener('keydown', ['$event'])
  onKeyDown(e: KeyboardEvent) {
    if (
      this.navigationKeys.indexOf(e.key) > -1 || // Allow: navigation keys: backspace, delete, arrows etc.
      (e.key === 'a' && e.ctrlKey === true) || // Allow: Ctrl+A
      (e.key === 'c' && e.ctrlKey === true) || // Allow: Ctrl+C
      (e.key === 'v' && e.ctrlKey === true) || // Allow: Ctrl+V
      (e.key === 'x' && e.ctrlKey === true) || // Allow: Ctrl+X
      (e.key === 'a' && e.metaKey === true) || // Allow: Cmd+A (Mac)
      (e.key === 'c' && e.metaKey === true) || // Allow: Cmd+C (Mac)
      (e.key === 'v' && e.metaKey === true) || // Allow: Cmd+V (Mac)
      (e.key === 'x' && e.metaKey === true)    // Allow: Ctrl+X (Mac)
    ) {
      // let it happen, don't do anything
      return;
    }
    // Ensure that it is a alphaNumeric and stop the keypress
    if ( this.IsNotalphaNumeric(e.key)) {
      e.preventDefault();
    }
  }

  IsNotalphaNumeric(key){
    return !(this.alphaNumerics.includes(key) || this.alphaNumerics.toUpperCase().includes(key)) ;
  }


Then we add the other two event handlers. The two methods below intercept ClipboardEvent and DragEvent to modify the text, so that alphanumeric-only strings are inserted into the host element.

  @HostListener('keyup', ['$event'])
  onKeyUp(e: KeyboardEvent) {
    if (!this.alphaNumeric) {
      return;
    }
  }

  @HostListener('paste', ['$event'])
  onPaste(event: ClipboardEvent) {
    const pastedInput: string = event.clipboardData.getData('text/plain');
    this.pasteData(pastedInput);
    event.preventDefault();
  }

  @HostListener('drop', ['$event'])
  onDrop(event: DragEvent) {
    const textData = event.dataTransfer.getData('text');
    this.inputElement.focus();
    this.pasteData(textData);
    event.preventDefault();
  }
  
  Very good. All infrastructure has been established and we use a selector alphanumeric to bind this directive to the host element. The code snippet below shows an example usage.
  
<input type="text" alphanumeric  maxlength="3">

Simple enough and we meet customers’ needs. Of course, server side validation is always a MUST.
