# MailChimpSubscribe #

Package for subscribing users in Mailchimp lists using FormIt. Adds a snippet for retrieving Mailchimp lists in a single select TV and a FormIt hook for subscribing Mailchimp users to the list provided in the pages Template Variable.

## Todo
* Add option to add Mailchimp list ID as FormIt property instead of needing to set in inside the page TV.

### Functionalities ###

* FormIt hook for subscribing users to Mailchimp lists based on TV value
* MailChimpGetLists: Snippet for creating a select list of MailChimp lists.

### How do I get set up? ###

* Add MailChimp API Key in systemsettings: mailchimpsubscribe.mailchimp_api_key
* Create a new single select TV variable and set the input option values to:    
    
```
#!php

@EVAL return $modx->runSnippet('MailChimpGetLists');
```

* Add MailChimp List ID TV in systemsettings: mailchimpsubscribe.list_tv
* Add MailChimpSubscribe to your FormIt hooks
* Add in your chunk the placeholder fi.error.mailchimp, which holds all MailChimp error messages.
* Add a field called newsgroup, if the value of this field is set to yes, the user will be subscribed to the mailchimp list.

### Dependencies ###

* FormIt


### Example usage ###


```
#!html

[[!FormIt?
    &hooks=`spam,MailChimpSubscribe,redirect`
    &validate=`email:email:required,name:required,company_name:required,nospam:blank`
    &redirectTo=`[[++page_newsletter_thanks]]`
    &validationErrorMessage=`true`
    &store=`1`
    &submitVar=`newsletter-submit`
]]
        
<form action="[[~[[*id]]]]" role="form" method="post" novalidate>
    <input type="hidden" name="nospam" value="" />
    <input type="hidden" name="newsgroup" value="yes"/>
    
    [[!+fi.error.mailchimp:notempty=`
        <p class="error">[[!+fi.error.mailchimp]]</p>
    `]]
           
    <div class="form-group [[!+fi.error.name:notempty=`has-error`]]">
        <input type="text" name="name" placeholder="Name" value="[[!+fi.name]]">
    </div>
        
    <div class="form-group [[!+fi.error.email:notempty=`has-error`]]">
        <input type="text" name="email" placeholder="Email" value="[[!+fi.email]]">
    </div>
    
    <div class="form-group [[!+fi.error.company_name:notempty=`has-error`]]">
        <input type="text" name="company_name" placeholder="Company" value="[[!+fi.company_name]]">
    </div>
    
    <div class="form-group">
        <input type="submit" name="newsletter-submit" value="Submit">
    </div>
        
</form>
```