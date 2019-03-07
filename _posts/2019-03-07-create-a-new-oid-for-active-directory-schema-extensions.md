---
layout: post
title: "Create a New OID for Active Directory Schema Extensions"
author: rob_derickson
tags: activedirectory,ad,schema,oid,powershell
---

This article is for individuals who need an OID to create custom Active Directory schema classes or attributes, and do not already have a Private Enterprise Number (PEN) from http://pen.iana.org. This is specific to Active Directory. If you are using some other LDAP directory, refer to its documentation or register for a PEN.

There are a lot of scary warnings on the internet to get your OID right before customizing the Active Directory schema. I will start by showing you how to create a base OID you can use for all of your schema extensions. Then I will suggest a (possibly incorrect) way to extend your base OID for your custom schema objects.

## Create your base OID
Run this bit of PowerShell code to create a base OID beginning with the prefix `1.2.840.113556.1.8000.2554` suggested by Microsoft.

```powershell
# Prefix suggested by Microsoft
$Prefix = '1.2.840.113556.1.8000.2554'

# Create a new GUID and remove the dashes
$Guid = (New-Guid).guid.Replace('-','')

# Define substring arguments for picking apart your GUID
$SubstringArgs = (0,4),(4,4),(8,4),(12,4),(16,4),(20,6),(26,6)

# Create a collection of your GUID parts, and convert them from Hexadecimal to Decimal
$OidSuffixParts = foreach ($ArgPair in $SubstringArgs) {
    [Convert]::ToInt64($Guid.Substring($ArgPair[0],$ArgPair[1]),16)
}

# Join all the GUID parts with a period between them, and then join the Prefix and Suffix
$Suffix = $OidSuffixParts -join '.'
$Prefix, $Suffix) -join '.'
```

I ended up with this OID: `1.2.840.113556.1.8000.2554.29303.55750.9464.19531.42101.14752338.11883148`

## Create and follow an OID usage policy
I don't know if this is the right way to use OIDs in AD, but this is the way I do it.

### Assign a number to any class and attribute you plan to modify or create
Suggested practice for extending the AD schema is to create an auxiliary class and attach that to the native class in AD. Assign a number for each aux class you create. This number will be appended to your base ID to create an OID for that class.

| Class | OID Number |
|-------|------------|
| Rob-User | 1 |
| Rob-Group | 2 |
| Rob-Computer | 3 |
| Rob-Contact | 4 |
| Rob-Organization-Unit | 5 |

Create and maintain a similar table for each of your custom schema attributes. Example:

| Atrribute | OID Number |
|-------|------------|
| Rob-Custom-Attrib1 | 1 |
| Rob-Favorite-Color | 2 |

### Append these numbers to your base OID when creating a new schema attribute

Every time you create an auxiliary class, append a period and the class's number to the end of your base OID as a new base for the class. Using the OID I created above, my Group class's base OID would be `1.2.840.113556.1.8000.2554.29303.55750.9464.19531.42101.14752338.11883148.3`.

For every attribute you add to this class, append a period and your attribute's OID number to the end of the class's OID to get your attribute's OID. My Rob-Favorite-Color OID would be `1.2.840.113556.1.8000.2554.29303.55750.9464.19531.42101.14752338.11883148.3.2`.

That's it! Now you have a base OID you can build on when customizing Active Directory. This scheme should not interfere with AD, and no other schema extensions or AD updates should interfere with it.

#### Reference articles
I developed this solution after reading the following article and examining the related VBScript. I'm sorry if they no longer exist:

* [Microsoft document on obtaining an OID](https://docs.microsoft.com/en-us/windows/desktop/AD/obtaining-an-object-identifier-from-microsoft)
* [VBScript to create a base OID](https://gallery.technet.microsoft.com/scriptcenter/56b78004-40d0-41cf-b95e-6e795b2e8a06)