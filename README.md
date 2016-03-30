# OkHttp simple-client app vulnerable to CVE-2016-2402

This is a fork of the default [simple-client](https://github.com/square/okhttp/blob/okhttp_31/samples/simple-client) from the okhttp project.

Simple-client is a Java app that just does a GET request to https://api.github.com and fetches the names of okhttp's contributors.

This fork has been edited so that OkHttp 3.0.1 is used for networking connections and certificate pinning is also used.

OkHttp 3.0.1 is vulnerable to CVE-2016-2402 - this app demonstrates the flaw.

For more information please read:

* [https://koz.io/pinning-cve-2016-2402](https://koz.io/pinning-cve-2016-2402)
* [https://www.cigital.com/blog/ineffective-certificate-pinning-implementations/](https://www.cigital.com/blog/ineffective-certificate-pinning-implementations/)


## Building

To build the app, first clone the repository and then run:

`ant build`

## Usage

If you don't have the private key of a CA trusted by your system's JRE, you'll have to add the proxy's certificate to your CA store.
Let's say CA_CERT.pem is your proxy's CA certificate.

`$cp /etc/ssl/certs/java/cacerts .`

`$keytool -import -trustcacerts -alias ikozCA -file CA_CERT.pem -keystore cacerts -storepass changeit`

Then start simple-client using the following parameters to connect through a local proxy

`$java -DproxyHost=127.0.0.1 -DproxyPort=8080 -Djavax.net.ssl.trustStore=cacerts -jar certPinningVulnerableOkHttp.jar`

John Kozyrakis