# HTML

- Tags
- Forms

---
## Tags
---
- acronym
- ins/del
- abbr
- wbr
- base
- small

### acronym

Will show a box with informations when hovering over the text that has the **acronyme** tag:  
`<p>The microblogging site <acronym title="Founded in 2006"> Twitter</acronym> has recently struggled with downtimes.</p>`

### ins/del

Will strike through a word with **del**:  
`<del>likes</del> <ins>LOVES</ins> his new iPod.`

### abbr

Used to define abbreviated words:  
`<abbr title="Sergeant">Sgt.</abbr> Pepper's Lonely Hearts Club is my favorite album.`

### wbr

Will let the browser break the word if needed:  
`<span>How do you say Supercalifragilistic<wbr>expialidocious?</span>`

### base

Will define the base URL/target for all relative URLs in a document. There can be only one **base** tag in a document and it must be placed in the head tag:

```html
<head>
  <base href="https://www.w3schools.com/images/" target="_blank" />
</head>
```

### small 

Will display some phrasing content one font-size smaller than its parent.  

```html
<p>
  <small>Some text</small>
</p>
```

---
## Form
---

Will send the values of the inputs present in the form: input, textarea, radio, checkbox, select...

- form
- input

### form
```html
<form method="[url]" action="post | get">
  <label for="email">Email</label>
  <input type="email" name="email" id="email" />
</form>
```

#### method
The url to which the form must send the values.

#### post | get
The action to perform when submitting the form.

#### novalidate 
Add it to the **form** tag to disable validation by the browser.

#### formnovalidate 
Add it to a **button** or **input** tag to disable validation by the browser.

#### enctype="multipart/form-data"
Will inform the browser that the data will be of multiple format(string, files...), not just string.

### input
`<input type="[type]" />`
`<input type="text"/>` // will expect string values  
`<input type="email"/>` // will expect some special characters  
`<input type="hidden"/>` // will hide the value  
`<input type="number"/>` // will accept decimal values  
`<input type="number" step="0.01"/>` // the step attribute will allow decimal numbers  

## Protocols

- file
- http
- https
- ftp
- ssl/ssh

### File protocol
When a file is opened on a computer