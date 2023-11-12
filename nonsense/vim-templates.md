# Vim Templates

### Templates and Skeletons

Vim allows us to use skeleton files (templates) to scaffold the
creation of new files. That is, whenever a new file is created the
content of the template is read into the Vim buffer. For example, for
a **javascript** file it can be a an empty
**{choose your favorite pattern}**
pattern file, for a **Rails** controller can be an empty
controller class, for a **Bash** script can be a file
starting with the #! shebang line. The idea is that for a specific
file extension like **.rb**, **.js** or
**.sh**, Vim can populate a new file with the contents
of a template.

### An example skeleton file

Let's create a sample javascript function template, and assume for now
that we are psychopaths that store each javascript function in their
own isolated file, the template:


```language-vim
function Foo() {
  return true;
}
```

We create and save this template in
**~/.vim/templates/function.js**
You can save your templates wherever you want, but that seems to be a
good place for them. The name is irrelevant, but if you plan to have a
lot of templates, better name them with something accordingly to their
content.

### Populating new files

To use this skeleton template when creating a new javascript file we
need to add the following line to our **vimrc** file:

```vim
autocmd BufNewFile *.js 0read ~/.vim/templates/function.js
```

Which translates to: When a new file with extension
**.js** is created, insert the content of
**~/.vim/templates/function.js** at the beginning of the
buffer.

What if I don't want to autoload the template in all my new javascript
files, just the ones suffixed with **_function.js**, not
a problem, just change the match pattern on the
**autocmd**:

```vim
autocmd BufNewFile *_function.js 0read ~/.vim/templates/function.js
```

This is excellent! Yay, now everytime I create a new .js file the
template will be loaded into the buffer, wait... every time? This will
get tedious very soon. Can we load templates at demand? I'm glad you
asked, yes, yes we can.

### Inserting templates at demand

As you probably already realized, we can use the
**read** command to insert the content of any file into
the buffer:

```vim
# will insert the content of the template below the current line
:read ~/.vim/templates/function.js

# Will insert the content of the template at the start of the file
:0read ~/.vim/templates/function.js
```

Now I have to type the whole path? I hear you, why spend 5 seconds
typing the full path to the file if we can add functions and map
keys-combinations to them:

```vim
function! LoadTemplate(template) abort
  execute "read ~/.vim/templates/". a:template
endfunction

nnoremap <C-t>f :call LoadTemplate("function.js")<CR>;
```

And now you can use **Ctrl+t f** to load the skeleton
and map tons of key combinations with every letter on your keyboard to
different templates and memorize all of them like an
**emacs** user, or call the function with

**:call LoadTemplate("function.js")**

to insert your template bellow the current line; which seems to be
longer than our initial approach, let's measure:

**read ~/.vim/templates/function.js** => **33** keyboard strokes   
**call LoadTemplate("function.js")** => **32** keyboard strokes

renaming function to LT... 

**call LT("function.js")** => **22** keyboard strokes, Flawless Victory!

### Having fun

Templates are fun, you can change **BufNewFile** with
**BufWritePre**, create a signature template and append
it to the file on save! Now, that is an evil plan...

```vim
autocmd BufWritePre * $read ~/.vim/templates/signature
```

Until the next one.
