<template>
  <div
    class="relative flex items-top justify-center min-h-screen bg-gray-100 sm:items-center sm:pt-0"
  >
    <button @click="exportText('txt')">exportText(text)</button>
    <button @click="exportText('docx')">exportText(docx)</button>
  </div>
</template>

<script>
import * as docx from "docx";
function saveAs(file, filename) {
  if (window.navigator.msSaveOrOpenBlob)
    // IE10+
    window.navigator.msSaveOrOpenBlob(file, filename);
  else {
    // Others
    var a = document.createElement("a"),
      url = URL.createObjectURL(file);
    a.href = url;
    a.download = filename;
    document.body.appendChild(a);
    a.click();
    setTimeout(function () {
      document.body.removeChild(a);
      window.URL.revokeObjectURL(url);
    }, 0);
  }
}

export default {
  name: "NuxtTutorial",
  methods: {
    exportText(type) {
      const content = [
        "Title: python beginner\\n\\n\\n",
        "Introduce: \\n\\n\\n\\n\\n",
        "Modules:\\n\\n1. Mod 1\\n\\nSummarize:\\n\\nsum1\\n\\nLesson Ideas:\\n\\n1. l1\\n\\nExpound:\\n\\napplebananakiwi\\n\\n\\n2. M2\\n\\nSummarize:\\n\\n\\n\\nLesson Ideas:\\n\\n1. \\n2. \\n3. \\n4. \\n\\nExpound:\\n\\n\\n\\n\\n3. M3\\n\\nSummarize:\\n\\n\\n\\nLesson Ideas:\\n\\n1. \\n\\nExpound:\\n\\ntststt\\n",
        "Conclusion: \\n\\n\\n\\n\\n",
      ];

      if (type === "txt") {
        const file = new Blob(content, {
          type: "text/plain",
        });
        saveAs(file, "export.txt");
      } else if (type === "docx") {
        const doc = new docx.Document({
          sections: [
            {
              properties: {},
              children: content.map(
                (item) =>
                  new docx.Paragraph({
                    children: [new docx.TextRun(item)],
                  })
              ),
            },
          ],
        });
        docx.Packer.toBlob(doc).then((file) => saveAs(file, "export.docx"));
      } else {
        return content;
      }
    },
  },
};
</script>
