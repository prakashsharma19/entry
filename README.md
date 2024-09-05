<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Test PDF Loading</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f4;
            padding: 20px;
        }
        .container {
            max-width: 800px;
            margin: 0 auto;
            background: #fff;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }
        button {
            padding: 10px;
            margin: 10px 0;
            font-size: 16px;
            border: 1px solid #ddd;
            border-radius: 4px;
            background-color: #007bff;
            color: #fff;
            border: none;
            cursor: pointer;
        }
        button:hover {
            background-color: #0056b3;
        }
        #pdfViewer {
            width: 100%;
            height: 600px;
            border: 1px solid #ddd;
            margin-top: 20px;
            display: none;
        }
    </style>
</head>
<body>

    <div class="container">
        <h1>Test PDF Loading</h1>
        <button onclick="loadPdf()">Load Test PDF</button>
        <iframe id="pdfViewer" src="" frameborder="0"></iframe>
    </div>

    <script>
        function loadPdf() {
            const pdfViewer = document.getElementById('pdfViewer');
            const testPdfUrl = 'https://arxiv.org/pdf/2305.02372'; // Example PDF URL
            pdfViewer.src = testPdfUrl;  // Set the PDF URL
            pdfViewer.style.display = 'block';  // Show the PDF viewer
        }
    </script>

</body>
</html>
