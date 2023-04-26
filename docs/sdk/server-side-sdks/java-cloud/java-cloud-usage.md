---
title: DevCycle Java Cloud Server SDK Usage
sidebar_label: Usage
sidebar_position: 3
---

[![Maven](https://badgen.net/maven/v/maven-central/com.devcycle/java-server-sdk)](https://search.maven.org/artifact/com.devcycle/java-server-sdk)
[![GitHub](https://img.shields.io/github/stars/devcyclehq/java-server-sdk.svg?style=social&label=Star&maxAge=2592000)](https://github.com/DevCycleHQ/java-server-sdk)


## User Object
The user object is required for all methods. The only required field in the user object is userId

See the User class in [Java User model doc](https://github.com/DevCycleHQ/java-server-sdk/blob/main/docs/User.md) for all accepted fields.

```java
User user = User.builder()
    .userId("a_user_id")
    .build();
```

## Getting All Features
This method will fetch all features for a given user and return them as Map<String, Feature>

```java
import com.devcycle.sdk.server.cloud.api.DVCCloudClient;
import com.devcycle.sdk.server.common.exception.DVCException;
import com.devcycle.sdk.server.common.model.Feature;
import com.devcycle.sdk.server.common.model.User;

public class MyClass {
    
    private DVCCloudClient dvcCloudClient;
    
    public MyClass() {
        dvcCloudClient = new DVCCloudClient("<DVC_SERVER_SDK_KEY>");
    }
    
    public void allFeatures() throws DVCException {
        User user = User.builder()
                .userId("a_user_id")
                .country("US")
                .build();

        Map<String, Feature> features = dvcCloudClient.allFeatures(user);
    }
}
```

## Getting All Variables
This method will fetch all variables for a given user and returned as Map&lt;String, Feature&gt;

To get values from your Variables, the `value` field inside the variable object can be accessed.

```java
import com.devcycle.sdk.server.cloud.api.DVCCloudClient;
import com.devcycle.sdk.server.common.exception.DVCException;
import com.devcycle.sdk.server.common.model.User;
import com.devcycle.sdk.server.common.model.Variable;

public class MyClass {

    private DVCCloudClient dvcCloudClient;

    public MyClass() {
        dvcCloudClient = new DVCCloudClient("<DVC_SERVER_SDK_KEY>");
    }

    public void allVariables() throws DVCException {
        User user = User.builder()
                .userId("a_user_id")
                .country("US")
                .build();
        
        Map<String, Variable> variables = dvcCloudClient.allVariables(user);
    }
}
```

## Get and Use Variable By Key
This method will fetch a specific variable by key for a given user. It will return the variable
object from the server unless an error occurs or the server has no response. In that case it will return a variable object with the value set to whatever was passed in as the `defaultValue` parameter.

To get values from your Variables, the `value` field inside the variable object can be accessed.


```java
import com.devcycle.sdk.server.cloud.api.DVCCloudClient;
import com.devcycle.sdk.server.common.exception.DVCException;
import com.devcycle.sdk.server.common.model.User;
import com.devcycle.sdk.server.common.model.Variable;

public class MyClass {

    private DVCCloudClient dvcCloudClient;

    public MyClass() {
        dvcCloudClient = new DVCCloudClient("<DVC_SERVER_SDK_KEY>");
    }

    public void setFlag() throws DVCException {
        User user = User.builder()
                .userId("a_user_id")
                .country("US")
                .build();

        String key = "turn_on_super_cool_feature";
        Boolean defaultValue = true;
        Variable variable = dvcCloudClient.variable(user, key, defaultValue);

        if ((Boolean) variable.getValue()) {
            // New Feature code here
        } else {
            // Old code here
        }
    }
}
```

## Track Event

To POST custom event for a user, pass in the user and event object.

```java
import com.devcycle.sdk.server.cloud.api.DVCCloudClient;
import com.devcycle.sdk.server.common.exception.DVCException;
import com.devcycle.sdk.server.common.model.DVCResponse;
import com.devcycle.sdk.server.common.model.Event;
import com.devcycle.sdk.server.common.model.User;

public class MyClass {

    private DVCCloudClient dvcCloudClient;

    public MyClass() {
        dvcCloudClient = new DVCCloudClient("<DVC_SERVER_SDK_KEY>");
    }

    public void addAnEvent() throws DVCException {
        User user = User.builder()
                .userId("a_user_id")
                .country("US")
                .build();

        Event event = Event.builder()
                .date(Instant.now().toEpochMilli())
                .target("test target")
                .type("test event")
                .value(new BigDecimal(600))
                .build();

        DVCResponse response = dvcCloudClient.track(user, event);
    }
}
```

## EdgeDB

EdgeDB allows you to save user data to our EdgeDB storage so that you don't have to pass in all the user data every time you identify a user. Read more about [EdgeDB](/home/feature-management/edgedb/what-is-edgedb).

To get started, contact us at support@devcycle.com to enable EdgeDB for your project.

Once you have EdgeDB enabled in your project, pass in the enableEdgeDB option to turn on EdgeDB mode for the SDK:

```java
import com.devcycle.sdk.server.cloud.api.DVCCloudClient;
import com.devcycle.sdk.server.cloud.model.DVCCloudOptions;

import com.devcycle.sdk.server.common.model.User;
import com.devcycle.sdk.server.common.model.Variable;

User user = User.builder()
                .userId("test_user")
                .country("US")
                .email("example@example.com")
                .build();

User onlyUserId = User.builder()
                .userId("test_user");
                
DVCCloudOptions dvcCloudOptions = DVCCloudOptions.builder()
                .enableEdgeDB(true)
                .build();

private DVCCloudClient dvcCloudClient;

public MyClass() {
    dvcCloudClient = new DVCCloudClient("<DVC_SERVER_SDK_KEY>", dvcCloudOptions);

    Variable<Boolean> testBooleanVariable = dvcCloud.variable(user, "test-boolean-variable", false);

    Variable<String> onlyCountryVariable = dvcCloud.variable(onlyUserId, "test-string-country-variable", "Not Available");
}
```

This will send a request to our EdgeDB API to save the custom data under the user `test_user`.

In the example, Email and Country are associated to the user `test_user`. In your next identify call for the same `userId`, you may omit any of the data you've sent already as it will be pulled from the EdgeDB storage when segmenting to experiments and features.