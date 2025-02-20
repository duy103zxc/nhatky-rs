## Check broken image
```rust
pub fn check_broken_image<P: AsRef<Path> + Copy>(&self, img_path: P)  -> Option<BrokenImageInfo<P>> {
        if let Err(e) = image::open(&img_path) {
            let error_string = e.to_string();
            if error_string.contains("spectral selection is not allowed in non-progressive scan") {
                println!("Skipped");
            }

            Some(BrokenImageInfo {
                path: img_path,
                err_msg: error_string,
            })
        } else {
            None
        }
    }
```