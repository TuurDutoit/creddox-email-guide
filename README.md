Creddox Email Templates Guide
===
Learn how to make your own email templates for [Creddox] objects.  
A guide by [PursuitPower].

## Table of contents
 * [Intro](#intro)
 * [Basics](#basics)
   * [Creating an email template](#creating-an-email-template)
   * [Base structure](#base-structure)
   * [RelatedToType](#relatedtotype)
   * [Styles](#styles)
 * [Merging fields](#merging-fields)
   * [apex:outputField](#apexoutputfield)
   * [Numbers](#numbers)
   * [Currencies](#currencies)
   * [Percents](#percents)
   * [Checkboxes](#checkboxes)
   * [Dates](#dates)
 * [Relational fields](#relational-fields)
 * [Related lists](#related-lists)
 * [Testing templates](#testing-templates)
 * [Resources](#resources)

 
## Intro
In this guide, you'll learn how to create email templates in Salesforce, specifically for Creddox objects. I assume you have at least a basic understanding of what email templates are; knowing how to make one will make it easier to follow along. You can read up on the subject in the [Salesforce docs](https://help.salesforce.com/htviewhelpdoc?id=admin_emailtemplates.htm).  
A basic understanding of HTML will also be very helpful.

Salesforce supports 4 types of email templates:

 * Plain text
 * HTML with letterhead
 * HTML without letterhead (custom HTML)
 * Visualforce
 
We usually write email templates in Visualforce, because it's more powerful than text or HTML. You can use whatever type you prefer, even for Creddox objects, but this guide will only cover Visualforce templates.
 

## Basics
First things first: let's make a new email template in Salesforce.

### Creating an email template
1. Open up Salesforce in your browser and navigate to the Setup area. There, type `email templates` in the search field and click on `Email Templates`. Select the right folder (or create a new one) and click on `New Template`.
2. Select `Visualforce` and click `Next`.
3. In the top part of the page, fill out some metadata: select `Available For Use` if you want users to see the template and give it a name.
4. In the bottom part, provide a subject for the email (you can change this in the next step), select a recipient type (for Creddox objects you'll probably want to select `User`), and optionally a relatedTo type; I'll explain what exactly this is later on.
5. Click `Next`.

### Base structure
> I you lose track somewhere along the way, you can check out the sample templates in the `samples` folder (links will be scattered around this guide), examples in the `examples` folder, or just look at some of the default Creddox templates in Salesforce.

#### Visualforce
Click `Edit Template` to enter the template editor.  
You should now see an editor with some default content. Let's take a closer look.

On the first line, you see the `<messaging:emailTemplate ...>` _tag_. Every Visualforce email template **must** start with such a tag, because it provides some crucial information in the following _attributes_:

 * `subject`: the subject line of the email. This can contain variables, just like the body, as explained later on.
 * `recipientType`: the type of person this email can be sent to. For Creddox objects you'll probably want to use `User` (this will send emails to your own employees), but you could also select `Contact` or `Lead`.
 * `relatedToType`: if you selected a relatedTo type when creating the template, you'll see this attribute. I'll explain what this is in the next part.

Note this tag (like every other tag) is closed at the bottom of the editor with `</messaging:emailTemplate>`.

Inside the `<messaging:emailTemplate>` tag, there is a `<messaging:plainTextEmailBody>` tag. This is where we'll put the content of the email.  
We don't want to use plain text emails, but HTML, so we change this to `<messaging:htmlEmailBody>`. Don't forget to change the closing tag too!

Now you can remove the default content and start writing your own.  
Your template should look like this now: [Step 1](samples/step1.html)

#### HTML
> All the following goes inside the `<messaging:htmlEmailBody>` tag.

> HTML is what powers webpages (including Salesforce!), carries around their content and links the Web together. Interestingly enough, it's also used for emails.  
Be warned though: browsers might be enormously powerful, providing a full-fledged platform, but email clients are often very simple, buggy and/or not very standards-compliant. Simple email templates might work fine, but once you start to go wild, things might get weird.

> For a quick guide to HTML, check out [MDN](https://developer.mozilla.org/en-US/Learn/Getting_started_with_the_web/HTML_basics).

Now let's take a look at the essentials:

Every email (or _document_) starts with a `doctype`. I won't go into details about this, because there's absolutely nothing interesting to be told, so just use the HTML5 doctype: `<!DOCTYPE html>` on the first line of the document.

The rest of the content is wrapped in `<html></html>` tags. Inside those, there are two very important ones:

 * `<head></head>`: In the `head` tag, you'll put all the metadata about the document. This includes all the things needed for the document, but invisible to the recipient, e.g. style definitions used to customize the content.
 * `<body></body>`: The `body` tag is where you'll put the actual content of the email.

Your template should look like this now: [Step 2](samples/step2.html)  
You can now start putting headings, paragraphs, lists, tables, etc. in the `body` tag.



### RelatedToType
I've mentioned the `relatedToType` attribute of the `<messaging:emailTemplate>` tag a few times now, and you're probably wondering what it means. As we'll see later, it is possible to merge variables, data from your Salesforce records, into your templates. In order to do this, every template is linked to a specific object. As a result, every email generated from a template, is linked to one record, from the `relatedToType` object.

Let's illustrate this with an example. The default `Project Credential Basic` template aggregates some data from a Project Credential object and is sent to an employee (`recipientType="User"`). The template's `relatedToType` attribute is set to `project_credential__c`, the API name of the Project Credential object.  
This is how you can find the API name of objects:

 * For standard objects, it's just the singular name of the object, in CamelCase, e.g. `Contact`, `AccountContactRole`. The full list of API namee of standard object can be found [here](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_objects_list.htm).
 * For custom objects, search for `objects` in the setup area and click on `Objects`, then click on the name of the desired object. At the top of the page, you can find the API name (which usually ends in `__c`; this is part of the name).

Your template should look like this now: [Step 3](samples/step3.html).


### Styles
Without any styles, your emails will render as black text on a white background. That's very dull, so fortunately, it's possible to brighten things up.  
Styling is done with CSS (read up on [MDN](https://developer.mozilla.org/en-US/Learn/CSS)), usually in a `<style>` tag in the head. You can do a lot of funky stuff with CSS, but unfortunately, this is also where some email clients get in trouble. I advise to keep styling to a minimum, and make sure to test it in a few different email clients.

You probably want to use the same (or similar) styles in all your templates, to empower your brand identity. Copying styles over to every template can get very tedious, though, especially when you have to make changes to them. Fortunately, Salesforce got you covered.  
Visualforce supports **components**, bits of content that can easily be reused in all your templates. Creddox includes one for email styles by default: `email_styles`. You can find your org's components in the setup area: type `components` in the search bar and click `Visualforce Components`.  
You can add your styles to Creddox's `email_styles` component, or create your own.

You can include a component anywhere in your template like this: `<c:component_name />` (this is a _self-closing_ tag, meaning it doesn't need a closing tag). For example, to include the `email_styles` component: `<c:email_styles />`.

Your template should look like this now: [Step 4](samples/step4.html).



## Merging fields
The whole point of using email templates, is to insert (or **merge**) data into them. In Visualforce, this is done with some special syntax: `{!data}`, where `data` is the name of the _variable_ you want to insert.  
You can get data from a few different places:

 * `recipient`: holds data about the recipient. Depending on the `recipientType` you specified, this can be a User, a Contact or a Lead.
 * `relatedTo`: the record this email is linked to. It belongs to the object you specified in `relatedToType`.
 * Global variables: Salesforce exposes some variables to all Visualforce pages and templates. There is a list [here](https://developer.salesforce.com/docs/atlas.en-us.pages.meta/pages/pages_variables_global.htm).

For example, you could start your email with:

```
<h1>Dear {!recipient.Name},</h1>
<p>Here is the description of the {!relatedTo.Name} project:</p>
<p>{!relatedTo.Project_Description__c}</p>
```

Inside the `{! }` tags, you can do some more advanced stuff, like maths, logic and text transformations, like Formula fields. Docs on this can be found [here](https://developer.salesforce.com/docs/atlas.en-us.pages.meta/pages/pages_variables.htm).


### apex:outputField
The `<apex:outputField value="{!data}" />` tag, outputs a field's content, based on the type of field you give it, e.g. a checkbox is rendered as a checkbox, a currency uses the right format and a percent is followed by a `%`. Sometimes, you'll want some more customization options, which are covered below.


### Numbers
`<apex:outputField>` works like a charm for numbers, but it's a bit verbose. For whole numbers, you can use a little shortcut:

```
{!TEXT(relatedTo.Mandays__c)}
```

### Currencies
Currencies can be rendered using the `<apex:outputField>` tag, like so:

```
<apex:outputField value="{!relatedTo.Contract_Value__c} />"
```
Might output `647.700,00€`, based on the recipient's or org's currency locale.

For more customization, you can use MessageFormat:

```
<apex:outputText value="{0, number, €###,###.00}">
  <apex:param value="{!relatedTo.Contract_Value__c}" />
</apex:outputText>
```
Outputs something like `€647,700.00`.

The `{0, number, €###,###.00}` part tells the `<apex:outputText>` tag how to format the value provided by the `<apex:param>` tag.

### Percents
For percents, also use the `<apex:outputField>` tag:

```
<apex:outputField value="{!relatedTo.Completeness_Score__c} />"
```

### Checkboxes
Checkboxes can be rendered in two ways: as regular checkboxes, or as yes/no.

For regular checkboxes, use the `<apex:outputField>` tag (again):

```
<apex:outputField value="{!relatedTo.Selected_for_credential_to_web__c} />"
```
Will look like ![Unchecked checkbox](img/checkbox_unchecked.gif) if the record is not selected or ![Checked checkbox](img/checkbox_checked.gif) if it is.

If you want to somehow use or transform the data - make an Excel sheet, for example - it might be easier to use yes/no (or something similar). To accomplish this, we'll use the `IF()` function:

```
{!IF( relatedTo.Selected_for_credential_to_web__c, 'yes', 'no' )}
```
Will output `yes` if the record is selected, or `no` if it isn't.

### Dates
For dates, you can (again) use the `<apex:outputField>` tag, which should output dates in the recipient's locale

```
<apex:outputField value="{!relatedTo.Project_Start_Date__c}" />
```
Might look like: `21/01/2016` (depending on the recipient's locale).

Date fields in Creddox objects where only the month and year make sense (like project start and end dates), have formatted counterparts. For example, the `Project_Start_Date__c` field is accompanied by `Project_Start_Date_Formatted__c`. These are formula's outputting regular strings:

```
{!relatedTo.Project_Start_Date_Formatted__c}
```
Will look like: `jan 2016`.



## Relational fields
In email templates, it's also possible to access fields of Lookup records. The syntax is very similar, but there are some differences nonetheless.  
For standard objects, you append `__r` to the parent's API name. For custom objects, your change the trailing `c` to `r`, to also get `__r` at the end.

For example, you could use `{!relatedTo.Account__r.Name}` to get the name of the related Account, or `{!relatedTo.Contact_Full_Name__r.Name}` to get the name of the associated Contact (where `Contact Full Name` is a `Lookup(Contact)`).  
If you use `__c` at the end (for custom objects) or just the field name (for standard objects), you'll get the ID of the related record, with `__r` you get the actual record.

## Related lists
With Master-Detail relationships, it's a bit more complex, because we're dealing with **lists** of records, and because the name of the relationship is defined by the child object.

To get the name from a child to its parent, it's just the name of the Master-Detail field, with `__r` at the end. This is the same as Lookup relationships.  
To get the name from a parent to its child records, navigate to the child object in the setup area and click on the name of the Master-Detail field. You'll see a setting called `Child Relationship Name`. Append `__r` to this name and you have the API name.

For this example, we'll use the Credential objects: the Project vs Person (PvP) object is a junction object between the Project Credential (PrC) and People Credential (PeC) objects, which means it has Master-Detail relationships with the PrC and PeC objects. Or, in other words, a PrC or PeC record each have a list of associated PvP records, and each PvP record has one PrC and one PeC record. Lets draw this out:

```
    ┌ PvP ─ PeC
PrC ┼ PvP ─ PeC
    └ PvP ┐
PrC - PvP ┼ PeC
PrC - PvP ┘
```

So, if we want to include data about PeC records in an email about a PrC record, we have to go through the PrC's associated PvP records. To get the PvP records from the PrC record, we have to find the child relationship name. So navigate to the PvP object in the setup area, click on `Project Name` (which is a `Master-Detail(PrC)`) and locate the `Child Relationship Name`, in this case `People_Intersection`. Now we know that `{!relatedTo.People_Interection__r}` will get us the list of PvP records.

Now it's probably time to start making a table with Visualforce. This is how it works:
 1. The whole table is wrapped in a `<apex:dataTable>` tag, passing a list of records in the `value` attribute, and the name of the iteratee in the `var` attribute. For every record in the list, this tag will repeat its contents, setting the variable defined in the `var` attribute to the current record.
 2. Inside the table, we add `<apex:column>` tags, providing them a name for their header and a value. This tag can be self-closing (providing its value in a `value` attribute), or not.

In the case of our example, this is what the `dataTable` tag looks like:

```
<apex:dataTable value="{!relatedTo.People_Intersection__r}" var="pvp">

</apex:dataTable>
```

Before we can add columns, we need to find the name of the PeC record associated with each PvP record. This a relationship to the parent of the PvP record, so we can just look at the corresponding field name: `Person__c`. This means I can get the PeC record with the following: `{!pvp.Person__r}`. If I wanted the name of the person, I could use `{!pvp.Person__r.Name}`.
Of course, we can still use data on the PvP record: `{!pvp.Assignment_Ongoing__c}` would get the Assignment Ongoing checkbox.  
Now, let's use this in our table:

```
<apex:dataTable value="{!relatedTo.People_Intersection__r}" var="pvp">
  <apex:column headerValue="Employee" value="{!pvp.Person__r.Name}" />
  <apex:column headerValue="Assignment Ongoing">
    <apex:outputField value="{!pvp.Assignment_Ongoing__c}" />
  </apex:column>
</apex:dataTable>
```
This would result in something like this:

Employee    | Assignment Ongoing
------------|-------------------
John Doe    | ![yes](img/checkbox_checked.gif)
Nico Reagan | ![no](img/checkbox_unchecked.gif)





## Resources
 * [Salesforce docs on email templates](https://help.salesforce.com/htviewhelpdoc?id=admin_emailtemplates.htm)
 * [Salesforce docs on Visualforce email templates](https://developer.salesforce.com/docs/atlas.en-us.200.0.pages.meta/pages/pages_email_templates_intro.htm)
 * [Salesforce docs on Visualforce](https://developer.salesforce.com/docs/atlas.en-us.200.0.workbook_vf.meta/workbook_vf/visualforce_workbook_intro.htm)
 * [Salesforce docs on Visualforce expressions](https://developer.salesforce.com/docs/atlas.en-us.pages.meta/pages/pages_variables.htm)
 * [Introduction to HTML on MDN](https://developer.mozilla.org/en-US/Learn/Getting_started_with_the_web/HTML_basics)
 * [Introduction to CSS on MDN](https://developer.mozilla.org/en-US/Learn/CSS)
 * [List of standard API names](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_objects_list.htm)
 * [List of global Visualforce variables](https://developer.salesforce.com/docs/atlas.en-us.pages.meta/pages/pages_variables_global.htm)




[Creddox]: https://pursuitpower.com/creddox
[PursuitPower]: https://pursuitpower.com
