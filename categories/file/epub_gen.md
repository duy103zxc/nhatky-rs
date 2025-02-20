```rust

// Generate novel's content for the .epub
fn gen_content(
    epub_gen: &mut epub_builder::EpubBuilder<epub_builder::ZipLibrary>,
    novel: NovelContent,
) -> &mut epub_builder::EpubBuilder<epub_builder::ZipLibrary> {
    let mut chap_num = 0;
    for chapter in novel.chapter_list {
        chap_num += 1;
        epub_gen
            .add_content(
                epub_builder::EpubContent::new(
                    format!("chapter_{}.xhtml", chap_num),
                    import(&chapter).as_bytes(),
                )
                .title((chapter.name).to_string())
                .reftype(epub_builder::ReferenceType::Text),
            )
            .expect("Can't build this content page for the .epub file");
    }
    epub_gen
}

pub fn gen_epub(
    book: NovelContent,
    book_language: &str,
    is_vertical: &bool,
) -> epub_builder::Result<()> {
    let output = utils::gen_file(&book.title, "epub").unwrap();

    let epub_mode = css_gen(is_vertical);
    let mut binding = epub_builder::EpubBuilder::new(epub_builder::ZipLibrary::new()?)?;
    let gen_epub = binding
        .metadata("author", book.author.trim())?
        .metadata("title", &book.title)?
        .epub_direction(epub_mode.epub_direction)
        .metadata("lang", book_language)?
        .metadata("generator", "Buukuru")?
        .add_metadata_opf(epub_builder::MetadataOpf {
            name: String::from("primary-writing-mode"),
            content: epub_mode.p_writing_mode,
        })
        .epub_version(epub_builder::EpubVersion::V30)
        .stylesheet(epub_mode.epub_css.as_bytes())?;

    let generated_content = gen_content(gen_epub, book);
    generated_content
        .inline_toc()
        .generate(output)
        .expect("Error generating .epub file");
    Ok(())
}

```