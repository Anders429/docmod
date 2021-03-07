# doc_item

[![GitHub Workflow Status](https://img.shields.io/github/workflow/status/Anders429/doc_item/Tests/master)](https://github.com/Anders429/doc_item/actions)
[![crates.io](https://img.shields.io/crates/v/doc_item)](https://crates.io/crates/doc_item)
[![docs.rs](https://docs.rs/doc_item/badge.svg)](https://docs.rs/doc_item)
[![MSRV](https://img.shields.io/badge/rustc-1.50.0+-yellow.svg)](#minimum-supported-rust-version)
[![License](https://img.shields.io/crates/l/doc_item)](#license)

Attributes for item-level documentation customization.

This crate provides attributes for adding various features to items when they are documented by
[`rustdoc`](https://doc.rust-lang.org/rustdoc/what-is-rustdoc.html). This includes defining
item-info docboxes, annotating an item's minimum version, and marking an item to be displayed as
semi-transparent on module lists.

This allows for enhanced documentation, similar to what is done in the standard library with the
[`staged_api`](https://doc.rust-lang.org/beta/unstable-book/language-features/staged-api.html)
feature and what is available on nightly with the
[`doc_cfg`](https://doc.rust-lang.org/beta/unstable-book/language-features/doc-cfg.html) feature.
However, this crate provides even more customization, allowing for use of custom CSS classes and
text within docboxes.

## Usage

### Defining an Experimental API
Marking an item as experimental (similar to what is done in the standard library through the
[`#[unstable]`](https://rustc-dev-guide.rust-lang.org/stability.html#unstable) attribute) can be
done as follows:

```rust
/// This is an experimental API.
///
/// The docbox will indicate the function is experimental. It will also appear semi-transparent on
/// module lists.
#[doc_item::docbox(content="<span class='emoji'>🔬</span> This is an experimental API.", class="unstable")]
#[doc_item::short_docbox(content="Experimental", class="unstable")]
#[doc_item::semi_transparent]
pub fn foo() {}
```

### Creating Custom-Styled Docboxes
You can create your own custom styles to customize the display of docboxes. Define your item's
docbox as follows:

```rust
/// An item with a custom docbox.
///
/// The docbox will be a different color.
#[doc_item::docbox(content="A custom docbox", class="custom")]
#[doc_item::short_docbox(content="Custom", class="custom")]
pub fn foo() {}
```

Next, create a style definition in a separate HTML file.
```html
<style>
    .stab.custom {
        background: #f5ffd6;
        border-color: #b9ff00;
    }
</style>
```

Finally, include the HTML file's contents in your documentation:

```bash
$ RUSTDOCFLAGS="--html-in-header custom.html" cargo doc --no-deps --open
```

And instruct [docs.rs](https://docs.rs/) to include the HTML file's contents as well by adding to your `Cargo.toml`:

```toml
[package.metadata.docs.rs]
rustdoc-args = [ "--html-in-header", "custom.html" ]
```

## Minimum Supported Rust Version
This crate is guaranteed to function properly on `rustc 1.50.0` and up. It may compile on earlier
versions, but it is not guaranteed that all features will display properly.

## Nightly Stability
As [docs.rs](https://docs.rs/) builds documentation on the
[`nightly`](https://rust-lang.github.io/rustup/concepts/channels.html) channel, this crate will
attempt to maintain functionality on `nightly`. As this crate's functionality relies on injecting
HTML into the generated documentation, and internal layout of HTML is subject to change, `nightly`
functionality may occasionally break. Please report issues as you find them on the associated github
repository.

## License
This project is licensed under either of

* Apache License, Version 2.0
([LICENSE-APACHE](https://github.com/Anders429/more_ranges/blob/HEAD/LICENSE-APACHE) or
http://www.apache.org/licenses/LICENSE-2.0)
* MIT license
([LICENSE-MIT](https://github.com/Anders429/more_ranges/blob/HEAD/LICENSE-MIT) or
http://opensource.org/licenses/MIT)

at your option.

### Contribution
Unless you explicitly state otherwise, any contribution intentionally submitted for inclusion in the work by you, as defined in the Apache-2.0 license, shall be dual licensed as above, without any additional terms or conditions.
