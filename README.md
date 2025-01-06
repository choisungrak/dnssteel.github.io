<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>총 길이 및 중량 계산기</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: flex-start;
            height: 100vh;
            margin: 0;
            padding-top: 20px;
            background-color: #f4f4f4;
            user-select: none;
        }
        .calculator {
            border: 1px solid #ccc;
            padding: 8px;
            background: #fff;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
            border-radius: 10px;
            width: 100%;
            max-width: 900px;
            text-align: center;
            margin-bottom: 40px; /* 줄 간격 넓힘 */
        }
        .calculator h1 {
            color: red;
            margin-bottom: 32px; /* 간격을 넓힘 */
            font-size: 1.68em;
        }
        .calculator h2 {
            color: black;
            margin-bottom: 20px;
            font-size: 1.008em; /* 40% smaller */
            text-align: center;
            line-height: 1.5; /* 줄 간격 추가 */
        }
        .input-group {
            display: flex;
            align-items: center;
            margin-bottom: 12px; /* 입력창 밑의 여백 증가 */
            justify-content: center;
        }
        .input-group label {
            margin-right: 5px;
            flex: 0 0 auto;
        }
        .input-group .length-input,
        .input-group .quantity-input {
            padding: 6px;
            font-size: 1em;
            border: 1px solid #ccc;
            border-radius: 5px;
            margin-right: 8px;
        }
        .input-group .length-input {
            width: 90px;
        }
        .input-group .quantity-input {
            width: 50px;
        }
        .input-group .thickness-select,
        .input-group .material-select {
            padding: 6px;
            font-size: 1em;
            border: 1px solid #ccc;
            border-radius: 5px;
            width: 200px; /* 넓이를 키움 */
        }
        .button {
            padding: 6px 12px;
            font-size: 1.2em;
            border: 1px solid #ccc;
            border-radius: 5px;
            cursor: pointer;
            background: #007bff; /* 파란색 */
            color: #fff;
            text-align: center;
            display: inline-block;
            width: 100%;
            max-width: 300px;
        }
        .result-container {
            display: flex;
            flex-direction: column;
            align-items: center;
            margin-top: 20px; /* 주문방법 아래의 줄간격 2배로 */
        }
        .result-group {
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 10px;
            border: 3px solid #ccc;
            border-radius: 5px;
            width: 78%;
            max-width: 624px;
            margin-bottom: 20px; /* 줄 간격 넓힘 */
        }
        .result, .weight-result, .sub-result {
            margin-top: 6px;
            font-size: 1.56em;
            color: black; /* 글자색 변경 */
            padding: 6px;
            text-align: center;
            width: 100%;
        }
        .sub-result.red {
            color: red;
        }
        .pipe-size-group {
            border: 1px solid #ccc;
            padding: 10px;
            border-radius: 10px;
            margin-bottom: 20px; /* 입력창 밑의 여백 증가 */
            display: inline-block;
            text-align: left;
            width: 65%;
            max-width: 520px;
            text-align: center;
        }
        .pipe-size-group .input-group {
            justify-content: center;
        }
        .order-instruction {
            color: black;
            margin-top: 20px;
            font-size: 1.2em;
            font-weight: bold;
        }
        .order-instruction.red {
            color: red;
        }
        .material-image {
            display: flex;
            justify-content: center; /* 사진 간격 제거 */
            margin-bottom: 50px; /* 1번 옵션 밑의 간격 절반으로 줄임 */
        }
        .material-image div {
            margin: 0;
        }
        .material-image img {
            width: 150px;
            height: auto;
            cursor: pointer;
            border: 2px solid transparent;
            border-radius: 5px;
            margin: 0 4px; /* 사진들 사이의 간격 제거 */
        }
        .material-image img.selected {
            border: 2px solid blue;
        }
        img.responsive {
            width: 100%;
            max-width: 500px;
            height: auto;
        }
        @media (max-width: 768px) {
            .calculator {
                width: 100%;
                padding: 16px;
            }
            .pipe-size-group {
                width: 100%;
            }
            .input-group .length-input,
            .input-group .quantity-input {
                width: 70px;
            }
            .input-group .thickness-select,
            .input-group .material-select {
                width: 150px;
            }
            .material-image img {
                width: 100px;
            }
            img.responsive {
                width: 100%;
                height: auto;
            }
            .result-group .sub-result {
                font-size: 1em;
                white-space: nowrap; /* 한줄에 보이도록 설정 */
            }
        }
    </style>
    <script>
        document.addEventListener('contextmenu', event => event.preventDefault());
        document.addEventListener('selectstart', event => event.preventDefault());

        function selectMaterial(imageElement, materialName) {
            document.querySelectorAll('.material-image img').forEach(img => {
                img.classList.remove('selected');
            });
            imageElement.classList.add('selected');
            document.getElementById('material').value = materialName;
            updateMaterialInfo();
        }

        function updateMaterialInfo() {
            const material = document.getElementById('material').value;
            const width = document.getElementById('width').value;
            const height = document.getElementById('height').value;
            const thickness = document.getElementById('thickness').value;
            document.getElementById('materialOutput').innerHTML = `각파이프 선택 : <span class="red">${material}</span>`;
            document.getElementById('sizeOutput').innerHTML = `각파이프 사이즈 : <span class="red">${width} x ${height} x ${thickness}t</span>`;
        }

        document.getElementById('width').addEventListener('input', updateMaterialInfo);
        document.getElementById('height').addEventListener('input', updateMaterialInfo);
        document.getElementById('thickness').addEventListener('change', updateMaterialInfo);

        function formatNumber(num) {
            return num.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",");
        }

        function calculateTotalLengthAndWeight() {
            let totalLength = 0;
            let cuttingFee = 0;
            let cuttingUnitCost = 0;
            let cuttingUnitCostText = "";

            const width = parseFloat(document.getElementById('width').value);
            const height = parseFloat(document.getElementById('height').value);

            for (let i = 1; i <= 10; i++) {
                const length = parseFloat(document.getElementById('length' + i).value);
                const quantity = parseInt(document.getElementById('quantity' + i).value);
                
                if (!isNaN(length) && !isNaN(quantity) && length > 0 && quantity > 0) {
                    totalLength += length * quantity;
                    if (length < 1000) {
                        cuttingFee += quantity;
                    }
                }
            }

            const totalLengthM = Math.ceil(totalLength / 1000);

            if (width + height <= 100) {
                cuttingUnitCost = 1000;
                cuttingUnitCostText = "절단비(15x15~50x50)";
            } else if (width + height < 200) {
                cuttingUnitCost = 2000;
                cuttingUnitCostText = "절단비(60x60~100x50)";
            } else {
                cuttingUnitCost = 3000;
                cuttingUnitCostText = "절단비(100x100)";
            }

            document.getElementById('result').textContent = `총 길이: ${formatNumber(totalLength)} mm`;
            document.getElementById('resultM').innerHTML = `구매 수량 : <span class="red">${totalLengthM} M</span>`;
            document.getElementById('cuttingUnitCost').innerHTML = `절단비 선택 : <span class="red">${cuttingUnitCostText}</span>`;
            document.getElementById('cuttingFee').innerHTML = `절단비 수량 : <span class="red">${cuttingFee} 개</span>`;

            const thickness = parseFloat(document.getElementById('thickness').value);

            if (isNaN(width) || isNaN(height) || isNaN(thickness) || width <= 0 || height <= 0 || thickness <= 0 || totalLength <= 0) {
                document.getElementById('weightResult').innerHTML = `중량: <span class="red">0 KG</span>`;
                return;
            }

            const weight = ((width * 2 + height * 2) * thickness * totalLength * 7.8) / 1000000;

            document.getElementById('weightResult').innerHTML = `중량: <span class="red">${weight.toFixed(1)} KG</span>`;
        }

        window.onload = updateMaterialInfo;
    </script>
