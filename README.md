# SwiftPods catalog

The public catalog that lets a [SwiftPods](https://github.com/swiftpods-org/swiftpods)
Podfile name a Swift package instead of spelling out its repository:

```ruby
target 'MyApp' do
  spm 'Datadog', '~> 3.15'
  spm 'Firebase', '~> 13.0', products: %w[FirebaseCore FirebaseAnalytics]
end
```

SwiftPods looks the name up here, case-insensitively, and uses the repository
and default products it finds. Versions stay in the Podfile: Swift Package
Manager reads them from the package's tags, so a package is registered once,
not once per release.

## Entries

One file per package in `Packages/`, named after the package:

```ruby
# Packages/Datadog.podfile
package 'Datadog', git: 'https://github.com/DataDog/dd-sdk-ios.git', products: ['DatadogCore']
```

- `git`: the package's official repository.
- `products`: what a target links when the Podfile names no products. Omit it
  when there is no sensible default (Firebase, Facebook); the Podfile must then
  choose with `products:`.

## Adding a package

Open a pull request with one new file. It is accepted when:

1. the name is the one the package is known by (its `Package.swift` name or its
   main product), and is not taken;
2. `git` is the package's official repository, maintained by its authors;
3. the default products exist in that repository's `Package.swift`;
4. `swiftpods catalog lint` passes (CI runs it).

Changing an existing entry's repository needs a link that shows the authors
moved it. A catalog maps names to code that apps will build and ship, so
redirects are reviewed as carefully as new entries.

## Private catalogs

A team can keep its own catalog in the same format and list it in its
Podfiles; declared sources replace this one unless it is listed too:

```ruby
source 'git@github.com:team/swiftpods-catalog.git'
source 'https://github.com/swiftpods-org/swiftpods-catalog.git'
```
