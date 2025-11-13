A utility section that adds buttons to the previous and next journal entries, if present.

${cbt_journal.navigation_section("^Journal/%d+%-%d+%-%d+$")}

## Implementation

```space-lua
-- priority: 10

function cbt_journal.navigation_section(pattern)
  local pages = query[[
    from index.tag "page"
    where _.name:match(pattern)
    select {
      name = _.name
    }
    order by name desc
  ]];

  if #pages == 0 then
    return ""
  end

  -- Find current index
  local currentIndex
  for i, p in ipairs(pages) do
    if p.name == editor.getCurrentPage() then
      currentIndex = i
      break
    end
  end

  if not currentIndex then
    return ""
  end

  -- Get previous and next
  local nextp = pages[currentIndex - 1] and pages[currentIndex - 1].name
  local prev = pages[currentIndex + 1] and pages[currentIndex + 1].name

  -- Render buttons
  local buttons = {}

  if prev then
    table.insert(buttons, widgets.button("← prev", function()
      editor.navigate(prev)
    end))
  end

  table.insert(buttons, dom.span { style = "flex-grow: 1" })

  if nextp then
    table.insert(buttons, widgets.button("next →", function()
      editor.navigate(nextp)
    end))
  end

  return widget.html(dom.div {
    style = "display: flex",
    table.unpack(buttons)
  })
end

slashcommand.define {
  name = "Navigation Section",
  run = function()
    editor.insertAtCursor("${cbt_journal.navigation_section(\"^Journal/%d+%-%d+%-%d+$\")}\n", false, true)
  end
}
```
