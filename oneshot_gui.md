# What it is

<div style="text-align: justify">
One-Shot Signature is a complete solution for the digital signature of documents within your application. It is designed so that no sensitive data has to be sent away from your premises, as only hashes of the documents to be signed need to be transmitted to the signature service.
<br></br>
Documents are signed through the creation of <strong>single-use</strong> digital certificates, which are created when needed and immediately used to electronically sign all documents included in a given transaction. Digital signatures will include a time stamp, proving the existence and integrity of the documents at the time of signature.
</div>

# How it works

<div style="text-align: justify">
A valid digital signature requires a certificate, emitted by a trusted a Certification Authority (CA). This certificate is used to establish that the document was signed by a specific entity (in our case, the user). Although certificate generation is largely automated within One-Shot Signature, having an idea of what is going on under the hood will help us to better understand its operation.
<br></br>
Within the context of a Public Key Infrastructure (PKI), the entity responsible for registering new digital identities is called a Registration Authority (RA). RA employ Registration Authority Officers (RAO) to add new user identities to the infrastructure and request the creation of new digital signature certificates for its users. Each of these certificates can then be used to digitally sign documents.
<br></br>
In the case of One-Shot Signature, certificates are generated on the spot every time a new set of documents requires a signature. Through the One-Shot Signature service, you will play the role of a RAO, providing identifying data for each user and requesting the generation of signature certificates. Once user registration data has been provided and the certificate is ready to be generated, the end user will activate the generation of the digital certificate and complete the signature procedure.
<br></br>
The service is given with One-Shot Optimizer that is a server system exposing http RESTful APIs by means of which, business applications are enabled to require the digital signature. 
<br></br>
One-Shot Optimizer performs the most computationally expensive workload of the signature process, thus reducing the data traffic on the local network and make the most of the cryptographic hardware acceleration. The documnets to be signed are processed in the customer business layer and are not send to Uanataca Services, instead is sent a hash of the document created using a hash algorithm. 
</div>
<br></br>