</head>
<body>
<div class="calculator">
    <h1>디엔에스스틸 주문방법 계산기</h1>
    <h2>1. 각파이프의 종류를 선택해 주세요.</h2>
    <div class="material-image">
        <div>
            <img src="https://gi.esmplus.com/dnssteel/%EC%95%84%EC%97%B0%EA%B0%81%ED%8C%8C%EC%9D%B4%ED%94%84.jpg" alt="아연각파이프" onclick="selectMaterial(this, '아연 각파이프')">
        </div>
        <div>
            <img src="https://gi.esmplus.com/dnssteel/%EC%B9%BC%EB%9D%BC%EA%B0%81%ED%8C%8C%EC%9D%B4%ED%94%84.jpg" alt="칼라각파이프" onclick="selectMaterial(this, '칼라 각파이프')">
        </div>
        <div>
            <img src="https://gi.esmplus.com/dnssteel/%ED%9D%91%EA%B4%80%EA%B0%81%ED%8C%8C%EC%9D%B4%ED%94%84.jpg" alt="흑관각파이프" onclick="selectMaterial(this, '흑관 각파이프')">
        </div>
    </div>
    <h2>2. 각파이프 사이즈를 입력한 후 두께를 선택해주세요.</h2>
    <img src='https://gi.esmplus.com/dnssteel/%EB%84%A4%EC%9D%B4%EB%B2%84%EA%B0%81%ED%8C%8C%EC%9D%B4%ED%94%84%2024%EB%85%84%EB%8F%84%20%EC%88%98%EC%A0%95%EC%A4%91%20light.jpg' alt='네이버각파이프' class="responsive">
    <form id="calculatorForm">
        <div class="pipe-size-group">
            <div class="input-group">
                <label for="material">종류:</label>
                <select id="material" class="material-select" onchange="updateMaterialInfo()">
                    <option value="">선택하세요</option>
                    <option value="아연 각파이프">아연 각파이프</option>
                    <option value="흑관 각파이프">흑관 각파이프</option>
                    <option value="칼라 각파이프">칼라 각파이프</option>
                </select>
            </div>
            <div class="input-group">
                <label for="width">가로 (mm):</label>
                <input type="number" id="width" class="length-input" placeholder="가로" step="0.01">
            </div>
            <div class="input-group">
                <label for="height">세로 (mm):</label>
                <input type="number" id="height" class="length-input" placeholder="세로" step="0.01">
            </div>
            <div class="input-group">
                <label for="thickness">두께 (mm):</label>
                <select id="thickness" class="thickness-select" onchange="updateMaterialInfo()">
                    <option value="">선택하세요</option>
                    <option value="1.4">1.4t</option>
                    <option value="2.0">2.0t</option>
                    <option value="2.9">2.9t</option>
                </select>
            </div>
        </div>
        <h2>3. 각파이프의 절단사이즈와 수량을 입력해주세요.</h2>
        <div class="input-group">
            <label for="length1">1. 사이즈 (mm):</label>
            <input type="number" id="length1" class="length-input" placeholder="길이" step="0.01">
            <label for="quantity1">수량:</label>
            <input type="number" id="quantity1" class="quantity-input" placeholder="수량" step="1">
        </div>
        <div class="input-group">
            <label for="length2">2. 사이즈 (mm):</label>
            <input type="number" id="length2" class="length-input" placeholder="길이" step="0.01">
            <label for="quantity2">수량:</label>
            <input type="number" id="quantity2" class="quantity-input" placeholder="수량" step="1">
        </div>
        <div class="input-group">
            <label for="length3">3. 사이즈 (mm):</label>
            <input type="number" id="length3" class="length-input" placeholder="길이" step="0.01">
            <label for="quantity3">수량:</label>
            <input type="number" id="quantity3" class="quantity-input" placeholder="수량" step="1">
        </div>
        <div class="input-group">
            <label for="length4">4. 사이즈 (mm):</label>
            <input type="number" id="length4" class="length-input" placeholder="길이" step="0.01">
            <label for="quantity4">수량:</label>
            <input type="number" id="quantity4" class="quantity-input" placeholder="수량" step="1">
        </div>
        <div class="input-group">
            <label for="length5">5. 사이즈 (mm):</label>
            <input type="number" id="length5" class="length-input" placeholder="길이" step="0.01">
            <label for="quantity5">수량:</label>
            <input type="number" id="quantity5" class="quantity-input" placeholder="수량" step="1">
        </div>
        <div class="input-group">
            <label for="length6">6. 사이즈 (mm):</label>
            <input type="number" id="length6" class="length-input" placeholder="길이" step="0.01">
            <label for="quantity6">수량:</label>
            <input type="number" id="quantity6" class="quantity-input" placeholder="수량" step="1">
        </div>
        <div class="input-group">
            <label for="length7">7. 사이즈 (mm):</label>
            <input type="number" id="length7" class="length-input" placeholder="길이" step="0.01">
            <label for="quantity7">수량:</label>
            <input type="number" id="quantity7" class="quantity-input" placeholder="수량" step="1">
        </div>
        <div class="input-group">
            <label for="length8">8. 사이즈 (mm):</label>
            <input type="number" id="length8" class="length-input" placeholder="길이" step="0.01">
            <label for="quantity8">수량:</label>
            <input type="number" id="quantity8" class="quantity-input" placeholder="수량" step="1">
        </div>
        <div class="input-group">
            <label for="length9">9. 사이즈 (mm):</label>
            <input type="number" id="length9" class="length-input" placeholder="길이" step="0.01">
            <label for="quantity9">수량:</label>
            <input type="number" id="quantity9" class="quantity-input" placeholder="수량" step="1">
        </div>
        <div class="input-group">
            <label for="length10">10. 사이즈 (mm):</label>
            <input type="number" id="length10" class="length-input" placeholder="길이" step="0.01">
            <label for="quantity10">수량:</label>
            <input type="number" id="quantity10" class="quantity-input" placeholder="수량" step="1">
        </div>
        <h2 style="margin-bottom: 30px;">4. 계산하기를 눌러주세요.</h2>
        <div class="button" type="button" onclick="calculateTotalLengthAndWeight()">계산하기</div>
    </form>
    <div class="result-container">
        <div class="result-group">
            <div class="result" id="result">총 길이: 0 mm</div>
            <div class="weight-result" id="weightResult">중량: 0 Kg</div>
        </div>
        <div class="result-group">
            <div class="sub-result" id="orderMethod">주문방법</div>
            <div class="sub-result" id="materialOutput">각파이프 선택 : <span class="red"></span></div>
            <div class="sub-result" id="sizeOutput">각파이프 사이즈 : <span class="red"></span></div>
            <div class="sub-result" id="resultM" style="margin-bottom: 40px;">구매 수량 : <span class="red">0 M</span></div> <!-- 줄간격 2배로 -->
            <div class="sub-result" id="cuttingUnitCost">절단비 선택 : <span class="red"></span></div>
            <div class="sub-result" id="cuttingFee">절단비 수량 : <span class="red">0 개</span></div>
        </div>
    </div>
    <div class="order-instruction">위 주문방법을 참고해 주문해주시면 됩니다.</div>
    <div class="order-instruction red">절단사이즈는 꼭! 배송요청사항에 남겨주시기 바랍니다.</div>
    <div class="material-images">
        <img src='https://gi.esmplus.com/dnssteel/%EA%B3%84%EC%82%B0%EA%B8%B0%EC%9D%B4%EB%AF%B8%EC%A7%80/5.jpg' class="responsive"><br />
        <img src='https://gi.esmplus.com/dnssteel/%EA%B3%84%EC%82%B0%EA%B8%B0%EC%9D%B4%EB%AF%B8%EC%A7%80/6.jpg' class="responsive"><br />
        <img src='https://gi.esmplus.com/dnssteel/%EA%B3%84%EC%82%B0%EA%B8%B0%EC%9D%B4%EB%AF%B8%EC%A7%80/7.jpg' class="responsive"><br />
    </div>
