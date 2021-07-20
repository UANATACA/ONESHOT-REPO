# What it is

<div style="text-align: justify">
The solution for electronic signature with a One Time certificate issue. One-Shot API is the solution for Uanataca One-Shot Signature service, a complete option for the electronic signature of documents within your application or web.
<br></br>
It is designed so that no sensitive data has to be sent away from your premises, as only hashes of the documents to be signed need to be transmitted to the signature service. Documents are signed through the creation of single-use digital certificates, which are created at the moment for immediately be used to electronically sign all documents included in a given transaction.
<br></br> 
Electronic signatures include a time stamp, providing to the document signature with a reliable date and time.
<br></br>
Through this system we offer our One-shot Signature service, an autonomous solution adapting to multiple use cases.
</div>

# How it works

<div style="text-align: justify">
The API is given with One-Shot Optimizer that is a server system exposing http RESTful API by means of which, business applications are enabled to require the electronic signature.
<br></br>
One-Shot Optimizer performs the most computationally expensive workload of the signature process, thus reducing the data traffic on the local network and make the most of the cryptographic hardware acceleration. The documents to be signed are processed in the customer business layer and are not send to Uanataca Services, instead is sent a hash of the document created using a hash algorithm.
<br></br>
Uanataca is a Qualified Trusted Service Provider that issue digital certificates by a trusted Certification Authority (CA). The role of Registration Authority Officials (RAO) is needed to add new user requests to the infrastructure and so request the creation of new digital certificates for its users. Each of these certificates can then be used to electronically sign documents.
<br></br>
One-Shot Signature certificates are generated on the spot every time a new set of documents requires a signature. Through the One-Shot Signature service, you will play the role of a RAO, providing identifying data for each user and requesting the generation of signature certificates. Once user registration data has been provided and the certificate is ready to be generated, the end user will activate the generation of the digital certificate and complete the signature procedure.
<br></br>
The electronic signatures are performed in Uanataca Trusted Service Center side, where signature keys are generated and stored in a Qualified Electronic Signature Creation Device (QSCD) system.
</div>
<br></br>

![img](https://github.com/UANATACA/ONESHOT-REPO/raw/main/img/oneshot-hiw.png)


# Classic Workflow


<div style="text-align: justify">
In a common One-Shot Signature service, an OTP (One-Time Password) code is sent by sms to the end user directly from Uanataca services. The OTP verification is used to activate the issuing of single-use digital signature certificate and the usage of it to sign the signature request documents.
<br></br>
One-Shot Signature can use other external authentication methods instead of Uanataca SMS. This alternative methods are subjected to approval.
<br></br>
The following images summarize One-Shot Signature flow involving both authentication methods:
</div>
<br></br>

> Uanataca SMS

![img](https://github.com/UANATACA/ONESHOT-REPO/raw/main/img/oneshot_flc1.png)

</br>

1. The business application creates a new digital signature request, providing all required user data
2. One-Shot Optimizer returns an identifier for the certificate request
3. The business application provides the documents to be signed by the end user
4. The business application retrieve the service contract
5. The business application shows the documents to be signed and the service contract to its end user
6. After reviewing the documents, the end user agrees to sign them
7. The business application starts the signature process by requesting the generation of a One-Time Password (OTP) token for the signature
8. Uanataca services sends the OTP directly to the end user through an SMS message
9. By introducing the OTP, the end user identifies himself as the subject of the signature certificate
10. The business application provides the OTP and the identifier of the signature request to the One-Shot Optimizer
11. One-Shot Optimizer takes care of computing the hash of the documents to be signed in the business layer
12. Hashes are sent together with the request identifier and OTP code to Uanataca Services
13. The end user signature certificate is generated and used to sign the hashes
14. The signed hashes and the signature identifier are returned to the One-Shot Optimizer
15. One-Shot Optimizer generates the signed document envelopment, combining the original documents with the signed hashes
16. Finallly, the business application calls One-Shot Optimizer API to obtain the signed documents
<br></br>

> Other authentication methods


![img](https://github.com/UANATACA/ONESHOT-REPO/raw/main/img/oneshot_flc2.png)

</br>

1. The business application creates a new digital signature request, providing all required user data
2. One-Shot Optimizer returns an identifier for the certificate request
3. The business application provides the documents to be signed by the end user
4. The business application retrieve the service contract
5. The business application shows the documents to be signed and the service contract to its end user
6. After reviewing the documents, the end user agrees to sign them
7. The client application starts the signature process with an authentication method provided by the client
8. One-Shot Optimizer takes care of computing the hash of the documents to be signed in the business layer
9. Hashes are sent together with the request identifier and an id of the busuness authentication method to Uanataca Services
10. The end user signature certificate is generated and used to sign the hashes
11. The signed hashes and the signature identifier are returned to the One-Shot Optimizer
12. One-Shot Optimizer generates the signed document envelopment, combining the original documents with the signed hashes
13. Finally, the business application calls One-Shot Optimizer API to obtain the signed documents

</br>

This section presents the workflow for a simple use case of the One-Shot Signature service with a step-by-step description of the API calls required to allow a user to digitally sign a document provided by the client application. 

<a href="#section/Video-tutorials/Signature-Workflow-(SMS)"><img src="https://raw.githubusercontent.com/UANATACA/ONESHOT-REPO/main/img4.png"></a><a href="#section/Video-tutorials/Signature-Workflow-(SMS)"><b>&nbsp;Watch on video!</b></a>

You can follow the next example using the developers One-Shot Optimizer configured for test environment in https://one-shot.developers.uanataca.com, or you can find the instructions to set up your One-Shot Opimizer in the <a href="#section/Configuration"> configuration section</a>.

</br>

*The basic digital signature process involves the following steps:*

**1) RETRIEVE AN EXISTING TOKEN FOR THE RAO**

**2) CREATE A NEW DIGITAL SIGNATURE REQUEST**

**3) UPLOAD A DOCUMENT**

**4) RETRIEVE SERVICE CONTRACT**

**5) GENERATE AN OTP (only for Uanataca SMS)**

**6) SIGN THE DOCUMENT**

**7) RETRIEVE SIGNED DOCUMENT**

**8) DELETE DOCUMENTS FROM OPTIMIZER**

</br>

> **STEP 1: RETRIEVE AN EXISTING TOKEN FOR THE RAO**

</br>

**API Reference:** <a href="#tag/Tokens/paths/~1api~1v1~1tokens/get">List Tokens</a>

The test One-Shot Optimizer is pre-configured with a Registration Authority Officer (RAO) account ready to be used within the test environment. This account has an associated token, that can be used to identify the RAO in API calls.

	curl -X GET https://one-shot.developers.uanataca.com/api/v1/tokens

On the clean machine, this should return a single token:

	{
		"status": "200 OK",
		"details": {
			"6d1cae4d55be4cdf9cac50ee36f73406": {
				"username": "9001800",
				"password": true,
				"pin": true
			}
		}
	}

This output tells us that a single token "6d1cae4d55be4cdf9cac50ee36f73406" exists. This token is associated to the RAO account with id "9001800" and can be used in place of the password and pin.

To use tokens in a production environment, you will need to create them first with the corresponding <a href="#tag/Tokens/paths/~1api~1v1~1token/post">Create token</a> API call.


</br>

> **STEP 2: CREATE A NEW DIGITAL SIGNATURE REQUEST**

</br>