![img](https://i.ibb.co/JBvGZGG/oneshot-wkf.png)


# Flow chart

<div style="text-align: justify">
In a common One-Shot Signature service, an OTP (One-Time Password) code is sent by sms to the end user directly from Uanataca services. The OTP verification is used to activate the issuing of single-use digital signature certificate and the usage of it to sign the signature request documents.
<br></br>
One-Shot Signature can use other external authentication methods instead of Uanataca SMS. This alternative methods are subjected to approval.
<br></br>
The following images summarize One-Shot Signature flow involving both authentication methods:
</div>
<br></br>

> Uanataca SMS

![img](https://i.ibb.co/MNr9WV9/oneshot-flc1.png)

</br>

1. The client application creates a new digital signature request, providing all required user data
2. One-Shot Optimizer returns an identifier for the certificate request
3. The client application provides the documents to be signed by the end user
4. The client application shows the documents to be signed to its end user
5. After reviewing the document, the end user agrees to sign it
6. The client application starts the signature process by requesting the generation of a One-Time Password (OTP) token for the signature
7. Uanataca services sends the OTP directly to the end user through an SMS message
8. By introducing the OTP, the end user identifies himself as the subject of the signature certificate
9. The client application provides the OTP and the identifier of the signature request to the One-Shot Optimizer
10. One-Shot Optimizer takes care of computing the hash of the documents to be signed in the business layer
11. Hashes are sent together with the request identifier and OTP code to Uanataca Services
12. The end user signature certificate is generated and used to sign the hashes
13. The signed hashes and the signature identifier are returned to the One-Shot Optimizer
14. One-Shot Optimizer generates the signed document envelopment, combining the original documents with the signed hashes
15. Finallly, the client application calls One-Shot Optimizer API to obtain the signed documents
<br></br>

> Other authentication methods


![img](https://i.ibb.co/FJCPhCF/oneshot-flc2.png)

</br>

1. The client application creates a new digital signature request, providing all required user data
2. One-Shot Optimizer returns an identifier for the certificate request
3. The client application provides the documents to be signed by the end user
4. The client application shows the documents to be signed to its end user
5. After reviewing the document, the end user agrees to sign it
6. The client application starts the signature process with an authentication method provided by the client
7. One-Shot Optimizer takes care of computing the hash of the documents to be signed in the business layer
8. Hashes are sent together with the request identifier and an id of the busuness authentication method to Uanataca Services
9. The end user signature certificate is generated and used to sign the hashes
10. The signed hashes and the signature identifier are returned to the One-Shot Optimizer
11. One-Shot Optimizer generates the signed document envelopment, combining the original documents with the signed hashes
12. Finallly, the client application calls One-Shot Optimizer API to obtain the signed documents



# Configuration

One-Shot Optimizer can be supplied as a **Docker** or as a **Virtual Machine** image.


## Hardware requirements


**CPU:** modern multicore (minimum 4 core)

**RAM:** 8GB

**HDD:** 200 GB


## One-Shot Optimizer on Docker


This configuration requires a server with a Linux CentOS operating system.

</br>


> STEP 1: Install Docker and Docker-Compose.

*Docker*

Run the following commands in this order.

	sudo yum update
	sudo yum install -y yum-utils
	yum install -y yum-utils device-mapper-persistent-data lvm2
	yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
	sudo yum install -y docker-ce docker-ce-cli containerd.io
	sudo systemctl start docker



*Docker-Compose*

Run the following commands in this order.


	sudo curl -L "https://github.com/docker/compose/releases/download/1.28.5/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
	chmod +x /usr/local/bin/docker-compose


Run command **docker-compose version** to check the installation. The outcome should show this information:


![img](https://i.ibb.co/WphfmXH/signbox-docker1.png) 

</br>

> STEP 2: Extract and copy One-Shot Optimizer zip content to the server.

Extract all `oneshot_optimizer_docker.zip` content in a local folder.

Move One-Shot Optimizer folder to the path **/opt** in the server.

The outcome should look like this:

![img](https://i.ibb.co/2M57krb/oneshot-docker2.png)

</br>

> STEP 3: Mapping volumes (for environments with a pool of One-Shot Optimizer).

In high performance environments with a pool of One-Shot Optimizer, service settings and logs must be stored in a shared volume outside Optimizer servers. These volumes must be defined in `docker-compose.yml` file in each One-Shot Optimizer.

	cd /opt/oneshot_optimizer

Docker-compose.yml settings file:

![img](https://i.ibb.co/mH3jGd0/oneshot-docker3.png)

</br>

> STEP 4: Load One-Shot Docker images.

Run the following commands:

	cd /opt/oneshot_optimizer
	docker image load -i oneshot_optimizer.tar
	docker image load -i oneshot_imgconverter.tar

Remove image files:

	rm /opt/oneshot_optimizer/oneshot_optimizer.tar
	rm /opt/oneshot_optimizer/oneshot_imgconverter.tar

</br>

> STEP 5: Launch the service.

Run:

	docker-compose up -d

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

![img](https://i.ibb.co/2gcwTGf/oneshot-docker4.png)

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

One-Shot Optimizer is already configured to use the Uanataca test environment. To start testing One-Shot Signature, it is enough to configure your Uanataca Billing account for test environment.

To do so, open the settings file `custom.ini` and insert the provided username and password under the [BILLING] section.

Custom.ini folder:

**/opt/oneshot_optimizer/common/etc/custom.ini** or custom mapped volume (Docker)

**/opt/bit4id/oneshot_optimizer/etc/custom.ini** (Virtual Machine)

**Password must be inserted in base64 format.** Run this command to convert your password to base64

	echo -n <<billing_password>> | base64

Custom.ini settings:

	1 | [BILLING]
	2 | USERNAME = billing-test-account@organization
	3 | PASSWORD = base64-password

Once you are done editing the file, restart the One-Shot Optimizer service to changes take effect.

	systemctl restart optimizer
<br></br>

> PRODUCTION environment

**Requirements:**

- Billing credentials for Uanataca production environment.
- Certificate (.cer) and key (.key) files to connect to the Uanataca production environment.
- The Id number for the Registration Authority that will issue the certificates.

<blockquote style="background-color: #faf3ac; border-color: #5a5a5a; color: #3b3b3b;">⚠ We recommend starting from a clean copy of One-Shot Optimizer for this step. This prevents leftover files from the test environment from causing errors in production.</blockquote>

You should have received a certificate (.cer) and key (.key) file to be used to identify your application in communications with the production signature service. Place both files in the certificates folder, replacing the certificates for the test environment present in the virtual machine.

**/opt/oneshot_optimizer/common/etc/certs** or custom mapped volume (Docker)

**/opt/bit4id/oneshot_optimizer/etc/certs/** (Virtual Machine)

![img](https://i.ibb.co/dtcK4h6/oneshot-docker5.png)

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
	15| DEFAULT_PROFILE                = PFnubeQAFCiudadano


Under [ENV] section, set environment to `prod`

[ENV]

 `ENVIRONMENT = prod`

Under the [BILLING] section, introduce the production Uanataca Billing credentials. **Password must be inserted in base64 format**. Run this command to convert your password to base64

	echo -n <<billing_password>> | base64

[BILLING]:

`USERNAME = billing-prod-account@organization`<br>
`PASSWORD = base64-password`

Under the [RA] section, make sure that the DEFAULT_RA matches your Registration Authority's identifer and the DEFAULT_PROFILE is set to the correct profile. Typically, this will be either `PFnubeQAFCiudadano` for EU eIDAS qualified certificates or `PFnubeNC` for non-qualified certificates.

[RA]:

`DEFAULT_RA = 999`<br>
`DEFAULT_PROFILE = PFnubeQAFCiudadano`


Once you are done editing the file, restart the One-Shot Optimizer service to changes take effect.

	systemctl restart optimizer
<br></br>


# Workflow

This section section presents the workflow for a simple use case of the One-Shot Signature service with a step-by-step description of the API calls required to allow a user to digitally sign a document provided by the client application.

You can follow the example using the developers One-Shot Optimizer configured for test environment in https://one-shot.developers.uanataca.com, or you can find the instructions to set up your One-Shot Opimizer in the <a href="#section/Configuration"> configuration section</a>.


The basic digital signature process involves the following steps:

- Retrieve an existing token for the RAO
- Create a new Digital Signature Request
- Upload a document
- Generate an OTP (only for Uanataca SMS)
- Sign the document
- Retrieve the signed document
- Delete documents from Optimizer

> STEP 1: Retrieve an existing token for the RAO

API reference: <a href="#tag/Tokens/paths/~1api~1v1~1tokens/get">List tokens</a>

The test One-Shot Optimizer is pre-configured with a Registration Authority Officer (RAO) account ready to be used within the test environment. This account has an associated token, that can be used to identify the RAO in API calls.

	curl -X GET https://one-shot.developers.uanataca.com/api/v1/tokens

On the clean machine, this should return a single token:

	1 | {
	2 |     "status": "200 OK",
	3 |     "details": {
	4 |         "6d1cae4d55be4cdf9cac50ee36f73406": {
	5 |             "username": "9001800",
	6 |             "password": true,
	7 |             "pin": true
	8 |         }
	9 |     }
	10| }

This output tells us that a single token "6d1cae4d55be4cdf9cac50ee36f73406" exists. This token is associated to the RAO account with id "9001800" and can be used in place of the password and pin.

To use tokens in a production environment, you will need to create them first with the corresponding <a href="#tag/Tokens/paths/~1api~1v1~1token/post">Create token</a> API call.
</br>

> STEP 2: Create a new Digital Signature Request

API reference: <a href="#tag/Requests/paths/~1api~1v1~1request/post">Create request</a>

Within the One-Shot Signature Service, all data pertaining to a given digital signature is collected within a Digital Signature Request. This includes both the identifying information of the signing user, which is provided when you create the signature request, and the document or documents to be signed, which we will upload later.

This call must include enough information to identify both the signing user and the RAO approving the request. The full description of the arguments accepted by this endpoint can be found in the API call detailed documentation, but for now it is enough to include at least the following:

	1 | curl --location --request POST 'https://one-shot.developers.uanataca.com/api/v1/request' \
	2 |      --form 'token=6d1cae4d55be4cdf9cac50ee36f73406' \
	3 |      --form 'profile=PFnubeQAFCiudadano' \
	4 |      --form 'given_name=name_of_the_user' \
	5 |      --form 'surname_1=surname_of_the_user' \
	6 |      --form 'email=user-example@domain.com' \
	7 |      --form 'mobile_phone_number=+343391234567' \
	8 |      --form 'document_front=@document_front.png' \
	9 |      --form 'document_rear=@document_rear.png' \
	10|      --form 'document_owner=@document_owner.png'

where token is the token representing the RAO credentials obtained in the previous step.

If the signature request is completed successfully, we will get the unique identifier assigned to it:

	1 | {
	2 |     "status": "201 Created",
	3 |     "details": 1464
	4 | }

The request code will be used to identify this digital signature request in subsequent calls.
</br>

> STEP 3: Upload a document

API reference: <a href="#tag/Documents/paths/~1api~1v1~1document~1{pk}/post">Upload document</a>


After creating the digital signature request, we can associate to it all pdf documents that should be signed by the user.

	curl -F "file=@doc.pdf" -X POST https://one-shot.developers.uanataca.com/api/v1/document/1464

Note that the number at the end of the call is the request id we obtained in the previous step.

If the upload is successful, the response will contain the identifier assigned to each document:

	1 | {
	2 |     "status": "200 OK",
	3 |     "details": "712c29ac-a2dc-4530-8c67-d0f227d8294b"
	4 | }

</br>

> STEP 4: Generate an OTP

API reference: <a href="#tag/Requests/paths/~1api~1v1~1otp~1{pk}/post">Generate OTP code</a>

<blockquote style="background-color: #faf3ac; border-color: #5a5a5a; color: #3b3b3b;">⚠ This step is only used for flows using Uanataca SMS.</blockquote>

Once the documents to be signed are ready, we need to generate a secure One-time password (OTP) that will allow the user to sign them. The OTP is generated by calling the otp endpoint and the resulting OTP is sent as an SMS message directly to the phone number we provided when creating the signature request.

When calling the OTP endpoint, provide the request identifier returned by the request endpoint:

	curl -X POST https://one-shot.developers.uanataca.com/api/v1/otp/1464

A successful call will look like this:

	1 | {
	2 |     "status": "200 OK",
	3 |     "details": "OTP generated"
	4 | }

With this call, an SMS with the secret code is sent to the mobile phone number associated to the signature request.
</br>

> STEP 5: Sign the document

API reference: <a href="#tag/Requests/paths/~1api~1v1~1sign~1{pk}/post">Sign</a>

In this step we are going to issue the digital signature certificate and sign all documents previously uploaded for the signature request.

Call the sign endpoint with the request id and json parameters containing the OTP:

	curl -d @params.json -H "Content-Type: application/json -X POST https://one-shot.developers.uanataca.com/api/v1/sign/1464

params.json for Uanataca SMS:

	1 | {
	2 |     "secret": "123456"
	3 | }

params.json for other authentication methods:

	1 | {
	2 |     "secret": "123456"
	3 |     "ext_unlock_type": "biometric"
	4 |     "ext_unlock_value": "12345678-12345678"
	5 | }

</br>

A successful call will result in the following response:

	1 | {
	2 |     "status": "200 OK",
	3 |     "details": "Documents correctly signed"
	4 | }

</br>

> STEP 6: Retrieve signed document

API reference: <a href="#tag/Documents/paths/~1api~1v1~1document~1{pk}~1{type}~1{uid}/get">Retrieve document</a>

Once the signature is done, the next step is getting the signed documents.

To do this, query with an HTTP GET request the endpoint /api/v1/document/{pk}/{type}/{uid}, where {pk} is the Request's unique identifier, {type} is the type of the document (it can be "original" for the uploaded document or "signed" for the digitally-signed version) and {uid} is the document unique identifier.

	curl -X GET https://one-shot.developers.uanataca.com/api/v1/document/1464/signed/712c29ac-a2dc-4530-8c67-d0f227d8294b

The response by the server will be the document in binary format:

	1 | %PDF
	2 | ...

</br>

> STEP 7: Delete documents from Optimizer

API reference: <a href="#tag/Documents/paths/~1api~1v1~1documents~1{pk}/delete">Delete all request documents</a>

<blockquote style="background-color: #faf3ac; border-color: #5a5a5a; color: #3b3b3b;">⚠ While the service will not delete uploaded files unless explicitly requested through an API call, it is <strong>strongly recommended</strong> that you backup any files that you want to preserve using an alternative system.</blockquote>

Delete all documents associated to an ended digital signature request.

	curl -X DELETE https://one-shot.developers.uanataca.com/api/v1/documents/1464


</br>



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


# API Reference