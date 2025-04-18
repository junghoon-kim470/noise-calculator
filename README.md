<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>소음 계산기</title>
    <style>
        body {
            font-family: sans-serif;
            padding: 20px;
            background-color: #f4f4f9;
        }
        h1 {
            text-align: center;
            color: #007bff;
        }
        input, label, button {
            display: block;
            width: 100%;
            margin: 10px 0;
            font-size: 16px;
        }
        button {
            padding: 10px;
            background-color: #007bff;
            color: white;
            border: none;
            border-radius: 5px;
        }
        .result {
            margin-top: 20px;
            padding: 10px;
            background-color: #e9ecef;
            border-radius: 5px;
            font-size: 18px;
        }
    </style>
</head>
<body>
    <h1>소음 계산기</h1>
    <label for="noise">집회 소음값 (dB):</label>
    <input type="number" id="noise" placeholder="예: 70">

    <label for="background">배경 소음값 (dB):</label>
    <input type="number" id="background" placeholder="예: 60">

    <label>
        <input type="checkbox" id="removeBackground"> 배경소음 제거
    </label>

    <button onclick="calculate()">계산하기</button>

    <div class="result" id="result"></div>

    <script>
        function calculate() {
            const noise = parseFloat(document.getElementById('noise').value);
            const background = parseFloat(document.getElementById('background').value);
            const removeBackground = document.getElementById('removeBackground').checked;
            const resultDiv = document.getElementById('result');

            if (isNaN(noise) || isNaN(background)) {
                resultDiv.innerHTML = "정확한 값을 입력하세요.";
                return;
            }

            let leq = noise;
            let lmax = noise;

            if (removeBackground) {
                const noisePower = Math.pow(10, noise / 10);
                const bgPower = Math.pow(10, background / 10);
                const netPower = noisePower - bgPower;

                if (netPower <= 0) {
                    resultDiv.innerHTML = "배경소음이 더 크거나 같아 계산할 수 없습니다.";
                    return;
                }

                leq = 10 * Math.log10(netPower);
            }

            resultDiv.innerHTML = `
                <strong>등가소음도 (Leq):</strong> ${leq.toFixed(2)} dB<br>
                <strong>최고소음도 (Lmax):</strong> ${lmax.toFixed(2)} dB
            `;
        }
    </script>
</body>
</html>