**API Reference:** <a href="#tag/Requests/paths/~1api~1v1~1request/post">Create Request</a>

Within the One-Shot Signature Service, all data pertaining to a given digital signature is collected within a Digital Signature Request. This includes both the identifying information of the signing user, which is provided when you create the signature request, and the document or documents to be signed, which we will upload later.

This call must include enough information to identify both the signing user and the RAO approving the request. The full description of the arguments accepted by this endpoint can be found in the API call detailed documentation, but for now it is enough to include at least the following:

	curl --location --request POST 'https://one-shot.developers.uanataca.com/api/v1/request' \
		 --form 'token=6d1cae4d55be4cdf9cac50ee36f73406' \
		 --form 'profile=PFnubeQAFCiudadano' \
		 --form 'given_name=name_of_the_user' \
		 --form 'surname_1=surname_of_the_user' \
		 --form 'email=user-example@domain.com' \
		 --form 'mobile_phone_number=+343391234567' \
		 --form 'document_front=@document_front.png' \
		 --form 'document_rear=@document_rear.png' \
		 --form 'document_owner=@document_owner.png'

where token is the token representing the RAO credentials obtained in the previous step.

If the signature request is completed successfully, we will get the unique identifier assigned to it:

	{
		"status": "201 Created",
		"details": 1464
	}

The request code will be used to identify this digital signature request in subsequent calls.


</br>

> **STEP 3: UPLOAD A DOCUMENT**

</br>

**API Reference:** <a href="#tag/Documents/paths/~1api~1v1~1document~1{pk}/post">Upload Document</a>


After creating the digital signature request, we can associate to it all pdf documents that should be signed by the user.

	curl -F "file=@doc.pdf" -X POST https://one-shot.developers.uanataca.com/api/v1/document/1464

Note that the number at the end of the call is the request id we obtained in the previous step.

If the upload is successful, the response will contain the identifier assigned to each document:

	{
		"status": "200 OK",
		"details": "712c29ac-a2dc-4530-8c67-d0f227d8294b"
	}

</br>

> **STEP 4: RETRIEVE SERVICE CONTRACT**

</br>

**API Reference:** <a href="#tag/Documents/paths/~1api~1v1~1document~1{pk}~1contract/get">Retrieve Contract</a>

As a Trusted Service Provider, Uanataca must inform certificate applicants of the terms and conditions governing the issuance of certificates. 

A service contract is generated in each digital certificate issue. The user must view the service contract to sign later along with the documents to be signed in the signature request, using the one time certificate issued on the spot.

	curl -X GET https://one-shot.developers.uanataca.com/api/v1/document/1464/contract

The response by the server will be the service contract document file in binary format:

	%PDF
	...

</br>

> **STEP 5: GENERATE AN OTP (only for Uanataca SMS)**

</br>

**API Reference:** <a href="#tag/Requests/paths/~1api~1v1~1otp~1{pk}/post">Generate OTP code</a>

<blockquote style="background-color: #faf3ac; border-color: #5a5a5a; color: #3b3b3b;">This step is only used for flows using Uanataca SMS.</blockquote>

Once the documents to be signed are ready, we need to generate a secure One-time password (OTP) that will allow the user to sign them. The OTP is generated by calling the otp endpoint and the resulting OTP is sent as an SMS message directly to the phone number we provided when creating the signature request.

When calling the OTP endpoint, provide the request identifier returned by the request endpoint:

	curl -X POST https://one-shot.developers.uanataca.com/api/v1/otp/1464

A successful call will look like this:

	{
		"status": "200 OK",
		"details": "OTP generated"
	}

With this call, an SMS with the secret code is sent to the mobile phone number associated to the signature request.


</br>

> **STEP 6: SIGN THE DOCUMENT**

</br>

**API Reference:** <a href="#tag/Requests/paths/~1api~1v1~1sign~1{pk}/post">Sign</a>

In this step we are going to issue the digital signature certificate and sign all documents previously uploaded for the signature request.

Call the sign endpoint with the request id and json parameters containing the OTP:

	curl -d @params.json -H "Content-Type: application/json -X POST https://one-shot.developers.uanataca.com/api/v1/sign/1464

params.json for Uanataca SMS:

	{
		"secret": "123456"
	}

params.json for other authentication methods:

	{
		"secret": "123456"
		"ext_unlock_type": "biometric"
		"ext_unlock_value": "12345678-12345678"
	}

</br>

A successful call will result in the following response:

	{
		"status": "200 OK",
		"details": "Documents correctly signed"
	}

</br>

> **STEP 7: RETRIEVE SIGNED DOCUMENT**

</br>

API reference: <a href="#tag/Documents/paths/~1api~1v1~1document~1{pk}~1{type}~1{uid}/get">Retrieve Document</a>

Once the signature is done, the next step is getting the signed documents.

To do this, query with an HTTP GET request the endpoint /api/v1/document/{pk}/{type}/{uid}, where {pk} is the Request's unique identifier, {type} is the type of the document (it can be "original" for the uploaded document or "signed" for the digitally-signed version) and {uid} is the document unique identifier.

	curl -X GET https://one-shot.developers.uanataca.com/api/v1/document/1464/signed/712c29ac-a2dc-4530-8c67-d0f227d8294b

The response by the server will be the document in binary format:

	%PDF
	...

</br>

> **STEP 8: DELETE DOCUMENTS FROM OPTIMIZER**

</br>

**API reference:** <a href="#tag/Documents/paths/~1api~1v1~1documents~1{pk}/delete">Delete All Documents</a>

<blockquote style="background-color: #faf3ac; border-color: #5a5a5a; color: #3b3b3b;">⚠ While the service will not delete uploaded files unless explicitly requested through an API call, it is <strong>strongly recommended</strong> that you backup any files that you want to preserve using an alternative system.</blockquote>

Delete all documents associated to an ended digital signature request.

	curl -X DELETE https://one-shot.developers.uanataca.com/api/v1/documents/1464


</br>


# Video ID Workflows


## 1-Step validation

In 1-step mode Video ID, a request approval also implies its validation. For this reason, executing the validation step is not required.

</br>

![img](https://raw.githubusercontent.com/UANATACA/ONESHOT-REPO/test2/img/oneshot_1step.png)

</br>

You can follow the next example using the developers One-Shot Optimizer configured for test environment in https://one-shot.developers.uanataca.com, or you can find the instructions to set up your One-Shot Opimizer in the <a href="#section/Configuration"> configuration section</a>.

</br>

*This process involves the following steps:*


**1) CREATE A NEW VIDEO ID SIGNATURE REQUEST**

**2) REQUEST APPROVAL**

**3) UPLOAD A DOCUMENT**

**4) RETRIEVE SERVICE CONTRACT**

**5) GENERATE AN OTP (only for Uanataca SMS)**

**6) SIGN THE DOCUMENT**

**7) RETRIEVE SIGNED DOCUMENT**

**8) DELETE DOCUMENTS FROM OPTIMIZER**

</br>

> **STEP 1: CREATE A NEW VIDEO ID SIGNATURE REQUEST**

</br>

**API Reference:** <a href="#tag/Video-ID/paths/~1api~1v1~1videoid/post">Create Video ID Request</a>

