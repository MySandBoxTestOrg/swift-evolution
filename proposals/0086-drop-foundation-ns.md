# Drop NS Prefix in Swift Foundation

* Proposal: [SE-0086](https://github.com/apple/swift-evolution/blob/master/proposals/0086-drop-foundation-ns.md)
* Authors: Tony Parker <anthony.parker@apple.com>, Philippe Hausler <phausler@apple.com>
* Status: **Under Review** (May 9...16, 2016)
* Review manager:  [Doug Gregor](https://github.com/DougGregor)

##### Related radars or Swift bugs

* [SE-0069](https://github.com/apple/swift-evolution/blob/master/proposals/0069-swift-mutability-for-foundation.md): Swift Mutability for Foundation

##### Revision history

* **v1** Initial version

## Introduction

As part of _Swift 3 API Naming_ and the introduction of _Swift Core Libraries_, we are dropping the `NS` prefix from key Foundation types in Swift.

[Swift Evolution Discussion Thread](http://thread.gmane.org/gmane.comp.lang.swift.evolution/16298)

[Review Thread](http://thread.gmane.org/gmane.comp.lang.swift.evolution/16509)

## Motivation

A large proportion of the value that comes from using many programming languages derives from the libraries that ship with the compiler. These libraries provide common functionality, which in turn establish common design patterns for software written in the language. When a strong foundation of patterns and types is established, all higher level libraries and applications benefit. Code can easily interact with other libraries without awkward translation layers or annoying impedance mismatches.

With Swift, we established the most fundamental of these libraries as the _Swift Standard Library_. The standard library provides the most important and fundamental data structures and is intentionally kept small and focused. To provide a higher level of functionality than the standard library could (or should) include, we established another project called the _Swift Core Libraries_. These libraries are important enough to include on all platforms that Swift supports:

* Unit testing (**swift-corelibs-xctest**)
* Scheduling, multithreading, and locking primitives (**swift-corelibs-dispatch**)
* Internationalization, localization, additional model types, and portability (**swift-corelibs-foundation**)

We believe that the best way to establish these libraries as _fundamental_ and _native_ Swift libraries is to work towards making their naming style match the convention established by the standard library.

The first step was establishing naming conventions:

* [Swift 3 API Naming Guidelines](https://swift.org/documentation/api-design-guidelines/)
* [SE-0023: API Design Guidelines](https://github.com/apple/swift-evolution/blob/master/proposals/0023-api-guidelines.md)

The second step was adjusting standard library API and importing the Cocoa SDK according to those conventions:

* [SE-0006: Apply API Guidelines to the Standard Library](https://github.com/apple/swift-evolution/blob/master/proposals/0006-apply-api-guidelines-to-the-standard-library.md)
* [SE-0005: Better Translation of Objective-C APIs Into Swift](https://github.com/apple/swift-evolution/blob/master/proposals/0005-objective-c-name-translation.md)

The next step is to adjust the API of the Swift Core Libraries. This proposal is focused on **swift-corelibs-foundation**.

In addition to adopting the guidelines for method names, the names of the fundamental types should follow the spirit of the guidelines too. The type names should be clear, concise, and omit needless words or prefixes. In combination with adopting Swift semantics for many of these types ([SE-0069](https://github.com/apple/swift-evolution/blob/master/proposals/0069-swift-mutability-for-foundation.md)), and continued improvement to the implementations, this will make core library API feel like it belongs to the Swift language instead of like a foreign invader.

> Note: All changes proposed are for Swift only; Objective-C has no change.

## Proposed solution

We propose the following set of rules for deciding if the `NS` prefix should be dropped:

0. If the class is specifically for Objective-C, or inherently tied to the Objective-C runtime and `NS` namespace, keep `NS` prefix. Examples: `NSObject`, `NSAutoreleasePool`, `NSException`, `NSProxy`.
0. If the class is platform-specific, keep the `NS` prefix. Many of these types are located in Foundation, but actually belong to the namespace of a higher-level framework like AppKit or UIKit. The higher level frameworks are keeping their prefixes, so these types should match. Examples: `NSUserNotification`, `NSBackgroundActivity`, `NSXPCConnection`.
0. If the class has a value-type equivalent, then keep the `NS` prefix, per [SE-0069](https://github.com/apple/swift-evolution/blob/master/proposals/0069-swift-mutability-for-foundation.md). Examples: `NSArray`, `NSString`, `NSPersonNameComponents`. 

> Note: As opposed to the original proposal from the beginning of Swift 3, this proposal narrows the scope of prefix-dropped types and takes advantage of Swift's ability to nest types to avoid polluting the global namespace with very general names.

## Detailed design

### Drop `NS` prefix

The following types and symbols will drop their `NS` prefix in Swift.

Objective-C Name | Swift Name | Note
---------- | ---------- | ----
NSAttributedString | AttributedString | See note at bottom about future value types
NSBlockOperation | BlockOperation |
NSBundle | Bundle |
NSByteCountFormatter | ByteCountFormatter |
NSCache | Cache |
NSCacheDelegate | CacheDelegate |
NSCachedURLResponse | CachedURLResponse |
NSCalendar | Calendar |
NSComparisonPredicate | ComparisonPredicate |
NSComparisonResult | ComparisonResult |
NSCompoundPredicate | CompoundPredicate |
NSCoder | Coder |
NSCoding | Coding |
NSCondition | Condition |
NSConditionLock | ConditionLock |
NSDateComponentsFormatter | DateComponentsFormatter |
NSDateFormatter | DateFormatter |
NSDateIntervalFormatter | DateIntervalFormatter |
NSDiscardableContent | DiscardableContent |
NSDistributedLock | DistributedLock |
NSDistributedNotificationCenter | DistributedNotificationCenter |
NSEnergyFormatter | EnergyFormatter |
NSFileHandle | FileHandle |
NSFileManager | FileManager |
NSFileManagerDelegate | FileManagerDelegate |
NSFileWrapper | FileWrapper |
NSFormatter | Formatter |
NSHashTable | HashTable |
NSHost | Host |
NSHTTPCookie | HTTPCookie |
NSHTTPCookieStorage | HTTPCookieStorage |
NSHTTPURLResponse | HTTPURLResponse |
NSInputStream | InputStream |
NSJSONSerialization | JSONSerialization |
NSKeyedArchiver | KeyedArchiver |
NSKeyedArchiverDelegate | KeyedArchiverDelegate |
NSKeyedUnarchiver | KeyedUnarchiver |
NSKeyedUnarchiverDelegate | KeyedUnarchiverDelegate |
NSLengthFormatter | LengthFormatter |
NSLocale | Locale |
NSLock | Lock |
NSLocking | Locking |
NSMapTable | MapTable |
NSMassFormatter | MassFormatter |
NSMessagePort | MessagePort |
NSMutableURLRequest | MutableURLRequest |
NSNetService | NetService |
NSNetServiceBrowser | NetServiceBrowser |
NSNetServiceBrowserDelegate | NetServiceBrowserDelegate |
NSNetServiceDelegate | NetServiceDelegate |
NSNotificationCenter | NotificationCenter |
NSNotificationName | NotificationName |
NSNotificationQueue | NotificationQueue |
NSNumberFormatter | NumberFormatter |
NSOperatingSystemVersion | OperatingSystemVersion |
NSOperation | Operation |
NSOperationQueue | OperationQueue |
NSOutputStream | OutputStream | The Swift standard library has a type named `OutputStream` which will be renamed to `OutputStreamable`.
NSPersonNameComponentsFormatter | PersonNameComponentsFormatter |
NSPipe | Pipe |
NSPointerArray | PointerArray |
NSPointerFunctions | PointerFunctions |
NSPort | Port |
NSPortDelegate | PortDelegate |
NSPortMessage | PortMessage |
NSPredicate | Predicate |
NSProcessInfo | ProcessInfo |
NSProgress | Progress |
NSProgressReporting | ProgressReporting |
NSPropertyListSerialization | PropertyListSerialization |
NSPurgeableData | PurgeableData |
NSQualityOfService | QualityOfService |
NSRecursiveLock | RecursiveLock |
NSRegularExpression | RegularExpression |
NSRunLoop | RunLoop |
NSScanner | Scanner |
NSSocketNativeHandle | SocketNativeHandle |
NSSocketPort | SocketPort |
NSSortDescriptor | SortDescriptor |
NSStream | Stream |
NSStreamDelegate | StreamDelegate |
NSTask | Task |
NSTextCheckingResult | TextCheckingResult |
NSThread | Thread |
NSTimeInterval | TimeInterval |
NSTimer | Timer |
NSTimeZone | TimeZone |
NSUndoManager | UndoManager |
NSURLAuthenticationChallenge | URLAuthenticationChallenge |
NSURLAuthenticationChallengeSender | URLAuthenticationChallengeSender |
NSURLCache | URLCache |
NSURLCredential | URLCredential |
NSURLCredentialStorage | URLCredentialStorage |
NSURLProtectionSpace | URLProtectionSpace |
NSURLProtocol | URLProtocol |
NSURLProtocolClient | URLProtocolClient |
NSURLRequest | URLRequest |
NSURLResponse | URLResponse |
NSURLSession | URLSession |
NSURLSessionConfiguration | URLSessionConfiguration |
NSURLSessionDataDelegate | URLSessionDataDelegate |
NSURLSessionDataTask | URLSessionDataTask |
NSURLSessionDelegate | URLSessionDelegate |
NSURLSessionDownloadDelegate | URLSessionDownloadDelegate |
NSURLSessionDownloadTask | URLSessionDownloadTask |
NSURLSessionStreamDelegate | URLSessionStreamDelegate |
NSURLSessionStreamTask | URLSessionStreamTask |
NSURLSessionTask | URLSessionTask |
NSURLSessionTaskDelegate | URLSessionTaskDelegate |
NSURLSessionUploadTask | URLSessionUploadTask |
NSUserDefaults | UserDefaults |
NSXMLDocument | XMLDocument |
NSXMLDTD | XMLDTD |
NSXMLDTDNode | XMLDTDNode |
NSXMLElement | XMLElement |
NSXMLNode | XMLNode |
NSXMLParser | XMLParser |
NSXMLParserDelegate | XMLParserDelegate | 

### Hoisted types

The following types will be lifted up into a class container as a sub-type.

Old Name | New Name | Note
---------- | ---------- | ----
NSActivityOptions | ProcessInfo.ActivityOptions |
NSAppleEventSendOptions | NSAppleEventDescriptor.SendOptions |
NSAttributedStringEnumerationOptions | AttributedString.EnumerationOptions |
NSBackgroundActivityResult | NSBackgroundActivityScheduler.Result |
NSByteCountFormatterCountStyle | ByteCountFormatter.CountStyle |
NSByteCountFormatterUnits | ByteCountFormatter.Units |
NSCalculationError | Decimal.CalculationError |
NSCalendarOptions | Calendar.Options |
NSCalendarUnit | Calendar.Unit |
NSComparisonPredicateModifier | ComparisonPredicate.Modifier |
NSComparisonPredicateOptions | ComparisonPredicate.Options |
NSCompoundPredicateType | CompoundPredicate.LogicalType |
NSDataBase64DecodingOptions | NSData.Base64DecodingOptions | `Data` will have a `typealias`
NSDataBase64EncodingOptions | NSData.Base64EncodingOptions | `Data` will have a `typealias`
NSDataReadingOptions | NSData.ReadingOptions | `Data` will have a `typealias`
NSDataSearchOptions | NSData.SearchOptions | `Data` will have a `typealias`
NSDataWritingOptions | NSData.WritingOptions | `Data` will have a `typealias`
NSDateFormatterBehavior | DateFormatter.Behavior |
NSDateFormatterStyle | DateFormatter.Style |
NSDateIntervalFormatterStyle | DateIntervalFormatter.Style |
NSDecodingFailurePolicy | Coder.DecodingFailurePolicy |
NSDirectoryEnumerationOptions | FileManager.DirectoryEnumerationOptions |
NSDistributedNotificationOptions | DistributedNotificationCenter.Options |
NSEnergyFormatterUnit | EnergyFormatter.Unit |
NSExpressionType | NSExpression.ExpressionType |
NSFileCoordinatorReadingOptions | FileCoordinator.ReadingOptions |
NSFileCoordinatorWritingOptions | FileCoordinator.WritingOptions |
NSFileManagerItemReplacementOptions | FileManager.ItemReplacementOptions |
NSFileManagerUnmountOptions | FileManager.UnmountOptions |
NSFileVersionAddingOptions | FileVersion.AddingOptions |
NSFileVersionReplacingOptions | FileVersion.ReplacingOptions |
NSFileWrapperReadingOptions | FileWrapper.ReadingOptions |
NSFileWrapperWritingOptions | FileWrapper.WritingOptions |
NSFormattingContext | Formatter.Context |
NSFormattingUnitStyle | Formatter.UnitStyle |
NSHTTPCookieAcceptPolicy | HTTPCookie.AcceptPolicy |
NSInsertionPosition | NSPositionalSpecifier.InsertionPosition |
NSItemProviderErrorCode | NSItemProvider.ErrorCode |
NSJSONReadingOptions | JSONSerialization.ReadingOptions |
NSJSONWritingOptions | JSONSerialization.WritingOptions |
NSLengthFormatterUnit | LengthFormatter.Unit |
NSLinguisticTaggerOptions | NSLinguisticTagger.Options |
NSLocaleLanguageDirection | Locale.LanguageDirection |
NSMachPortOptions | NSMachPort.Options |
NSMassFormatterUnit | MassFormatter.Unit |
NSMatchingFlags | RegularExpression.MatchingFlags |
NSMatchingOptions | RegularExpression.MatchingOptions |
NSMeasurementFormatterUnitOptions | MeasurementFormatter.UnitOptions |
NSNetServiceOptions | NetService.Options |
NSNetServicesError | NetService.ErrorCode |
NSNotificationCoalescing | NotificationQueue.NotificationCoalescing |
NSNotificationSuspensionBehavior | DistributedNotificationCenter.SuspensionBehavior |
NSNumberFormatterBehavior | NumberFormatter.Behavior |
NSNumberFormatterPadPosition | NumberFormatter.PadPosition |
NSNumberFormatterRoundingMode | NumberFormatter.RoundingMode |
NSNumberFormatterStyle | NumberFormatter.Style |
NSOperationQueuePriority | Operation.QueuePriority |
NSPersonNameComponentsFormatterOptions | PersonNameComponentsFormatter.Options |
NSPersonNameComponentsFormatterStyle | PersonNameComponentsFormatter.Style |
NSPointerFunctionsOptions | PointerFunctions.Options |
NSPostingStyle | NotificationQueue.PostingStyle |
NSPredicateOperatorType | ComparisonPredicate.Operator |
NSProcessInfoThermalState | ProcessInfo.ThermalState |
NSPropertyListFormat | PropertyListSerialization.PropertyListFormat |
NSPropertyListMutabilityOptions | PropertyListSerialization.MutabilityOptions |
NSPropertyListReadOptions | PropertyListSerialization.ReadOptions |
NSPropertyListWriteOptions | PropertyListSerialization.WriteOptions |
NSRegularExpressionOptions | RegularExpression.Options |
NSRelativePosition | NSRelativeSpecifier.RelativePosition |
NSSearchPathDirectory | FileManager.SearchPathDirectory |
NSSearchPathDomainMask | FileManager.SearchPathDomainMask |
NSStreamEvent | Stream.Event |
NSStreamStatus | Stream.Status |
NSStringCompareOptions | NSString.CompareOptions | Also on `String`. See below for more information.
NSStringEncoding | NSString.Encoding | Also on `String`. See below for more information.
NSStringEncodingConversionOptions | NSString.EncodingConversionOptions | Also on `String`. See below for more information.
NSStringEnumerationOptions | NSString.EnumerationOptions |
NSTaskTerminationReason | Task.TerminationReason |
NSTestComparisonOperation | NSSpecifierTest.TestComparisonOperation |
NSTextCheckingType | TextCheckingResult.CheckingType |
NSTimeZoneNameStyle | TimeZone.NameStyle |
NSURLCacheStoragePolicy | URLCache.StoragePolicy |
NSURLCredentialPersistence | URLCredential.Persistence |
NSURLRelationship | FileManager.URLRelationship |
NSURLRequestCachePolicy | URLRequest.CachePolicy |
NSURLRequestNetworkServiceType | URLRequest.NetworkServiceType |
NSURLSessionAuthChallengeDisposition | URLSession.AuthChallengeDisposition |
NSURLSessionResponseDisposition | URLSession.ResponseDisposition |
NSURLSessionTaskState | URLSessionTask.State |
NSUserNotificationActivationType | NSUserNotification.ActivationType |
NSVolumeEnumerationOptions | FileManager.VolumeEnumerationOptions |
NSWhoseSubelementIdentifier | NSWhoseSpecifier.SubelementIdentifier |
NSXMLDocumentContentKind | XMLDocument.ContentKind |
NSXMLDTDNodeKind | XMLDTDNode.Kind |
NSXMLNodeKind | XMLNode.Kind |
NSXMLParserError | XMLParser.ErrorCode |
NSXMLParserExternalEntityResolvingPolicy | XMLParser.ExternalEntityResolvingPolicy |
NSXPCConnectionOptions | NSXPCConnection.Options |

### Updated enumerations

`NSExpressionType` enumeration members drop their suffix.

```swift
extension Expression {
	public enum ExpressionType : UInt {
        case constantValue
        case evaluatedObject
        case variable
        case keyPath
        case function
        case unionSet
        case intersectSet
        case minusSet
        case subquery
        case aggregate
        case anyKey
        case block
        @available(OSX 10.11, iOS 9.0, *)
        case conditional
    }
}
```

Enumerations associated with `ComparisonPredicate` drop their suffix.

```swift
extension ComparisonPredicate {
    public struct Options : OptionSet {
        public init(rawValue: UInt)
        public static var caseInsensitive: ComparisonPredicate.Options { get }
        public static var diacriticInsensitive: ComparisonPredicate.Options { get }
        public static var normalized: ComparisonPredicate.Options { get }
    }
    
    public enum Modifier : UInt {
        case direct
        case all
        case any
    }

    public enum Operator : UInt {
        case lessThan
        case lessThanOrEqualTo
        case greaterThan
        case greaterThanOrEqualTo
        case equalTo
        case notEqualTo
        case matches
        case like
        case beginsWith
        case endsWith
        case `in`
        case customSelector
        case contains
        case between
    }
}

```

`NSDateFormatterStyle` and `NSDateIntervalFormatterStyle` will drop the style suffix. `no` will be renamed to `none`.

```swift
extension DateIntervalFormatter {
    @available(OSX 10.10, iOS 8.0, *)
    public enum Style : UInt {
        case none
        case short
        case medium
        case long
        case full
    }
}
```

`NSNumberFormatterStyle` will drop the style suffix. `no` will be renamed to `none`

```swift
extension NumberFormatter {
   public enum Style : UInt {
        case none
        case decimal
        case currency
        case percent
        case scientific
        case spellOut
        @available(OSX 10.11, iOS 9.0, *)
        case ordinal
        @available(OSX 10.11, iOS 9.0, *)
        case currencyISOCode
        @available(OSX 10.11, iOS 9.0, *)
        case currencyPlural
        @available(OSX 10.11, iOS 9.0, *)
        case currencyAccounting
    }
}
```

`NSXMLDocumentContentKind`, `NSXMLDTDNodeKind` and `NSXMLNodeKind` will be renamed.

```swift
extension XMLDocument {
    public enum ContentKind : UInt {
        case xml
        case xhtml
        case html
        case text
    }
}

extension XMLDTDNode {
    public enum Kind : UInt {
        case generalEntity
        case parsedEntity
        case unparsedEntity
        case parameterEntity
        case predefinedEntity
        case cdataAttribute
        case idAttribute
        case idRefAttribute
        case idRefsAttribute
        case entityAttribute
        case entitiesAttribute
        case nmTokenAttribute
        case nmTokensAttribute
        case enumerationAttribute
        case notationAttribute
        case undefinedElementDeclaration
        case emptyElementDeclaration
        case anyElementDeclaration
        case mixedElementDeclaration
        case elementDeclarationElement
    }
}

extension XMLNode {
    public enum Kind : UInt {
        case invalid
        case document
        case element
        case attribute
        case namespace
        case processingInstruction
        case comment
        case text
        case dtd
        case entityDeclaration
        case attributeDeclaration
        case elementDeclaration
        case notationDeclaration
    }
}
```

### Keep `NS` prefix

Classes and types not enumerated above will keep their `NS` prefix. Future API will be decided on a case-by-case basis following the rules outlined above.

### NSStringEncoding

`NSStringEncoding` has a number of free floating constants which will be renamed into members of a `RawRepresentable` structure named `String.Encoding`.

Previously the API was exposed as:

```swift
public typealias NSStringEncoding = UInt
public var NSASCIIStringEncoding: UInt { get }
public var NSNEXTSTEPStringEncoding: UInt { get }
public var NSJapaneseEUCStringEncoding: UInt { get }
public var NSUTF8StringEncoding: UInt { get }
public var NSISOLatin1StringEncoding: UInt { get }
public var NSSymbolStringEncoding: UInt { get }
public var NSNonLossyASCIIStringEncoding: UInt { get }
public var NSShiftJISStringEncoding: UInt { get }
public var NSISOLatin2StringEncoding: UInt { get }
public var NSUnicodeStringEncoding: UInt { get }
public var NSWindowsCP1251StringEncoding: UInt { get }
public var NSWindowsCP1252StringEncoding: UInt { get }
public var NSWindowsCP1253StringEncoding: UInt { get }
public var NSWindowsCP1254StringEncoding: UInt { get }
public var NSWindowsCP1250StringEncoding: UInt { get }
public var NSISO2022JPStringEncoding: UInt { get }
public var NSMacOSRomanStringEncoding: UInt { get }
public var NSUTF16StringEncoding: UInt { get }
public var NSUTF16BigEndianStringEncoding: UInt { get }
public var NSUTF16LittleEndianStringEncoding: UInt { get }
public var NSUTF32StringEncoding: UInt { get }
public var NSUTF32BigEndianStringEncoding: UInt { get }
public var NSUTF32LittleEndianStringEncoding: UInt { get }
```

With the renaming change `String.Encoding` will be exposed as:

```swift
extension String {
	public struct Encoding : RawRepresentable {
		public var rawValue: UInt
		public init(rawValue: UInt)
		
		public static var ascii { get }
		public static var nextstep { get }
		public static var japaneseEUC { get }
		public static var utf8 { get }
		public static var isoLatin1 { get }
		public static var symbol { get }
		public static var nonLossyASCII { get }
		public static var shiftJIS { get }
		public static var isoLatin2 { get }
		public static var unicode { get }
		public static var windowsCP1251 { get }
		public static var windowsCP1252 { get }
		public static var windowsCP1253 { get }
		public static var windowsCP1254 { get }
		public static var windowsCP1250 { get }
		public static var iso2022JP { get }
		public static var macOSRoman { get }
		public static var utf16 { get }
		public static var utf16BigEndian { get }
		public static var utf16LittleEndian { get }
		public static var utf32 { get }
		public static var utf32BigEndian { get }
		public static var utf32LittleEndian { get }
	}
}
```

## Impact on existing code

All Swift projects will have to run through a migration step to use the new names.

## Alternatives considered

### Drop every `NS` in Foundation

We considered simply dropping the prefix from all types. However, this would cause quite a few conflicts with standard library types. Also, although Foundation's framework boundary is an easy place to programmatically draw the line for the drop-prefix behavior, the reality is that Foundation has API that feels like it belongs to higher level frameworks as well. We believe this approach better identifies the best candidates for dropping the prefix.

### Keep `NS` on future value types

While it is tempting to consider leaving the `NS` prefix on some names to "leave room" for future types, we believe we can instead mitigate any issue of possible future conflict with a variety of techniques. There are clear benefits for this proposal today: 

* We have a consistent set of rules about which types keep the prefix and which ones drop it. 
* There is no requirement to attempt prediction of all potential future API. Even seemingly straightforward changes now may take on a different shape given future language improvements or lessons learned.
* We believe the Foundation library and API adds real value today, and supports the goals outlined for the `corelibs` project.

If, in the future, a time comes when we wish to introduce a new concept that may want to use an unprefixed Foundation name, we have many options available to us:

* Code migration
* Extend existing name with new API
* Deprecation of existing name and replacement with new name
* Use a different name

We have used all of these techniques successfully in the past for Objective-C and Swift API.