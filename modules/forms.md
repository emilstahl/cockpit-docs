### Forms

Forms have been always a pain. Especially handling the data after form submission.
We're happy to say that now Cockpit will do the dirty job for you. Cockpit collects the data + transfers it via email. We choose not to build a form builder, because we think you know HTML very well and we don't want to step in your code flow.

#### Usage

This is a really basic contact form with a name, email and message field.

    <form name="contact">
      <p>
        <label for="name">Name</label>
        <input id="name" name="name"/>
      </p>
      <p>
        <label for="email">Email</label>
        <input id="email" name="email"/>
      </p>
      <p>
        <label for="message">Message</label>
        <textarea id="message" name="message"></textarea>
      </p>
      <p>
        <button type="submit">Send</button>
      </p>
    </form>


To make it work with Cockpit, create a form in the Cockpit backend called **contact**. Then simply replace the &lt;form&gt; tag with the form function and **wrap all fields we want to collect with form[...]**. We also added a success message div to be shown once the form has been successfully submitted:


    <?php cockpit("forms:form", "contact");?>
      <div class="form-message-success">
        Thank You! I'll get back to you real soon...
      </div>

      <p>
        <label for="name">Name</label>
        <input id="name" name="form[name]"/>
      </p>
      <p>
        <label for="email">Email</label>
        <input id="email" name="form[email]"/>
      </p>
      <p>
        <label for="message">Message</label>
        <textarea id="message" name="form[message]"></textarea>
      </p>
      <p>
        <button type="submit">Send</button>
      </p>
    </form>


**Done!** Happy collecting form data.

---

### Custom form validation

Cockpit provides a simple way to hook into form submission:

Create a folder named __forms__ in the __/custom__ folder. Now let's say you want custom validation for your form called __contact__.
Create a file named __contact.php__ in the __forms__ folder.

**Example:**

```
<?php

// check site_url field (form[site_url]) for a valid url pattern
if (!filter_var($app->param('form/site_url'), FILTER_VALIDATE_URL)) {

    return false;
}
```
As you can see, whenever you return false you're cancelling form submission.

---

### Custom email templates

To customize the email layout send by the form module, first create folder named __emails__ in the __/custom/forms__ folder.
Now let's say you want to create an email template for your form called __inquiry__ with the fields ```form[product]``` and ```form[quantity]```. Create a file named __inquiry.php__ in the __emails__ folder: ```/custom/forms/emails/inquiry.php```.

**Example:**

```
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<body>
    Order for product: {{$product}}, {{$quantity}} pieces.
</body>
</html>
```

<div class="uk-alert">
    Use field names as variables. Use the Lexy syntax or plain PHP.
</div>

---

## Module API


##### get_form( $name )

Get form meta information array.

```
$form_meta = cockpit('forms:get_form', 'formname');
```

---

##### form( $name, $options = [] )

Init form in your front-end.

```
<?php cockpit('forms:form', 'formname') ?>

  ...
</form>
```

Options:

```
<?php cockpit('forms:form', 'formname', ['id' => 'myformId', 'class'=>'anycssclass']) ?>

```

---


##### entries( $name )

Get entries collection.

```
$entries = cockpit('forms:entries', 'formname');

// get amount of entries
$count = $entries->count();

// find an entry
$entry = $entries->findOne(['email' => 'name@anydomain.com']);

// find and sort many entries
$entry = $entries->find(['country' => 'Germany'])->sort(["created"=> -1])->toArray();

```