This call must include preliminary information to identify the signer. 


	curl -i -X POST https://one-shot.developers.uanataca.com/api/v1/videoid \
		-H 'Content-Type: application/json' \
		-d '{
			"mobile_phone_number": "+34699999999",
			"email": "mail@domain",
			"registration_authority": "1234",
			"profile": "PFnubeQAFCiudadano",
			"videoid_mode": 1,
			"webhook_url": "my-webhook-url.com"
		}'

If the signature request is completed successfully, both video and request unique identifiers are returned, as well as the video id link assigned to it:

	{
		"status": "200 OK",
		"details": {
			"videoid_pk": 150,
			"videoid_link": "https://cms.access.bit4id.org:13035/lcmpl/videoid/ZWxlY3Ryb25pY2lkOkl3YlBNdTktcXpBTU1yd0ROeUR0VWNRNk02bVVmVV9SQnZqYnFOR0Vhc2(...)",
			"request_pk": 45836
		}
	}

The request starts at **VIDEOPENDING** status after creation. The request_pk output parameter will be used to identify this digital signature request in subsequent calls. 

</br>

> At this point, the workflow progress will depend on the video-identification successful completion. This action will change request status from **VIDEOPENDING** to **VIDEOREVIEW**. 

<blockquote style="background-color: #faf3ac; border-color: #5a5a5a; color: #3b3b3b;">⚠ In case the process is not totally completed or has failed for any reason, the request will change to <b>VIDEOINCOMPLETE</b> or <b>VIDEOERROR</b> respectively.</blockquote>

To inform both business app and Video ID RAO about this change at the time it takes place, we recommend to enhance the process with the usage of a listener **Webhook**. A Webhook is an event notification sent to a URL of your choice. Users can configure them to trigger events on one site to invoke behavior on another. Check our documentation for <a href='#section/Configuration/Webhooks'>Webhook Configuration</a>.  

If request data needs to be modified, use the <a href="#tag/Video-ID/paths/~1api~1v1~1request~1{request_pk}/put">Update Request</a> call. Check API Reference.

If request data needs to be retrieved, use the <a href="#tag/Requests/paths/~1api~1v1~1request~1{request_pk}/get">Get Request</a> call. Check API Reference.

</br>

> **STEP 2: REQUEST APPROVAL**

</br>

**API Reference:** <a href="#tag/Video-ID/paths/~1api~1v1~1videoid~1{request_pk}~1validate/post">Approve Request</a>

This call makes the request ready for signature. Its status changes to **ENROLLREADY** and webhook intervention at this point is important for business app status update. In 1-step mode, both validation and approval occur when executing this call.

	curl -i -X POST 'https://api.uanataca.com/api/v1/request/45836/approve' \
		-H 'Content-Type: application/json' \
		-d '{
			"username": "1000279",
			"password": "3DPTm:N4",
			"pin": "23bYQq9a",
			"rao": 123
		}'

The response is a JSON object with added request approval information. 

	{
		"status": "200 OK",
		"details": "Request approved successfully"
	}

In case of not approving a request for any reason, the call <a href="#tag/Requests/paths/~1api~1v1~1requests~1{id}~1cancel/delete">Cancel Request</a> must be executed. Check API Reference.

</br>

> **STEP 3: UPLOAD A DOCUMENT**

</br>

**API Reference:** <a href="#tag/Documents/paths/~1api~1v1~1document~1{pk}/post">Upload Document</a>


After creating the digital signature request, we can associate to it all pdf documents that should be signed by the user.

	curl -F "file=@doc.pdf" -X POST https://one-shot.developers.uanataca.com/api/v1/document/45836

Note that the number at the end of the call is the request id we obtained in the previous step.

If the upload is successful, the response will contain the identifier assigned to each document:

	{
		"status": "200 OK",
		"details": "712c29ac-a2dc-4530-8c67-d0f227d8294b"
	}

</br>

> **STEP 4: RETRIEVE SERVICE CONTRACT**

</br>


**API Reference:** <a href="#tag/Documents/paths/~1api~1v1~1document~1{pk}~1contract/get">Retrieve Contract</a>

As a Trusted Service Provider, Uanataca must inform certificate applicants of the terms and conditions governing the issuance of certificates. 

A service contract is generated in each digital certificate issue. The user must view the service contract to sign later along with the documents to be signed in the signature request, using the one time certificate issued on the spot.

	curl -X GET https://one-shot.developers.uanataca.com/api/v1/document/1464/contract

The response by the server will be the service contract document file in binary format:

	%PDF
	...

</br>

> **STEP 5: GENERATE AN OTP (only for Uanataca SMS)**

</br>

**API Reference:** <a href="#tag/Requests/paths/~1api~1v1~1otp~1{pk}/post">Generate OTP code</a>

<blockquote style="background-color: #faf3ac; border-color: #5a5a5a; color: #3b3b3b;">This step is only used for flows using Uanataca SMS.</blockquote>

Once the documents to be signed are ready, we need to generate a secure One-time password (OTP) that will allow the user to sign them. The OTP is generated by calling the otp endpoint and the resulting OTP is sent as an SMS message directly to the phone number we provided when creating the signature request.

When calling the OTP endpoint, provide the request identifier returned by the request endpoint:

	curl -X POST https://one-shot.developers.uanataca.com/api/v1/otp/45836

A successful call will look like this:

	{
		"status": "200 OK",
		"details": "OTP generated"
	}

With this call, an SMS with the secret code is sent to the mobile phone number associated to the signature request.

</br>

> **STEP 6: SIGN THE DOCUMENT**

</br>

**API Reference:** <a href="#tag/Requests/paths/~1api~1v1~1sign~1{pk}/post">Sign</a>

In this step, the request changes to **ISSUED** status, which means digital signature certificate is created and inmediately signs all previously uploaded documents associated to the signature request.

Call the sign endpoint with the request id and json parameters containing the OTP:

	curl -d @params.json -H "Content-Type: application/json -X POST https://one-shot.developers.uanataca.com/api/v1/sign/45836

params.json for Uanataca SMS:

	{
		"secret": "123456"
	}

params.json for other authentication methods:

	{
		"secret": "123456"
		"ext_unlock_type": "biometric"
		"ext_unlock_value": "12345678-12345678"
	}

</br>

A successful call will result in the following response:

	{
		"status": "200 OK",
		"details": "Documents correctly signed"
	}

</br>

> **STEP 7: RETRIEVE SIGNED DOCUMENT**

</br>

**API reference:** <a href="#tag/Documents/paths/~1api~1v1~1document~1{pk}~1{type}~1{uid}/get">Retrieve document</a>

Once the signature is done, the next step is getting the signed documents.

To do this, query with an HTTP GET request the endpoint /api/v1/document/{pk}/{type}/{uid}, where {pk} is the Request's unique identifier, {type} is the type of the document (it can be "original" for the uploaded document or "signed" for the digitally-signed version) and {uid} is the document unique identifier.

	curl -X GET https://one-shot.developers.uanataca.com/api/v1/document/45836/signed/712c29ac-a2dc-4530-8c67-d0f227d8294b

The response by the server will be the document in binary format:

	%PDF
	...

</br>

> **STEP 8: DELETE DOCUMENTS FROM OPTIMIZER**

</br>

**API Reference:** <a href="#tag/Documents/paths/~1api~1v1~1documents~1{pk}/delete">Delete All Documents</a>

<blockquote style="background-color: #faf3ac; border-color: #5a5a5a; color: #3b3b3b;">⚠ While the service will not delete uploaded files unless explicitly requested through an API call, it is <strong>strongly recommended</strong> that you backup any files that you want to preserve using an alternative system.</blockquote>

