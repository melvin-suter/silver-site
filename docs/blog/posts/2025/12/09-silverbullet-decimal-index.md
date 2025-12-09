---
title: silverbullet-decimal-index
date: 2025-12-09
draft: false
tags:
- silverbullet
---

I like the [Johnny.Decimal System](https://johnnydecimal.com/), so I try to use it in Silverbullet. To make navigation easier, I’ve created a widget, that shows me all matching notes under the current one.

```lua
-- priority: 30
event.listen {
  name = "hooks:renderBottomWidgets",
  run = function(e)
    local currentPage = editor.getCurrentPage():match("[^/]+$")

    -- check last path segment, that it is decimal system
    local isMatch = currentPage:match("^[A-Z] .*$") or currentPage:match("^[A-Z][0-9][0-9] .*$") or currentPage:match("^[0-9][0-9] .*$") or currentPage:match("^[0-9][0-9]\\.[0-9][0-9] .*$")
    
    if isMatch then
     
      local subPages = query[[
        from p = index.tag "page"
        where p.name:match("^"..editor.getCurrentPage().."/[^/]+$")
        select {
          Index = "[[" .. p.name .. "]]",
        }
      ]]
      print("index subpages", #subPages)
      if #subPages > 0 then
        return widget.new {markdown = "![[meta/templates/decimal-index]]"}
      end
    end
    
    return nil
    
  end
}
```