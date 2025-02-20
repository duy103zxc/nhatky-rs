```rust

pub fn html_to_dict_term(file_path: &str) -> Vec<DictTerm> {
    let html_content = fs::read_to_string(file_path).expect("Should have been able to read the file");
    let document = Document::from(html_content.as_str());

    document.find(Name("idx:entry")).map(|element| {
        let dict_term = element.find(Name("idx:orth")).next().unwrap().attr("value").unwrap();
        DictTerm {
            term: dict_term.to_owned(),
            definition: element.text().replace("\"", "-")
        } 
    }).collect::<Vec<_>>()
    
}

```