Delete all documents associated to an ended digital signature request.

	curl -X DELETE https://one-shot.developers.uanataca.com/api/v1/documents/45836

</br>


## 2-Step validation

In 2-Step mode Video ID, request validations and approvals are performed in different stages, by the same or different operators. 

</br>

![img](https://raw.githubusercontent.com/UANATACA/ONESHOT-REPO/test2/img/oneshot_2step.png)

</br>

You can follow the next example using the developers One-Shot Optimizer configured for test environment in https://one-shot.developers.uanataca.com, or you can find the instructions to set up your One-Shot Opimizer in the <a href="#section/Configuration"> configuration section</a>.

</br>

*This process involves the following steps:*


**1) CREATE A NEW VIDEO ID SIGNATURE REQUEST**

**2) REQUEST VALIDATION**

**3) REQUEST APPROVAL**

**4) UPLOAD A DOCUMENT**

**5) RETRIEVE SERVICE CONTRACT**

**6) GENERATE AN OTP (only for Uanataca SMS)**

**7) SIGN THE DOCUMENT**

**8) RETRIEVE SIGNED DOCUMENT**

**9) DELETE DOCUMENTS FROM OPTIMIZER**

</br>

> **STEP 1: CREATE A NEW VIDEO ID SIGNATURE REQUEST**

</br>

**API Reference:** <a href="#tag/Video-ID/paths/~1api~1v1~1videoid/post">Create Video ID Request</a>

This call must include preliminary information to identify the signer. 


	curl -i -X POST https://one-shot.developers.uanataca.com/api/v1/videoid \
		-H 'Content-Type: application/json' \
		-d '{
			"mobile_phone_number": "+34699999999",
			"email": "mail@domain",
			"registration_authority": "1234",
			"profile": "PFnubeQAFCiudadano",
			"videoid_mode": 1,
			"webhook_url": "my-webhook-url.com"
		}'

If the signature request is completed successfully, we will get the unique identifier assigned to it:

	{
		"status": "200 OK",
		"details": {
			"videoid_pk": 150,
			"videoid_link": "",
			"request_pk": 45836
		}
	}


The request starts at **VIDEOPENDING** status after creation. The request_pk output parameter will be used to identify this digital signature request in subsequent calls. 

</br>

> At this point, the workflow progress will depend on the video-identification successful completion. This action will change request status from **VIDEOPENDING** to **VIDEOREVIEW**. 

<blockquote style="background-color: #faf3ac; border-color: #5a5a5a; color: #3b3b3b;">⚠ In case the process is not totally completed or has failed for any reason, the request will change to <b>VIDEOINCOMPLETE</b> or <b>VIDEOERROR</b> respectively.</blockquote>

To inform both business app and Video ID RAO about this change at the time it takes place, we recommend to enhance the process with the usage of a listener **Webhook**. A Webhook is an event notification sent to a URL of your choice. Users can configure them to trigger events on one site to invoke behavior on another. Check our documentation for <a href='#section/Configuration/Webhooks'>Webhook Configuration</a>.  

If request data needs to be modified, use the <a href="#tag/Video-ID/paths/~1api~1v1~1request~1{request_pk}/put">Update Request</a> call. Check API Reference.

If request data needs to be retrieved, use the <a href="#tag/Requests/paths/~1api~1v1~1request~1{request_pk}/get">Get Request</a> call. Check API Reference.

</br>


> **STEP 2: REQUEST VALIDATION** `2-step mode only`

</br>

**API Reference:** <a href="#tag/Video-ID/paths/~1api~1v1~1videoid~1{request_pk}~1validate/post">Validate Request</a>

A Registration Authority Officer must validate the request data and evidences before approval. This call is used only for 2-step mode.  

	curl -i -X POST https://api.uanataca.com/api/v1/videoid/45836/validate \
		-H 'Content-Type: application/json' \
		-d '{
			"username": "5012345",
			"password": "Gy6F37xK",
			"pin": "belorado74",
			"rao": "1400"
		}'

The validation successful response changes the request to **CREATED** status as a JSON object containing full request information is returned.

	{
		"status": "200 OK",
		"details": "Videoid validated"
	}

This call makes the request ready for approval. Its status changes to **CREATED** and webhook intervention at this point is important for business app status update.

For unsuccessful validations leading to a request refusal, the corresponding call is  <a href="#tag/Video-ID/paths/~1api~1v1~1videoid~1{request_pk}~1refuse/post">Refuse Request</a>. Check API Reference.

</br>

> **STEP 3: REQUEST APPROVAL**

</br>

**API Reference:** <a href="#tag/Video-ID/paths/~1api~1v1~1videoid~1{request_pk}~1validate/post">Approve Request</a>

This call makes the request ready for signature. Its status changes to **ENROLLREADY**.

	curl -i -X POST 'https://api.uanataca.com/api/v1/request/45836/approve' \
		-H 'Content-Type: application/json' \
		-d '{
			"username": "1000279",
			"password": "3DPTm:N4",
			"pin": "23bYQq9a",
			"rao": 123
		}'

The response is a JSON object with added request approval information. 

	{
		"status": "200 OK",
		"details": "Request approved successfully"
	}

In case of not approving a request for any reason, the call <a href="#tag/Requests/paths/~1api~1v1~1requests~1{id}~1cancel/delete">Cancel Request</a> must be executed. Check API Reference.

</br>

> **STEP 4: UPLOAD A DOCUMENT**

</br>

**API Reference:** <a href="#tag/Documents/paths/~1api~1v1~1document~1{pk}/post">Upload Document</a>


After creating the digital signature request, we can associate to it all pdf documents that should be signed by the user.

	curl -F "file=@doc.pdf" -X POST https://one-shot.developers.uanataca.com/api/v1/document/45836

Note that the number at the end of the call is the request id we obtained in the previous step.

If the upload is successful, the response will contain the identifier assigned to each document:

	{
		"status": "200 OK",
		"details": "712c29ac-a2dc-4530-8c67-d0f227d8294b"
	}

</br>

> **STEP 5: RETRIEVE SERVICE CONTRACT**

</br>


**API Reference:** <a href="#tag/Documents/paths/~1api~1v1~1document~1{pk}~1contract/get">Retrieve Contract</a>

As a Trusted Service Provider, Uanataca must inform certificate applicants of the terms and conditions governing the issuance of certificates. 

A service contract is generated in each digital certificate issue. The user must view the service contract to sign later along with the documents to be signed in the signature request, using the one time certificate issued on the spot.

	curl -X GET https://one-shot.developers.uanataca.com/api/v1/document/1464/contract

The response by the server will be the service contract document file in binary format:

	%PDF
	...

</br>

> **STEP 6: GENERATE AN OTP**

</br>

**API Reference:** <a href="#tag/Requests/paths/~1api~1v1~1otp~1{pk}/post">Generate OTP code</a>

<blockquote style="background-color: #faf3ac; border-color: #5a5a5a; color: #3b3b3b;">This step is only used for flows using Uanataca SMS.</blockquote>

Once the documents to be signed are ready, we need to generate a secure One-time password (OTP) that will allow the user to sign them. The OTP is generated by calling the otp endpoint and the resulting OTP is sent as an SMS message directly to the phone number we provided when creating the signature request.

