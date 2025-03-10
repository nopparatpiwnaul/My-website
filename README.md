# My-website
<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>สร้างและพิมพ์บาร์โค้ด</title>
    <script src="https://cdn.jsdelivr.net/npm/jsbarcode@3.11.0/dist/JsBarcode.all.min.js"></script>
    <style>
        /* General Styles */
        body {
            font-family: 'Arial', sans-serif;
            background-color: #f4f4f9;
            margin: 0;
            padding: 0;
            color: #333;
        }

        header {
            background-color: #4CAF50;
            color: white;
            text-align: center;
            padding: 20px 0;
            font-size: 24px;
        }

        h2 {
            font-size: 28px;
            text-align: center;
            margin-top: 20px;
            color: #333;
        }

        .container {
            max-width: 800px;
            margin: 0 auto;
            padding: 20px;
            background-color: white;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
            border-radius: 8px;
        }

        /* Form Styles */
        form {
            display: flex;
            flex-direction: column;
        }

        label {
            font-size: 18px;
            margin-bottom: 5px;
        }

        input {
            font-size: 16px;
            padding: 10px;
            border-radius: 4px;
            border: 1px solid #ddd;
            margin-bottom: 15px;
            width: 100%;
        }

        input:focus {
            border-color: #4CAF50;
            outline: none;
        }

        button {
            background-color: #4CAF50;
            color: white;
            padding: 10px 20px;
            border: none;
            border-radius: 4px;
            font-size: 16px;
            cursor: pointer;
            margin-bottom: 10px;
            transition: background-color 0.3s ease;
        }

        button:hover {
            background-color: #45a049;
        }

        button:active {
            background-color: #388e3c;
        }

        /* Barcode Container */
        #barcodeContainer {
            margin-top: 30px;
            text-align: center;
        }

        svg {
            max-width: 100%;
            height: auto;
        }

        .barcode-text {
            font-size: 24px;
            font-weight: bold;
            margin-top: 10px;
            color: #333;
        }

        /* Footer */
        footer {
            text-align: center;
            padding: 20px 0;
            background-color: #f1f1f1;
            margin-top: 30px;
            font-size: 14px;
            color: #666;
        }

        /* Responsive Styles */
        @media (max-width: 768px) {
            .container {
                padding: 15px;
            }

            h2 {
                font-size: 24px;
            }

            button {
                font-size: 14px;
                padding: 8px 15px;
            }
        }
    </style>
</head>
<body>

    <header>
        สร้างและพิมพ์บาร์โค้ด
    </header>

    <div class="container">
        <h2>กรอกข้อมูลเพื่อสร้างบาร์โค้ด</h2>

        <form id="barcodeForm">
            <label for="ticketNumber">เลขที่ใบสั่ง:</label>
            <input type="text" id="ticketNumber" placeholder="กรอกเลขที่ใบสั่ง"><br>

            <label for="fineAmount">จำนวนค่าปรับ :</label>
            <input type="text" id="fineAmount" placeholder="กรอกจำนวนค่าปรับ"><br>

            <button type="button" onclick="generateBarcode()">สร้างบาร์โค้ด</button>
            <button type="button" onclick="printBarcode()">พิมพ์บาร์โค้ด</button>
            <button type="button" onclick="saveBarcode()">บันทึกบาร์โค้ด</button>
        </form>

        <div id="barcodeContainer">
            <svg id="barcode"></svg>
            <div id="barcodeText" class="barcode-text"></div>
        </div>
    </div>

    <footer>
        &copy; 2025 สร้างโดยนพรัตน์ ผิวนวล
    </footer>

    <script>
        function generateBarcode() {
            var fixedLine1 = "|099400015904822"; // ข้อมูลตายตัวของแถวที่ 1
            var ticketNumber = document.getElementById("ticketNumber").value.trim(); // รับเลขที่ใบสั่ง
            var fixedLine3 = ""; // แถวที่ 3 ว่าง
            var fineAmount = document.getElementById("fineAmount").value.trim() + "00"; // เติม '00' ต่อท้ายจำนวนค่าปรับ

            // ตรวจสอบว่าผู้ใช้กรอกข้อมูลครบหรือไม่
            if (!ticketNumber || !fineAmount) {
                alert("กรุณากรอกข้อมูลให้ครบถ้วน");
                return;
            }

            // สร้างข้อความรวมกันตามรูปแบบ
            var barcodeData = fixedLine1 + "\n" + ticketNumber + "\n" + fixedLine3 + "\n" + fineAmount;

            // แสดงบาร์โค้ด
            JsBarcode("#barcode", barcodeData, {
                format: "CODE128",
                displayValue: false  // ปิดการแสดงค่าใต้บาร์โค้ด
            });

            // แสดงข้อความใต้บาร์โค้ด
            document.getElementById("barcodeText").innerHTML = "เลขที่ใบสั่ง: " + ticketNumber;
        }

        function printBarcode() {
            var printWindow = window.open("", "_blank");
            printWindow.document.write(`
                <html>
                <head>
                    <title>พิมพ์บาร์โค้ด</title>
                    <style>
                        body { text-align: center; font-family: Arial, sans-serif; }
                        svg { width: 100%; max-width: 400px; }
                    </style>
                </head>
                <body>
                    <h2>บาร์โค้ดของคุณ</h2>
                    ${document.getElementById("barcodeContainer").innerHTML}
                    <script>
                        window.onload = function() {
                            window.print();
                        };
                    <\/script>
                </body>
                </html>
            `);
            printWindow.document.close();
        }

        function saveBarcode() {
            var ticketNumber = document.getElementById("ticketNumber").value.trim(); // รับค่าเลขที่ใบสั่ง
            if (!ticketNumber) {
                alert("กรุณากรอกเลขที่ใบสั่งก่อนบันทึก");
                return;
            }

            var barcodeSvg = document.getElementById("barcode"); // รับ svg ของบาร์โค้ด
            var ticketText = "เลขที่ใบสั่ง: " + ticketNumber; // ข้อความที่จะแสดงใต้บาร์โค้ด

            // แปลง SVG เป็น Canvas
            var canvas = document.createElement('canvas');
            var ctx = canvas.getContext('2d');
            var data = new XMLSerializer().serializeToString(barcodeSvg);
            var img = new Image();
            img.src = 'data:image/svg+xml;base64,' + btoa(data); // แปลง SVG เป็น Base64

            img.onload = function () {
                canvas.width = img.width;
                canvas.height = img.height + 50; // เพิ่มพื้นที่เพื่อวางข้อความใต้บาร์โค้ด

                ctx.drawImage(img, 0, 0); // วาดบาร์โค้ดลงบน canvas
                ctx.font = '24px Arial'; // ตั้งค่าฟอนต์
                ctx.fillStyle = '#000'; // กำหนดสีข้อความ
                ctx.fillText(ticketText, 10, img.height + 30); // วางข้อความใต้บาร์โค้ด

                // แปลง Canvas เป็น PNG
                var dataUrl = canvas.toDataURL('image/png');

                // สร้างลิงก์ดาวน์โหลด
                var link = document.createElement('a');
                link.href = dataUrl;
                link.download = ticketNumber + '_barcode.png'; // ตั้งชื่อไฟล์เป็นเลขที่ใบสั่ง + _barcode
                link.click(); // ทำการดาวน์โหลด
            };
        }
    </script>

</body>
</html>
