<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Text Formatter</title>
<style>
    body {
        font-family: 'Times New Roman', Times, serif;
        font-size: 12px;
        line-height: 1.6;
        margin: 20px;
    }
    #text-area {
        width: 100%;
        height: 200px;
        font-family: 'Times New Roman', Times, serif;
        font-size: 12px;
        padding: 10px;
        box-sizing: border-box;
    }
    #fixed-text {
        margin-top: 10px;
        padding: 10px;
        border: 1px solid #ccc;
        background-color: #f9f9f9;
    }
    a.email-link {
        text-decoration: underline;
        color: blue;
    }
</style>
</head>
<body>
    <h1>Text Formatter</h1>
    <p>Paste your text below and click "Fix" to format:</p>
    <textarea id="text-area" placeholder="Paste your text here..."></textarea>
    <br>
    <button onclick="formatText()">Fix</button>
    <div id="fixed-text"></div>

    <script>
        function formatText() {
            var textarea = document.getElementById('text-area');
            var fixedText = document.getElementById('fixed-text');
            var text = textarea.value.trim();

            // Replace email addresses with links
            var formattedText = text.replace(/\b[\w\.-]+@[\w\.-]+\.\w{2,}\b/g, function(match) {
                return '<a href="mailto:' + match + '" class="email-link">' + match + '</a>';
            });

            // Apply Times New Roman font and font size 12px
            fixedText.innerHTML = '<p style="font-family: \'Times New Roman\', Times, serif; font-size: 12px;">' + formattedText + '</p>';
        }
    </script>
</body>
</html>
