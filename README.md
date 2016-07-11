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
   * [Dates](#dates)
   * [Currencies](#currencies)
 * [Relational fields](#relational-fields)
 * [Related lists](#related-lists)
 * [Testing a template](#testing-a-template)
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
 * `relatedTo`: if you selected a relatedTo type when creating the template, you'll see this attribute. I'll explain what this is in the next part.

Note this tag (like every tag) is closed at the bottom of the editor with `</messaging:emailTemplate>`.

Inside the `<messaging:emailTemplate>` tag, there is a `<messaging:plainTextEmailBody>` tag. This is where we'll put the content of the email.  
We don't want to use plain text emails, but HTML, so we change this to `<messaging:htmlEmailBody>`. Don't forget to change the closing tag too!

Now you can remove the default content and start writing your own.  
Your template should look like this now: [Step 1](blob/master/samples/step1.html)

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

Your template should look like this now: [Step 2](blob/master/samples/step2.html)  
You can now start putting headings, paragraphs, lists, tables, etc. in the `body` tag.



### RelatedToType
I've mentioned the `relatedToType` attribute of the `<messaging:emailTemplate>` tag a few times now, and you're probably wondering what it means. As we'll see later, it is possible to merge variables, data from your Salesforce records, into your templates. In order to do this, every template is linked to a specific object. As a result, every email generated from a template, is linked to one record, from the `relatedToType` object.

Let's illustrate this with an example. The default `Project Credential Basic` template aggregates some data from a Project Credential object and is sent to an employee (`recipientType="User"`). The template's `relatedToType` attribute is set to `project_credential__c`, the API name of the Project Credential object.

Your template should look like this now: [Step 3](blob/master/samples/step3.html).


### Styles
Without any styles, your emails will render as black text on a white background. That's very dull, so fortunately, it's possible to brighten this up.  
Styling is done with CSS (read up on [MDN](https://developer.mozilla.org/en-US/Learn/CSS)), usually in a `<style>` tag in the head. You can do a lot of funky stuff with CSS, but unfortunately, this is also where some email clients get in trouble. I advise to keep styling to a minimum, and make sure to test it in a few different email clients.

You probably want to use the same (or similar) styles in all your templates, to empower your brand identity. Copying styles over to every template can get very tedious, though, especially when you have to make changes to them. Fortunately, Salesforce got you covered.  
Visualforce supports **components**, bits of content that can easily be reused in all your templates. Creddox includes one for email styles by default: `email_styles`. You can find your org's components in the setup area: type `components` in the search bar and click `Visualforce Components`.  
You can add your styles to Creddox's `email_styles` component, or create your own.

You can include a component anywhere in your template like this: `<c:component_name />` (this is a _self-closing_ tag, meaning it doesn't need a closing tag). For example, to include the `email_styles` component: `<c:email_styles />`.

Your template should look like this now: [Step 4](blob/master/samples/step4.html).






## Resources
 * [Salesforce docs on email templates](https://help.salesforce.com/htviewhelpdoc?id=admin_emailtemplates.htm)
 * [Salesforce docs on Visualforce email templates](https://developer.salesforce.com/docs/atlas.en-us.200.0.pages.meta/pages/pages_email_templates_intro.htm)
 * [Salesforce docs on Visualforce](https://developer.salesforce.com/docs/atlas.en-us.200.0.workbook_vf.meta/workbook_vf/visualforce_workbook_intro.htm)
 * [Introduction to HTML on MDN](https://developer.mozilla.org/en-US/Learn/Getting_started_with_the_web/HTML_basics)
 * [Introduction to CSS on MDN](https://developer.mozilla.org/en-US/Learn/CSS)




[Creddox]: https://pursuitpower.com/creddox
[PursuitPower]: https://pursuitpower.com
