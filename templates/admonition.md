<%_* 
const callouts = {
   note:     '🔵 ✏ Note',
   info:     '🔵 ℹ Info',
   todo:     '🔵 🔳 Todo',
   tip:      '🌐 🔥 Tip / Hint / Important',
   abstract: '🌐 📋 Abstract / Summary / TLDR',
   question: '🟡 ❓ Question / Help / FAQ',
   quote:    '🔘 💬 Quote / Cite',
   example:  '🟣 📑 Example',
   success:  '🟢 ✔ Success / Check / Done',
   warning:  '🟠 ⚠ Warning / Caution / Attention',
   failure:  '🔴 ❌ Failure / Fail / Missing',
   danger:   '🔴 ⚡ Danger / Error',
   bug:      '🔴 🐞 Bug',
};
%>

<%_* let calloutType = await tp.system.suggester(Object.values(callouts), Object.keys(callouts), true, 'Select admonition type.');%>

<%_* let foldState = await tp.system.suggester(["Expanded", "Collapsed"], [true, false], false, "Folding state of admonition?")%>

<%_*
  let title = await tp.system.prompt("Optional Title Text", "")
%>

<%_*
  let calloutContent = await tp.system.prompt("Optional Content Text (Shift Enter to Insert New Line)", "", false, true)
  calloutContent = calloutContent.replaceAll("\n", "\n> ")
%>

<%-*
if (calloutType != null) {
  let content = '> [!' + calloutType + ']' + foldState + ' ' + title + '\n> ' + calloutContent + '\n'
  content = '{{< admonition type="' + calloutType + '" title="' + title + '" open=' + foldState + ' >}}\n' + calloutContent + tp.file.cursor() + '\n{{< /admonition >}}';
  tR+=content
}
%>
