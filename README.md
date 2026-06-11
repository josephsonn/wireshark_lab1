# HTTP PCAP Credential Extraction Lab

## Project Overview
This repository documents the analysis of a packet capture (`.pcap`) file containing unencrypted HTTP network traffic. The primary objective of this lab was to investigate the traffic using Wireshark, locate insecure authentication attempts, and extract the transmitted login credentials.
</br>
<img src="https://imgur.com/Ut4GO9n.png"/>
</br>
<img src="https://imgur.com/Qr5qyOZ.png"/>

## Analysis Methodology
To isolate the credentials efficiently, the following network analysis steps were performed:

1. **Traffic Filtering**: Applied display filters to isolate HTTP methods commonly used for authentication:
    `http.request.method in {"GET","POST"}` OR: `http.request.method == "POST" || http.request.method == "GET"`
</br>
<img src=https://imgur.com/3vKTxee.png/>
</br>
<img src="https://imgur.com/YKcDLxA.png"/>

3. **Stream Reconstruction**: Located the relevant authentication request and used Wireshark's **Follow TCP Stream** feature to reconstruct the full plaintext conversation between the client and server.
</br>  
<img src="https://imgur.com/pQDkF9v.png"/>
</br>
<img src="https://imgur.com/CCYB8NS.png"/>

4. **Data Extraction**: Inspected the application payload and HTTP headers to identify the parameters holding the sensitive data.
</br>
<img src="https://imgur.com/X5E196m.png"/>

## Findings & Evidence 
Because the traffic used unencrypted HTTP instead of HTTPS, all transmitted data was exposed in cleartext.
</br>
<img src="https://imgur.com/twAuIXJ.png"/>

* **Target URL/Endpoint:** `http://juice-shop.herokuapp.com/rest/user/login`
* **HTTP Method Used:** `POST / GET`
* **Discovered Username:** `"someuser@gmail.com"`
* **Discovered Password:** `"ThisIsClearText123"`

## Security Recommendations
To mitigate the credential harvesting vulnerability demonstrated in this lab, the following controls should be implemented:
* **Enforce HTTPS**: Transport Layer Security (TLS/SSL) must be mandated across the entire application to encrypt credentials in transit.
* **Secure Cookie Attributes**: Implement `Secure` and `HttpOnly` flags on session cookies to prevent interception and cross-site scripting exposure.
