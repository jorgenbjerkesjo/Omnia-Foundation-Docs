Email
============================

The Email API allows you to send email from your solution.

You reach the Caching API through the following service

.. code-block:: c#

	OmniaApi.WorkWith(Ctx.Omnia()).Email(Ctx);
	

.. contents:: The API contains the following methods:
  :local:
  :depth: 1

SendEmail
#######

Sends an email message

.. code-block:: c#

	void OmniaApi.WorkWith(Ctx.Omnia()).Email(Ctx).SendEmail(string subject, string body, List<string> emailTo);
	
You can add multiple email addresses in the **emailTo** list

There is also two optional input parameters: **emailCC** and **emailBcc**, both of type **List<string>**. 

- Add email addresses to **emailCC** to add them to the the carbone copies list of the email .
- Add email addresses to **emailBCC** to add them to the the blind carbone copies list of the email.
	
