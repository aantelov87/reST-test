.. _Chap2-IntegrationGuidelines:

===================================
Chapter 2 - Integration Guidelines
===================================

This chapter outlines the basic integration requirements that you app must meet.

--------------------
Submitting a Request
--------------------

Service Endpoints
==================

There are two endpoints (base URLs) that you can make your API calls to. 

   - Stage if you are testing your integration, and 
   - Production for when you have finished testing and want your application to go live.

All URLs referenced in this guide must have one of the following bases:

.. table:: Truth table for "not"

   =====  =====
     A    not A
   =====  =====
   False  True
   True   False
   =====  =====
   
.. table:: Table 5 Service endpoints

==============	=====================================================
Environment      Endpoint
============== 	=====================================================
Stage            https://stage-secure-gateway.allopass.com/rest/v1/
-------------- 	-----------------------------------------------------
Production       https://secure-gateway.allopass.com/rest/v1/
==============  =====================================================

Authentication
=============== 

Overview
  All requests to HiPay TPP REST API require the Merchant to authenticate himself using
  the HTTP Basic Authentication.

Authentication
  Your API credentials can be found in the Merchant Interface in the Integration section.
  Most HTTP clients (including web-browsers) have built-in support for HTTP Basic Authentication. 
  If not, the following header must be included in all HTTP requests.

Authorization
  Basic base64("<API login>:<API password>")
 
.. note:: If the login and/or password is wrong, the 401 Unauthorized HTTP Status Code is returned.  
   
Character Encoding
==================    
All parameters accept UTF-8 encoded text via the API.
All other encodings must be converted to UTF-8 before sending them to the HiPay TPP API in order to guarantee that the data is not corrupted.   

--------------------
Response Handling
--------------------
The HTTP status code indicates whether a request succeeded or not.
  i.e. a 2xx status code indicates a success, whereas a 4xx or a 5xx status code indicates a failure.

Response Status Codes
=====================
   
The HiPay TPP API attempts to return appropriate HTTP status codes for every request.

These are the HTTP status codes you may receive and their meaning; they are listed below:

.. table:: Table 6 HTTP status codes

=======================  =============================================================================
HTTP status              Description
=======================  =============================================================================
200 OK                   The request was understood and successfully processed.
-----------------------  -----------------------------------------------------------------------------
400 Bad Request          The request was rejected due to a validation error.
                         An error message is displayed with an explanation of the error situation.
-----------------------  -----------------------------------------------------------------------------
401 Unauthorized         Payment Card Industry Data Security Standards
                         Your credentials are invalid.
-----------------------  -----------------------------------------------------------------------------						 
403 Forbidden            You are making a call to a resource you don't have permission to.

                           * i.g. you are trying to retrieve information about a token you don't own.
                         An error message is displayed with an explanation of the error situation.
-----------------------  -----------------------------------------------------------------------------						 
404 Not Found            The resource requested, such as a token, does not exist.
-----------------------  -----------------------------------------------------------------------------
405 Method Not Allowed   The HTTP method you used is not allowed for the requested URL.

                           * i.g. you are trying to retrieve information about a token you don't own. 
-----------------------  -----------------------------------------------------------------------------
500 Server Error         HiPay TPP server error. Report this to the HiPay TPP Technical Support
503 Service Unavailable  HiPay TPP API is temporarily unable to process the request. Try again later.
=======================  =============================================================================


Response Formats
-----------------
Overview
  The HiPay TPP API can respond to your requests in various formats.
  To specify your preferred response format, use the HTTP Accept request header.

Authentication
  Here are examples of possible headers.
  
    - Accept: application/json
    - Accept: application/xml
    - Accept: application/json, application/xml;q=0.8, */*;q=0.5
  Refer to the RFC 2616 HTTP Accept Header for details.

Responses in XML Format
  By default, HiPay TPP REST API returns XML with a root element of <response>.
  i.e. here is the default XML representation of a token lookup result.
		
.. code-block:: xml
    :linenos:

   	<?xml version="1.0" encoding="UTF-8"?>
   	<response>
   	  <state>completed</state>
   	  <reason/>
   	  <forward_url/>
   	  <test>false</test>
   	  <mid>00035167042</mid>
   	  <attempt_id>2015</attempt_id>
   	  <authorization_code>59351</authorization_code>
   	  ...
   	</response>

Responses in JSON Format
  The API also supports returning resource representation as JSON.
  Simply add the *Accept: application/json* header to any request.

.. code-block:: json
    :linenos:

   	POST /rest/v1/order HTTP/1.1
   	Host: secure-gateway.allopass.com
   	Accept: application/json
   	Connection: close

Here is the response to above request, represented as JSON.

.. code-block:: json
    :linenos:

   	{
   	  "state":"completed",
   	  "reason":"",
   	  "forwardUrl":"",
   	  "test":"false",
   	  "mid":"00035167042",
   	  "attemptId":"1",
   	  "authorizationCode":"59351",
   	  ...
	  }
	
 	
	
	
		
		