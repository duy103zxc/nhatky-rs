```rust
/// Displays formatted html that fits a CSS selector.
pub fn display_article(
    contents_selector: scraper::Selector,
    title_selector: scraper::Selector,
    request: scraper::Html,
) {
    let mut paragraphs = request
        .select(&contents_selector)
        .map(|x| from_read(x.inner_html().as_bytes(), 190))
        .collect::<Vec<String>>()
        .join("\n");

    paragraphs = filters::remove_references(paragraphs);
    paragraphs = filters::remove_square_brackets(paragraphs);
    paragraphs = filters::remove_links(paragraphs);

    let title = request
        .select(&title_selector)
        .map(|x| from_read(x.inner_html().as_bytes(), 50))
        .collect::<Vec<String>>()
        .join("");

    let article = ArticleDisplay {
        title: title,
        contents: paragraphs,
    };

    ui::ArticleDisplay::new(article).unwrap();
}

```


/// Code that cleans the article from HTML leftovers.
```rust
use regex::Regex;

pub fn remove_square_brackets(text: String) -> String {
    let square_bracket_regex =
        Regex::new(r"\[(?P<link>[a-zA-Z0-9\(\)\-\,[:space:]&=]+)\]").unwrap();
    square_bracket_regex.replace_all(&text, "$link").to_string()
}

pub fn remove_references(text: String) -> String {
    let reference_regex = Regex::new(r"\[+([0-9]+)\]+").unwrap();
    reference_regex.replace_all(&text, "").to_string()
}

pub fn remove_links(text: String) -> String {
    let link_regex = Regex::new(r": [/?/[a-zA-Z0-9-%:/(/)_.//&=]+/]+\n").unwrap();
    let note_regex =
        Regex::new(r": #cite_note-[0-9-]+\n|: #cite_note-[a-zA-Z_-]+-[0-9-]+\n").unwrap();

    let mut new_text = link_regex.replace_all(&text, "").to_string();
    new_text = note_regex.replace_all(&new_text, "").to_string();

    new_text
}
```