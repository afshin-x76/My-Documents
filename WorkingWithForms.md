# Working With Forms

In HTML, a form is a collection of elements inside `<form>...</form>` that allow a visitor to do things like enter text, select options, manipulate objects or controls, and so on, and then send that information back to the server.

a form must specify two things:
- **where**: the URL to which the data corresponding to the user’s input should be returned
- **how**: the HTTP method the data should be returned by

### GET and POST

GET and POST are the only HTTP methods to use when dealing with forms.

## Django’s role in forms

Django handles three distinct parts of the work involved in forms:

- preparing and restructuring data to make it ready for rendering
- creating HTML forms for the data
- receiving and processing submitted forms and data from the client

### The Django Form class

A form field is represented to a user in the browser as an HTML “widget” - a piece of user interface machinery. Each field type has an appropriate default Widget class, but these can be overridden as required.

### Instantiating, processing, and rendering forms

When rendering an object in Django, we generally:

1. get hold of it in the view (fetch it from the database, for example)
2. pass it to the template context
3. expand it to HTML markup using template variables

when we handle a model instance in a view, we typically retrieve it from the database. When we’re dealing with a form we typically instantiate it in the view.

When we instantiate a form, we can opt to leave it empty or pre-populate it, for example with:

- data from a saved model instance (as in the case of admin forms for editing)
- data that we have collated from other sources
- data received from a previous HTML form submission

## [Building a form](https://docs.djangoproject.com/en/3.2/topics/forms/#building-a-form)

### [Bound and unbound form instances](https://docs.djangoproject.com/en/3.2/topics/forms/#bound-and-unbound-form-instances)

- An unbound form has no data associated with it. When rendered to the user, it will be empty or will contain default values.
- A bound form has submitted data, and hence can be used to tell if that data is valid. If an invalid bound form is rendered, it can include inline error messages telling the user what data to correct.

The form’s is_bound attribute will tell you whether a form has data bound to it or not.


### [More on fields](https://docs.djangoproject.com/en/3.2/topics/forms/#more-on-fields)

**Widgets**

Each form field has a corresponding Widget class, which in turn corresponds to an HTML form widget such as `<input type="text">`.

**Field data**

Whatever the data submitted with a form, once it has been successfully validated by calling is_valid() (and is_valid() has returned True), the validated form data will be in the form.cleaned_data dictionary. This data will have been nicely converted into Python types for you.

Some field types need some extra handling. For example, files that are uploaded using a form need to be handled differently (they can be retrieved from request.FILES, rather than request.POST). For details of how to handle file uploads with your form, see Binding uploaded files to a form.

## [Working with form templates](https://docs.djangoproject.com/en/3.2/topics/forms/#working-with-form-templates)

### Form rendering options

- {{ form.as_table }} will render them as table cells wrapped in `<tr>` tags
- {{ form.as_p }} will render them wrapped in `<p>` tags
- {{ form.as_ul }} will render them wrapped in `<li>` tags

### Rendering fields manually

- {{ field.label }}
- {{ field.label_tag }}
- {{ field.id_for_label }}
- {{ field.value }}
- {{ field.html_name }}
- {{ field.help_text }}
- {{ field.errors }}
- {{ field.is_hidden }}
- {{ field.field }}

**Looping over hidden and visible fields**

Django provides two methods on a form that allow you to loop over the hidden and visible fields independently: hidden_fields() and visible_fields().

##### Reusable form templates

```
# In your form template:
{% include "form_snippet.html" %}

# In form_snippet.html:
{% for field in form %}
    <div class="fieldWrapper">
        {{ field.errors }}
        {{ field.label_tag }} {{ field }}
    </div>
{% endfor %}
```

If the form object passed to a template has a different name within the context, you can alias it using the with argument of the include tag:

```
{% include "form_snippet.html" with form=comment_form %}
```


# [Creating forms from models](https://docs.djangoproject.com/en/3.2/topics/forms/modelforms/#creating-forms-from-models)


#### Field types

Each model field has a corresponding default form field. For example, a CharField on a model is represented as a CharField on a form. A model ManyToManyField is represented as a MultipleChoiceField. Here is the full list of conversions:

[Click Here to see the list](https://docs.djangoproject.com/en/3.2/topics/forms/modelforms/#field-types)

**Notes:**

- ForeignKey is represented by django.forms.ModelChoiceField, which is a ChoiceField whose choices are a model QuerySet.
- ManyToManyField is represented by django.forms.ModelMultipleChoiceField, which is a MultipleChoiceField whose choices are a model QuerySet.

In addition, each generated form field has attributes set as follows:

- If the model field has blank=True, then required is set to False on the form field. Otherwise, required=True.
- The form field’s label is set to the verbose_name of the model field, with the first character capitalized.
- The form field’s help_text is set to the help_text of the model field.

#### [The save() method](https://docs.djangoproject.com/en/3.2/topics/forms/modelforms/#the-save-method)

very ModelForm also has a save() method. This method creates and saves a database object from the data bound to the form. A subclass of ModelForm can accept an existing model instance as the keyword argument instance; if this is supplied, save() will update that instance. If it’s not supplied, save() will create a new instance of the specified model.












