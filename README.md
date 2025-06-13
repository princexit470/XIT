# XIT[Uploading index (1).htm<!DOCTYPE html>
<html>
  <head>
    <title> XIT </title>
    <link rel="stylesheet" href="styles.css" />
  </head>
  <body>
      <h1 class="title"> MADE BY PRINCE  </h1>
      <p id=""></p>
      <script src="script.js"></script>
  </body>
</html>
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>QR & Barcode Generator</title>
  <script src="https://cdn.jsdelivr.net/npm/qr-code-styling/lib/qr-code-styling.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/jsbarcode@3.11.5/dist/JsBarcode.all.min.js"></script>
  <style>
    body { font-family: Arial; text-align: center; background: #eef; padding: 20px; }
    .container { background: white; padding: 20px; border-radius: 16px; box-shadow: 0 0 20px rgba(0,0,0,0.2); display: inline-block; }
    input, select, button { margin: 10px; padding: 10px; width: 250px; border-radius: 8px; border: 1px solid #ccc; }
    canvas, svg { margin: 20px auto; display: block; }
  </style>
</head>
<body>

<h2>QR & Barcode Generator</h2>

<div class="container">
  <select id="mode" onchange="switchMode()">
    <option value="qr">QR Code</option>
    <option value="barcode">Barcode</option>
  </select><br>

  <input type="text" id="data" placeholder="Enter your data here">

  <div id="qrOptions">
    <select id="qrStyle">
      <option value="square">Classic</option>
      <option value="dots">Rounded</option>
      <option value="rounded">Smooth</option>
    </select>
    <input type="color" id="qrFg" value="#000000"> Foreground
    <input type="color" id="qrBg" value="#ffffff"> Background
    <br>
    <label>Logo (PNG):</label>
    <input type="file" id="qrLogo" accept="image/png">
    <br>
    <label>Size:</label>
    <select onchange="updateSize(this.value)">
      <option value="300">300x300</option>
      <option value="500">500x500</option>
      <option value="1080">1080x1080</option>
      <option value="2160">2160x2160</option>
    </select>
    <input type="number" id="qrWidth" value="300">
    <input type="number" id="qrHeight" value="300">
  </div>

  <div id="barcodeOptions" style="display:none;">
    <select id="barcodeFormat">
      <option value="CODE128">CODE128</option>
      <option value="EAN13">EAN-13</option>
      <option value="UPC">UPC</option>
      <option value="CODE39">CODE39</option>
    </select>
    <input type="color" id="barcodeColor" value="#000000"> Color
    <br>
    <label>Width:</label><input type="number" id="barcodeWidth" value="2">
    <label>Height:</label><input type="number" id="barcodeHeight" value="100">
  </div>

  <br>
  <button onclick="generate()">Generate</button>
  <button onclick="download()">Download</button>

  <div id="output">
    <div id="qrArea"></div>
    <svg id="barcodeArea"></svg>
  </div>
</div>

<script>
  let qrCode;

  function switchMode() {
    const mode = document.getElementById('mode').value;
    document.getElementById('qrOptions').style.display = mode === 'qr' ? 'block' : 'none';
    document.getElementById('barcodeOptions').style.display = mode === 'barcode' ? 'block' : 'none';
    document.getElementById('qrArea').innerHTML = '';
    document.getElementById('barcodeArea').innerHTML = '';
  }

  function updateSize(val) {
    document.getElementById("qrWidth").value = val;
    document.getElementById("qrHeight").value = val;
  }

  function generate() {
    const mode = document.getElementById('mode').value;
    const data = document.getElementById('data').value;

    if (mode === 'qr') {
      const fg = document.getElementById('qrFg').value;
      const bg = document.getElementById('qrBg').value;
      const style = document.getElementById('qrStyle').value;
      const width = parseInt(document.getElementById('qrWidth').value);
      const height = parseInt(document.getElementById('qrHeight').value);
      const logoFile = document.getElementById('qrLogo').files[0];

      const options = {
        width,
        height,
        data,
        image: logoFile ? URL.createObjectURL(logoFile) : undefined,
        dotsOptions: { type: style, color: fg },
        backgroundOptions: { color: bg },
        imageOptions: { margin: 10 }
      };

      document.getElementById('qrArea').innerHTML = '';
      qrCode = new QRCodeStyling(options);
      qrCode.append(document.getElementById('qrArea'));
    }

    if (mode === 'barcode') {
      const format = document.getElementById('barcodeFormat').value;
      const color = document.getElementById('barcodeColor').value;
      const width = parseInt(document.getElementById('barcodeWidth').value);
      const height = parseInt(document.getElementById('barcodeHeight').value);

      JsBarcode("#barcodeArea", data, {
        format: format,
        lineColor: color,
        width: width,
        height: height,
        displayValue: true
      });
    }
  }

  function download() {
    const mode = document.getElementById('mode').value;
    if (mode === 'qr' && qrCode) {
      qrCode.download({ name: "qr-code", extension: "png" });
    } else if (mode === 'barcode') {
      const svg = document.getElementById("barcodeArea");
      const serializer = new XMLSerializer();
      const svgBlob = new Blob([serializer.serializeToString(svg)], {type:"image/svg+xml"});
      const url = URL.createObjectURL(svgBlob);
      const a = document.createElement("a");
      a.href = url;
      a.download = "barcode.svg";
      a.click();
    }
  }
</script>

</body>
</html>l…]()