When calling the OTP endpoint, provide the request identifier returned by the request endpoint:

	curl -X POST https://one-shot.developers.uanataca.com/api/v1/otp/45836

A successful call will look like this:

	{
		"status": "200 OK",
		"details": "OTP generated"
	}

With this call, an SMS with the secret code is sent to the mobile phone number associated to the signature request.

</br>

> **STEP 7: SIGN THE DOCUMENT**

</br>

**API Reference:** <a href="#tag/Requests/paths/~1api~1v1~1sign~1{pk}/post">Sign</a>

In this step, the request changes to **ISSUED** status, which means digital signature certificate is created and inmediately signs all previously uploaded documents associated to the signature request.

Call the sign endpoint with the request id and json parameters containing the OTP:

	curl -d @params.json -H "Content-Type: application/json -X POST https://one-shot.developers.uanataca.com/api/v1/sign/45836

params.json for Uanataca SMS:

	{
		"secret": "123456"
	}

params.json for other authentication methods:

	{
		"secret": "123456"
		"ext_unlock_type": "biometric"
		"ext_unlock_value": "12345678-12345678"
	}

</br>

A successful call will result in the following response:

	{
		"status": "200 OK",
		"details": "Documents correctly signed"
	}

</br>

> **STEP 8: RETRIEVE SIGNED DOCUMENT**

</br>

**API reference:** <a href="#tag/Documents/paths/~1api~1v1~1document~1{pk}~1{type}~1{uid}/get">Retrieve document</a>

Once the signature is done, the next step is getting the signed documents.

To do this, query with an HTTP GET request the endpoint /api/v1/document/{pk}/{type}/{uid}, where {pk} is the Request's unique identifier, {type} is the type of the document (it can be "original" for the uploaded document or "signed" for the digitally-signed version) and {uid} is the document unique identifier.

	curl -X GET https://one-shot.developers.uanataca.com/api/v1/document/45836/signed/712c29ac-a2dc-4530-8c67-d0f227d8294b

The response by the server will be the document in binary format:

	%PDF
	...

</br>

> **STEP 9: DELETE DOCUMENTS FROM OPTIMIZER**

</br>

**API Reference:** <a href="#tag/Documents/paths/~1api~1v1~1documents~1{pk}/delete">Delete All Documents</a>

<blockquote style="background-color: #faf3ac; border-color: #5a5a5a; color: #3b3b3b;">⚠ While the service will not delete uploaded files unless explicitly requested through an API call, it is <strong>strongly recommended</strong> that you backup any files that you want to preserve using an alternative system.</blockquote>

Delete all documents associated to an ended digital signature request.

	curl -X DELETE https://one-shot.developers.uanataca.com/api/v1/documents/45836

</br>


## External Mode

In External mode Video ID, digital evidences are uploaded to an independent Video ID platform. External mode Video ID can be executed in 1 or 2 steps.

</br>

![img](https://raw.githubusercontent.com/UANATACA/ONESHOT-REPO/test2/img/oneshot_ext.png)

</br>

You can follow the example using the developers One-Shot Optimizer configured for test environment in https://one-shot.developers.uanataca.com, or you can find the instructions to set up your One-Shot Opimizer in the <a href="#section/Configuration"> configuration section</a>.

</br>

*This process involves the following steps:*


**1) CREATE A NEW VIDEO ID SIGNATURE REQUEST**

**2) UPLOAD DATA & VIDEO**

**3) REQUEST VALIDATION**

**4) REQUEST APPROVAL**

**5) UPLOAD A DOCUMENT**

**6) RETRIEVE SERVICE CONTRACT**

**7) GENERATE AN OTP (only for Uanataca SMS)**

**8) SIGN THE DOCUMENT**

**9) RETRIEVE SIGNED DOCUMENT**

**10) DELETE DOCUMENTS FROM OPTIMIZER**

</br>

> **STEP 1: CREATE A NEW VIDEO ID SIGNATURE REQUEST**

</br>

**API Reference:** <a href="#tag/Video-ID/paths/~1api~1v1~1videoid/post">Create Video ID Request</a>

This call must include preliminary information to identify the signer. 

	curl -i -X POST https://one-shot.developers.uanataca.com/api/v1/videoid \
		-H 'Content-Type: application/json' \
		-d '{
			"mobile_phone_number": "+34699999999",
			"email": "mail@domain",
			"registration_authority": "1234",
			"profile": "PFnubeQAFCiudadano",
			"videoid_mode": 1
		}'

If the signature request is completed successfully, we will get the unique identifier assigned to it:

	{
		"status": "200 OK",
		"details": {
			"videoid_pk": 150,
			"videoid_link": "",
			"request_pk": 45836
		}
	}

The response is the a JSON containing important request parameters, in **VIDEOPENDING** status after creation. The request_pk output parameter will be used to identify this digital signature request in subsequent calls.

If request data needs to be modified, use the <a href="#tag/Video-ID/paths/~1api~1v1~1request~1{request_pk}/put">Update Request</a> call. Check API Reference.

If request data needs to be retrieved, use the <a href="#tag/Requests/paths/~1api~1v1~1request~1{request_pk}/get">Get Request</a> call. Check API Reference.

</br>

> **STEP 2: UPLOAD DATA & VIDEO**

</br>

A previously created Video ID Request needs a set of information defined as evidences. 

- The successful upload of **ALL** information will change the request status to **VIDEOREVIEW**. 
- The partial upload of the information will change the request status to **VIDEOINCOMPLETE**.
- If the upload process fails for any reason, the request status will change to **VIDEOERROR**. 

Data and images are uploaded by using the Upload Data Evidence call.

</br>

**API Reference:** <a href="#tag/Video-ID/paths/~1api~1v1~1videoid~1{request_pk}~1evidences/post">Upload Data Evidence</a>

<blockquote style="background-color: #faf3ac; border-color: #5a5a5a; color: #3b3b3b;">⚠ For this call the endpoint must be used is <b>lima.demo.bit4id.org</b> instead of <b>api.uanataca.com</b></blockquote>

Data objects in detail:

`acceptance` : Client acceptance parameters (e.g. Terms & Conditions,  Privacy Policy). This is a customizable JSON object. </br>
`videoid_data` : Set of information about the Request. Contains: </br>
- `images`: Pictures associated to the client's ID document plus a face selfie of him/her. </br>
- `ocr_data` : Text information extracted from the client's ID document via Optical Character Recognition (OCR). </br>
- `security_checks` : Set of validation fields associated to the client's identity (underaging, matching info, liveliness, etc) </br>
- `similarity_level` : Similarity level between document picture and face selfie. Ranges within 0 to 100] 


	curl -i -X POST https://lima.demo.bit4id.org/api/v1/videoid/45836/evidences \
		-H 'Content-Type: application/json' \
		-d '{
			"acceptance": {
				"description": "User Accepted Terms and Conditions and Privacy Policy",
				"url-doc-privacypolicy": "https://www.uanataca.com/public/pki/privacidad-PSC/",
				"ip": "186.0.91.53",
				"url-web-videoid": "https://cms.access.bit4id.org:13035/lcmpl/videoid/46b92251-4ba8-4930-a5aa-8631ec4666b6",
				"user-agent": "Mozilla/5.0 (Linux; Android 11; AC2003)",
				"date": 1622823879708,
				"url-doc-termsconditions": "https://www.uanataca.com/public/pki/terminos-VID/"
			},
			"videoid_data": {
				"images": {
					"document_front": "/9j/4AAQSkZJRgABAQAAAQABAAD/2wBDAAIBAQEBAQIBAQECAgICAgQDAgICAgUEBAM (...)",
					"document_rear": "/I7ye60+aOKS0mVGVSD9RVfyXukjmnS3cAEbpMVm6M1ncWqS3FszptO1lPRRDJ+orI8b (...)",
					"document_photo": "AkjOOwFfHFrrNlpXxcbU9QuIIIkvR56yddgHpX3GEj1PmanmdS/xV1ySVlv/AIbXLPO (...)",
					"document_owner": "SSVnovgCZ4Lhk+R3lJPUDJr5t/Z/wBV1DWfjRbeI75B5iQytcykc7yMEAV2/iwC0T34 (...)"
				},
				"ocr_data": {
					"given_name": "Name",
					"surname_1": "Surname 1",
					"surname_2": "Surname 2",
					"mobile_phone_number": "+34999999999",
					"email": "mail@domain",
					"serial_number": "A9999999E",
					"id_document_type": "IDC",
					"id_document_country": ES
				},
				"security_checks": {
					"otp_validation": true,
					"documents_match": true,
					"data_integrity": true,
					"document_notcopy": true,
					"document_notexpired": true,
					"document_notunderage": true,
					"liveliness": true
				},
				"similarity_level": "high"
			}
		}'


