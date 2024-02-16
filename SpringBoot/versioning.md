# Versioning the API
To future proof your API it is best to provide the ability to specify a version for your API.
That way if your requirements change and you need to the way the request is specified or the
values returned by a request you can allow users to use the old version or the new version
of the API by specifying the version they require.

It is best to design your API withi versioning in mind.

There are four ways to specify versions for your API.

|Description|Example|
|----|----|
|As part of the URL|https://example.com:8080/<version>/<request_name>|
|As a query parameter of the URL|https://example.com:8080/<request_name>?version=<version>|
|Header Versioning|X-API-VERSION=<version>|
|Content Versioning|Accept:application/vnd.company.app-v<version>+json|

## URI

```java
    @GetParam("/v1/name")
    public NameV1 getNameURIVersion1() {
        return new NameV1("Seth Fuller");
    }

    @GetParam("/v2/name")
    public NameV2 getNameURIVersion2() {
        return new NameV2("Seth", "Fuller");
    }
```

#### Request

## Query Parameter

```java
    @GetParam(path="/name", params="version=1")
    public NameV1 getNameRequestParamVersion1() {
        return new NameV1("Seth Fuller");
    }

    @GetParam(path="/name", params="version=2")
    public NameV2 getNameRequestParamVersion2() {
        return new NameV2("Seth", "Fuller");
    }
```

#### Request

## Header

### Custom Header

```java
    @GetParam(path="/name", headers="X-API-VERSION=1")
    public NameV1 getNameRequestHeaderVersion1() {
        return new NameV1("Seth Fuller");
    }

    @GetParam(path="/name", headers="X-API-VERSION=2")
    public NameV2 getNameRequestHeaderVersion2() {
        return new NameV2("Seth", "Fuller");
    }
```

#### Request

### Content Negotiation (Accept Header)

```java
    @GetParam(path="/name/content", produces="application/vnd.company.app-v1+json")
    public NameV1 getNameRequestAcceptVersion1() {
        return new NameV1("Seth Fuller");
    }

    @GetParam(path="/name/content", produces="application/vnd.company.app-v1+json")
    public NameV2 getNameRequestAcceptVersion2() {
        return new NameV2("Seth", "Fuller");
    }
```

#### Request

