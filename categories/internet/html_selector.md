pub fn fetcher(
    base_url: String,
    id: &str,
    list_selector: &str,
    item_selector: &str,
    element: &str
) -> Vec<String> {
    let given_url = base_url + id;
    let body = http::get_body_from_url(&given_url).unwrap();
    let document = Html::parse_document(&body);
    // Logging
    println!("Đang tải chương truyện với URL là: {}", &given_url);
    // Selector
    let list = Selector::parse(list_selector).unwrap();
    let item = Selector::parse(item_selector).unwrap();
    let body = Selector::parse("body").unwrap();

    let content_matcher = match document.select(&list).next() {
        Some(doc) => {
            doc.select(&item)
        },
        None => {
            document.select(&body).next().unwrap().select(&item)
        },
    };

    content_matcher.into_iter()
    .filter_map(|f| f.value().attr(element).map(|elem| elem.to_string())).collect()
}