Successful response status

	{
		"status": "200 OK",
		"details": "Videoid evidences uploaded"
	}

</br>

In the same way, binary multiformat Video is uploaded by using the Upload Video call.

</br>

**API Reference:** <a href="#tag/Video-ID/paths/~1api~1v1~1videoid~1{request_pk}~1evidences~1video/post">Upload Video</a>

<blockquote style="background-color: #faf3ac; border-color: #5a5a5a; color: #3b3b3b;">⚠ For this call the endpoint must be used is <b>lima.demo.bit4id.org</b> instead of <b>api.uanataca.com</b></blockquote>

	curl -i -X POST https://lima.demo.bit4id.org/v1/upload/video/30e57b02819a430d8386fd85be9f499f/ \
		-H 'Content-Type: multipart/form-data' \
		-F video=@sample_folder/sample_video.mp4 

Successful response status

	{
		"status": "200 OK",
		"details": "Videoid video upload completed"
	}

If the uploaded video needs to be retrieved, use <a href="#tag/Video-ID/paths/~1api~1v1~1download~1video~1{video_identifier}/get">Download Video</a>


</br>

> **STEP 4: REQUEST VALIDATION** `2-step mode only`

</br>

**API Reference:** <a href="#tag/Video-ID/paths/~1api~1v1~1videoid~1{request_pk}~1validate/post">Validate Request</a>

A Registration Authority Officer must validate the request data and evidences before approval. This call is used only for 2-step mode.  

	curl -i -X POST https://api.uanataca.com/api/v1/videoid/45836/validate \
		-H 'Content-Type: application/json' \
		-d '{
			"username": "5012345",
			"password": "Gy6F37xK",
			"pin": "belorado74",
			"rao": "1400"
		}'

The validation successful response status is a JSON object containing request information, as the request status changes to **CREATED**.

	{
		"status": "200 OK",
		"details": "Videoid validated"
	}

For unsuccessful validations leading to a request refusal, the corresponding call is  <a href="#tag/Video-ID/paths/~1api~1v1~1videoid~1{request_pk}~1refuse/post">Refuse Request</a>. Check API Reference.

</br>

> **STEP 5: REQUEST APPROVAL**

</br>

**API Reference:** <a href="#tag/Video-ID/paths/~1api~1v1~1videoid~1{request_pk}~1validate/post">Approve Request</a>

This call makes the request ready for signature. Its status changes to **ENROLLREADY**. In 1-step mode, both validation and approval occur when executing this call.

	curl -i -X POST 'https://api.uanataca.com/api/v1/request/45836/approve' \
		-H 'Content-Type: application/json' \
		-d '{
			"username": "1000279",
			"password": "3DPTm:N4",
			"pin": "23bYQq9a",
			"rao": 123
		}'

The response is a JSON object with added request approval information. 

	{
		"status": "200 OK",
		"details": "Request approved successfully"
	}

In case of not approving a request for any reason, the call <a href="#tag/Requests/paths/~1api~1v1~1requests~1{id}~1cancel/delete">Cancel Request</a> must be executed. Check API Reference.

</br>

> **STEP 6: UPLOAD A DOCUMENT**

</br>

**API Reference:** <a href="#tag/Documents/paths/~1api~1v1~1document~1{pk}/post">Upload Document</a>


After creating the digital signature request, we can associate to it all pdf documents that should be signed by the user.

	curl -F "file=@doc.pdf" -X POST https://one-shot.developers.uanataca.com/api/v1/document/45836

Note that the number at the end of the call is the request id we obtained in the previous step.

If the upload is successful, the response will contain the identifier assigned to each document:

	{
		"status": "200 OK",
		"details": "712c29ac-a2dc-4530-8c67-d0f227d8294b"
	}

</br>

> **STEP 7: RETRIEVE SERVICE CONTRACT**

</br>


**API Reference:** <a href="#tag/Documents/paths/~1api~1v1~1document~1{pk}~1contract/get">Retrieve Contract</a>

As a Trusted Service Provider, Uanataca must inform certificate applicants of the terms and conditions governing the issuance of certificates. 

A service contract is generated in each digital certificate issue. The user must view the service contract to sign later along with the documents to be signed in the signature request, using the one time certificate issued on the spot.

	curl -X GET https://one-shot.developers.uanataca.com/api/v1/document/1464/contract

The response by the server will be the service contract document file in binary format:

	%PDF
	...

</br>

> **STEP 8: GENERATE AN OTP**

</br>

**API Reference:** <a href="#tag/Requests/paths/~1api~1v1~1otp~1{pk}/post">Generate OTP code</a>

<blockquote style="background-color: #faf3ac; border-color: #5a5a5a; color: #3b3b3b;">This step is only used for flows using Uanataca SMS.</blockquote>

Once the documents to be signed are ready, we need to generate a secure One-time password (OTP) that will allow the user to sign them. The OTP is generated by calling the otp endpoint and the resulting OTP is sent as an SMS message directly to the phone number we provided when creating the signature request.

When calling the OTP endpoint, provide the request identifier returned by the request endpoint:

	curl -X POST https://one-shot.developers.uanataca.com/api/v1/otp/45836

A successful call will look like this:

	{
		"status": "200 OK",
		"details": "OTP generated"
	}

With this call, an SMS with the secret code is sent to the mobile phone number associated to the signature request.

</br>

> **STEP 9: SIGN THE DOCUMENT**

</br>

**API Reference:** <a href="#tag/Requests/paths/~1api~1v1~1sign~1{pk}/post">Sign</a>

In this step, the request changes to **ISSUED** status, which means digital signature certificate is created and inmediately signs all previously uploaded documents associated to the signature request.

Call the sign endpoint with the request id and json parameters containing the OTP:

	curl -d @params.json -H "Content-Type: application/json -X POST https://one-shot.developers.uanataca.com/api/v1/sign/45836

params.json for Uanataca SMS:

	{
		"secret": "123456"
	}

params.json for other authentication methods:

	{
		"secret": "123456"
		"ext_unlock_type": "biometric"
		"ext_unlock_value": "12345678-12345678"
	}