</div>
<script>
    function selectMaterial(imageElement, materialName) {
        document.querySelectorAll('.material-image img').forEach(img => {
            img.classList.remove('selected');
        });
        imageElement.classList.add('selected');
        document.getElementById('material').value = materialName;
        updateMaterialInfo();
    }

    function updateMaterialInfo() {
        const material = document.getElementById('material').value;
        const width = document.getElementById('width').value;
        const height = document.getElementById('height').value;
        const thickness = document.getElementById('thickness').value;
        document.getElementById('materialOutput').innerHTML = `각파이프 선택 : <span class="red">${material}</span>`;
        document.getElementById('sizeOutput').innerHTML = `각파이프 사이즈 : <span class="red">${width} x ${height} x ${thickness}t</span>`;
    }

    document.getElementById('width').addEventListener('input', updateMaterialInfo);
    document.getElementById('height').addEventListener('input', updateMaterialInfo);
    document.getElementById('thickness').addEventListener('change', updateMaterialInfo);

    function formatNumber(num) {
        return num.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",");
    }

    function calculateTotalLengthAndWeight() {
        let totalLength = 0;
        let cuttingFee = 0;
        let cuttingUnitCost = 0;
        let cuttingUnitCostText = "";

        const width = parseFloat(document.getElementById('width').value);
        const height = parseFloat(document.getElementById('height').value);

        for (let i = 1; i <= 10; i++) {
            const length = parseFloat(document.getElementById('length' + i).value);
            const quantity = parseInt(document.getElementById('quantity' + i).value);
            
            if (!isNaN(length) && !isNaN(quantity) && length > 0 && quantity > 0) {
                totalLength += length * quantity;
                if (length < 1000) {
                    cuttingFee += quantity;
                }
            }
        }

        const totalLengthM = Math.ceil(totalLength / 1000);

        if (width + height <= 100) {
            cuttingUnitCost = 1000;
            cuttingUnitCostText = "절단비(15x15~50x50)";
        } else if (width + height < 200) {
            cuttingUnitCost = 2000;
            cuttingUnitCostText = "절단비(60x60~100x50)";
        } else {
            cuttingUnitCost = 3000;
            cuttingUnitCostText = "절단비(100x100)";
        }

        document.getElementById('result').textContent = `총 길이: ${formatNumber(totalLength)} mm`;
        document.getElementById('resultM').innerHTML = `구매 수량 : <span class="red">${totalLengthM} M</span>`;
        document.getElementById('cuttingUnitCost').innerHTML = `절단비 선택 : <span class="red">${cuttingUnitCostText}</span>`;
        document.getElementById('cuttingFee').innerHTML = `절단비 수량 : <span class="red">${cuttingFee} 개</span>`;

        const thickness = parseFloat(document.getElementById('thickness').value);

        if (isNaN(width) || isNaN(height) || isNaN(thickness) || width <= 0 || height <= 0 || thickness <= 0 || totalLength <= 0) {
            document.getElementById('weightResult').innerHTML = `중량: <span class="red">0 KG</span>`;
            return;
        }

        const weight = ((width * 2 + height * 2) * thickness * totalLength * 7.8) / 1000000;

        document.getElementById('weightResult').innerHTML = `중량: <span class="red">${weight.toFixed(1)} KG</span>`;
    }

    window.onload = updateMaterialInfo;
</script>
</body>
</html>
