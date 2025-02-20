### Generate a writable file
```rust
pub fn gen_file(name: &str, book_format: &str) -> Result<File, std::io::Error> {
    OpenOptions::new()
        .write(true)
        .create(true)
        .append(true)
        .open(format!("{}.{}", name, book_format))
}
```
### Write to a file

```rust
pub fn gen_txt(novel: NovelContent) -> Result<(), anyhow::Error> {
    novel.chapter_list.into_iter().for_each(|chapter| {
        let filename = format!("{}.txt", &chapter.name);
        let f = OpenOptions::new()
            .append(true)
            .create_new(true)
            .write(true)
            .open(filename).unwrap();
        let mut f = BufWriter::new(f);
        f.write_all(gen_str(chapter.name, &chapter.content).as_bytes())
            .expect("Can't write to the file");
        f.flush().unwrap();
    });
    Ok(())
}
```