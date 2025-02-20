```rust
let encoded_query =
        percent_encoding::utf8_percent_encode(&args.query, NON_ALPHANUMERIC).to_string();

    for site in config.sites {
        let url = site.url.replace("%s", &encoded_query);
        println!("{}:\t{}", site.name, url);
        open::that(url).context("could not open the URL")?;
    }
```
use this crate: `open = "5.0.1"`