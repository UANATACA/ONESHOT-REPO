# Introduction

## What is One-Shot Signature

One-Shot Signature is a complete solution for the digital signature of documents within your application. It is designed so that no sensitive data has to be sent away from your premises, as only hashes of the documents to be signed need to be transmitted to the signature service.

Documents are signed through the creation of **single-use** signature certificates, which are created when needed and immediately used to electronically sign all documents included in a given transaction. The digital signature will include a time stamp, proving the existence and integrity of the documents at the time of signature.

The signature procedure is activated with a code sent directly to the signatary by SMS, allowing the end user to complete the signature without requiring the installation of dedicated software. 
</br>
![img](https://i.ibb.co/JBvGZGG/oneshot-wkf.png)

## Basic digital signature procedure

A valid digital signature requires a certificate, emitted by a trusted a Certification Authority (CA). This certificate is used to establish that the document was signed by a specific entity (in our case, the user) that is known to the CA. Although certificate generation is largely automated within One-Shot Signature, having an idea of what is going on under the hood will help us to better understand its operation.

Within the context of a Public Key Infrastructure (PKI), the entity responsible for registering new digital identities is called a Registration Authority (RA). RA employ Registration Authority Officers (RAO) to add new user identities to the infrastructure and request the creation of new digital signature certificates for its users. Each of these certificates can then be used to digitally sign documents, such as contracts.

In the case of One-Shot Signature, certificates are generated on the spot every time a new set of documents requires a signature. Through the One-Shot Signature API, you will play the role of a RAO, providing identifying data for each user and requesting the generation of signature certificates. Once user registration data has been provided and the certificate is ready to be generated, the end user will receive a One-Time Password (OTP) through an SMS message, which can be used to initiate the generation of the certificate and complete the signature procedure. After a successful signature, you will be able to retrieve the signed documents.

## Step-by-step One-Shot Signature operation

The One-Shot Signature workflow involves the following components:

**One-Shot Signature Service:** Uanataca's digital signature service, responsible for the actual digital signature.

**One-Shot Optimizer:** software component of the digital signature solution, it runs on a system under your control and takes care of the manipulation of the documents to be signed, so that no sensitive documents need to leave your network (only hashes of the documents need to be sent to the One-Shot Signature Service).

The One-Shot Optimizer exposes a RESTful HTTP API that allows any application capable of performing HTTP requests to interact with the signature service.

**Client Business Application:** an application that provides a business-specific service and requires digital signature as part of its operation.

**OTP:** a One-Time Password token used to simplify the digital signature procedure.


A common workflow involving the One-Shot Signature Service can be summarized by the following image:
<br></br>
![img](https://i.ibb.co/n1PHDmR/oneshot.jpg)
</br>
1. The client application requests the creation and approval of a new digital signature certificate, providing all required data through API calls.
2. The One-Shot Optimizer API returns an identifier for the certificate request.
3. The client application provides One-Shot Optimizer the document to be signed by the end user.
4. The client application presents the document to be signed to its end user.
5. After reviewing the document, the end user agrees to sign it.
6. The client application starts the signature process by requesting the generation of a One-Time Password (OTP) token for the signature.
7. The One-Shot Signature service sends the OTP directly to the end user through an SMS message.
8. By introducing the OTP, the end user identifies himself as the subject of the signature certificate.
9. The client application provides the OTP and the identifier of the signature request to the One-Shot Optimizer.
10. The One-Shot Optimizer takes care of computing the hash of the document to be signed...
11. and sends them together with the request identifier and OTP to the One-Shot Signature service.
12. The One-Shot Signature service generates the signature certificate for the end user and uses it to sign the hash.
13. The signed hashes and the signature identifier are returned to the One-Shot Optimizer...
14. who takes care of generating the signed document envelope, combining the documents with the signed hashes.
15. Finally, the client application calls the One-Shot Optimizer API to obtain the signed document.

# Configuration

**One-Shot Optimizer configuration**

The local component of One-Shot Signature is the One-Shot Optimizer, which is provided pre-installed in a virtual machine. This page provides a guide to the configuration of the One-Shot Optimizer service and its integration within your network.

**Before you begin**

This document assumes that you received an up-to-date image of the One-Shot Optimizer virtual machine and a billing account with its associate username and password.

To begin setting up One-Shot Optimizer, start the virtual machine and log into it using the provided credentials.

**Network configuration**

The network configuration is controlled by the file ifcfg-ens33, which can be found in the path /etc/sysconfig/network-scripts. Open that file and introduce the correct IP address, network mask, gateway and DNS for your network.

Once you are done, restart the network service with the command service network restart.

**Configuration of the One-Shot Optimizer service for testing**

**Requirements:**

- An up-to-date One-Shot Optimizer Virtual Machine image.
- Billing credentials for the Uanataca test environment.

The machine you received is already configured to use the Uanataca test environment. To start testing One-Shot Signature, it is enough to configure it to use your test environment billing account.

To do so, open the file custom.ini in /opt/bit4id/oneshot_optimizer/etc and introduce the provided username and password under the [BILLING] section.

	1 | [BILLING]
	2 | USERNAME = my-test-account@uanataca.com
	3 | PASSWORD = my-test-password

Once you are done modifying the file, restart the One-Shot Optimizer service with **systemctl restart optimizer** so that changes take effect.

**Configuration of the One-Shot Optimizer service for production**

**Requirements:**

- An up-to-date One-Shot Optimizer Virtual Machine image.
- Billing credentials for the Uanataca production environment.
- Certificate (.cer) and key (.key) files to connect to the Uanataca production environment.
- The Id number for the Registration Authority that will produce the certificates.

Once you are ready to deploy One-Shot Optimizer in a production environment, you will need to configure it to use the production credentials.

<blockquote style="background-color: #faf3ac; border-color: #5a5a5a; color: #3b3b3b;">⚠ We recommend starting from a clean copy of the One-Shot Optimizer virtual machine for this step. This prevents leftover files from the test environment from causing errors in production.</blockquote>

You should have received a certificate (.cer) and key (.key) file to be used to identify your application in communications with the production signature service. Place both in the /opt/bit4id/oneshot_optimizer/etc/certs/ folder, replacing the certificates for the test environment present in the virtual machine.

Open the custom.ini file in /opt/bit4id/oneshot_optimizer/etc and configure it to interact with the production environment. The parts that you will likely need to change are listed below:

	1 | [ENV]
	2 | ENVIRONMENT = prod
	3 |
	4 | [TSA]
	5 | URL      = https://tsa.uanataca.com/tsa/tss03
	6 |
	7 | [BILLING]
	8 | USERNAME = example@uanataca.com
	9 | PASSWORD = my-password
	10|
	11| [RA]
	12| NO_REQUIRED_DOCUMENTS          = true
	13|
	14| DEFAULT_RA                     = 999
	15| DEFAULT_PROFILE                = PFnubeAFCiudadano

Under [ENV], set 'ENVIRONMENT = prod'.

Under the [BILLING] section, introduce the production billing credentials.

Under the [RA] section, make sure that the DEFAULT\_RA id matches your Registration Authority's identifer and the DEFAULT_PROFILE is set to the correct profile. Typically, this will be either PFnubeQAFCiudadano for qualified certificates or PFnubeNC for non-qualified certificates.

You should also make sure that the NO\_REQUIRED_DOCUMENTS is correctly set to true or false depending on whether your RA is set to require (false) or not (true) uploads of the user's documents to generate a certificate.

Once again, you will need to restart the One-Shot Optimizer service with **systemctl restart optimizer** before the changes take effect.

**Additional information**

**Logs**

The logs generated during normal One-Shot Optimizer operation can be found on the path /opt/bit4id/oneshot_optimizer/logs/optimizer.log.

**File permanence**

All files uploaded to One-Shot Optimizer are saved on the /opt/bit4id/oneshot_optimizer/tmp folder of the virtual machine. This includes documents (original and signed) as well as images used as a graphical representation of the signature in the signed pdf. Documents are organized in folders, grouped by request id.

<blockquote style="background-color: #faf3ac; border-color: #5a5a5a; color: #3b3b3b;">⚠ While the service will not delete uploaded files unless explicitly requested through an API call, it is **strongly recommended** that you backup any files that you want to preserve using an alternative system.</blockquote>


# Workflow

## Using One-Shot Signature

This section section presents the workflow for a simple use case of the One-Shot Signature service with a step-by-step description of the API calls required to allow a user to digitally sign a document provided by the client application.

You can follow the example using the One-Shot Optimizer virtual machine configured for the test environment. You can find the instructions to set up the virtual machine in the configuration page.

Before we begin, let's recap the objects and entities involved in the operation:

**Registration Authority Officer (RAO)** is an agent authorized by a Registration Authority to enroll new accounts into a private key infrastructure system. For the One-Shot Signature service, this means the agent in charge of collecting all identifying information for each user and creating new signature requests.

**Token** One-Shot Signature uses tokens to identify the RAO who creates each signature request. While the use of tokens is entirely optional, it is often convenient to create and use a token for a RAO instead of introducing the full credentials every time that they are required.

**Document** A PDF file to be signed.

**One-Time Password (OTP)** a secret code sent by SMS directly to your users so that they can confirm their identity and complete the digital signature.

## Step By Step

The basic digital signature process involves the following steps:

- Retrieve an existing token for the RAO
- Create a new Digital Signature Request
- Upload a document
- Generate an OTP
- Sign the document
- Retrieve the signed document


**Retrieve an existing token for the RAO**

The test One-Shot Optimizer virtual machine is pre-configured with a RAO account ready to be used within the test environment. This account has an associated token, that can be used to identify the RAO in API calls. We can list existing tokens with the /api/v1/tokens API endpoint.

Assuming that https://oneshot.demo.uanataca.com is the URL for the One-Shot Optimizer virtual machine, we can access the endpoint with:

	curl -X GET https://oneshot.demo.uanataca.com/api/v1/tokens

On the clean machine, this should return a single token:

	1 | {
	2 |     "status": "200 OK",
	3 |     "details": {
	4 |         "791ef6ff519b41cfa2f311e6cd144586": {
	5 |             "username": "9900123",
	6 |             "password": true,
	7 |             "pin": true
	8 |         }
	9 |     }
	10| }

This output tells us that a single token "791ef6ff519b41cfa2f311e6cd144586" exists. This token is associated to the RAO account with id "9900123" and can be used in place of the password and pin.

To use tokens in a production environment, you will need to create them first with the corresponding API call.

Tokens can also be generated without providing all credentials. In that case, the missing credentials (pin or password) have to be provided as additional arguments every time the token is used.

**Create a new Digital Signature Request**

Within the One-Shot Signature Service, all data pertaining to a given digital signature is collected within a Digital Signature Request. This includes both the identifying information of the signing user, which is provided when you create the signature request, and the document or documents to be signed, which we will upload later.

We can crete a new signature request with a POST call to the api/v1/request API endpoint. This call must include enough information to identify both the signing user and the RAO initiating the request. The full description of the arguments accepted by this endpoint can be found in the detailed documentation, but for now it is enough to include at least the following:

	1 | curl --location --request POST 'https://oneshot.demo.uanataca.com/api/v1/request' \
	2 |      --form 'token=791ef6ff519b41cfa2f311e6cd144586' \
	3 |      --form 'profile=PFnubeAFCiudadano' \
	4 |      --form 'given_name=name_of_the_user' \
	5 |      --form 'surname_1=surname_of_the_user' \
	6 |      --form 'email=user-example@domain.com' \
	7 |      --form 'mobile_phone_number=+393391234567' \
	8 |      --form 'document_front=@document_front.png' \
	9 |      --form 'document_rear=@document_rear.png' \
	10|      --form 'document_owner=@document_owner.png'

where token is the token representing the RAO credentials obtained in the previous step.

Note that here we are providing images of the user's identification document. These may not be required in production if your Registration Authority allows enrollment without documents.

If the signature request is completed successfully, we will get the unique identifier assigned to it:

	1 | {
	2 |     "status": "201 Created",
	3 |     "details": 1464
	4 | }

The request code will be used to identify this digital signature request in subsequent calls.

**Upload a document**

After creating the digital signature request, we can associate to it all documents that should be signed by the user. For each document, we make a multipart/form-data HTTP POST request with the **PDF** document to upload.

	curl -F "file=@doc.pdf" -X POST https://oneshot.demo.uanataca.com/api/v1/document/1464

Note that the number at the end of the call is the request id we obtained in the previous step.

If the upload is successful, the response will contain the identifier assigned to the document:

	1 | {
	2 |     "status": "200 OK",
	3 |     "details": "712c29ac-a2dc-4530-8c67-d0f227d8294b"
	4 | }

**Generate an OTP**

Once the documents to be signed are ready, we need to generate a secure One time password (OTP) that will allow the user to sign them. The OTP is generated by calling the otp endpoint and the resulting OTP is sent as an SMS message directly to the phone number we provided when creating the signature request.

When calling the OTP endpoint, provide the request identifier returned by the request endpoint:

	curl -X POST https://oneshot.demo.uanataca.com/api/v1/otp/1464

A successful call will look like this:

	1 | {
	2 |     "status": "200 OK",
	3 |     "details": "OTP generated"
	4 | }

With this call, an SMS with the secret is sent to the mobile phone number associated to the signature request.

**Sign the document**

In this step we are going to finalize the creation of the signature certificate and sign all documents previously uploaded for the signature request.

Call the sign endpoint with the request id and json parameters containing the OTP:

params.json:

	1 | {
	2 |     "secret": "000000"
	3 | }

</br>

	curl -d @params.json -H "Content-Type: application/json -X POST https://oneshot.demo.uanataca.com/api/v1/sign/1464

A successful call will result in the following response:

	1 | {
	2 |     "status": "200 OK",
	3 |     "details": "Documents correctly signed"
	4 | }

**Retrieve signed document**

The only remaining step is getting the signed document.

To do this, query with an HTTP GET request the endpoint /api/v1/document/{pk}/{type}/{uid}, where {pk} is the Request's unique identifier, {type} is the type of the document (it can be "original" for the uploaded document or "signed" for the digitally-signed version) and {uid} is the unique id string assigned to the document.

	curl -X GET https://oneshot.demo.uanataca.com/api/v1/document/1464/signed/712c29ac-a2dc-4530-8c67-d0f227d8294b

The response by the server will be the document in binary format:

	1 | %PDF
	2 | ...

# API Reference