---
title: Setup App and API Protection for Java on Linux
code_lang: linux
type: multi-code-lang
code_lang_weight: 30
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

This guide explains how to set up App and API Protection (AAP) for Java applications running on Linux. The setup involves:
1. Installing the Datadog Agent
2. Configuring your Java application
3. Enabling App and API Protection monitoring

## Prerequisites

- Linux operating system
- Java application
- Root or sudo privileges
- Systemd (for service management)

## Datadog Agent Setup
Install the Datadog Agent by following the [setup instructions for Linux hosts](/agent/?tab=Linux).

## Library setup

To enable App and API Protection capabilities, you need the Datadog Java tracing library (version 0.94.0 or higher) installed in your application environment.

### Download the library

Download the latest version of the Datadog Java library:

```bash
wget -O dd-java-agent.jar 'https://dtdg.co/latest-java-tracer'
```

### Verify compatibility

To check that your service's language and framework versions are supported for App and API Protection capabilities, see [Single Step Instrumentation Compatibility][2].

## Service configuration

Start your Java application with the Datadog agent and App and API Protection enabled:

```bash
java -javaagent:/path/to/dd-java-agent.jar -Ddd.appsec.enabled=true -Ddd.service=<MY_SERVICE> -Ddd.env=<MY_ENV> -jar path/to/app.jar
```
### With APM Tracing Disabled

If you want to use Application Security Management without APM tracing functionality, you can deploy with <a href="/security/application_security/setup/standalone/java">Standalone App and API Protection</a>. This configuration reduces the amount of APM data sent to Datadog to the minimum required by App and API Protection products.

To enable standalone mode:
1. Set `DD_APM_TRACING_ENABLED=false` environment variable
2. Keep `DD_APPSEC_ENABLED=true` environment variable
3. This configuration will minimize APM data while maintaining full security monitoring capabilities

```bash
java -javaagent:/path/to/dd-java-agent.jar -Ddd.appsec.enabled=true -Ddd.apm.tracing.enabled=false -Ddd.service=<MY_SERVICE> -Ddd.env=<MY_ENV> -jar path/to/app.jar
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

[1]: https://docs.datadoghq.com/tracing/trace_collection/automatic_instrumentation/single-step-apm/?tab=linux
[2]: /security/application_security/setup/java/compatibility
[3]: /security/application_security/setup/java/troubleshooting
[4]: /security/application_security/setup/standalone/java
[5]: https://app.datadoghq.com/security/appsec
