# Designing for security

## Client

We can do nothing about the client. If it is hacked it is hacked. Outside our control

## Network

We can make sure that the document we ask for is the one we get. CAS (Content Addressable Storage) is a way of doing
this. We include the hash of the document as the name of the document. This means that if the document is changed
then the hash will change and document can be checked by the client

## Server

We want to be able to use 'any server' to serve our documents from. CAS is the solution to this. If the server changes
the files, they corrupt them and stop them working

## Urls

Pretty much every URL has to be content addressed. Example

* @laoban@/laoban.json/core.laoban.json/<hash>
* @laoban@/templates/javascript/<hash>

This is a small change. The fileOps can check them. It means 'the file I asked for is the file I got', but we keep
readability

## Root

* The list of all inits we support - we already have this
* The list of all templates we support: making of name to value (new idea but we need it)
* We need a name for the 'core.laoban.json' sort of things, and a list of those.

And those things need descriptions and documentation and...

Let's specify these in DNS and validate against a public key that is shipped with the application.
So we have the three urls and a signature, and check the three urls against the signature.

Then we can trust these urls (the initurl gets us going). From that we

## How can we trust a template?

The .template.json file was addressed with a CAS. So we know it's the one we want. It's urls involve a sha. We can check
the name to see if it is a registered template

The template comes from the laoban.json file, and that was created by init probably using

## Root documents

The root documents are

* The list of inits
*
* 