</br>

A successful call will result in the following response:

	{
		"status": "200 OK",
		"details": "Documents correctly signed"
	}

</br>

> **STEP 10: RETRIEVE SIGNED DOCUMENT**

</br>

**API reference:** <a href="#tag/Documents/paths/~1api~1v1~1document~1{pk}~1{type}~1{uid}/get">Retrieve document</a>

Once the signature is done, the next step is getting the signed documents.

To do this, query with an HTTP GET request the endpoint /api/v1/document/{pk}/{type}/{uid}, where {pk} is the Request's unique identifier, {type} is the type of the document (it can be "original" for the uploaded document or "signed" for the digitally-signed version) and {uid} is the document unique identifier.

	curl -X GET https://one-shot.developers.uanataca.com/api/v1/document/45836/signed/712c29ac-a2dc-4530-8c67-d0f227d8294b

The response by the server will be the document in binary format:

	%PDF
	...

</br>

> **STEP 11: DELETE DOCUMENTS FROM OPTIMIZER**

</br>

**API Reference:** <a href="#tag/Documents/paths/~1api~1v1~1documents~1{pk}/delete">Delete All Documents</a>

<blockquote style="background-color: #faf3ac; border-color: #5a5a5a; color: #3b3b3b;">⚠ While the service will not delete uploaded files unless explicitly requested through an API call, it is <strong>strongly recommended</strong> that you backup any files that you want to preserve using an alternative system.</blockquote>

Delete all documents associated to an ended digital signature request.

	curl -X DELETE https://one-shot.developers.uanataca.com/api/v1/documents/45836

</br>


# Configuration

One-Shot Optimizer can be supplied as a **Docker** or as a **Virtual Machine** image.
See the configuration description in:<br>
<a href="#section/Configuration/One-Shot-Optimizer-on-Docker"> One-Shot Optimizer on Docker</a><br>
<a href="#section/Configuration/One-Shot-Optimizer-on-Virtual-Machine-(OVA)"> One-Shot Optimizer on Virtual Machine</a>


## Hardware requirements


**CPU:** modern multicore (minimum 4 core)

**RAM:** 8GB

**HDD:** 200 GB


## One-Shot Optimizer on Docker


This configuration requires a server with a Linux CentOS operating system.

<a href="#section/Video-tutorials/Docker-Optimizer-Configuration"><img src="https://raw.githubusercontent.com/UANATACA/ONESHOT-REPO/main/img4.png"></a><a href="#section/Video-tutorials/Docker-Optimizer-Configuration"><b>&nbsp;Watch on video!</b></a>


> STEP 1: Install Docker and Docker-Compose.

*Docker*

Run the following commands in this order.

	sudo yum update -y
	yum install -y yum-utils device-mapper-persistent-data lvm2
	yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
	sudo yum install -y docker-ce docker-ce-cli containerd.io
	sudo systemctl start docker



*Docker-Compose*

Run the following commands in this order.


	sudo curl -L "https://github.com/docker/compose/releases/download/1.28.5/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
	chmod +x /usr/local/bin/docker-compose


Run command **docker-compose version** to check the installation. The outcome should show this information:


