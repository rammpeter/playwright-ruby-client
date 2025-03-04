---
sidebar_position: 10
---

# Selectors

Selectors can be used to install custom selector engines. See [Working with selectors](https://playwright.dev/python/docs/selectors) for more
information.

## register

```
def register(name, contentScript: nil, path: nil, script: nil)
```

An example of registering selector engine that queries elements based on a tag name:

```ruby
tag_selector = <<~JAVASCRIPT
{
    // Returns the first element matching given selector in the root's subtree.
    query(root, selector) {
        return root.querySelector(selector);
    },
    // Returns all elements matching given selector in the root's subtree.
    queryAll(root, selector) {
        return Array.from(root.querySelectorAll(selector));
    }
}
JAVASCRIPT

# Register the engine. Selectors will be prefixed with "tag=".
playwright.selectors.register("tag", script: tag_selector)
playwright.chromium.launch do |browser|
  page = browser.new_page()
  page.content = '<div><button>Click me</button></div>'

  # Use the selector prefixed with its name.
  button = page.query_selector('tag=button')
  # Combine it with other selector engines.
  page.click('tag=div >> text="Click me"')

  # Can use it in any methods supporting selectors.
  button_count = page.locator('tag=button').count
  button_count # => 1
end
```


