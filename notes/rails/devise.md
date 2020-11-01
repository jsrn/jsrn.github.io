---
layout: notes
---

# Devise

Preventing user enumeration:

```ruby
Devise.setup do |config|
  config.paranoid = true
end
```
