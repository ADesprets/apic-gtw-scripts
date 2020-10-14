IBM API Connect Sample Gateway scripts
In this article we provide simple sample of gateway script useful for IBM API Connect

# Introduction
Two programation languages: XSLT and GatewayScript

## Why coding?

Useful links to official documentation:
[Using context variables in GatewayScript and XSLT policies with the DataPower API Gateway](https://www.ibm.com/support/knowledgecenter/SSMNED_v10/com.ibm.apic.toolkit.doc/rapic_apigw_apis_gws_xslt.html)

[API Connect context variables](https://www.ibm.com/support/knowledgecenter/SSMNED_v10/com.ibm.apic.toolkit.doc/rapim_context_var.html)

[OAuth context variables](https://www.ibm.com/support/knowledgecenter/SSMNED_v10/com.ibm.apic.toolkit.doc/rapic_oauth_context_vars.html)

## Guidelines
### Don't
Use carefully the different tools such as Existing policies, Global Policies, User Defined Policies, and code within an API.
Another aspect is do I need to use a piece of code or default policies

### Do

# Useful tips
### Automation

## Returns a simple JSON message in the response
```
context.message.header.set('Content-Type', 'application/json');
context.message.body.write({"msg": "hello"});
```
![Simple gateway script](./images/simpleresponse.png)

## Manipulate headers
### Add a header for the response

### Propagate Authorization header
```
var au = context.request.header.get('Authorization');
context.message.header.set('Authorization', au));
```

## Write a message to the console
```
var all_apis = context.get('api');
console.error('>>> all_apis: ' + JSON.stringify(all_apis));
```

## Manipulate context Variable          
```
var clientID = context.get('client.app.id');
context.message.header.set('mycid', clientID);
```


## Perform URI mapping
In this sample we specify a small piece of code where we remove the base path in order to invoke the backend API.
There is small gateway script to specify the new path based on the request.path

```
var rpath = context.get('request.path');
var bpath = context.get('api.root');
var dest_path = rpath.substr(bpath.length + 1);
context.set('dest_path', dest_path);
```

In the invoke you can use this variable like this:
`$(target-url)$(dest_path)`
