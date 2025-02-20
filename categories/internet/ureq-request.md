```rust

pub fn reqwester(url: &str, refer_site: &str) -> Result<Response, ureq::Error> {
    let user_agent = "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/126.0.0.0 Safari/537.36";
    
    let ureq_agent: Agent = ureq::AgentBuilder::new()
    .timeout_read(Duration::from_secs(30))
    .build();

    let body = ureq_agent.get(url)
        .set("User-Agent", user_agent)
        .set("Referer", refer_site)
        .call();
    body
}
```