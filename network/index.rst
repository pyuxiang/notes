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


Set up VPN server using OpenVPN
===============================

- ASUS router: OpenVPN configuration support
- .ovpn file to load configuration, RS2048 certificate
- OpenVPN Connect
- What is OpenVPN

So turns out exposing SSH is quite a bad idea.

- Set up VPN: https://www.reddit.com/r/qnap/comments/dgmowi/tutorial_how_to_connect_your_qnap_safely_from_the/
- Security risks for port forwarding: https://security.stackexchange.com/questions/84001/is-there-a-risk-in-opening-port-22-for-ssh-access-with-a-network/84008
- More comments: https://www.quora.com/Is-it-really-a-bad-practice-to-open-port-22-and-SSH-to-the-production-server-directly
- Comprehensive: https://stribika.github.io/2015/01/04/secure-secure-shell.html

Also to enable key-based SSH login.

- https://askubuntu.com/questions/46930/how-can-i-set-up-password-less-ssh-login/46935#46935

And use 2FA where possible, otherwise shoulder-surfing is all it takes
to acquire username + password for full breach into server.

Sunk cost fallacy, but abandon my paid GoDaddy SSL certificates and use
OpenSSL certificates for wildcard domain protection. HTTPS is always better
than HTTP access. I should disable port 80 (HTTP) forwarding as well.

Admin only SSH access might be a good thing after all.
NAS configured for network sharing, not all this fancy administration stuff.
Restrict SSH to minimize attack surfaces, especially since I'm not exactly
experienced in securing networks.


Windows sharing
===============

https://forum.opnsense.org/index.php?topic=14687.0

First note client-cert is not provided for OpenVPN configurations in NAS,
can simply disable requirement by adding ``client-cert-not-required`` in the
.ovpn config file. Need to make sure ``script-security 1`` is set (default 3).

Doesn't matter whether connect VPN to router or NAS, both exposes all services
within the internal network. NAS might be safer since likely more frequently
updated than router firmware.

Consider setting up a `bastion host <https://en.wikipedia.org/wiki/Bastion_host#:~:text=A%20bastion%20host%20is%20a,the%20threat%20to%20the%20computer.>`_
for more focused security implementation (although a user did mention it
effectively only serves as an additional indirection layer).

Update 1
--------

After hours of debugging, then I realised it's an IP address thing.

1. Both QNAP NAS and router have OpenVPN.
2. Router assigns to ``10.8.0.xxx``, which cannot be used to access the
   QNAP NAS in the ``192.168.1.xxx`` subnet.
3. QVPN opens a direct tunnel to the VPN client for connection.

Not sure what a solution would look like, tried static routing on router
but doesn't work out.
At least this might be a good thing since it adds barriers between services.
Essentially the network is properly configured now, now need to clean up
the user list in the NAS.

This might yield some results: https://superuser.com/questions/1271965/how-to-access-192-x-address-space-from-10-x-address-space

Actually I take it back, it seems to work - setting the static route from 192.x
10.x on QNAP NAS actually restricted services to other devices on the network,
including the router configuration. Well, it's a problem for another day.
Currently disabling using the ``Use this connection as a default gateway for
remote devices`` checkbox in the QVPN OpenVPN setup.

Turns out OpenVPN assigns to 10.8.0.x subnet, and the DNS server is essentially
the NAS itself at 10.8.0.1. That will be the winning connection.
Also figured out a longstanding issue of having "Access Denied" for
regular users, turns out I disabled the Microsoft Networking as well as
Apple Networking services for the user, which in turn did not allow me to
connect using ``explorer.exe`` via WIN. This is under Application Privilege
in the Users panel.
