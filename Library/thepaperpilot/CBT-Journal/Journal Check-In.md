---
command: "CBT Journal: Check-In"
suggestedName: "Journal/${os.date('%d.%m.%Y')} ${date.time()}"
confirmName: false
openIfExists: true
tags: meta/template/page
priority: 10
pageDecoration:
  disableTOC: true
frontmatter: |
  pageDecoration:
    disableTOC: true
---
## ${os.date('%d.%m.%Y')} ${date.time()} Check-In
${"$"}{cbt_journal.temperature_section("temperature", "red")}

### What sensitivities are you experiencing?

${"$"}{cbt_journal.tracker_section({
  touch = "✋",
  sound = "🔊",
  visual = "🚨"
})}

### What caused this check-in

|^|

### What feelings or sensations are you having?

-