![img](https://github.com/UANATACA/ONESHOT-REPO/raw/main/img/oneshot-docker1.png)

</br>

> STEP 2: Extract and copy One-Shot Optimizer zip content to the server.

Extract all `oneshot_optimizer_docker.zip` content in a local folder.

Move One-Shot Optimizer folder to the path **/opt** in the server.

The outcome should look like this:

![img](https://github.com/UANATACA/ONESHOT-REPO/raw/main/img/oneshot-docker2.png)


</br>

> STEP 3: Mapping volumes (for environments with a pool of One-Shot Optimizer).

In high performance environments with a pool of One-Shot Optimizer, service settings, data and logs must be stored in a shared volume outside Optimizer servers. These volumes must be defined in `docker-compose.yml` file in each One-Shot Optimizer.

	cd /opt/oneshot_optimizer

Docker-compose.yml settings file:

![img](https://github.com/UANATACA/ONESHOT-REPO/raw/test3/img/oneshot-docker3.png)

</br>

> STEP 4: Load One-Shot Docker images.

Run the following commands:

	cd /opt/oneshot_optimizer
	docker image load -i oneshot_optimizer.tar
	docker image load -i oneshot_imgconverter.tar

Remove image files:

	rm -rf /opt/oneshot_optimizer/oneshot_optimizer.tar
	rm -rf /opt/oneshot_optimizer/oneshot_imgconverter.tar

</br>

> STEP 5: Launch the service.

Run the following commands:

	cd /opt/oneshot_optimizer
	docker-compose up -d

Check service status:

	docker-compose ps

					Name              			  Command               State                Ports
	--------------------------------------------------------------------------------------------------------------------------------------------------
	oneshot_optimizer_imgconverter_1   /opt/bit4id/imgconverter/b ...   Up      0.0.0.0:49153->5013/tcp,:::49153->5013/tcp
	oneshot_optimizer_nginx_1          /docker-entrypoint.sh ngin ...   Up      0.0.0.0:443->443/tcp,:::443->443/tcp, 0.0.0.0:80->80/tcp,:::80->80/tcp
	oneshot_optimizer_optimizer_1      oneshot_optimizer start          Up

All services must be UP.

</br>

> STEP 6: Service settings.

See <a href="#section/Configuration/Service-settings">service settings</a> configuration section.

</br>


## One-Shot Optimizer on Virtual Machine

<div style="text-align: justify">
The Virtual Machine is supplied in an OVA file. One-Shot Optimizer image is compatible with common virtual environments like VMWare, AWS, Azure or VirtualBox.
</div>
</br>

> STEP 1: Import One-Shot Optimizer (VM) in the virtual environment.

<div style="text-align: justify">
Adjust the system requirements for optimal usage considering host terminal resources described in <a href="#section/Configuration/Hardware-requirements"> hardware requirements</a>.
</div>

</br>

> STEP 2: Network configuration.

The network settings are configured on the file `ifcfg-ens33`, which can be found in the path **/etc/sysconfig/network-scripts**. Edit the file and insert the correct IP address, network mask, gateway and DNS for your network.

Then restart network services with command **service network restart**.

Example:

![img](https://github.com/UANATACA/ONESHOT-REPO/raw/main/img/oneshot-docker4.png)

</br>

> STEP 3: Service settings.

See <a href="#section/Configuration/Service-settings">service settings</a> configuration section.

</br>


## Service settings

<div style="text-align: justify">
One-Shot Signature can be configured to use in test or production environment.
</div>
<br></br>


> TEST environment

**Requirements:**

- Billing credentials for Uanataca test environment.

The `settings.ini` file contains default parameters that can also be adjusted via API using <a href='#tag/Settings/paths/~1api~1v1~1settings/post'>Update Settings</a> call.

Except by `tsa_url`, all parameters shown below are replaced every time the <a href='#tag/Video-ID/paths/~1api~1v1~1videoid/post'>Create Video ID Request</a> call is executed. 

settings.ini file location:

**/opt/oneshot_optimizer/common/etc/settings.ini** or custom mapped volume (Docker)

**/opt/bit4id/oneshot_optimizer/etc/settings.ini** (Virtual Machine)

The following is a view of the settings.ini file. 

<blockquote style="background-color: #faf3ac; border-color: #5a5a5a; color: #3b3b3b;">⚠ If billing password is specified, it must be previously converted into Base64 format.</blockquote>

Run the following command to convert your password to base64:

	echo -n <<billing_password>> | base64

</br>

settings.ini file:

	1 |	[general]
	2 |	environment = test
	3 |
	4 |	[tsa]
	5 |	tsa_url = https://tsa.access.bit4id.org:13035/tsa/test01
	6 |
	7 |	[billing]
	8 |	billing_username = user@uanataca.com
	9 |	billing_password = ejVxTnFrZkI=
	10|
	11|	[request]
	12|	default_profile = PFnubeQAFCiudadano
	13|	default_ra = 1000

Once you are done editing the file, restart the One-Shot Optimizer service to changes take effect.

Virtual Machine:

	systemctl restart optimizer

Docker:

	cd /opt/oneshot_optimizer
	docker-compose restart optimizer


<br></br>

> PRODUCTION environment

**Requirements:**

- Billing credentials for Uanataca production environment.
- Certificate (.cer) and key (.key) files to connect to the Uanataca production environment.
- The Id number for the Registration Authority that will issue the certificates.

<blockquote style="background-color: #faf3ac; border-color: #5a5a5a; color: #3b3b3b;">⚠ We recommend starting from a clean copy of One-Shot Optimizer for this step. This prevents leftover files from the test environment from causing errors in production.</blockquote>

You should have received a certificate (.cer) and key (.key) file to be used to identify your application in communications with the production signature service. Place both files in the certificates folder.

settings.ini file location:

**/opt/oneshot_optimizer/common/etc/certs/prod** or custom mapped volume (Docker)

**/opt/bit4id/oneshot_optimizer/etc/certs/prod** (Virtual Machine)

![img](https://github.com/UANATACA/ONESHOT-REPO/raw/test3/img/oneshot-docker5.png)

The following is a view of the settings.ini file. **Important: If billing password is specified, it must be previously converted into Base64 format.** Run the following command to convert your password to base64:

	echo -n <<billing_password>> | base64

</br>

settings.ini file:

	1 |	[general]
	2 |	environment = prod
	3 |
	4 |	[tsa]
	5 |	tsa_url = https://tsa.uanataca.com/tsa/tss03
	6 |
	7 |	[billing]
	8 |	billing_username = user@uanataca.com
	9 |	billing_password = ejVxTnFrZkI=
	10|
	11|	[request]
	12|	default_profile = PFnubeQAFCiudadano
	13|	default_ra = 1000


The file `settings.ini` contains default parameters that can also be adjusted via API using <a href='#tag/Tokens/paths/~1api~1v1~1token/post'>Update Settings</a> call.

Except by `tsa_url`, all parameters shown below are replaced every time the <a href='#tag/Video-ID/paths/~1api~1v1~1videoid/post'>Create Video ID Request</a> call is executed.  

Once you are done editing the file, restart the One-Shot Optimizer service to changes take effect.

Virtual Machine:

	systemctl restart optimizer

Docker:

	cd /opt/oneshot_optimizer
	docker-compose restart optimizer

<br></br>


# Webhook Configuration


One-Shot API requires a Webhook implemented on customer business side to manage our service callbacks. Every request status change will trigger a simple event-notification via HTTP POST, consisting on a JSON object to an URL that must be explicitly included as a **required parameter** in the <a href='#tag/Video-ID/paths/~1api~1v1~1videoid/post'>Create Video ID Request</a> call when using Uanataca 1-step or 2-step mode. 

The following is a sample view of the JSON object that is sent as a callback at every status change:

	{
		"status": "VIDEOINCOMPLETE", 
		"date": "2021-07-20T08:08:21.132394", 
		"previous_status": "VIDEOPENDING", 
		"request": 46760, 
		"registration_authority": 455
	}

Where:

**status** is the most recent status, this is, the status that triggered the notification.</br>
**date** is the date of the request status change in datetime format.

**previous_status** is the status inmediately previous to last change.

**request** is the request unique id.

**registration_authority** is the Registration Authority id number the request is associated.

</br>

> **Sample code**

In this sample, every JSON object is stored in a file named 'videoid'.

The webhook parameter used in the <a href='#tag/Video-ID/paths/~1api~1v1~1videoid/post'>Create Video ID Request</a> call is defined as:

	{host}/videoid

where {host} is the IP or domain from the server exposing the webhook.

</br>

*Python*

	import web
	import datetime
	
	urls = (
	        '/videoid, 'videoid',
	        )
	
	app = web.application(urls, globals())
	app = app.wsgifunc()
	
	class video:
		def POST(self):
			data = web.data()
			f = open("status.json",'w')
			f.write(data)
			f.close()
			return ''

	if __name__ == "__main__":
	    app.run()


*PHP*

	<?php
	
	//videoid.json

	$post = file_get_contents('php://input',true);
	$file_handle = fopen('/videoid/status.json', 'w');
	fwrite($file_handle, $post);
	fclose($file_handle);
	
	?>


# Logs

Service logs file `optimizer.log` is stored in a local folder in One-Shot Optimizer.

Docker path:

**/opt/oneshot_optimizer/common/logs** or custom mapped volume

Virtual Machine path:

**/opt/bit4id/oneshot_optimizer/logs**



# Postman collection

A postman collection is available as a support for a quick start.<br>
It is only required to edit `host`variable in Postman environment with the IP or domain of One-Shot Optimizer.

<a href="https://cdn.bit4id.com/es/uanataca/public/oneshot/Uanataca_One-Shot_Postman.zip">One-Shot Postman collection download</a>

<div id="APIReference" style="padding-top: 60px;"><h1>API Reference<h1></div>

# Video tutorials

Need a better understanding of One-Shot service? Check the video tutorials below and follow step-by-step instructions! They will guarantee you use our API efficiently for the best experience in document signatures.

## Docker Optimizer Configuration

The Docker Optimizer configuration requires an One-Shot Optimizer image package and a server using Linux Operating System with Internet access. Check our <a href="#section/Configuration/One-Shot-Optimizer-on-Docker">documentation</a>.<br></br>

<figure class="video_container">
  <iframe width="560" margin-left="500px" height="315" src="https://www.youtube.com/embed/amGfQeMZQBA" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</figure>

## Signature Workflow (SMS)

This tutorial covers the step-by-step <a href="https://oneshot-test.redoc.ly/#section/Workflow">workflow</a> for document signature using Uanataca's One-Shot service via SMS.<br></br>

<figure class="video_container">
	<iframe width="560" height="315" src="https://www.youtube.com/embed/-XxjUfgEPRw" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</figure>

## Signature Workflow (External Authentication)

This tutorial covers the step-by-step <a href="https://oneshot-test.redoc.ly/#section/Workflow">workflow</a> for document signature using Uanataca's One-Shot service via Exthernal Authentication. <br></br>

<figure class="video_container">
	<iframe width="560" height="315" src="https://www.youtube.com/embed/5w0sdhJ3y7I" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</figure>

## Signature Image Configuration

Using images in signatures is an optional feature. Learn how to set signature image parameters in our One-Shot API <a href="https://oneshot-test.redoc.ly/#tag/Requests/paths/~1api~1v1~1sign~1{pk}/post">sign</a> call.<br></br> 

<figure class="video_container">
	<iframe width="560" height="315" src="https://www.youtube.com/embed/YTDqwhBQZ5c" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</figure>
