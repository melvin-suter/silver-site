---
title: silverbullet-files-handling
date: 2025-12-09
draft: false
tags:
- silverbullet
---

I love my Silverbullet notes instance. But the file handling was not perfect for me. So I created a small setup, that will help me with handling the files, and mapping them to pages:

**Uploads**

I’ve created a upload button in my action bar, that handles where to put the file. It just keeps the original file name. But this in your config:

```lua
config.set {
  actionButtons = {
    {
      icon = "upload",
      run = function()
        local file = editor.uploadFile("", nil)
        local newFileName = editor.getCurrentPage().."_files/".. file.name
        space.writeDocument(newFileName, file.content)
        editor.insertAtCursor("[["..newFileName.."]]")
      end,
      description = "Add a new attachement"
    }
 }
}
```

**Showing the Files**

To see which files belong to a site, I have this widget added under `meta/files-widget`, but you can put it where ever you like:

```lua
-- priority: 20
event.listen {
  name = "hooks:renderBottomWidgets",
  run = function(e)
    -- Get Files
    local files = space.listFiles()

    -- Loop
    local hasFiles = false
    local filesOut = "| File | Size | Changed |\n"
    filesOut = filesOut .. "|---|---|---|\n"
    for _, file in ipairs(files) do
        -- If lies under the current page
        if(starts_with(file.name, editor.getCurrentPage().. "_files/")) then
          -- not .md files
          if(not ends_with(file.name, ".md")) then
            
            filesOut = filesOut .. "|[["..file.name.."]]"
            filesOut = filesOut .. "|" .. human_readable_bytes(file.size)
            filesOut = filesOut .."|"..os.date("%Y-%m-%d %H:%M:%S", file.lastModified / 1000)
            filesOut = filesOut .."|\n"
            hasFiles = true
          end
        end
    end
    if hasFiles then
      return widget.new {markdown = filesOut}
    else
      return nil
    end
  end
}
```