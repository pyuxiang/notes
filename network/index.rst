===============================================================================
Web
===============================================================================

For SSL certificate, I used the GoDaddy SSL wizard to automate the process.
This `help page <https://sg.godaddy.com/help/request-my-ssl-certificate-562>`_
should help.

1. Create private key and Certificate Signing Request (CSR)
2. Request for the SSL certificate using the CSR, which will be made
   downloadable from GoDaddy.
3. Under the QNAP Certificate page, upload the private key, the SSL certificate
   as well as the accompanying intermediate certificates.

Certificate example:

.. sourcecode:: none

    # 3ec8ee14ec39b375.crt

    -----BEGIN CERTIFICATE-----
    MIITAlVTMRAwDgYDVQQIEwdBcml6b25hMRMwEQYDV1UECxMkaHR0cDovL2NlcnRz
    LmdvZGFkZHkuY29tL3JlcG9zaXRvcXR5IC0gRzIwHhcNMjAwOTEzMjAyNDEyWhcN
    ...
    MjExMDE1MjAybhMiFwH+fEDKtvtkFr45VBcIYqf3N3YotvTu/VNsQXU4BlDccNWH
    RyI+PsaKyKfF4wLI
    -----END CERTIFICATE-----

Intermediate certificate example:

.. sourcecode:: none

    # gd_bundle-g2-g1.crt

    -----BEGIN CERTIFICATE-----
    MIIE0DCCA7igAwIBAgIBBzANBgkqhkiG9w0BAQNjb3R0c2RhbGUxGjAYBgNVBAoT
    EUdvRGFkZHkuY29tLCB05+Z5Yg4MotdqYkSnE3ANkR/0yBOtg2DZ2HKocyQetawi
    ...
    DsoXiWJYRBuriSUBAA/NxBti21G00w9QQc7CoZDDu+8CL9IVVO5EFdkKrqeKM+2x
    LXY2JtwE65/3YR8V3Idv7kaWKK2hJn0KCacuBKONvPi8BDAB
    -----END CERTIFICATE-----
    -----BEGIN CERTIFICATE-----
    MIIEfTCCA2WgAwIBAgIDG+cVMA0GCSqGSIb3DQEBCwUAMGMxCzAJBgNVBAYTAlVT
    MSEwHwYDVQQKExhUaGUgR28gRGFkZHkgR3JvdXBBdXRob3JpdHkwHhcNMTQwMTAx
    ...
    MDcwMDAwWhcNMzEwNTMa6R+ghldMZvNhkREg5L4wn3qkKQmw4TRfZHcYQFHfjDCm
    rw==
    -----END CERTIFICATE-----
    -----BEGIN CERTIFICATE-----
    MIIEADCCAuigAwIBAgIBADANBgkqhkiG9w0BAQUFADBjMQswCQYDVQQGEwJVUzEh
    MB8GA1UEChMYVGhlIEdvIERhZGR5IEdyb3VwLCBJbmMuMTEwLwYDVQQLEyhHbyBE
    ...
    YWRkeSBDbGFzcyAygyuV+I6ShHIRQLIQTO7ErBBDpqWeCtWVYpoNz4iCxTIM5Cuf
    ReYNnyicsbkqWletNw+vHX/bvZ8=
    -----END CERTIFICATE-----

Private key example:

.. sourcecode:: none

    # private.key

    -----BEGIN PRIVATE KEY-----
    MIIEvgIBADANBgkqhkiG9w0BAQEFAASCBKgwggSkAgEAAoIR2e4gjmdL8fAgxiJ6
    SZ1EWmdHUG/vTpYCofrQvhiki0VqladegHqF8dvhAoGBAaWKQPg6Myd3rIrC9bQv
    ...
    mcIkTdISsnXDCWAwTQj6Gk30val+yhqRwbgDgsaO6hlrsngSCKMcMezWWPSOky/4
    KEqBEVJlk+OSoMrWVS5iaDh1
    -----END PRIVATE KEY-----
