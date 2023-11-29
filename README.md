# CCADB-Tools
Tools or services used in conjunction with or used by the [CCADB](https://www.ccadb.org).

## API_AddUpdateIntermediateCert

**Status:** In use

**Description:** APIs to enable Certificate Authorities (CAs) to automate retrieving and updating intermediate certificate data in the CCADB. 

**Usage:** See README in https://github.com/mozilla/CCADB-Tools/tree/master/API_AddUpdateIntermediateCert
    
**Used By:** This service is only available to CAs whose root certificates are included within the root stores of CCADB root store members.

## EVChecker

**Status:** In use

**Description:** Extracts EV-enablement data from ExtendedValidation.cpp. Returns a JSON list for each root cert in ExtendedValidation.cpp containing: EV OID, cert SHA256 Fingerprint, Cert Issuer, Cert Serial Number.

**Usage:** See the README in https://github.com/mozilla/CCADB-Tools/tree/master/EVChecker

**Used By:** Used to fill in the “ExtendedValidation.cpp OIDs” fields on root certs in the CCADB. There is a CCADB home page report that alerts when that does not match the published “Mozilla EV Policy OID(s)” field.

## cacheck

**Status:** In use

**Description:** Web UI for viewing crt.sh's cached certificate linting results. Traverses down the CA hierarchy starting at the specified certificate, for the specified date range and cert statuses, summarizes the specified lint test findings.

**Usage:** See README in https://github.com/mozilla/CCADB-Tools/tree/master/cacheck

**Used By:** Mozilla CA Program Managers, other root store members of the CCADB, and CAs.

## cacompliance

**Status:** BROKEN -- Bugzilla changed, and implementation was never finished.

**Description:** Identify the CA Compliance bugs that need attention. The intent was to improve our workflow for CA Incident Bugs, such as provide a site for getting a CA Incident Digest that would quickly identify the CA incident bugs that we need to pay attention to.

**Usage:** 

**Used By:** Would be used by Mozilla CA Program Managers and other root store members of the CCADB.

## capi

**Status:** In use

**Description:**  Checks that certificates for the 3 test websites provided by the CA for their root certificate work as expected, are configured correctly, and pass lint tests. Runs tests to verify that the three sample websites required by the CAB Forum Baseline Requirements (for valid, expired, and revoked certificates) do in fact chain up to the specified root certificate and that the status of TLS certificates are as expected (valid, expired, or revoked). Lint tests are also run on the test website TLS certificates.

**Usage:** See README in https://github.com/mozilla/CCADB-Tools/tree/master/capi

```sh
TESTINFO=$(cat <<EOF
{ "CertificateDetails":[ { "RecordID":"234571894056781356", "Name":"SSL.com Root Certification Authority ECC", "PEM":"-----BEGIN CERTIFICATE----- MIICjTCCAhSgAwIBAgIIdebfy8FoW6gwCgYIKoZIzj0EAwIwfDELMAkGA1UEBhMC VVMxDjAMBgNVBAgMBVRleGFzMRAwDgYDVQQHDAdIb3VzdG9uMRgwFgYDVQQKDA9T U0wgQ29ycG9yYXRpb24xMTAvBgNVBAMMKFNTTC5jb20gUm9vdCBDZXJ0aWZpY2F0 aW9uIEF1dGhvcml0eSBFQ0MwHhcNMTYwMjEyMTgxNDAzWhcNNDEwMjEyMTgxNDAz WjB8MQswCQYDVQQGEwJVUzEOMAwGA1UECAwFVGV4YXMxEDAOBgNVBAcMB0hvdXN0 b24xGDAWBgNVBAoMD1NTTCBDb3Jwb3JhdGlvbjExMC8GA1UEAwwoU1NMLmNvbSBS b290IENlcnRpZmljYXRpb24gQXV0aG9yaXR5IEVDQzB2MBAGByqGSM49AgEGBSuB BAAiA2IABEVuqVDEpiM2nl8ojRfLliJkP9x6jh3MCLOicSS6jkm5BBtHllirLZXI 7Z4INcgn64mMU1jrYor+8FsPazFSY0E7ic3s7LaNGdM0B9y7xgZ/wkWV7Mt/qCPg CemB+vNH06NjMGEwHQYDVR0OBBYEFILRhXMw5zUE044CkvvlpNHEIejNMA8GA1Ud EwEB/wQFMAMBAf8wHwYDVR0jBBgwFoAUgtGFczDnNQTTjgKS++Wk0cQh6M0wDgYD VR0PAQH/BAQDAgGGMAoGCCqGSM49BAMCA2cAMGQCMG/n61kRpGDPYbCWe+0F+S8T kdzt5fxQaxFGRrMcIQBiu77D5+jNB5n5DQtdcj7EqgIwH7y6C+IwJPt8bYBVCpk+ gA0z5Wajs6O7pdWLjwkspl1+4vAHCGht0nxpbl/f5Wpl -----END CERTIFICATE-----", "TestWebsiteValid":"https://test-dv-ecc.ssl.com", "TestWebsiteRevoked":"https://revoked-ecc-dv.ssl.com", "TestWebsiteExpired":"https://expired-ecc-dv.ssl.com" } ] }
EOF
)

curl -X POST -H "Content-Type: application/json" --data "$TESTINFO" http://127.0.0.1:8080/fromCertificateDetails
```

**Used By:** Automatically run by CCADB in the TEST WEBSITES tab in Add/Update Root Request cases.

## certdataDiffCCADB

**Status:** In use

**Description:** Identifies the differences between a given certdata.txt file and the corresponding data in the CCADB, in regards to which root certificates are included in Mozilla’s root store, and which trust bits are enabled for each root certificate.

**Usage:** See README in https://github.com/mozilla/CCADB-Tools/tree/master/certdataDiffCCADB 

**Used By:** Mozilla root store managers run a report in the CCADB that compares a CCADB report of current data with the cerdata.txt in Firefox Beta.

## certificate

**Status:** In use

**Description:** Parses the PEM of a certificate and outputs a JSON response that CCADB uses to fill in the data on the certificate record in the CCADB.

**Usage:** See README in https://github.com/mozilla/CCADB-Tools/tree/master/certificate

**Used By:** Used by the CCADB to create a root or intermediate certificate record corresponding to the certificate PEM.

## crlVerification

**Status:** In use

**Description:** Verify that there is an entry for the certificate in the CRL. Checks that the revoked certificate’s entry for a given CRL is as expected: there is an entry in the given CRL whose serial matches the given serial, its revocation date matches that which is provided, and revocation reason matches the provided reason.

**Usage:** See README in https://github.com/mozilla/CCADB-Tools/tree/master/crlVerification
Example: 
```sh
curl -d '{"crl": "http://crl.ws.symantec.com/pca1-g3.crl","serial": "fc788d52d4441678243b9882cb15b4","revocationDate": "2019/05/07"}' http://localhost:8080/crlVerification
```

**Used By:** Automatically run by CCADB when a certificate is indicated to be revoked. Also can be invoked via the 'Verify Revocation' button on intermediate certificate records. 

## evReady

**Status:** In use

**Description:** Checks if the root cert and its chain are properly configured for EV Treatment. This test is done for verification of root inclusion requests for which EV is to be enabled.

**Usage:** See README in https://github.com/mozilla/CCADB-Tools/tree/master/evReady

**Used By:** Used by CAs and root store operators.

## oneCRLDiffCCADB

**Status:** In Use

**Description:** Compare CCADB data against OneCRL data. Compares the "OneCRL Status" field from each certificate in [PublicIntermediateCertsRevokedWithPEMCSV](https://ccadb.my.salesforce-sites.com/mozilla/PublicIntermediateCertsRevokedWithPEMCSV) and attempts to find it within [OneCRL](https://firefox.settings.services.mozilla.com/v1/buckets/blocklists/collections/certificates/records).

**Usage:** See the README in https://github.com/mozilla/CCADB-Tools/tree/master/oneCRLDiffCCADB

**Used By:** The "Data Integrity - OneCRL cert-storage" report in the CCADB
