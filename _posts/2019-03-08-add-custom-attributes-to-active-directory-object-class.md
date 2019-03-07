---
layout: post
title: "Add Custom Attributes To Active Directory Object Classes"
author: "Rob Derickson"
tags: activedirectory,ad,schema,powershell,aws
---

Extending your Active Directory schema can be scarey, but it may not be as scarey as you think. I will show you how to use the ["ADSchema"](https://www.powershellgallery.com/packages/ADSchema/) module in the ["PowerShell Gallery"](https://www.powershellgallery.com/) to add custom attributes to existing object classes.

If you do not know what an OID is, read ["Create A New OID for Active Directory Schema Extensions"]({{ site.baseurl }}{% post_url /_posts/2019-03-07-create-a-new-oid-for-active-directory-schema-extensions %}) first. That article will walk you through creating an OID that you probably should not be using in production. Anyway, have a firm OID strategy before proceeding.

The basic steps to adding custom attributes to object classes goes like this:
1. Create a new schema attribute.
2. Create a new auxiliary object class.
3. Add your new attribute to your new class.
4. Attach your new class to an existing AD object class.

**Note:** When you create a custom schema attribute, you cannot delete it. This is one reason creating an auxiliary class is important. You can unbind the auxiliary class from an existing class if you decide you do not want those attributes anymore.

## Install the ADSchema module from the PowerShell Gallery
The ["ADSchema"](https://www.powershellgallery.com/packages/ADSchema/) module from ["Andy Schneider"](https://github.com/SchneiderAndy/) has everything you need to extend your AD schema. It even has a cmdlet to generate an OID that looks a little more sensible than the ones I generate.

Open PowerShell, and install the ADSchema module:  
```powershell
Install-Module 'ADSchema' -Scope 'CurrentUser'
```

## Create your custom attribute and auxiliary class
In my example, I will create a new attribute for groups named `robAwsAccountId`. In the code sample below, replace every instance of _your-&lt;type&gt;-oid-here_ with your own OIDs. I created the following OIDs for my use:
| Type | OID |
|------|-----|
| Class | 1.2.840.113556.1.8000.2554.29303.55750.9464.19531.42101.14752338.11883148.2 |
| Attribute | 1.2.840.113556.1.8000.2554.29303.55750.9464.19531.42101.14752338.11883148.2.1 |

```powershell
# Create your custom attribute
New-ADSchemaAttribute -Name 'robAwsAccountId' -Description 'AWS account ID' -AttributeType 'String' -AttributeID 'your-attribute-oid-here'

# Create your auxiliary class
New-ADSchemaClass -Name 'robGroup' -AdminDescription 'Group class to host custom attributes' -Category 'Auxiliary' -AttributeID 'your-class-oid-here'

# Add your new attribute to your new class
Add-ADSchemaAttributeToClass -Attribute 'robAwsAccountId' -Class 'robGroup'

# Bind your new auxiliary class to your 'group' class
Add-ADSchemaAuxiliaryClassToClass -AuxiliaryClass 'robAwsAccountId' -Class 'group'

# Try it out
Set-ADGroup 'AWS-Master-Billing' -Add @{robAwsAccountId = '012345678912'}
Get-ADGroup 'AWS-Master-Billing' -Properties 'robAwsAccountId'
```

Now if I wanted to use `robAwsAccountId` in SAML assertions to AWS, I could do that. I do not want to do that, however. So I won't.

## Related links
* ["ADSchema module repository"](https://github.com/SchneiderAndy/ADSchema)
* ["ADSchema module in the PowerShell Galery"](https://www.powershellgallery.com/packages/ADSchema/)