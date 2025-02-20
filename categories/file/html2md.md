use nanohtml2text::html2text;
use std::str::FromStr;

use super::view::View;

pub fn get_markdown_content(html: &str) -> String {
    html2text(html)
}