---
title: Sandboxes
parent: Home
hide_hero: true
nav_order: 1
permalink: /sandboxes/
---

# Bulk Data Sandboxes

<a id="smart"/>
## SMART Reference Implementation
The SMART team maintains an open access
[bulk data sandbox](https://bulk-data.smarthealthit.org/) for testing.

### Connect
The sandbox can be configured many different ways, but the default setup holds 100 patients and requires no authentication:

`https://bulk-data.smarthealthit.org/fhir`

### Example
Using bulk-data-client:
```sh
git clone https://github.com/smart-on-fhir/bulk-data-client.git
cd bulk-data-client
npm ci

node . \
  --fhir-url https://bulk-data.smarthealthit.org/fhir \
  --_type Patient
```
Using cumulus-etl:
```sh
pipx install cumulus-etl

cumulus-etl export \
  'https://bulk-data.smarthealthit.org/fhir/$export?_type=Patient' \
  ./downloads
```

<a id="epic"/>
## Epic
### Setup
This sandbox requires authentication when doing bulk exports.
#### Register your application
You will need to register an application in 
[Vendor Services](https://vendorservices.epic.com/Developer/Apps).

- You'll want to make a Backend Systems application type.
- Using the Incoming API feature.
- Add whatever resources you would like, but you'll want at least "Patient.Search (R4)" and all the four "Bulk Data" permissions (Delete, File, Kick-off, Status).
- Enable OAuth 2.0, and you can leave the related settings alone at their defaults.

Once registered, your application will be assigned some IDs. Take note of the Non-Production Client ID. You'll need to provide this ID later to your bulk export client.
#### Assign a public/private key pair (.pem file)
You'll also need to provide a PEM, for authentication to the sandbox. This is a public/private key pair that you can generate and provide to Epic. (For authenticating with a "real" Epic server, you'll want to generate a different kind of file – a JWK Set file – and publicly host it. But for sandbox use, uploading a PEM file is sufficient.)

Epic has documentation for 
[generating public and private PEM files](https://vendorservices.epic.com/Article?docId=oauth2&section=Creating-Key-Pair).
Follow those steps, then on your Vendor Services application page,
click on the "Upload public key" button:

![Epic upload button](../assets/epic_upload.png)

Upload the `publickey509.pem` file from Epic's instructions and click Save.
This change may take some time (up to a couple of hours) to propagate across Epic's infrastructure.

You will later need to provide your bulk export client with the `privatekey.pem` file.

### Connect
This is the Epic sandbox group URL:

`https://fhir.epic.com/interconnect-fhir-oauth/api/FHIR/R4/Group/e3iabhmS8rsueyz7vaimuiaSmfGvi.QwjVXJANlPOgR83`

### Example
Using cumulus-etl:
```sh
pipx install cumulus-etl

cumulus-etl export \
  --smart-client-id=xxx \
  --smart-key=./privatekey.pem \
  'https://fhir.epic.com/interconnect-fhir-oauth/api/FHIR/R4/Group/e3iabhmS8rsueyz7vaimuiaSmfGvi.QwjVXJANlPOgR83/$export?_type=Patient' \
  ./downloads
```
### Further Reading
Epic's [Bulk Export documentation](https://fhir.epic.com/Documentation?docId=fhir_bulk_data)
might also be helpful.


<a id="cerner"/>
## Oracle Cerner

### Setup
This sandbox requires authentication when doing bulk exports.

#### Register your application
You will need to register an application in the [Code Console](https://code-console.cerner.com/console/apps).
- You'll want to make a System application type.
- For the Millennium Bulk Data product.
- Add whatever resources you would like, but you'll want at least Patient.

Once registered, your application will be assigned some IDs. Take note of the client ID (not the application ID, which you won't need for this). You'll need to provide the client ID later to your bulk export client.

#### Assign a public/private key pair (.jwks file)
You'll also need to provide a JWKS, for authentication. This is a public/private key pair that you can generate and provide to Oracle.

To generate your own JWKS:
```sh
jose jwk gen -s -i "{\"alg\":\"RS384\",\"kid\":\"`uuidgen`\"}" -o private.jwks
jose jwk pub -s -i private.jwks -o public.jwks
```

View your application details in the Code Console and click on "Cerner Central System Account Details". You'll then see a section labeled "JSON Web Key Set" with an Edit button. Enter the contents of your public.jwks file.

You will later need to provide your bulk export client with the private.jwks file.

### Connect
This is the Oracle sandbox group URL:

`https://fhir-ehr-code.cerner.com/r4/ec2458f2-1e24-41c8-b71b-0e701af7583d/Group/11ec-d16a-c763b73e-98e8-a31715e6a2bf`

### Example
```sh
git clone https://github.com/smart-on-fhir/bulk-data-client.git
cd bulk-data-client
npm ci

# Edit your config file to include your client ID and the first element from your JWKS file

node . \
  --config ./your-config.js \
  --fhir-url https://fhir-ehr-code.cerner.com/r4/ec2458f2-1e24-41c8-b71b-0e701af7583d \
  --group 11ec-d16a-c763b73e-98e8-a31715e6a2bf \
  --_type Patient
```
```sh
pipx install cumulus-etl

cumulus-etl export \
  --smart-client-id=xxx \
  --smart-key=./private.jwks \
  'https://fhir-ehr-code.cerner.com/r4/ec2458f2-1e24-41c8-b71b-0e701af7583d/Group/11ec-d16a-c763b73e-98e8-a31715e6a2bf/$export?_type=Patient' \
  ./downloads
```

### Further Reading
Oracle’s [Bulk Export documentation](https://docs.oracle.com/en/industries/health/millennium-platform-apis/mfbda/bulk_data_access.html) might also be helpful.
