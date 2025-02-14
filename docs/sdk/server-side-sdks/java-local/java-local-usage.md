---
title: Java Local Server SDK Usage
sidebar_label: Usage
sidebar_position: 3
description: Using the SDK
sidebar_custom_props: { icon: material-symbols:toggle-on }
---

[![Maven](https://badgen.net/maven/v/maven-central/com.devcycle/java-server-sdk)](https://search.maven.org/artifact/com.devcycle/java-server-sdk)
[![GitHub](https://img.shields.io/github/stars/devcyclehq/java-server-sdk.svg?style=social&label=Star&maxAge=2592000)](https://github.com/DevCycleHQ/java-server-sdk)

## DevCycleUser Object

The user object is required for all methods. The only required field in the user object is userId.

See the DevCycleUser class in [Java DevCycleUser model doc](https://github.com/DevCycleHQ/java-server-sdk/blob/main/docs/DevCycleUser.md) for all accepted fields.

```java
DevCycleUser user = DevCycleUser.builder()
        .userId("a_user_id")
        .country("US")
        .build();
```

## Get and use Variable by key

This method will fetch a specific variable value by key for a given user. The default value will be used in cases where
the user is not segmented into a feature using that variable, or the project configuration is unavailable
to be fetched from DevCycle's CDN.

```java
Boolean variableValue = client.variableValue(user, "super_cool_feature", true);
if (variableValue.booleanValue()) {
    // New Feature code here
} else {
    // Old code here
}
```

The default value can be of type `String`, `Boolean`, `Number`, or `Object`.

If you would like to get the full Variable Object you can use `variable()` instead. This contains fields such as:
`key`, `value`, `type`, `defaultValue`, `isDefaulted`.

## Getting All Variables

This method will fetch all variables for a given user and returned as Map&lt;String, Feature&gt;.
If the project configuration is unavailable, this will return an empty map.

To get values from your Variables, the `value` field inside the variable object can be accessed.

```java
Map<String, Variable> variables = client.allVariables(user);
```

## Getting All Features

This method will fetch all features for a given user and return them as Map&lt;String, Feature&gt;.
If the project configuration is unavailable, this will return an empty map.

```java
Map<String, Feature> features = client.allFeatures(user);
```

## Track Event

To POST custom event for a user, pass in the user and event object.

```java
DevCycleEvent event = DevCycleEvent.builder()
        .date(Instant.now().toEpochMilli())
        .target("test target")
        .type("test event")
        .value(new BigDecimal(600))
        .build();

client.track(user, event);
```

## Set Client Custom Data

To assist with segmentation and bucketing you can set a custom data map that will be used for all variable and feature evaluations. User specific custom data will override client custom data.

```java
// create a map of custom data
Map<String,Object> customData = new HashMap();
customData.put("some-key", "some-value");

// set the map into the DevCycle client
client.setClientCustomData(customData);
```

## Override Logging

The SDK logs to stdout by default and does not require any specific logging package. To integrate with your own logging system, such as Java Logging or SLF4J, you can create a wrapper that implements the IDevCycleLogger interface. Then you can set the logger into the Java Server SDK setting the Custom Logger property in the options object used to initialize the client.

```java
IDevCycleLogger loggingWrapper = new IDevCycleLogger() {
    @Override
    public void debug(String message) {
        // Your logging implementation here
    }

    @Override
    public void info(String message) {
        // Your logging implementation here
    }

    @Override
    public void warning(String message) {
        // Your logging implementation here
    }

    @Override
    public void error(String message) {
        // Your logging implementation here
    }

    @Override
    public void error(String message, Throwable throwable) {
        // Your logging implementation here
    }
};

// Set the logger in the options before creating the DevCycleLocalClient
DevCycleLocalOptions options = DevCycleLocalOptions.builder().customLogger(loggingWrapper).build();
```

## EdgeDB

:::caution

EdgeDB is currently not available when using Local Bucketing.

:::
