### XHTML Generator
```rust
fn to_xhtml(content: &Vec<String>, chap_title: &str) -> String {
    // Start
    let start = format!(
        r##"<?xml version='1.0' encoding='utf-8'?>
<html xmlns="http://www.w3.org/1999/xhtml" lang="ja" xml:lang="ja">
    <head>
        <title>{}</title>
        <meta http-equiv="Content-Type" content="text/html; charset=utf-8"/>
        <link rel="stylesheet" type="text/css" href="stylesheet.css"/>
    </head>
    <body>"##,
        chap_title
    );
    let chapter_title = format!("\n\t<h2>{}</h2>\n", chap_title);
    let mut file: String = start.to_string() + chapter_title.as_str();

    for line in content {
        match line.as_str() {
            "" => {}
            _ => {
                file += format!("\t\t<p>{}</p>\n", &line.trim()).as_str();
            }
        }
    }
    file += "\n\t</body>\n</html>";
    // End
    file
}
```
### CSS for English and Japanese

```rust

fn css_gen(is_vertical: &bool) -> EpubMode {
    let vertical_css = r##"@page {
  margin-bottom: 5pt;
  margin-top: 5pt;
}

html {
  -webkit-writing-mode: vertical-rl;
  -moz-writing-mode: vertical-rl;
  -ms-writing-mode: vertical-rl;
  writing-mode: vertical-rl;
}

p {
  display: block;
  height: auto;
  text-indent: inherit;
  width: auto;
  margin: 0;
  padding: 0;
}"##;

```


```rust
pub fn compress_xhtml(html_input: &str) -> String {
    format!(r##"<?xml version='1.0' encoding='utf-8'?>
<html xmlns="http://www.w3.org/1999/xhtml" lang="ja" xml:lang="ja">
    <head>
        <title>Untitled</title>
        <meta http-equiv="Content-Type" content="text/html; charset=utf-8"/>
        <link rel="stylesheet" type="text/css" href="stylesheet.css"/>
    </head>
    <body>{}{}"##, html_input, "\n\t</body>\n</html>")
}

```
