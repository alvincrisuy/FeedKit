# FeedKit

An RSS, Atom and JSON Feed parser written in Swift

[![build status](https://travis-ci.org/nmdias/FeedKit.svg)](https://travis-ci.org/nmdias/FeedKit)
[![cocoapods compatible](https://img.shields.io/badge/cocoapods-compatible-brightgreen.svg)](https://cocoapods.org/pods/FeedKit)
[![carthage compatible](https://img.shields.io/badge/carthage-compatible-brightgreen.svg)](https://github.com/Carthage/Carthage)
[![language](https://img.shields.io/badge/spm-compatible-brightgreen.svg)](https://swift.org)
[![release](https://img.shields.io/badge/release-6.1.0-orange.svg)](https://github.com/nmdias/FeedKit/releases)

## Features

- [x] [Atom](https://tools.ietf.org/html/rfc4287)
- [x] [RSS2](http://cyber.law.harvard.edu/rss/rss.html)
- [x] [JSON](https://jsonfeed.org/version/1)  
- [x] Namespaces
    - [x] [Dublin Core](http://web.resource.org/rss/1.0/modules/dc/)
    - [x] [Syndication](http://web.resource.org/rss/1.0/modules/syndication/)
    - [x] [Content](http://web.resource.org/rss/1.0/modules/content/)
    - [x] [Media RSS](http://www.rssboard.org/media-rss)
    - [x] [iTunes Podcasting Tags](https://help.apple.com/itc/podcasts_connect/#/itcb54353390)
- [x] Dates Support
    - [x] [RFC822](https://www.ietf.org/rfc/rfc0822.txt)
    - [x] [RFC3999](https://www.ietf.org/rfc/rfc3339.txt)
    - [x] [ISO8601](http://www.w3.org/TR/NOTE-datetime)
- [x] [Documentation](http://cocoadocs.org/docsets/FeedKit)
- [x] Unit Test Coverage

## Requirements

![xcode](https://img.shields.io/badge/xcode-8.0%2b-lightgrey.svg)
![ios](https://img.shields.io/badge/ios-8.0%2b-lightgrey.svg)
![tvos](https://img.shields.io/badge/tvos-9.0%2b-lightgrey.svg)
![watchos](https://img.shields.io/badge/watchos-2.0%2b-lightgrey.svg)
![mac os](https://img.shields.io/badge/mac%20os-10.9%2b-lightgrey.svg)
![mac os](https://img.shields.io/badge/ubuntu-16.04+-lightgrey.svg)

Installation >> [`instructions`](https://github.com/nmdias/FeedKit/blob/master/INSTALL.md) <<

## Usage

Build a URL pointing to an RSS, Atom or JSON Feed.
```swift
let feedURL = URL(string: "http://images.apple.com/main/rss/hotnews/hotnews.rss")!
```

FeedKit will do asynchronous parsing on the main queue by default. You can safely update your UI from within the result closure.
```swift
FeedParser(URL: feedURL)?.parseAsync { result in
    
    switch result {
    case let .atom(feed):       break
    case let .rss(feed):        break
    case let .json(feed):       break
    case let .failure(error):   break
    }

}
```     

If a different queue is specified, you are responsible to manually bring the result closure to whichever queue is apropriate. Usually `DispatchQueue.main.async`. If you're unsure, don't provide the `queue` parameter.
```swift
FeedParser(URL: feedURL)?.parseAsync(queue: myQueue, result: { (result) in 
    // Do your thing
})
```

Alternatively, you can also parse `synchronously`.
```swift
FeedParser(URL: feedURL)?.parse()
```

#### Initializers

> An aditional initializer can be found for `Data` objects.
```swift
FeedParser(data: data)
```

#### Feed Models
FeedKit provides `strongly typed` models for `RSS`, `Atom` and `JSON Feed` formats.    
```swift
result.rssFeed      // Really Simple Syndication Feed Model
result.atomFeed     // Atom Syndication Format Feed Model
result.jsonFeed     // JSON Feed Model
```


#### Parsing Success
You can check if a Feed was `successfully` parsed or not.
```swift
result.isSuccess    // If parsing was a success
result.isFailure    // If parsing failed
result.error        // An error, if any
```

### Model Preview
Safely bind a feed of your choosing:
```swift
guard let feed = result.rssFeed, result.isSuccess else {
    print(result.error)
    return
}
```
Then go through it's properties. The RSS, Atom and JSON Feed Models are rather extensive. These are just a preview.
#### RSS

```swift
feed.title
feed.link
feed.description
feed.language
feed.copyright
feed.managingEditor
feed.webMaster
feed.pubDate
feed.lastBuildDate
feed.categories
feed.generator
feed.docs
feed.cloud
feed.rating
feed.ttl
feed.image
feed.textInput
feed.skipHours
feed.skipDays
//...
feed.dublinCore
feed.syndication
feed.iTunes
// ...

let item = feed.items?.first

item?.title
item?.link
item?.description
item?.author
item?.categories
item?.comments
item?.enclosure
item?.guid
item?.pubDate
item?.source
//...
item?.dublinCore
item?.content
item?.iTunes
item?.media
// ...
```

> Refer to the [`documentation`](http://cocoadocs.org/docsets/FeedKit) for the complete model properties and description

#### Atom

```swift
feed.title
feed.subtitle
feed.links
feed.updated
feed.authors
feed.contributors
feed.id
feed.generator
feed.icon
feed.logo
feed.rights
// ...

let entry = feed.entries?.first

entry?.title
entry?.summary
entry?.authors
entry?.contributors
entry?.links
entry?.updated
entry?.categories
entry?.id
entry?.content
entry?.published
entry?.source
entry?.rights
// ...
```

> Refer to the [`documentation`](http://cocoadocs.org/docsets/FeedKit) for the complete model properties and description

#### JSON

```swift
feed.version
feed.title
feed.homePageURL
feed.feedUrl
feed.description
feed.userComment
feed.nextUrl
feed.icon
feed.favicon
feed.author
feed.expired
feed.hubs
feed.extensions
// ...

let item = feed.items?.first

item?.id
item?.url
item?.externalUrl
item?.title
item?.contentText
item?.contentHtml
item?.summary
item?.image
item?.bannerImage
item?.datePublished
item?.dateModified
item?.author
item?.url
item?.tags
item?.attachments
item?.extensions
// ...
```

> Refer to the [`documentation`](http://cocoadocs.org/docsets/FeedKit) for the complete model properties and description

## License

FeedKit is released under the MIT license. See [LICENSE](https://github.com/nmdias/FeedKit/blob/master/LICENSE) for details.



