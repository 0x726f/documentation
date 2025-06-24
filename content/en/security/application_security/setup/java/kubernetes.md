---
title: Setup App and API Protection for Java in Kubernetes
code_lang: kubernetes
type: multi-code-lang
code_lang_weight: 20
further_reading:
- link: "/security/application_security/how-it-works/"
  tag: "Documentation"
  text: "How App and API Protection Works"
- link: "/security/default_rules/?category=cat-application-security"
  tag: "Documentation"
  text: "OOTB App and API Protection Rules"
- link: "/security/application_security/troubleshooting"
  tag: "Documentation"
  text: "Troubleshooting App and API Protection"
---

{{< partial name="app_and_api_protection/callout.html" >}}

{{< partial name="app_and_api_protection/java/overview.html" >}}

This guide explains how to set up App and API Protection (AAP) for Java applications running in Kubernetes. The setup involves:
1. Installing the Datadog Agent
2. Configuring your Java application deployment
3. Enabling App and API Protection monitoring

## Prerequisites

- Kubernetes cluster
- Java application containerized with Docker
- kubectl configured to access your cluster
- Helm (recommended for Agent installation)

## Datadog Agent Setup
Install the Datadog Agent by following the [setup instructions for Kubernetes](/agent/?tab=cloud_and_container).

## Library setup

To enable App and API Protection capabilities, you need the Datadog Java tracing library (version 0.94.0 or higher) installed in your application environment.

### Download the library

Download the latest version of the Datadog Java library:

```dockerfile
ADD 'https://dtdg.co/latest-java-tracer' /dd-java-agent.jar
```

### Verify compatibility

To check that your service's language and framework versions are supported for App and API Protection capabilities, see [Single Step Instrumentation Compatibility][2].

## Service configuration

Start your Java application with the Datadog agent and App and API Protection enabled:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: your-java-app
spec:
  template:
    spec:
      containers:
      - name: your-java-app
        image: your-java-app-image
        env:
        - name: DD_APPSEC_ENABLED
          value: "true"
        - name: DD_SERVICE
          value: "<MY_SERVICE>"
        - name: DD_ENV
          value: "<MY_ENV>"
        command: ["java"]
        args: ["-javaagent:/dd-java-agent.jar", "-jar", "/app.jar"]
```

### With APM Tracing Disabled

If you want to use Application Security Management without APM tracing functionality, you can deploy with <a href="/security/application_security/setup/standalone/java">Standalone App and API Protection</a>. This configuration reduces the amount of APM data sent to Datadog to the minimum required by App and API Protection products.

To enable standalone mode:
1. Set `DD_APM_TRACING_ENABLED=false` environment variable
2. Keep `DD_APPSEC_ENABLED=true` environment variable
3. This configuration will minimize APM data while maintaining full security monitoring capabilities


## Configure your Java application

### Add the Java agent to your container

Add the following to your Dockerfile:

```dockerfile
# Download the Datadog Java agent
ADD 'https://dtdg.co/latest-java-tracer' /dd-java-agent.jar

# Set environment variables
ENV DD_APPSEC_ENABLED=true
ENV DD_SERVICE=<YOUR_SERVICE_NAME>
ENV DD_ENV=<YOUR_ENVIRONMENT>
```

### Update your Kubernetes deployment

Modify your deployment YAML to include the Java agent:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: your-java-app
spec:
  template:
    spec:
      containers:
      - name: your-java-app
        image: your-java-app-image
        env:
        - name: DD_APPSEC_ENABLED
          value: "true"
        - name: DD_SERVICE
          value: "<YOUR_SERVICE_NAME>"
        - name: DD_ENV
          value: "<YOUR_ENVIRONMENT>"
        command: ["java"]
        args: ["-javaagent:/dd-java-agent.jar", "-jar", "/app.jar"]
```

## Deploy your application

Apply your updated deployment:

```bash
kubectl apply -f your-deployment.yaml
```

## Verify setup

To verify that App and API Protection is working correctly:

1. Send some traffic to your application
2. Check the [Application Signals Explorer][5] in Datadog
3. Look for security signals and vulnerabilities

## Troubleshooting

If you encounter issues while setting up App and API Protection for your Java application, see the [Java App and API Protection troubleshooting guide][3].

## Further Reading

{{< partial name="whats-next/whats-next.html" >}}

[1]: https://docs.datadoghq.com/tracing/trace_collection/automatic_instrumentation/single-step-apm/?tab=kubernetes
[2]: /security/application_security/setup/java/compatibility
[3]: /security/application_security/setup/java/troubleshooting
[4]: https://docs.datadoghq.com/security/application_security/standalone/
[5]: https://app.datadoghq.com/security/appsec
