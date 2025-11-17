This is a “section” that renders a table of check-ins associated with the given day, filtered by using the expected prefix of check-in pages.

This isn't like the other, more typical, sections. This one is likely too specialized to be of general use. You'll want to make the select clause specific to your check-ins and perhaps just write the query directly in the page template.

Additionally, since it's data is from a page query rather than front matter, and there is no actuL widget to display, there is no example to show. The code itself is the real example for you to reference for your own use.

```space-lua
-- priority: 10

function cbt_journal.checkins_section(prefix)
  return query[[
    from index.tag "page"
    where _.name:startsWith(prefix)
    select {
      ["check-in"] = "[[" .. _.name .. "|" .. _.name:gsub("^" .. prefix, "") .. "]]",
      temperature = _.temperature,
      touch = _.touch,
      sound = _.sound,
      visual = _.visual
    }
    order by created desc
  ]];
end

slashcommand.define {
  name = "Check-Ins Section",
  run = function()
    editor.insertAtCursor("${cbt_journal.checkins_section(\"Journal/${os.date('%d-%m-%Y')} \")}\n", false, true)
  end
}
```
