# AEPOptimize Public APIs

This API reference guide provides usage information for the Optimize extension's public functions, classes and enums. 

## Static Functions

- [clearCachedPropositions](#clearCachedPropositions)
- [extensionVersion](#extensionVersion)
- [getPropositions](#getPropositions)
- [onPropositionsUpdate](#onPropositionsUpdate)
- [registerExtensions](#registerExtensions)
- [resetIdentities](#resetIdentities)
- [updatePropositions](#updatePropositions)
- [updatePropositionsWithCompletionHandler](#updatePropositionsWithCompletionHandler)

---

### clearCachedPropositions

This API clears out the client-side in-memory propositions cache.

#### Swift

##### Syntax

```swift
static func clearCachedPropositions()
```

##### Example

```swift
Optimize.clearCachedPropositions()
```

#### Objective-C

##### Syntax

```objc
+ (void) clearCachedPropositions;
```

##### Example

```objc
[AEPMobileOptimize clearCachedPropositions];
```

---

### extensionVersion

This property contains the version information for currently installed AEPOptimize extension.

#### Swift

##### Syntax

```swift
static var extensionVersion: String
```

##### Example

```swift
let extensionVersion = Optimize.extensionVersion
```

#### Objective-C

##### Syntax

```objc
+ (nonnull NSString*) extensionVersion;
```

##### Example

```objc
NSString *extensionVersion = [AEPMobileOptimize extensionVersion];
```

---

### getPropositions

This API retrieves the previously fetched propositions, for the provided decision scopes, from the in-memory extension propositions cache. The completion handler is invoked with the decision propositions corresponding to the given decision scopes. If a certain decision scope has not already been fetched prior to this API call, it will not be contained in the returned propositions.

#### Swift

##### Syntax

```swift
static func getPropositions(for decisionScopes: [DecisionScope], 
                            _ completion: @escaping ([DecisionScope: Proposition]?, Error?) -> Void)
```

##### Example

```swift
let decisionScope1 = DecisionScope(activityId: "xcore:offer-activity:1111111111111111", 
                                   placementId: "xcore:offer-placement:1111111111111111" 
                                   itemCount: 2)
let decisionScope2 = DecisionScope(name: "myScope")

Optimize.getPropositions(for: [decisionScope1, decisionScope2]) { propositionsDict, error in
  if let error = error {
    // handle error
    return
  }

  if let propositionsDict = propositionsDict {
    // get the propositions for the given decision scopes

    if let proposition1 = propositionsDict[decisionScope1] {
      // read proposition1 offers
    }

    if let proposition2 = propositionsDict[decisionScope2] {
      // read proposition2 offers
    }
  }
}
```

#### Objective-C

##### Syntax

```objc
+ (void) getPropositions: (NSArray<AEPDecisionScope*>* _Nonnull) decisionScopes 
              completion: (void (^ _Nonnull)(NSDictionary<AEPDecisionScope*, AEPProposition*>* _Nullable propositionsDict, NSError* _Nullable error)) completion;
```

##### Example

```objc
AEPDecisionScope* decisionScope1 = [[AEPDecisionScope alloc] initWithActivityId: @"xcore:offer-activity:1111111111111111" 
                                                                   placementId: @"xcore:offer-placement:1111111111111111" 
                                                                     itemCount: 2];
AEPDecisionScope* decisionScope2 = [[AEPDecisionScope alloc] initWithName: @"myScope"];

[AEPMobileOptimize getPropositions: @[decisionScope1, decisionScope2] 
                        completion: ^(NSDictionary<AEPDecisionScope*, AEPProposition*>* propositionsDict, NSError* error) {
  if (error != nil) {
    // handle error   
    return;
  }

  AEPProposition* proposition1 = propositionsDict[decisionScope1];
  // read proposition1 offers

  AEPProposition* proposition2 = propositionsDict[decisionScope2];
  // read proposition2 offers
}];
```

---

### onPropositionsUpdate

This API registers a permanent callback which is invoked whenever the Edge extension dispatches a response Event received from the Experience Edge Network upon a personalization query. The personalization query requests can be triggered by the `updatePropositions(for:withXdm:andData:)` API, Edge extension `sendEvent(experienceEvent:_:)` API or launch consequence rules.

#### Swift

##### Syntax

```swift
static func onPropositionsUpdate(perform action: @escaping ([DecisionScope: Proposition]?) -> Void)
```

##### Example

```swift
Optimize.onPropositionsUpdate { propositionsDict in
  if let propositionsDict = propositionsDict {
    // handle propositions
  }
}
```

#### Objective-C

##### Syntax

```objc
+ (void) onPropositionsUpdate: (void (^ _Nonnull)(NSDictionary<AEPDecisionScope*, AEPProposition*>* _Nullable)) action;
```

##### Example

```objc
[AEPMobileOptimize onPropositionsUpdate: ^(NSDictionary<AEPDecisionScope*, AEPProposition*>* propositionsDict) {
  // handle propositions
}];
```

---

### registerExtensions

This `MobileCore` API can be invoked to register the Optimize extension.

#### Swift

##### Syntax

```swift
static func registerExtensions(_ extensions: [NSObject.Type], 
                               _ completion: (() -> Void)? = nil)
```

##### Example

```swift
MobileCore.registerExtensions([Optimize.self, ...]) {
    // Processing upon registration completion
}
```

#### Objective-C

##### Syntax

```objc
+ (void) registerExtensions: (NSArray<Class*>* _Nonnull) extensions 
                 completion: (void (^ _Nullable)(void)) completion;
```

##### Example

```objc
[AEPMobileCore registerExtensions:@[AEPMobileOptimize.class, ...] completion:^{
  // Processing upon registration completion
}];
```

---

### resetIdentities

This `MobileCore` API can also be invoked to clear out the client-side data for Optimize extension, e.g. in-memory propositions cache.

#### Swift

##### Syntax

```swift
static func resetIdentities()
```

##### Example

```swift
MobileCore.resetIdentities()
```

#### Objective-C

##### Syntax

```objc
+ (void) resetIdentities;
```

##### Example

```objc
[AEPMobileCore resetIdentities];
```

---

### updatePropositions

This API dispatches an Event for the Edge network extension to fetch decision propositions, for the provided decision scopes array, from the decisioning services enabled in the Experience Edge. The returned decision propositions are cached in-memory in the Optimize SDK extension and can be retrieved using `getPropositions(for:_:)` API.

#### Swift

##### Syntax
```swift
static func updatePropositions(for decisionScopes: [DecisionScope], 
                               withXdm xdm: [String: Any]?,
                               andData data: [String: Any]? = nil)
```

##### Example
```swift
let decisionScope1 = DecisionScope(activityId: "xcore:offer-activity:1111111111111111", 
                                   placementId: "xcore:offer-placement:1111111111111111" 
                                   itemCount: 2)
let decisionScope2 = DecisionScope(name: "myScope")

Optimize.updatePropositions(for: [decisionScope1, decisionScope2] 
                            withXdm: ["xdmKey": "xdmValue"] 
                            andData: ["dataKey": "dataValue"])
```

#### Objective-C

##### Syntax
```objc
+ (void) updatePropositions: (NSArray<AEPDecisionScope*>* _Nonnull) decisionScopes 
                    withXdm: (NSDictionary<NSString*, id>* _Nullable) xdm
                    andData: (NSDictionary<NSString*, id>* _Nullable) data;
```

##### Example
```objc
AEPDecisionScope* decisionScope1 = [[AEPDecisionScope alloc] initWithActivityId: @"xcore:offer-activity:1111111111111111" 
                                                                   placementId: @"xcore:offer-placement:1111111111111111" 
                                                                     itemCount: 2];
AEPDecisionScope* decisionScope2 = [[AEPDecisionScope alloc] initWithName: @"myScope"];

[AEPMobileOptimize updatePropositions: @[decisionScope1, decisionScope2] 
                              withXdm: @{@"xdmKey": @"xdmValue"} 
                              andData: @{@"dataKey": @"dataValue"}];
```

---

### updatePropositionsWithCompletionHandler

This API dispatches an event for the Edge network extension to fetch decision propositions, for the provided decision scopes array, from the decisioning services enabled in the Experience Edge. The returned decision propositions are cached in-memory in the Optimize SDK extension and can be retrieved using `getPropositions` API.

Completion callback passed to `updatePropositions` supports network timeout and fatal errors returned by edge network along with fetched propositions data. The SDK's internal retry mechanism handles the recoverable HTTP errors. As a result, recoverable HTTP errors are not returned through this callback.

#### Swift

##### Syntax
```swift
static func updatePropositions(for decisionScopes: [DecisionScope], 
                               withXdm xdm: [String: Any]?,
                               andData data: [String: Any]? = nil,
                               _completion: (([DecisionScope: OptimizeProposition]?, Error?) -> Void)? = nil)
```

##### Example
```swift
let decisionScope1 = DecisionScope(activityId: "xcore:offer-activity:1111111111111111", 
                                   placementId: "xcore:offer-placement:1111111111111111" 
                                   itemCount: 2)
let decisionScope2 = DecisionScope(name: "myScope")

Optimize.updatePropositions(for: [decisionScope1, decisionScope2] 
                            withXdm: ["xdmKey": "xdmValue"] 
                            andData: ["dataKey": "dataValue"]){ data, error in
    if let error = error as? AEPOptimizeError {
                // handle error
    }
}
```

#### Objective-C

##### Syntax
```objc
+ (void) updatePropositions: (NSArray<AEPDecisionScope*>* _Nonnull) decisionScopes 
                    withXdm: (NSDictionary<NSString*, id>* _Nullable) xdm
                    andData: (NSDictionary<NSString*, id>* _Nullable) data  
                    completion: (void (^ _Nonnull)(NSDictionary<AEPDecisionScope*, AEPOptimizeProposition*>* _Nullable propositionsDict, NSError* _Nullable error)) completion;
```

##### Example
```objc
AEPDecisionScope* decisionScope1 = [[AEPDecisionScope alloc] initWithActivityId: @"xcore:offer-activity:1111111111111111" 
                                                                   placementId: @"xcore:offer-placement:1111111111111111" 
                                                                     itemCount: 2];
AEPDecisionScope* decisionScope2 = [[AEPDecisionScope alloc] initWithName: @"myScope"];

[AEPMobileOptimize updatePropositions: @[decisionScope1, decisionScope2] 
                              withXdm: @{@"xdmKey": @"xdmValue"} 
                              andData: @{@"dataKey": @"dataValue"}] 
                              completion: ^(NSDictionary<AEPDecisionScope*, AEPOptimizeProposition*>* propositionsDict, NSError* error) {
  if (error != nil) {
    // handle error
    return;
  }

  AEPOptimizeProposition* proposition1 = propositionsDict[decisionScope1];
  // read proposition1 offers

  AEPOptimizeProposition* proposition2 = propositionsDict[decisionScope2];
  // read proposition2 offers
}];
```

## Public classes

| Type | Swift | Objective-C |
| ---- | ----- | ----------- |
| class | `DecisionScope` | `AEPDecisionScope` |
| class | `Proposition` | `AEPProposition` |
| class | `Offer` | `AEPOffer` |
| class | `AEPOptimizeError` | `AEPOptimizeError` |

### DecisionScope

This class represents the decision scope which is used to fetch the decision propositions from the Edge decisioning services. The encapsulated scope name can also represent the Base64 encoded JSON string created using the provided activityId, placementId and itemCount.

```swift
/// `DecisionScope` class is used to create decision scopes for personalization query requests to Experience Edge Network.
@objc(AEPDecisionScope)
public class DecisionScope: NSObject, Codable {
    /// Decision scope name
    @objc public let name: String

    /// Creates a new decision scope using the given scope `name`.
    ///
    /// - Parameter name: string representation for the decision scope.
    @objc
    public init(name: String) {
        self.name = name
    }

    /// Creates a new decision scope using the given `activityId`, `placementId` and `itemCount`.
    ///
    /// This initializer creates a scope name by Base64 encoding the JSON string created using the provided data.
    ///
    /// If `itemCount` == 1, JSON string is
    ///
    ///     {"activityId":#activityId,"placementId":#placementId}
    /// otherwise,
    ///
    ///     {"activityId":#activityId,"placementId":#placementId,"itemCount":#itemCount}
    /// - Parameters:
    ///   - activityId: unique activity identifier for the decisioning activity.
    ///   - placementId: unique placement identifier for the decisioning activity offer.
    ///   - itemCount: number of offers to be returned from the server.
    @objc
    public convenience init(activityId: String, placementId: String, itemCount: UInt = 1) {
        let name = "\(activityId: activityId, placementId: placementId, itemCount: itemCount)".base64Encode()

        self.init(name: name ?? "")
    }
}
```

### Proposition

This class represents the decision propositions received from the decisioning services, upon a personalization query request to the Experience Edge network.

```swift
/// `Proposition` class
@objc(AEPProposition)
public class Proposition: NSObject, Codable {
    private let items: [Offer]

    /// Unique proposition identifier
    @objc public let id: String

    /// Array containing proposition decision options
    @objc public lazy var offers: [Offer] = {...}()

    /// Decision scope string
    @objc public let scope: String

    /// Scope details dictionary
    @objc public var scopeDetails: [String: Any]
}
```

The `Proposition` class extension provides a method for generating XDM data for Proposition Reference field group which can be used for proposition tracking.

```swift
/// `Proposition` extension
@objc
public extension Proposition {
    /// Creates a dictionary containing XDM formatted data for `Experience Event - Proposition Reference` field group from the given proposition.
    ///
    /// The Edge `sendEvent(experienceEvent:_:)` API can be used to dispatch this data in an Experience Event along with any additional XDM, free-form data, or override dataset identifier.
    ///
    /// - Note: The returned XDM data does not contain an `eventType` for the Experience Event.
    /// - Returns A dictionary containing XDM data for the propositon reference.
    func generateReferenceXdm() -> [String: Any] {...}
}
```

### Offer

This class represents the proposition option received from the decisioning services, upon a personalization query to the Experience Edge network.

```swift
/// `Offer` class
@objc(AEPOffer)
public class Offer: NSObject, Codable {
    /// Unique Offer identifier
    @objc public let id: String

    /// Offer revision detail at the time of the request
    @objc public let etag: String

    /// Offer priority score
    @objc public let score: Double

    /// Offer schema string
    @objc public let schema: String

    /// Offer metadata
    @objc public let meta: [String: Any]?

    /// Offer type as represented in enum `OfferType`
    @objc public let type: OfferType

    /// Optional Offer language array
    @objc public let language: [String]?

    /// Offer content string
    @objc public let content: String

    /// Optional Offer characteristics dictionary
    @objc public let characteristics: [String: String]?

    /// Weak reference to Proposition instance
    @objc weak var proposition: Proposition?
}
```

The `Offer` class extension provides methods for generating XDM data for Proposition Interactions field group which can be used for proposition tracking. It also contains direct methods for  tracking proposition display and tap interactions.

```swift
/// `Offer` extension
@objc
public extension Offer {
    /// Creates a dictionary containing XDM formatted data for `Experience Event - Proposition Interactions` field group from the given proposition option.
    ///
    /// The Edge `sendEvent(experienceEvent:_:)` API can be used to dispatch this data in an Experience Event along with any additional XDM, free-form data, or override dataset identifier.
    /// If the proposition reference within the option is released and no longer valid, the method returns `nil`.
    ///
    /// - Note: The returned XDM data also contains the `eventType` for the Experience Event with value `display`.
    /// - Returns A dictionary containing XDM data for the propositon interactions.
    func generateDisplayInteractionXdm() -> [String: Any]? {...}

    /// Creates a dictionary containing XDM formatted data for `Experience Event - Proposition Interactions` field group from the given proposition option.
    ///
    /// The Edge `sendEvent(experienceEvent:_:)` API can be used to dispatch this data in an Experience Event along with any additional XDM, free-form data, or override dataset identifier.
    /// If the proposition reference within the option is released and no longer valid, the method returns `nil`.
    ///
    /// - Note: The returned XDM data also contains the `eventType` for the Experience Event with value `click`.
    /// - Returns A dictionary containing XDM data for the propositon interactions.
    func generateTapInteractionXdm() -> [String: Any]? {...}

    /// Dispatches an event for the Edge extension to send an Experience Event to the Edge network with the display interaction data for the given proposition item.
    func displayed() {...}

    /// Dispatches an event for the Edge extension to send an Experience Event to the Edge network with the tap interaction data for the given proposition item.
    func tapped() {...}
}
```

### AEPOptimizeError

This class represents the error details returned by the Edge Network while fetching propositions.

Error details received from Edge response along with AEPError object returned with values:

* _AEPError.callbackTimeout_ is returned when request timeout without any response.
* _AEPError.serverErrors_ is returned for HTTP Status 500.
* _AEPError.invalidRequest_ is returned for HTTP Status 400 - 499 (except 408 and 429).

```swift
@objc(AEPOptimizeError)
public class AEPOptimizeError: NSObject, Error {
    // This is a URI reference (RFC3986) that identifies the problem type  
    public let type: String?

    // This is the HTTP status code generated by the server for this occurrence of the problem.
    public let status: Int?

    // This is a short, human-readable summary of the problem type.
    public let title: String?

    // This is human-readable description of the problem type.
    public let detail: String?

    // This is a map of additional properties that aid in debugging such as the request ID or the org ID. In some cases, it might contain data specific to the error at hand, such as a list of validation errors.
    public let report: [String: Any]?

    // This ia a mandatory AEPError representing the high level error status
    public var aepError = AEPError.unexpected

    // Initializer for AEPOptimizeError based based on the Error details returned by Edge respose
    public init(type: String?, status: Int?, title: String?, detail: String?, aepError: AEPError? = nil) {...}
}
```

## Public enums

| Type | Swift | Objective-C |
| ---- | ----- | ----------- |
| enum | `OfferType` | `AEPOfferType` |

### OfferType

An enum indicating the type of an Offer, derived from the proposition item `format` field in personalization query response.

```swift
/// Enum representing the supported Offer Types.
@objc(AEPOfferType)
public enum OfferType: Int, Codable {
    /// Unknown Offer type
    case unknown = 0

    /// JSON Offer
    case json = 1

    /// Plain text Offer
    case text = 2

    /// Html Offer
    case html = 3

    /// Image Offer
    case image = 4

    /// Initializes OfferType with the provided format string.
    /// - Parameter format: Offer format string
    init(from format: String) {
        switch format {
        case "application/json":
            self = .json

        case "text/plain":
            self = .text

        case "text/html":
            self = .html

        default:
            self = format.starts(with: "image/") ? .image : .unknown
        }
    }
}
```