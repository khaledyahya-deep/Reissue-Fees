# Reissue-Fees
<!DOCTYPE html>
<html lang="ar">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>صفحة الإدخال والعرض</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            direction: rtl;
            background-color: #f4f4f9;
            margin: 0;
            padding: 20px;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
        }
        .container {
            width: 90%;
            max-width: 600px;
            background-color: #ffffff;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        }
        h1 {
            text-align: center;
            color: #333;
            margin-bottom: 20px;
            font-size: 24px;
        }
        label {
            display: block;
            margin-bottom: 5px;
            color: #555;
            font-weight: bold;
        }
        input {
            width: 100%;
            padding: 10px;
            margin-bottom: 15px;
            border: 1px solid #ddd;
            border-radius: 5px;
            font-size: 16px;
            box-sizing: border-box;
        }
        input:focus {
            border-color: #4CAF50;
            outline: none;
            box-shadow: 0 0 5px rgba(76, 175, 80, 0.5);
        }
        .input-group {
            display: flex;
            gap: 10px;
            align-items: flex-end;
        }
        .input-group input {
            flex: 1;
        }
        button {
            width: 100%;
            padding: 12px;
            background-color: #4CAF50;
            color: white;
            border: none;
            border-radius: 5px;
            font-size: 16px;
            cursor: pointer;
            transition: background-color 0.3s ease;
        }
        button:hover {
            background-color: #45a049;
        }
        .output {
            margin-top: 20px;
            padding: 15px;
            background-color: #f9f9f9;
            border: 1px solid #ddd;
            border-radius: 5px;
            white-space: pre-line;
            font-size: 14px;
            color: #333;
        }
    </style>
</head>
<body>

<div class="container">
    <h1>إدخال البيانات</h1>
    
    <label for="modificationFee">رسوم التعديل:</label>
    <input type="text" id="modificationFee" placeholder="أدخل رسوم التعديل">
    
    <label for="infantPriceDifference">فرق السعر للرضيع:</label>
    <input type="text" id="infantPriceDifference" placeholder="أدخل فرق السعر للرضيع">
    
    <label for="childPriceDifference">فرق السعر للطفل:</label>
    <input type="text" id="childPriceDifference" placeholder="أدخل فرق السعر للطفل">
    
    <label for="adultPriceDifference">فرق السعر للبالغ:</label>
    <input type="text" id="adultPriceDifference" placeholder="أدخل فرق السعر للبالغ">
    
    <div class="input-group">
        <div>
            <label for="safaFee">رسوم صفا للشخص الواحد:</label>
            <input type="text" id="safaFee" placeholder="أدخل رسوم صفا">
        </div>
        <div>
            <label for="aviationTax">ضرائب الطيران للشخص الواحد:</label>
            <input type="text" id="aviationTax" placeholder="أدخل ضرائب الطيران">
        </div>
        <div>
            <label for="passengerCount">عدد المسافرين:</label>
            <input type="text" id="passengerCount" placeholder="أدخل عدد المسافرين">
        </div>
    </div>
    
    <div class="input-group">
        <div>
            <label for="hours">عدد الساعات:</label>
            <input type="text" id="hours" placeholder="أدخل عدد الساعات">
        </div>
        <div>
            <label for="noShowFee">رسوم عدم الظهور (No Show):</label>
            <input type="text" id="noShowFee" placeholder="أدخل رسوم عدم الظهور">
        </div>
    </div>
    
    <button onclick="generateOutput()">إنشاء النص</button>
    
    <div class="output" id="output">
        <!-- الناتج سيظهر هنا -->
    </div>
</div>

<script>
    function generateOutput() {
        // الحصول على القيم من مربعات الإدخال
        const modificationFee = parseFloat(document.getElementById('modificationFee').value) || 0;
        const childPriceDifference = parseFloat(document.getElementById('childPriceDifference').value) || 0;
        const adultPriceDifference = parseFloat(document.getElementById('adultPriceDifference').value) || 0;
        const infantPriceDifference = parseFloat(document.getElementById('infantPriceDifference').value) || 0;
        const safaFee = parseFloat(document.getElementById('safaFee').value) || 0;
        const aviationTax = parseFloat(document.getElementById('aviationTax').value) || 0;
        const passengerCount = parseInt(document.getElementById('passengerCount').value) || 1; // افتراضيًا 1 لتجنب القسمة على صفر
        const hours = document.getElementById('hours').value;
        const noShowFee = document.getElementById('noShowFee').value;

        // حساب فرق الضرائب
        const taxDifference = (safaFee + aviationTax) * passengerCount;

        // حساب الإجماليات
        const totalInfant = infantPriceDifference ? modificationFee + infantPriceDifference + (taxDifference / passengerCount) : 0;
        const totalChild = childPriceDifference ? modificationFee + childPriceDifference + (taxDifference / passengerCount) : 0;
        const totalAdult = adultPriceDifference ? modificationFee + adultPriceDifference + (taxDifference / passengerCount) : 0;
        const grandTotal = totalInfant + totalChild + totalAdult;

        // إنشاء النص الديناميكي
        let outputText = `رسوم التعديل: ${modificationFee} دينار`;

        // إضافة فرق السعر للرضيع إذا تم إدخاله
        if (infantPriceDifference) {
            outputText += `
فرق السعر للرضيع: ${infantPriceDifference} دينار`;
        }

        // إضافة فرق السعر للطفل إذا تم إدخاله
        if (childPriceDifference) {
            outputText += `
فرق السعر للطفل: ${childPriceDifference} دينار`;
        }

        // إضافة فرق السعر للبالغ إذا تم إدخاله
        if (adultPriceDifference) {
            outputText += `
فرق السعر للبالغ: ${adultPriceDifference} دينار`;
        }

        // إضافة فرق الضرائب إذا تم إدخاله
        if (safaFee || aviationTax || passengerCount) {
            outputText += `
فرق الضرائب: ${taxDifference} دينار

`; // مسافة تحت فرق الضرائب
        }

        // إضافة الإجمالي للرضيع إذا تم إدخاله
        if (infantPriceDifference) {
            outputText += `
الإجمالي للرضيع: ${totalInfant.toFixed(2)} دينار`;
        }

        // إضافة الإجمالي للطفل إذا تم إدخاله
        if (childPriceDifference) {
            outputText += `
الإجمالي للطفل: ${totalChild.toFixed(2)} دينار`;
        }

        // إضافة الإجمالي للبالغ إذا تم إدخاله
        if (adultPriceDifference) {
            outputText += `
الإجمالي للبالغ: ${totalAdult.toFixed(2)} دينار

`; // مسافة تحت الإجمالي للبالغ
        }

        // إضافة إجمالي رسوم التعديل
        outputText += `
إجمالي رسوم التعديل: ${grandTotal.toFixed(2)} دينار`;

        // إضافة رسوم عدم الظهور إذا تم إدخالها
        if (noShowFee) {
            outputText += `

في حالة عدم الظهور خلال ${hours || 0} ساعات من وقت الإقلاع:
رسوم التعديل: ${noShowFee === "غير قابلة للتعديل" ? "غير قابلة للتعديل" : noShowFee + " دينار"} + فرق السعر والضرائب (إن وجد)`;
        }

        // إضافة الملاحظة الثابتة
        outputText += `

ملاحظة:
السعر والإمكانية غير ثابتين.
هذه أقل إمكانية متاحة حاليًا.`;

        // عرض النص في منطقة الإخراج
        document.getElementById('output').innerText = outputText;
    }
</script>

</body>
</html>
