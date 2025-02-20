```rust
pub fn check_img_format(filename: &PathBuf) -> bool {
    static IMAGE_FORMAT: &[&'static str; 9] = &["jpg", "jpeg", "jpe", "jfif", "jfi", "jif", "png", "bmp", "gif"];
    filename.extension().and_then(OsStr::to_str).is_some_and(|ext| IMAGE_FORMAT.contains(&ext))
}

```