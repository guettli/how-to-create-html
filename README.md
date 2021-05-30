# How to create HTML with Python

There are several hundert ways how you could create HTML with the
programming language Python.

## Libraries

* https://github.com/Knio/dominate
* https://github.com/basxsoftwareassociation/htmlgenerator
* [format_html()](https://docs.djangoproject.com/en/dev/ref/utils/#django.utils.html.format_html)
* Django Templates
* Jinja Templates

## Thoughts

Somehow I have the feeling, that I have not found the most simple way up to now.

My context: The cursor is blinking inside a Python file in my favorite IDE.

I want to create a string containing HTML.

Sending the string over the network or writing it to a file is not part of the
current question.

I like [f-strings](https://docs.python.org/3/tutorial/inputoutput.html#formatted-string-literals):

```
name = 'Peter'
html = f'<div>Hello {name}!</div>'
```

BUT: Above code breaks if "name" contains unsafe characters which need to be escaped like `&` and `<`.

I like [format_html()](https://docs.djangoproject.com/en/3.1/ref/utils/#django.utils.html.format_html) of Django:

```
name = 'Peter'
html = format_html('<div>Hello {name}!</div>', name=name)
```

The great thing about it: If `name = 'Peter & Mary'` then the `&` would get escaped correctly.

And if I use this:
```
name = format_html('<b>{}</b>', 'Peter & Mary')
html = format_html('<div>Hello {name}!</div>', name=name)
```

the real magic happens: The `&` gets escaped, but the `<b>` stays a html tag.

So what is so cool about f-strings which I don't get with `format_html()`?

- f-strings can access attributes. This works: f'... {my_object.my_attribute} ...'
- f-strings automatically have the local and global variables. With `format_html()` I would need to do `format_html('... {my_local} ', locals())`

Since up to now I use Django, I could use a Django Template:

```
from django.template import Template, Context
template = Template('Hello {{name}}!')
context = Context(dict(name='Peter & Mary'))
html = template.render(context)
```

Or shorter
```
from django.template import Template, Context
html = Template('Hello {{name}}!').render(Context(dict(name='Peter & Mary')))
```

The vision:

```
name = 'Peter & Mary`
html = h'Hello {name}!'
```

This would be great: If I could use `h'` and get all the f-string magic (access to local variables and `obj.attr`) and conditional_escape support.

