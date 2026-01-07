<!DOCTYPE html>

<html lang="zh-CN">

<head>

&nbsp;   <meta charset="UTF-8">

&nbsp;   <meta name="viewport" content="width=device-width, initial-scale=1.0">

&nbsp;   <title>特征值与特征向量几何可视化</title>

&nbsp;   <style>

&nbsp;       \* {

&nbsp;           margin: 0;

&nbsp;           padding: 0;

&nbsp;           box-sizing: border-box;

&nbsp;           font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;

&nbsp;       }

&nbsp;       

&nbsp;       body {

&nbsp;           background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);

&nbsp;           min-height: 100vh;

&nbsp;           display: flex;

&nbsp;           justify-content: center;

&nbsp;           align-items: center;

&nbsp;           padding: 20px;

&nbsp;       }

&nbsp;       

&nbsp;       .container {

&nbsp;           width: 100%;

&nbsp;           max-width: 1600px;

&nbsp;           background: white;

&nbsp;           border-radius: 16px;

&nbsp;           box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);

&nbsp;           overflow: hidden;

&nbsp;       }

&nbsp;       

&nbsp;       header {

&nbsp;           background: linear-gradient(90deg, #1a2980 0%, #26d0ce 100%);

&nbsp;           color: white;

&nbsp;           padding: 25px 40px;

&nbsp;           text-align: center;

&nbsp;       }

&nbsp;       

&nbsp;       h1 {

&nbsp;           font-size: 2.5rem;

&nbsp;           margin-bottom: 8px;

&nbsp;       }

&nbsp;       

&nbsp;       .subtitle {

&nbsp;           font-size: 1.1rem;

&nbsp;           opacity: 0.9;

&nbsp;           font-weight: 300;

&nbsp;       }

&nbsp;       

&nbsp;       .main-content {

&nbsp;           display: flex;

&nbsp;           padding: 30px;

&nbsp;           gap: 30px;

&nbsp;       }

&nbsp;       

&nbsp;       .left-panel {

&nbsp;           flex: 1.2;

&nbsp;           display: flex;

&nbsp;           flex-direction: column;

&nbsp;           gap: 30px;

&nbsp;       }

&nbsp;       

&nbsp;       .right-panel {

&nbsp;           flex: 2;

&nbsp;           display: flex;

&nbsp;           flex-direction: column;

&nbsp;           gap: 30px;

&nbsp;       }

&nbsp;       

&nbsp;       .control-panel {

&nbsp;           background-color: #f8fafc;

&nbsp;           padding: 25px;

&nbsp;           border-radius: 12px;

&nbsp;           box-shadow: 0 5px 15px rgba(0, 0, 0, 0.05);

&nbsp;       }

&nbsp;       

&nbsp;       .matrix-input {

&nbsp;           margin-bottom: 25px;

&nbsp;       }

&nbsp;       

&nbsp;       .matrix-input h3 {

&nbsp;           margin-bottom: 15px;

&nbsp;           color: #1a2980;

&nbsp;           font-size: 1.3rem;

&nbsp;           border-bottom: 2px solid #26d0ce;

&nbsp;           padding-bottom: 8px;

&nbsp;       }

&nbsp;       

&nbsp;       .matrix-grid {

&nbsp;           display: grid;

&nbsp;           grid-template-columns: repeat(2, 1fr);

&nbsp;           gap: 15px;

&nbsp;           max-width: 220px;

&nbsp;           margin: 0 auto 20px;

&nbsp;       }

&nbsp;       

&nbsp;       .matrix-input input {

&nbsp;           width: 100%;

&nbsp;           padding: 14px;

&nbsp;           font-size: 1.3rem;

&nbsp;           text-align: center;

&nbsp;           border: 2px solid #e2e8f0;

&nbsp;           border-radius: 8px;

&nbsp;           transition: all 0.3s;

&nbsp;           font-weight: 500;

&nbsp;       }

&nbsp;       

&nbsp;       .matrix-input input:focus {

&nbsp;           border-color: #26d0ce;

&nbsp;           box-shadow: 0 0 0 3px rgba(38, 208, 206, 0.2);

&nbsp;           outline: none;

&nbsp;       }

&nbsp;       

&nbsp;       .matrix-input input.error {

&nbsp;           border-color: #e53e3e;

&nbsp;           background-color: #fff5f5;

&nbsp;       }

&nbsp;       

&nbsp;       .error-message {

&nbsp;           color: #e53e3e;

&nbsp;           font-size: 0.9rem;

&nbsp;           margin-top: 8px;

&nbsp;           text-align: center;

&nbsp;           min-height: 20px;

&nbsp;       }

&nbsp;       

&nbsp;       .input-hint {

&nbsp;           color: #718096;

&nbsp;           font-size: 0.9rem;

&nbsp;           text-align: center;

&nbsp;           margin-top: 10px;

&nbsp;       }

&nbsp;       

&nbsp;       .buttons {

&nbsp;           display: flex;

&nbsp;           gap: 15px;

&nbsp;           margin-bottom: 25px;

&nbsp;       }

&nbsp;       

&nbsp;       button {

&nbsp;           flex: 1;

&nbsp;           padding: 16px;

&nbsp;           font-size: 1.1rem;

&nbsp;           border: none;

&nbsp;           border-radius: 8px;

&nbsp;           cursor: pointer;

&nbsp;           transition: all 0.3s;

&nbsp;           font-weight: 600;

&nbsp;       }

&nbsp;       

&nbsp;       #generate-btn {

&nbsp;           background: linear-gradient(90deg, #1a2980 0%, #26d0ce 100%);

&nbsp;           color: white;

&nbsp;       }

&nbsp;       

&nbsp;       #generate-btn:hover:not(:disabled) {

&nbsp;           transform: translateY(-2px);

&nbsp;           box-shadow: 0 7px 14px rgba(26, 41, 128, 0.2);

&nbsp;       }

&nbsp;       

&nbsp;       #generate-btn:disabled {

&nbsp;           opacity: 0.6;

&nbsp;           cursor: not-allowed;

&nbsp;       }

&nbsp;       

&nbsp;       #reset-btn {

&nbsp;           background-color: #e2e8f0;

&nbsp;           color: #4a5568;

&nbsp;       }

&nbsp;       

&nbsp;       #reset-btn:hover {

&nbsp;           background-color: #cbd5e0;

&nbsp;           transform: translateY(-2px);

&nbsp;       }

&nbsp;       

&nbsp;       .eigen-info {

&nbsp;           background-color: white;

&nbsp;           padding: 20px;

&nbsp;           border-radius: 10px;

&nbsp;           box-shadow: 0 5px 15px rgba(0, 0, 0, 0.05);

&nbsp;       }

&nbsp;       

&nbsp;       .eigen-info h3 {

&nbsp;           margin-bottom: 15px;

&nbsp;           color: #1a2980;

&nbsp;           font-size: 1.3rem;

&nbsp;       }

&nbsp;       

&nbsp;       .eigen-values, .eigen-vectors {

&nbsp;           margin-bottom: 20px;

&nbsp;       }

&nbsp;       

&nbsp;       .eigen-item {

&nbsp;           display: flex;

&nbsp;           align-items: center;

&nbsp;           margin-bottom: 12px;

&nbsp;           padding: 12px;

&nbsp;           background-color: #f8fafc;

&nbsp;           border-radius: 8px;

&nbsp;       }

&nbsp;       

&nbsp;       .eigen-color {

&nbsp;           width: 20px;

&nbsp;           height: 20px;

&nbsp;           border-radius: 50%;

&nbsp;           margin-right: 15px;

&nbsp;       }

&nbsp;       

&nbsp;       .color-red {

&nbsp;           background-color: #e53e3e;

&nbsp;       }

&nbsp;       

&nbsp;       .color-blue {

&nbsp;           background-color: #3182ce;

&nbsp;       }

&nbsp;       

&nbsp;       .flip-note {

&nbsp;           color: #d69e2e;

&nbsp;           font-weight: 600;

&nbsp;           background-color: #fffaf0;

&nbsp;           padding: 5px 10px;

&nbsp;           border-radius: 4px;

&nbsp;           margin-top: 10px;

&nbsp;           display: none;

&nbsp;       }

&nbsp;       

&nbsp;       .loading-indicator {

&nbsp;           display: none;

&nbsp;           text-align: center;

&nbsp;           color: #3182ce;

&nbsp;           font-weight: 500;

&nbsp;           margin: 10px 0;

&nbsp;       }

&nbsp;       

&nbsp;       .loading-indicator.active {

&nbsp;           display: block;

&nbsp;       }

&nbsp;       

&nbsp;       .auto-update-label {

&nbsp;           display: flex;

&nbsp;           align-items: center;

&nbsp;           justify-content: center;

&nbsp;           gap: 8px;

&nbsp;           margin-top: 15px;

&nbsp;           color: #4a5568;

&nbsp;           font-size: 0.95rem;

&nbsp;       }

&nbsp;       

&nbsp;       .auto-update-label input {

&nbsp;           width: 18px;

&nbsp;           height: 18px;

&nbsp;       }

&nbsp;       

&nbsp;       .visualization-area {

&nbsp;           display: flex;

&nbsp;           flex-direction: column;

&nbsp;           gap: 30px;

&nbsp;           flex: 1;

&nbsp;       }

&nbsp;       

&nbsp;       .canvas-row {

&nbsp;           display: flex;

&nbsp;           gap: 30px;

&nbsp;           height: 320px;

&nbsp;       }

&nbsp;       

&nbsp;       .canvas-container {

&nbsp;           flex: 1;

&nbsp;           background-color: white;

&nbsp;           border-radius: 12px;

&nbsp;           box-shadow: 0 5px 15px rgba(0, 0, 0, 0.05);

&nbsp;           overflow: hidden;

&nbsp;           position: relative;

&nbsp;           height: 100%;

&nbsp;       }

&nbsp;       

&nbsp;       canvas {

&nbsp;           display: block;

&nbsp;           width: 100%;

&nbsp;           height: 100%;

&nbsp;       }

&nbsp;       

&nbsp;       .canvas-title {

&nbsp;           position: absolute;

&nbsp;           top: 15px;

&nbsp;           left: 15px;

&nbsp;           background-color: rgba(255, 255, 255, 0.9);

&nbsp;           padding: 8px 16px;

&nbsp;           border-radius: 6px;

&nbsp;           font-weight: 600;

&nbsp;           color: #1a2980;

&nbsp;           box-shadow: 0 3px 8px rgba(0, 0, 0, 0.1);

&nbsp;           z-index: 10;

&nbsp;       }

&nbsp;       

&nbsp;       .canvas-title.transform {

&nbsp;           left: auto;

&nbsp;           right: 15px;

&nbsp;       }

&nbsp;       

&nbsp;       .legend {

&nbsp;           display: flex;

&nbsp;           justify-content: center;

&nbsp;           gap: 20px;

&nbsp;           margin-top: 10px;

&nbsp;           flex-wrap: wrap;

&nbsp;       }

&nbsp;       

&nbsp;       .legend-item {

&nbsp;           display: flex;

&nbsp;           align-items: center;

&nbsp;           font-size: 0.9rem;

&nbsp;       }

&nbsp;       

&nbsp;       .dynamic-explanation {

&nbsp;           background: linear-gradient(135deg, #f0f4ff 0%, #e6f7ff 100%);

&nbsp;           padding: 25px;

&nbsp;           border-radius: 12px;

&nbsp;           box-shadow: 0 5px 15px rgba(0, 0, 0, 0.05);

&nbsp;           border-left: 4px solid #1a2980;

&nbsp;           flex: 0.8;

&nbsp;       }

&nbsp;       

&nbsp;       .dynamic-explanation h3 {

&nbsp;           margin-bottom: 15px;

&nbsp;           color: #1a2980;

&nbsp;           font-size: 1.3rem;

&nbsp;           display: flex;

&nbsp;           align-items: center;

&nbsp;           gap: 10px;

&nbsp;       }

&nbsp;       

&nbsp;       .dynamic-explanation h3::before {

&nbsp;           content: "💡";

&nbsp;           font-size: 1.5rem;

&nbsp;       }

&nbsp;       

&nbsp;       .dynamic-content {

&nbsp;           line-height: 1.6;

&nbsp;           max-height: 300px;

&nbsp;           overflow-y: auto;

&nbsp;           padding-right: 10px;

&nbsp;       }

&nbsp;       

&nbsp;       .dynamic-content p {

&nbsp;           margin-bottom: 15px;

&nbsp;       }

&nbsp;       

&nbsp;       .highlight {

&nbsp;           background-color: #fffacd;

&nbsp;           padding: 3px 6px;

&nbsp;           border-radius: 4px;

&nbsp;           font-weight: 600;

&nbsp;       }

&nbsp;       

&nbsp;       .value-highlight {

&nbsp;           color: #1a2980;

&nbsp;           font-weight: 600;

&nbsp;           background-color: #e6f7ff;

&nbsp;           padding: 2px 6px;

&nbsp;           border-radius: 4px;

&nbsp;       }

&nbsp;       

&nbsp;       .math-explanation {

&nbsp;           background-color: white;

&nbsp;           padding: 20px;

&nbsp;           border-radius: 10px;

&nbsp;           box-shadow: 0 5px 15px rgba(0, 0, 0, 0.05);

&nbsp;       }

&nbsp;       

&nbsp;       .math-explanation h3 {

&nbsp;           margin-bottom: 15px;

&nbsp;           color: #1a2980;

&nbsp;           font-size: 1.3rem;

&nbsp;       }

&nbsp;       

&nbsp;       .math-explanation p {

&nbsp;           line-height: 1.6;

&nbsp;           margin-bottom: 15px;

&nbsp;       }

&nbsp;       

&nbsp;       .formula {

&nbsp;           font-family: 'Cambria Math', 'Times New Roman', serif;

&nbsp;           font-size: 1.2rem;

&nbsp;           background-color: #f8fafc;

&nbsp;           padding: 15px;

&nbsp;           border-radius: 8px;

&nbsp;           text-align: center;

&nbsp;           margin: 15px 0;

&nbsp;           border-left: 4px solid #26d0ce;

&nbsp;       }

&nbsp;       

&nbsp;       .footer {

&nbsp;           text-align: center;

&nbsp;           padding: 20px;

&nbsp;           color: #718096;

&nbsp;           font-size: 0.9rem;

&nbsp;           border-top: 1px solid #e2e8f0;

&nbsp;           margin-top: 20px;

&nbsp;       }

&nbsp;       

&nbsp;       @media (max-width: 1400px) {

&nbsp;           .main-content {

&nbsp;               flex-direction: column;

&nbsp;           }

&nbsp;           

&nbsp;           .canvas-row {

&nbsp;               height: 280px;

&nbsp;           }

&nbsp;           

&nbsp;           .left-panel, .right-panel {

&nbsp;               width: 100%;

&nbsp;           }

&nbsp;       }

&nbsp;       

&nbsp;       @media (max-width: 768px) {

&nbsp;           .matrix-grid {

&nbsp;               max-width: 180px;

&nbsp;           }

&nbsp;           

&nbsp;           .matrix-input input {

&nbsp;               padding: 12px;

&nbsp;               font-size: 1.1rem;

&nbsp;           }

&nbsp;           

&nbsp;           .canvas-row {

&nbsp;               flex-direction: column;

&nbsp;               height: auto;

&nbsp;           }

&nbsp;           

&nbsp;           .canvas-container {

&nbsp;               height: 250px;

&nbsp;           }

&nbsp;           

&nbsp;           .dynamic-explanation {

&nbsp;               padding: 18px;

&nbsp;           }

&nbsp;       }

&nbsp;       

&nbsp;       .quick-matrix-buttons {

&nbsp;           display: grid;

&nbsp;           grid-template-columns: repeat(2, 1fr);

&nbsp;           gap: 10px;

&nbsp;           margin-top: 20px;

&nbsp;       }

&nbsp;       

&nbsp;       .quick-matrix-btn {

&nbsp;           padding: 10px;

&nbsp;           background-color: #e6f7ff;

&nbsp;           border: 1px solid #b3e0ff;

&nbsp;           border-radius: 6px;

&nbsp;           cursor: pointer;

&nbsp;           font-size: 0.9rem;

&nbsp;           text-align: center;

&nbsp;           transition: all 0.2s;

&nbsp;       }

&nbsp;       

&nbsp;       .quick-matrix-btn:hover {

&nbsp;           background-color: #cceeff;

&nbsp;           transform: translateY(-2px);

&nbsp;       }

&nbsp;       

&nbsp;       .matrix-preset-title {

&nbsp;           text-align: center;

&nbsp;           margin-top: 20px;

&nbsp;           color: #4a5568;

&nbsp;           font-size: 0.9rem;

&nbsp;       }

&nbsp;   </style>

</head>

<body>

&nbsp;   <div class="container">

&nbsp;       <header>

&nbsp;           <h1>特征值与特征向量几何可视化</h1>

&nbsp;           <p class="subtitle">直观理解线性变换中保持方向不变的向量</p>

&nbsp;       </header>

&nbsp;       

&nbsp;       <div class="main-content">

&nbsp;           <div class="left-panel">

&nbsp;               <div class="control-panel">

&nbsp;                   <div class="matrix-input">

&nbsp;                       <h3>输入 2×2 矩阵</h3>

&nbsp;                       <div class="matrix-grid">

&nbsp;                           <input type="text" id="a11" value="2" inputmode="decimal">

&nbsp;                           <input type="text" id="a12" value="1" inputmode="decimal">

&nbsp;                           <input type="text" id="a21" value="1" inputmode="decimal">

&nbsp;                           <input type="text" id="a22" value="2" inputmode="decimal">

&nbsp;                       </div>

&nbsp;                       <div class="error-message" id="matrix-error"></div>

&nbsp;                       <div class="input-hint">

&nbsp;                           提示：输入数字（支持小数和负数，如-1.5）

&nbsp;                       </div>

&nbsp;                   </div>

&nbsp;                   

&nbsp;                   <div class="auto-update-label">

&nbsp;                       <input type="checkbox" id="auto-update" checked>

&nbsp;                       <label for="auto-update">实时更新可视化</label>

&nbsp;                   </div>

&nbsp;                   

&nbsp;                   <div class="loading-indicator" id="loading-indicator">计算中...</div>

&nbsp;                   

&nbsp;                   <div class="buttons">

&nbsp;                       <button id="generate-btn" disabled>生成变换</button>

&nbsp;                       <button id="reset-btn">重置矩阵</button>

&nbsp;                   </div>

&nbsp;                   

&nbsp;                   <div class="matrix-preset-title">快速矩阵示例：</div>

&nbsp;                   <div class="quick-matrix-buttons">

&nbsp;                       <div class="quick-matrix-btn" data-matrix="2,1,1,2">对称矩阵</div>

&nbsp;                       <div class="quick-matrix-btn" data-matrix="1,2,3,4">一般矩阵</div>

&nbsp;                       <div class="quick-matrix-btn" data-matrix="0,-1,1,0">旋转矩阵</div>

&nbsp;                       <div class="quick-matrix-btn" data-matrix="2,0,0,0.5">缩放矩阵</div>

&nbsp;                       <div class="quick-matrix-btn" data-matrix="1,0,0,-1">镜像矩阵</div>

&nbsp;                       <div class="quick-matrix-btn" data-matrix="0.5,0,0,0.5">收缩矩阵</div>

&nbsp;                   </div>

&nbsp;               </div>

&nbsp;               

&nbsp;               <div class="eigen-info">

&nbsp;                   <h3>特征值与特征向量</h3>

&nbsp;                   <div class="eigen-values">

&nbsp;                       <div class="eigen-item">

&nbsp;                           <div class="eigen-color color-red"></div>

&nbsp;                           <div>

&nbsp;                               <strong>特征值 λ₁:</strong> <span id="lambda1">3.00</span>

&nbsp;                           </div>

&nbsp;                       </div>

&nbsp;                       <div class="eigen-item">

&nbsp;                           <div class="eigen-color color-blue"></div>

&nbsp;                           <div>

&nbsp;                               <strong>特征值 λ₂:</strong> <span id="lambda2">1.00</span>

&nbsp;                           </div>

&nbsp;                       </div>

&nbsp;                   </div>

&nbsp;                   

&nbsp;                   <div class="eigen-vectors">

&nbsp;                       <div class="eigen-item">

&nbsp;                           <div class="eigen-color color-red"></div>

&nbsp;                           <div>

&nbsp;                               <strong>特征向量 v₁:</strong> <span id="vector1">\[0.707, 0.707]</span>

&nbsp;                           </div>

&nbsp;                       </div>

&nbsp;                       <div class="eigen-item">

&nbsp;                           <div class="eigen-color color-blue"></div>

&nbsp;                           <div>

&nbsp;                               <strong>特征向量 v₂:</strong> <span id="vector2">\[-0.707, 0.707]</span>

&nbsp;                           </div>

&nbsp;                       </div>

&nbsp;                   </div>

&nbsp;                   

&nbsp;                   <div id="flip-note1" class="flip-note">特征值为负，变换后方向翻转</div>

&nbsp;                   <div id="flip-note2" class="flip-note">特征值为负，变换后方向翻转</div>

&nbsp;               </div>

&nbsp;           </div>

&nbsp;           

&nbsp;           <div class="right-panel">

&nbsp;               <div class="visualization-area">

&nbsp;                   <div class="canvas-row">

&nbsp;                       <div class="canvas-container">

&nbsp;                           <div class="canvas-title">原始图形：单位圆与网格</div>

&nbsp;                           <canvas id="original-canvas"></canvas>

&nbsp;                       </div>

&nbsp;                       

&nbsp;                       <div class="canvas-container">

&nbsp;                           <div class="canvas-title transform">变换后图形与特征向量</div>

&nbsp;                           <canvas id="transformed-canvas"></canvas>

&nbsp;                       </div>

&nbsp;                   </div>

&nbsp;                   

&nbsp;                   <div class="legend">

&nbsp;                       <div class="legend-item">

&nbsp;                           <div class="eigen-color color-red" style="margin-right: 8px;"></div>

&nbsp;                           <span>第一特征向量 (λ₁)</span>

&nbsp;                       </div>

&nbsp;                       <div class="legend-item">

&nbsp;                           <div class="eigen-color color-blue" style="margin-right: 8px;"></div>

&nbsp;                           <span>第二特征向量 (λ₂)</span>

&nbsp;                       </div>

&nbsp;                       <div class="legend-item">

&nbsp;                           <div style="width: 20px; height: 2px; background-color: #3498db; margin-right: 8px;"></div>

&nbsp;                           <span>单位圆</span>

&nbsp;                       </div>

&nbsp;                       <div class="legend-item">

&nbsp;                           <div style="width: 20px; height: 2px; background-color: #2ecc71; margin-right: 8px;"></div>

&nbsp;                           <span>网格</span>

&nbsp;                       </div>

&nbsp;                       <div class="legend-item">

&nbsp;                           <div style="width: 20px; height: 0; border-top: 2px dashed #e53e3e; margin-right: 8px;"></div>

&nbsp;                           <span>变换后向量</span>

&nbsp;                       </div>

&nbsp;                   </div>

&nbsp;               </div>

&nbsp;               

&nbsp;               <div class="dynamic-explanation">

&nbsp;                   <h3>当前矩阵变换动态解释</h3>

&nbsp;                   <div class="dynamic-content" id="dynamic-explanation-content">

&nbsp;                       <p>对于当前矩阵 <span class="value-highlight" id="matrix-display">\[\[2, 1], \[1, 2]]</span>：</p>

&nbsp;                       <p>红色特征向量 <span class="highlight">v₁ = \[0.707, 0.707]</span> 对应的特征值 <span class="value-highlight">λ₁ = 3.00</span>。在线性变换中，沿此方向的向量保持方向不变，长度缩放为原来的 <span class="value-highlight">3.00</span> 倍。</p>

&nbsp;                       <p>蓝色特征向量 <span class="highlight">v₂ = \[-0.707, 0.707]</span> 对应的特征值 <span class="value-highlight">λ₂ = 1.00</span>。沿此方向的向量保持方向不变，长度缩放为原来的 <span class="value-highlight">1.00</span> 倍（即长度不变）。</p>

&nbsp;                       <p><span class="highlight">总结：</span>在这个变换中，两个特征向量的方向均保持不变，红色向量方向被拉伸3倍，蓝色向量方向长度不变。变换将单位圆拉伸为椭圆，主轴沿特征向量方向。</p>

&nbsp;                   </div>

&nbsp;               </div>

&nbsp;           </div>

&nbsp;       </div>

&nbsp;       

&nbsp;       <div class="math-explanation">

&nbsp;           <h3>数学解释</h3>

&nbsp;           <p>对于给定的方阵 <strong>A</strong>，如果存在非零向量 <strong>v</strong> 和标量 <strong>λ</strong>，使得以下方程成立：</p>

&nbsp;           <div class="formula">A · v = λ · v</div>

&nbsp;           <p>那么 <strong>v</strong> 称为矩阵 <strong>A</strong> 的特征向量，<strong>λ</strong> 称为对应的特征值。</p>

&nbsp;           <p>几何意义：在线性变换 <strong>A</strong> 的作用下，特征向量 <strong>v</strong> 的方向保持不变（或反向），仅按特征值 <strong>λ</strong> 的比例进行缩放。</p>

&nbsp;           <p>当 λ > 0 时，方向不变；当 λ < 0 时，方向翻转；当 λ = 0 时，向量被压缩到原点。</p>

&nbsp;       </div>

&nbsp;       

&nbsp;       <div class="footer">

&nbsp;           <p>特征值与特征向量几何可视化 | 线性代数交互式教学工具</p>

&nbsp;       </div>

&nbsp;   </div>



&nbsp;   <script>

&nbsp;       // 获取DOM元素

&nbsp;       const originalCanvas = document.getElementById('original-canvas');

&nbsp;       const transformedCanvas = document.getElementById('transformed-canvas');

&nbsp;       const generateBtn = document.getElementById('generate-btn');

&nbsp;       const resetBtn = document.getElementById('reset-btn');

&nbsp;       const matrixInputs = {

&nbsp;           a11: document.getElementById('a11'),

&nbsp;           a12: document.getElementById('a12'),

&nbsp;           a21: document.getElementById('a21'),

&nbsp;           a22: document.getElementById('a22')

&nbsp;       };

&nbsp;       const matrixErrorEl = document.getElementById('matrix-error');

&nbsp;       const loadingIndicator = document.getElementById('loading-indicator');

&nbsp;       const autoUpdateCheckbox = document.getElementById('auto-update');

&nbsp;       const dynamicExplanationEl = document.getElementById('dynamic-explanation-content');

&nbsp;       const matrixDisplayEl = document.getElementById('matrix-display');

&nbsp;       const quickMatrixBtns = document.querySelectorAll('.quick-matrix-btn');

&nbsp;       

&nbsp;       // 特征值显示元素

&nbsp;       const lambda1El = document.getElementById('lambda1');

&nbsp;       const lambda2El = document.getElementById('lambda2');

&nbsp;       const vector1El = document.getElementById('vector1');

&nbsp;       const vector2El = document.getElementById('vector2');

&nbsp;       const flipNote1El = document.getElementById('flip-note1');

&nbsp;       const flipNote2El = document.getElementById('flip-note2');

&nbsp;       

&nbsp;       // 获取Canvas上下文

&nbsp;       const originalCtx = originalCanvas.getContext('2d');

&nbsp;       const transformedCtx = transformedCanvas.getContext('2d');

&nbsp;       

&nbsp;       // 输入验证状态

&nbsp;       let isValidInput = true;

&nbsp;       let validationTimeout = null;

&nbsp;       

&nbsp;       // 设置Canvas尺寸

&nbsp;       function resizeCanvases() {

&nbsp;           const container = document.querySelector('.canvas-container');

&nbsp;           const width = container.clientWidth;

&nbsp;           const height = container.clientHeight;

&nbsp;           

&nbsp;           originalCanvas.width = width;

&nbsp;           originalCanvas.height = height;

&nbsp;           transformedCanvas.width = width;

&nbsp;           transformedCanvas.height = height;

&nbsp;           

&nbsp;           // 重新绘制

&nbsp;           drawOriginal();

&nbsp;           if (window.currentMatrix) {

&nbsp;               drawTransformed(window.currentMatrix);

&nbsp;           }

&nbsp;       }

&nbsp;       

&nbsp;       // 初始化矩阵值

&nbsp;       let currentMatrix = \[\[2, 1], \[1, 2]];

&nbsp;       let eigenvalues = \[3, 1];

&nbsp;       let eigenvectors = \[\[Math.SQRT1\_2, Math.SQRT1\_2], \[-Math.SQRT1\_2, Math.SQRT1\_2]];

&nbsp;       

&nbsp;       // 验证输入是否为有效数字

&nbsp;       function validateInput(value) {

&nbsp;           // 允许空字符串（以便用户删除内容）

&nbsp;           if (value === '') return false;

&nbsp;           

&nbsp;           // 去除空格

&nbsp;           const trimmedValue = value.trim();

&nbsp;           

&nbsp;           // 检查是否为有效数字（支持小数和负数）

&nbsp;           const num = parseFloat(trimmedValue);

&nbsp;           if (isNaN(num)) return false;

&nbsp;           

&nbsp;           // 检查是否为有限数字

&nbsp;           if (!isFinite(num)) return false;

&nbsp;           

&nbsp;           return true;

&nbsp;       }

&nbsp;       

&nbsp;       // 验证所有矩阵输入

&nbsp;       function validateAllInputs() {

&nbsp;           clearTimeout(validationTimeout);

&nbsp;           

&nbsp;           validationTimeout = setTimeout(() => {

&nbsp;               let allValid = true;

&nbsp;               let errorMessage = '';

&nbsp;               

&nbsp;               // 检查每个输入框

&nbsp;               Object.entries(matrixInputs).forEach((\[id, input]) => {

&nbsp;                   const value = input.value;

&nbsp;                   

&nbsp;                   if (value === '') {

&nbsp;                       input.classList.add('error');

&nbsp;                       allValid = false;

&nbsp;                       if (!errorMessage.includes('矩阵元素不能为空')) {

&nbsp;                           errorMessage = '矩阵元素不能为空';

&nbsp;                       }

&nbsp;                   } else if (!validateInput(value)) {

&nbsp;                       input.classList.add('error');

&nbsp;                       allValid = false;

&nbsp;                       if (!errorMessage.includes('请输入有效的数字')) {

&nbsp;                           errorMessage = '请输入有效的数字';

&nbsp;                       }

&nbsp;                   } else {

&nbsp;                       input.classList.remove('error');

&nbsp;                   }

&nbsp;               });

&nbsp;               

&nbsp;               // 更新错误消息

&nbsp;               matrixErrorEl.textContent = errorMessage;

&nbsp;               

&nbsp;               // 更新按钮状态

&nbsp;               generateBtn.disabled = !allValid;

&nbsp;               

&nbsp;               // 更新全局验证状态

&nbsp;               isValidInput = allValid;

&nbsp;               

&nbsp;               // 如果所有输入有效且启用了自动更新，则立即更新可视化

&nbsp;               if (allValid \&\& autoUpdateCheckbox.checked) {

&nbsp;                   generateTransform();

&nbsp;               }

&nbsp;           }, 300); // 防抖延迟

&nbsp;       }

&nbsp;       

&nbsp;       // 计算特征值和特征向量 (针对2x2矩阵)

&nbsp;       function computeEigen(matrix) {

&nbsp;           const a = matrix\[0]\[0];

&nbsp;           const b = matrix\[0]\[1];

&nbsp;           const c = matrix\[1]\[0];

&nbsp;           const d = matrix\[1]\[1];

&nbsp;           

&nbsp;           // 计算特征值：λ² - (a+d)λ + (ad - bc) = 0

&nbsp;           const trace = a + d;

&nbsp;           const determinant = a \* d - b \* c;

&nbsp;           

&nbsp;           const discriminant = trace \* trace - 4 \* determinant;

&nbsp;           

&nbsp;           // 复数特征值情况

&nbsp;           if (discriminant < 0) {

&nbsp;               const realPart = trace / 2;

&nbsp;               const imagPart = Math.sqrt(-discriminant) / 2;

&nbsp;               

&nbsp;               eigenvalues = \[

&nbsp;                   {real: realPart, imag: imagPart},

&nbsp;                   {real: realPart, imag: -imagPart}

&nbsp;               ];

&nbsp;               

&nbsp;               lambda1El.textContent = `${realPart.toFixed(2)} + ${imagPart.toFixed(2)}i`;

&nbsp;               lambda2El.textContent = `${realPart.toFixed(2)} - ${imagPart.toFixed(2)}i`;

&nbsp;               vector1El.textContent = "复数特征向量";

&nbsp;               vector2El.textContent = "复数特征向量";

&nbsp;               

&nbsp;               // 隐藏翻转提示

&nbsp;               flipNote1El.style.display = 'none';

&nbsp;               flipNote2El.style.display = 'none';

&nbsp;               

&nbsp;               // 更新动态解释

&nbsp;               updateDynamicExplanation(matrix, eigenvalues, \[]);

&nbsp;               

&nbsp;               return { eigenvalues, eigenvectors: \[] };

&nbsp;           }

&nbsp;           

&nbsp;           // 实数特征值

&nbsp;           const sqrtDisc = Math.sqrt(discriminant);

&nbsp;           eigenvalues = \[

&nbsp;               (trace + sqrtDisc) / 2,

&nbsp;               (trace - sqrtDisc) / 2

&nbsp;           ];

&nbsp;           

&nbsp;           // 更新特征值显示

&nbsp;           lambda1El.textContent = eigenvalues\[0].toFixed(2);

&nbsp;           lambda2El.textContent = eigenvalues\[1].toFixed(2);

&nbsp;           

&nbsp;           // 显示或隐藏翻转提示

&nbsp;           flipNote1El.style.display = eigenvalues\[0] < 0 ? 'block' : 'none';

&nbsp;           flipNote2El.style.display = eigenvalues\[1] < 0 ? 'block' : 'none';

&nbsp;           

&nbsp;           // 计算特征向量

&nbsp;           eigenvectors = \[];

&nbsp;           

&nbsp;           // 对于每个特征值，解 (A - λI)v = 0

&nbsp;           eigenvalues.forEach((lambda, index) => {

&nbsp;               // 构造矩阵 (A - λI)

&nbsp;               const aMinusLambda = a - lambda;

&nbsp;               const dMinusLambda = d - lambda;

&nbsp;               

&nbsp;               // 解方程组 (a-λ)x + b\*y = 0, c\*x + (d-λ)y = 0

&nbsp;               // 我们需要找到非零解

&nbsp;               let v1, v2;

&nbsp;               

&nbsp;               // 方法：尝试找到一个非零向量满足方程

&nbsp;               // 首先检查b是否不为0，这样我们可以设x=1，解出y

&nbsp;               if (Math.abs(b) > 1e-10) {

&nbsp;                   v1 = 1;

&nbsp;                   v2 = -aMinusLambda / b;

&nbsp;               } 

&nbsp;               // 如果b接近0但c不为0，我们可以设y=1，解出x

&nbsp;               else if (Math.abs(c) > 1e-10) {

&nbsp;                   v2 = 1;

&nbsp;                   v1 = -dMinusLambda / c;

&nbsp;               }

&nbsp;               // 如果b和c都接近0，那么矩阵是对角矩阵

&nbsp;               // 检查(a-λ)和(d-λ)哪个为0

&nbsp;               else if (Math.abs(aMinusLambda) < 1e-10) {

&nbsp;                   // (a-λ)=0，那么x可以是任意值，y=0

&nbsp;                   v1 = 1;

&nbsp;                   v2 = 0;

&nbsp;               }

&nbsp;               else if (Math.abs(dMinusLambda) < 1e-10) {

&nbsp;                   // (d-λ)=0，那么y可以是任意值，x=0

&nbsp;                   v1 = 0;

&nbsp;                   v2 = 1;

&nbsp;               }

&nbsp;               else {

&nbsp;                   // 这种情况不应该发生，但作为后备方案

&nbsp;                   v1 = 1;

&nbsp;                   v2 = 0;

&nbsp;               }

&nbsp;               

&nbsp;               // 归一化

&nbsp;               const norm = Math.sqrt(v1 \* v1 + v2 \* v2);

&nbsp;               if (norm > 1e-10) {

&nbsp;                   v1 /= norm;

&nbsp;                   v2 /= norm;

&nbsp;               }

&nbsp;               

&nbsp;               eigenvectors.push(\[v1, v2]);

&nbsp;               

&nbsp;               // 更新特征向量显示

&nbsp;               if (index === 0) {

&nbsp;                   vector1El.textContent = `\[${v1.toFixed(3)}, ${v2.toFixed(3)}]`;

&nbsp;               } else {

&nbsp;                   vector2El.textContent = `\[${v1.toFixed(3)}, ${v2.toFixed(3)}]`;

&nbsp;               }

&nbsp;           });

&nbsp;           

&nbsp;           // 更新动态解释

&nbsp;           updateDynamicExplanation(matrix, eigenvalues, eigenvectors);

&nbsp;           

&nbsp;           return { eigenvalues, eigenvectors };

&nbsp;       }

&nbsp;       

&nbsp;       // 更新动态解释内容

&nbsp;       function updateDynamicExplanation(matrix, eigenvalues, eigenvectors) {

&nbsp;           // 更新矩阵显示

&nbsp;           const matrixStr = `\[\[${matrix\[0]\[0].toFixed(2)}, ${matrix\[0]\[1].toFixed(2)}], \[${matrix\[1]\[0].toFixed(2)}, ${matrix\[1]\[1].toFixed(2)}]]`;

&nbsp;           matrixDisplayEl.textContent = matrixStr;

&nbsp;           

&nbsp;           let explanationHTML = '';

&nbsp;           

&nbsp;           // 复数特征值情况

&nbsp;           if (eigenvalues.length > 0 \&\& eigenvalues\[0].imag !== undefined) {

&nbsp;               const realPart = eigenvalues\[0].real;

&nbsp;               const imagPart = eigenvalues\[0].imag;

&nbsp;               

&nbsp;               explanationHTML = `

&nbsp;                   <p>对于当前矩阵 <span class="value-highlight">${matrixStr}</span>：</p>

&nbsp;                   <p>该矩阵具有<span class="highlight">复数特征值</span>：<span class="value-highlight">λ₁ = ${realPart.toFixed(2)} + ${imagPart.toFixed(2)}i</span> 和 <span class="value-highlight">λ₂ = ${realPart.toFixed(2)} - ${imagPart.toFixed(2)}i</span>。</p>

&nbsp;                   <p>复数特征值表示该矩阵变换包含<span class="highlight">旋转</span>成分。在变换中，向量不仅会缩放，还会发生旋转，因此不存在保持方向不变的实特征向量。</p>

&nbsp;                   <p>从可视化中可以看到，单位圆被变换为一个旋转的椭圆，没有实向量在变换后保持原来的方向。</p>

&nbsp;               `;

&nbsp;           } 

&nbsp;           // 实数特征值情况

&nbsp;           else if (eigenvalues.length > 0 \&\& eigenvectors.length > 0) {

&nbsp;               const lambda1 = eigenvalues\[0];

&nbsp;               const lambda2 = eigenvalues\[1];

&nbsp;               const v1 = eigenvectors\[0];

&nbsp;               const v2 = eigenvectors\[1];

&nbsp;               

&nbsp;               // 构建特征向量字符串

&nbsp;               const v1Str = `\[${v1\[0].toFixed(3)}, ${v1\[1].toFixed(3)}]`;

&nbsp;               const v2Str = `\[${v2\[0].toFixed(3)}, ${v2\[1].toFixed(3)}]`;

&nbsp;               

&nbsp;               // 描述第一个特征向量的行为

&nbsp;               let eigen1Desc = '';

&nbsp;               if (Math.abs(lambda1) < 0.01) {

&nbsp;                   eigen1Desc = `<span class="highlight">v₁ = ${v1Str}</span> 对应的特征值 <span class="value-highlight">λ₁ = ${lambda1.toFixed(2)}</span>。在线性变换中，沿此方向的向量被<span class="highlight">压缩到原点</span>（长度缩放为0）。`;

&nbsp;               } else if (lambda1 > 0) {

&nbsp;                   const absLambda1 = Math.abs(lambda1);

&nbsp;                   eigen1Desc = `<span class="highlight">v₁ = ${v1Str}</span> 对应的特征值 <span class="value-highlight">λ₁ = ${lambda1.toFixed(2)}</span>。在线性变换中，沿此方向的向量<span class="highlight">保持方向不变</span>，长度缩放为原来的 <span class="value-highlight">${absLambda1.toFixed(2)}</span> 倍${lambda1 > 1 ? '（拉伸）' : lambda1 < 1 ? '（压缩）' : '（不变）'}。`;

&nbsp;               } else {

&nbsp;                   const absLambda1 = Math.abs(lambda1);

&nbsp;                   eigen1Desc = `<span class="highlight">v₁ = ${v1Str}</span> 对应的特征值 <span class="value-highlight">λ₁ = ${lambda1.toFixed(2)}</span>。在线性变换中，沿此方向的向量<span class="highlight">方向翻转</span>，长度缩放为原来的 <span class="value-highlight">${absLambda1.toFixed(2)}</span> 倍${absLambda1 > 1 ? '（拉伸并反向）' : absLambda1 < 1 ? '（压缩并反向）' : '（仅反向）'}。`;

&nbsp;               }

&nbsp;               

&nbsp;               // 描述第二个特征向量的行为

&nbsp;               let eigen2Desc = '';

&nbsp;               if (Math.abs(lambda2) < 0.01) {

&nbsp;                   eigen2Desc = `<span class="highlight">v₂ = ${v2Str}</span> 对应的特征值 <span class="value-highlight">λ₂ = ${lambda2.toFixed(2)}</span>。在线性变换中，沿此方向的向量被<span class="highlight">压缩到原点</span>（长度缩放为0）。`;

&nbsp;               } else if (lambda2 > 0) {

&nbsp;                   const absLambda2 = Math.abs(lambda2);

&nbsp;                   eigen2Desc = `<span class="highlight">v₂ = ${v2Str}</span> 对应的特征值 <span class="value-highlight">λ₂ = ${lambda2.toFixed(2)}</span>。在线性变换中，沿此方向的向量<span class="highlight">保持方向不变</span>，长度缩放为原来的 <span class="value-highlight">${absLambda2.toFixed(2)}</span> 倍${lambda2 > 1 ? '（拉伸）' : lambda2 < 1 ? '（压缩）' : '（不变）'}。`;

&nbsp;               } else {

&nbsp;                   const absLambda2 = Math.abs(lambda2);

&nbsp;                   eigen2Desc = `<span class="highlight">v₂ = ${v2Str}</span> 对应的特征值 <span class="value-highlight">λ₂ = ${lambda2.toFixed(2)}</span>。在线性变换中，沿此方向的向量<span class="highlight">方向翻转</span>，长度缩放为原来的 <span class="value-highlight">${absLambda2.toFixed(2)}</span> 倍${absLambda2 > 1 ? '（拉伸并反向）' : absLambda2 < 1 ? '（压缩并反向）' : '（仅反向）'}。`;

&nbsp;               }

&nbsp;               

&nbsp;               // 总结

&nbsp;               let summary = '';

&nbsp;               const negativeCount = eigenvalues.filter(val => val < 0).length;

&nbsp;               const zeroCount = eigenvalues.filter(val => Math.abs(val) < 0.01).length;

&nbsp;               const positiveCount = eigenvalues.length - negativeCount - zeroCount;

&nbsp;               

&nbsp;               if (zeroCount === 2) {

&nbsp;                   summary = `在这个变换中，两个特征向量方向都被压缩到原点，整个空间被压缩到低维空间。`;

&nbsp;               } else if (zeroCount === 1) {

&nbsp;                   summary = `在这个变换中，一个特征向量方向被压缩到原点，另一个方向${negativeCount === 1 ? '翻转并' : ''}缩放。变换将单位圆压缩为一条线段。`;

&nbsp;               } else if (negativeCount === 2) {

&nbsp;                   summary = `在这个变换中，两个特征向量方向都发生翻转，整个变换相当于先缩放再关于原点中心对称。`;

&nbsp;               } else if (negativeCount === 1) {

&nbsp;                   summary = `在这个变换中，一个特征向量方向保持，另一个方向翻转。变换将单位圆拉伸为椭圆，但有一个主轴方向发生翻转。`;

&nbsp;               } else {

&nbsp;                   summary = `在这个变换中，两个特征向量的方向均保持不变，单位圆被拉伸为椭圆，主轴沿特征向量方向。`;

&nbsp;               }

&nbsp;               

&nbsp;               explanationHTML = `

&nbsp;                   <p>对于当前矩阵 <span class="value-highlight">${matrixStr}</span>：</p>

&nbsp;                   <p>${eigen1Desc}</p>

&nbsp;                   <p>${eigen2Desc}</p>

&nbsp;                   <p><span class="highlight">总结：</span>${summary} 特征向量是变换中保持方向不变的向量，特征值的绝对值表示伸缩倍数，正负号表示方向是否翻转。</p>

&nbsp;               `;

&nbsp;           }

&nbsp;           

&nbsp;           dynamicExplanationEl.innerHTML = explanationHTML;

&nbsp;       }

&nbsp;       

&nbsp;       // 绘制原始图形（单位圆和网格）

&nbsp;       function drawOriginal() {

&nbsp;           const ctx = originalCtx;

&nbsp;           const width = originalCanvas.width;

&nbsp;           const height = originalCanvas.height;

&nbsp;           const centerX = width / 2;

&nbsp;           const centerY = height / 2;

&nbsp;           const scale = Math.min(width, height) \* 0.25;

&nbsp;           

&nbsp;           // 清空画布

&nbsp;           ctx.clearRect(0, 0, width, height);

&nbsp;           

&nbsp;           // 绘制背景网格

&nbsp;           ctx.strokeStyle = '#f0f0f0';

&nbsp;           ctx.lineWidth = 1;

&nbsp;           const gridSize = 20;

&nbsp;           

&nbsp;           // 垂直线

&nbsp;           for (let x = 0; x < width; x += gridSize) {

&nbsp;               ctx.beginPath();

&nbsp;               ctx.moveTo(x, 0);

&nbsp;               ctx.lineTo(x, height);

&nbsp;               ctx.stroke();

&nbsp;           }

&nbsp;           

&nbsp;           // 水平线

&nbsp;           for (let y = 0; y < height; y += gridSize) {

&nbsp;               ctx.beginPath();

&nbsp;               ctx.moveTo(0, y);

&nbsp;               ctx.lineTo(width, y);

&nbsp;               ctx.stroke();

&nbsp;           }

&nbsp;           

&nbsp;           // 绘制坐标轴

&nbsp;           ctx.strokeStyle = '#aaa';

&nbsp;           ctx.lineWidth = 2;

&nbsp;           ctx.beginPath();

&nbsp;           ctx.moveTo(0, centerY);

&nbsp;           ctx.lineTo(width, centerY);

&nbsp;           ctx.moveTo(centerX, 0);

&nbsp;           ctx.lineTo(centerX, height);

&nbsp;           ctx.stroke();

&nbsp;           

&nbsp;           // 绘制单位圆

&nbsp;           ctx.strokeStyle = '#3498db';

&nbsp;           ctx.lineWidth = 3;

&nbsp;           ctx.beginPath();

&nbsp;           ctx.arc(centerX, centerY, scale, 0, Math.PI \* 2);

&nbsp;           ctx.stroke();

&nbsp;           

&nbsp;           // 绘制正方形网格

&nbsp;           ctx.strokeStyle = '#2ecc71';

&nbsp;           ctx.lineWidth = 2;

&nbsp;           const squareSize = scale \* 0.5;

&nbsp;           

&nbsp;           ctx.beginPath();

&nbsp;           ctx.rect(centerX - squareSize, centerY - squareSize, squareSize \* 2, squareSize \* 2);

&nbsp;           ctx.stroke();

&nbsp;           

&nbsp;           // 绘制原始特征向量（如果存在）

&nbsp;           if (eigenvectors.length > 0 \&\& eigenvectors\[0].length > 0) {

&nbsp;               eigenvectors.forEach((vector, index) => {

&nbsp;                   const \[v1, v2] = vector;

&nbsp;                   

&nbsp;                   // 设置颜色

&nbsp;                   const color = index === 0 ? '#e53e3e' : '#3182ce';

&nbsp;                   ctx.strokeStyle = color;

&nbsp;                   ctx.fillStyle = color;

&nbsp;                   ctx.lineWidth = 3;

&nbsp;                   

&nbsp;                   // 原始特征向量

&nbsp;                   const arrowLength = scale \* 0.8;

&nbsp;                   const endX = centerX + v1 \* arrowLength;

&nbsp;                   const endY = centerY + v2 \* arrowLength;

&nbsp;                   

&nbsp;                   // 绘制箭头

&nbsp;                   drawArrow(ctx, centerX, centerY, endX, endY);

&nbsp;                   

&nbsp;                   // 标注特征向量名称

&nbsp;                   ctx.font = 'bold 12px Arial';

&nbsp;                   ctx.fillText(`v${index+1}`, endX + 8 \* (v1 > 0 ? 1 : -1), endY + 8 \* (v2 > 0 ? 1 : -1));

&nbsp;               });

&nbsp;           }

&nbsp;           

&nbsp;           // 标注坐标轴

&nbsp;           ctx.fillStyle = '#4a5568';

&nbsp;           ctx.font = '12px Arial';

&nbsp;           ctx.fillText('x', width - 10, centerY - 10);

&nbsp;           ctx.fillText('y', centerX + 10, 20);

&nbsp;           ctx.fillText('O', centerX + 10, centerY + 20);

&nbsp;       }

&nbsp;       

&nbsp;       // 绘制变换后的图形

&nbsp;       function drawTransformed(matrix) {

&nbsp;           const ctx = transformedCtx;

&nbsp;           const width = transformedCanvas.width;

&nbsp;           const height = transformedCanvas.height;

&nbsp;           const centerX = width / 2;

&nbsp;           const centerY = height / 2;

&nbsp;           const scale = Math.min(width, height) \* 0.25;

&nbsp;           

&nbsp;           // 清空画布

&nbsp;           ctx.clearRect(0, 0, width, height);

&nbsp;           

&nbsp;           // 绘制背景网格

&nbsp;           ctx.strokeStyle = '#f0f0f0';

&nbsp;           ctx.lineWidth = 1;

&nbsp;           const gridSize = 20;

&nbsp;           

&nbsp;           // 垂直线

&nbsp;           for (let x = 0; x < width; x += gridSize) {

&nbsp;               ctx.beginPath();

&nbsp;               ctx.moveTo(x, 0);

&nbsp;               ctx.lineTo(x, height);

&nbsp;               ctx.stroke();

&nbsp;           }

&nbsp;           

&nbsp;           // 水平线

&nbsp;           for (let y = 0; y < height; y += gridSize) {

&nbsp;               ctx.beginPath();

&nbsp;               ctx.moveTo(0, y);

&nbsp;               ctx.lineTo(width, y);

&nbsp;               ctx.stroke();

&nbsp;           }

&nbsp;           

&nbsp;           // 绘制坐标轴

&nbsp;           ctx.strokeStyle = '#aaa';

&nbsp;           ctx.lineWidth = 2;

&nbsp;           ctx.beginPath();

&nbsp;           ctx.moveTo(0, centerY);

&nbsp;           ctx.lineTo(width, centerY);

&nbsp;           ctx.moveTo(centerX, 0);

&nbsp;           ctx.lineTo(centerX, height);

&nbsp;           ctx.stroke();

&nbsp;           

&nbsp;           // 生成变换后的点

&nbsp;           const circlePoints = 100;

&nbsp;           

&nbsp;           // 绘制变换后的单位圆

&nbsp;           ctx.strokeStyle = '#3498db';

&nbsp;           ctx.lineWidth = 3;

&nbsp;           ctx.beginPath();

&nbsp;           

&nbsp;           for (let i = 0; i <= circlePoints; i++) {

&nbsp;               const angle = (i / circlePoints) \* Math.PI \* 2;

&nbsp;               const x = Math.cos(angle);

&nbsp;               const y = Math.sin(angle);

&nbsp;               

&nbsp;               // 应用矩阵变换

&nbsp;               const tx = matrix\[0]\[0] \* x + matrix\[0]\[1] \* y;

&nbsp;               const ty = matrix\[1]\[0] \* x + matrix\[1]\[1] \* y;

&nbsp;               

&nbsp;               const canvasX = centerX + tx \* scale;

&nbsp;               const canvasY = centerY + ty \* scale;

&nbsp;               

&nbsp;               if (i === 0) {

&nbsp;                   ctx.moveTo(canvasX, canvasY);

&nbsp;               } else {

&nbsp;                   ctx.lineTo(canvasX, canvasY);

&nbsp;               }

&nbsp;           }

&nbsp;           

&nbsp;           ctx.closePath();

&nbsp;           ctx.stroke();

&nbsp;           

&nbsp;           // 绘制变换后的正方形

&nbsp;           ctx.strokeStyle = '#2ecc71';

&nbsp;           ctx.lineWidth = 2;

&nbsp;           

&nbsp;           const squarePoints = \[

&nbsp;               \[-0.5, -0.5],

&nbsp;               \[0.5, -0.5],

&nbsp;               \[0.5, 0.5],

&nbsp;               \[-0.5, 0.5],

&nbsp;               \[-0.5, -0.5]

&nbsp;           ];

&nbsp;           

&nbsp;           ctx.beginPath();

&nbsp;           

&nbsp;           squarePoints.forEach((\[x, y], index) => {

&nbsp;               // 应用矩阵变换

&nbsp;               const tx = matrix\[0]\[0] \* x + matrix\[0]\[1] \* y;

&nbsp;               const ty = matrix\[1]\[0] \* x + matrix\[1]\[1] \* y;

&nbsp;               

&nbsp;               const canvasX = centerX + tx \* scale;

&nbsp;               const canvasY = centerY + ty \* scale;

&nbsp;               

&nbsp;               if (index === 0) {

&nbsp;                   ctx.moveTo(canvasX, canvasY);

&nbsp;               } else {

&nbsp;                   ctx.lineTo(canvasX, canvasY);

&nbsp;               }

&nbsp;           });

&nbsp;           

&nbsp;           ctx.stroke();

&nbsp;           

&nbsp;           // 绘制特征向量（如果存在实数特征值）

&nbsp;           if (eigenvectors.length > 0 \&\& eigenvalues.length > 0 \&\& typeof eigenvalues\[0] === 'number') {

&nbsp;               eigenvectors.forEach((vector, index) => {

&nbsp;                   const \[v1, v2] = vector;

&nbsp;                   const lambda = eigenvalues\[index];

&nbsp;                   

&nbsp;                   // 设置颜色

&nbsp;                   const color = index === 0 ? '#e53e3e' : '#3182ce';

&nbsp;                   ctx.strokeStyle = color;

&nbsp;                   ctx.fillStyle = color;

&nbsp;                   ctx.lineWidth = 3;

&nbsp;                   

&nbsp;                   // 原始特征向量

&nbsp;                   const arrowLength = scale \* 0.8;

&nbsp;                   const endX = centerX + v1 \* arrowLength;

&nbsp;                   const endY = centerY + v2 \* arrowLength;

&nbsp;                   

&nbsp;                   // 绘制原始特征向量（实线）

&nbsp;                   drawArrow(ctx, centerX, centerY, endX, endY);

&nbsp;                   

&nbsp;                   // 变换后的特征向量

&nbsp;                   // 注意：A\*v = λ\*v，所以变换后的向量是 λ\*v

&nbsp;                   const transformedLength = lambda \* arrowLength;

&nbsp;                   const transformedEndX = centerX + v1 \* transformedLength;

&nbsp;                   const transformedEndY = centerY + v2 \* transformedLength;

&nbsp;                   

&nbsp;                   // 绘制变换后的向量（虚线）

&nbsp;                   ctx.save();

&nbsp;                   ctx.strokeStyle = color;

&nbsp;                   ctx.setLineDash(\[5, 5]);

&nbsp;                   drawArrow(ctx, centerX, centerY, transformedEndX, transformedEndY);

&nbsp;                   ctx.restore();

&nbsp;                   

&nbsp;                   // 标注特征值

&nbsp;                   ctx.font = 'bold 14px Arial';

&nbsp;                   const offsetX = 12 \* (v1 > 0 ? 1 : -1);

&nbsp;                   const offsetY = 12 \* (v2 > 0 ? 1 : -1);

&nbsp;                   

&nbsp;                   // 如果特征值为负数，在标注中特别说明

&nbsp;                   if (lambda < 0) {

&nbsp;                       ctx.fillStyle = '#d69e2e'; // 使用橙色表示负数

&nbsp;                   }

&nbsp;                   

&nbsp;                   ctx.fillText(`λ${index+1}=${lambda.toFixed(2)}`, 

&nbsp;                               transformedEndX + offsetX, 

&nbsp;                               transformedEndY + offsetY);

&nbsp;                   

&nbsp;                   // 恢复颜色

&nbsp;                   ctx.fillStyle = color;

&nbsp;                   

&nbsp;                   // 标注特征向量名称

&nbsp;                   ctx.font = 'bold 12px Arial';

&nbsp;                   ctx.fillText(`v${index+1}`, endX + offsetX/2, endY + offsetY/2);

&nbsp;               });

&nbsp;           } else if (eigenvalues.length > 0 \&\& eigenvalues\[0].imag) {

&nbsp;               // 复数特征值情况

&nbsp;               ctx.fillStyle = '#e53e3e';

&nbsp;               ctx.font = 'bold 14px Arial';

&nbsp;               ctx.fillText('复数特征值', centerX - 50, centerY + 80);

&nbsp;               ctx.fillText('无实特征向量', centerX - 60, centerY + 100);

&nbsp;           }

&nbsp;           

&nbsp;           // 标注坐标轴

&nbsp;           ctx.fillStyle = '#4a5568';

&nbsp;           ctx.font = '12px Arial';

&nbsp;           ctx.fillText('x', width - 10, centerY - 10);

&nbsp;           ctx.fillText('y', centerX + 10, 20);

&nbsp;           ctx.fillText('O', centerX + 10, centerY + 20);

&nbsp;           

&nbsp;           // 如果特征值为负，添加说明文本

&nbsp;           if (eigenvalues.length > 0 \&\& typeof eigenvalues\[0] === 'number') {

&nbsp;               const negativeCount = eigenvalues.filter(val => val < 0).length;

&nbsp;               if (negativeCount > 0) {

&nbsp;                   ctx.fillStyle = '#d69e2e';

&nbsp;                   ctx.font = 'bold 12px Arial';

&nbsp;                   ctx.fillText(`有${negativeCount}个特征值为负，方向翻转`, width - 150, height - 15);

&nbsp;               }

&nbsp;           }

&nbsp;       }

&nbsp;       

&nbsp;       // 绘制箭头

&nbsp;       function drawArrow(ctx, fromX, fromY, toX, toY) {

&nbsp;           const headLength = 12;

&nbsp;           const dx = toX - fromX;

&nbsp;           const dy = toY - fromY;

&nbsp;           const angle = Math.atan2(dy, dx);

&nbsp;           

&nbsp;           // 绘制线

&nbsp;           ctx.beginPath();

&nbsp;           ctx.moveTo(fromX, fromY);

&nbsp;           ctx.lineTo(toX, toY);

&nbsp;           ctx.stroke();

&nbsp;           

&nbsp;           // 绘制箭头头部

&nbsp;           ctx.beginPath();

&nbsp;           ctx.moveTo(toX, toY);

&nbsp;           ctx.lineTo(toX - headLength \* Math.cos(angle - Math.PI / 6), 

&nbsp;                     toY - headLength \* Math.sin(angle - Math.PI / 6));

&nbsp;           ctx.moveTo(toX, toY);

&nbsp;           ctx.lineTo(toX - headLength \* Math.cos(angle + Math.PI / 6), 

&nbsp;                     toY - headLength \* Math.sin(angle + Math.PI / 6));

&nbsp;           ctx.stroke();

&nbsp;       }

&nbsp;       

&nbsp;       // 生成变换

&nbsp;       function generateTransform() {

&nbsp;           if (!isValidInput) {

&nbsp;               matrixErrorEl.textContent = '请输入有效的矩阵值';

&nbsp;               return;

&nbsp;           }

&nbsp;           

&nbsp;           // 显示加载指示器

&nbsp;           loadingIndicator.classList.add('active');

&nbsp;           

&nbsp;           // 使用requestAnimationFrame确保UI不会卡顿

&nbsp;           requestAnimationFrame(() => {

&nbsp;               try {

&nbsp;                   // 获取矩阵输入值

&nbsp;                   currentMatrix = \[

&nbsp;                       \[parseFloat(matrixInputs.a11.value) || 0, 

&nbsp;                        parseFloat(matrixInputs.a12.value) || 0],

&nbsp;                       \[parseFloat(matrixInputs.a21.value) || 0, 

&nbsp;                        parseFloat(matrixInputs.a22.value) || 0]

&nbsp;                   ];

&nbsp;                   

&nbsp;                   // 计算特征值和特征向量

&nbsp;                   computeEigen(currentMatrix);

&nbsp;                   

&nbsp;                   // 重新绘制图形

&nbsp;                   drawOriginal();

&nbsp;                   drawTransformed(currentMatrix);

&nbsp;                   

&nbsp;                   // 保存当前矩阵

&nbsp;                   window.currentMatrix = currentMatrix;

&nbsp;                   

&nbsp;                   // 清除错误消息

&nbsp;                   matrixErrorEl.textContent = '';

&nbsp;               } catch (error) {

&nbsp;                   console.error('生成变换时出错:', error);

&nbsp;                   matrixErrorEl.textContent = '计算错误，请检查矩阵输入';

&nbsp;               } finally {

&nbsp;                   // 隐藏加载指示器

&nbsp;                   loadingIndicator.classList.remove('active');

&nbsp;               }

&nbsp;           });

&nbsp;       }

&nbsp;       

&nbsp;       // 重置矩阵

&nbsp;       function resetMatrix() {

&nbsp;           matrixInputs.a11.value = 2;

&nbsp;           matrixInputs.a12.value = 1;

&nbsp;           matrixInputs.a21.value = 1;

&nbsp;           matrixInputs.a22.value = 2;

&nbsp;           

&nbsp;           // 验证输入

&nbsp;           validateAllInputs();

&nbsp;           

&nbsp;           // 生成变换

&nbsp;           if (isValidInput) {

&nbsp;               generateTransform();

&nbsp;           }

&nbsp;       }

&nbsp;       

&nbsp;       // 设置快速矩阵

&nbsp;       function setQuickMatrix(matrixStr) {

&nbsp;           const values = matrixStr.split(',');

&nbsp;           if (values.length === 4) {

&nbsp;               matrixInputs.a11.value = values\[0];

&nbsp;               matrixInputs.a12.value = values\[1];

&nbsp;               matrixInputs.a21.value = values\[2];

&nbsp;               matrixInputs.a22.value = values\[3];

&nbsp;               

&nbsp;               // 验证输入

&nbsp;               validateAllInputs();

&nbsp;               

&nbsp;               // 生成变换

&nbsp;               if (isValidInput) {

&nbsp;                   generateTransform();

&nbsp;               }

&nbsp;           }

&nbsp;       }

&nbsp;       

&nbsp;       // 初始化

&nbsp;       function init() {

&nbsp;           // 设置初始Canvas尺寸

&nbsp;           resizeCanvases();

&nbsp;           

&nbsp;           // 初始验证

&nbsp;           validateAllInputs();

&nbsp;           

&nbsp;           // 计算初始特征值和特征向量

&nbsp;           computeEigen(currentMatrix);

&nbsp;           

&nbsp;           // 绘制初始图形

&nbsp;           drawOriginal();

&nbsp;           drawTransformed(currentMatrix);

&nbsp;           

&nbsp;           // 保存当前矩阵

&nbsp;           window.currentMatrix = currentMatrix;

&nbsp;           

&nbsp;           // 事件监听

&nbsp;           generateBtn.addEventListener('click', generateTransform);

&nbsp;           resetBtn.addEventListener('click', resetMatrix);

&nbsp;           

&nbsp;           // 窗口大小变化时重新调整Canvas

&nbsp;           window.addEventListener('resize', resizeCanvases);

&nbsp;           

&nbsp;           // 为输入框添加输入事件监听

&nbsp;           Object.values(matrixInputs).forEach(input => {

&nbsp;               input.addEventListener('input', validateAllInputs);

&nbsp;               

&nbsp;               // 添加键盘事件，支持回车键触发变换

&nbsp;               input.addEventListener('keydown', (e) => {

&nbsp;                   if (e.key === 'Enter' \&\& isValidInput) {

&nbsp;                       generateTransform();

&nbsp;                   }

&nbsp;               });

&nbsp;               

&nbsp;               // 添加焦点事件，清除错误样式

&nbsp;               input.addEventListener('focus', () => {

&nbsp;                   input.classList.remove('error');

&nbsp;               });

&nbsp;           });

&nbsp;           

&nbsp;           // 自动更新复选框事件

&nbsp;           autoUpdateCheckbox.addEventListener('change', () => {

&nbsp;               // 如果启用自动更新且输入有效，立即更新

&nbsp;               if (autoUpdateCheckbox.checked \&\& isValidInput) {

&nbsp;                   generateTransform();

&nbsp;               }

&nbsp;           });

&nbsp;           

&nbsp;           // 快速矩阵按钮事件

&nbsp;           quickMatrixBtns.forEach(btn => {

&nbsp;               btn.addEventListener('click', () => {

&nbsp;                   const matrixStr = btn.getAttribute('data-matrix');

&nbsp;                   setQuickMatrix(matrixStr);

&nbsp;               });

&nbsp;           });

&nbsp;       }

&nbsp;       

&nbsp;       // 页面加载完成后初始化

&nbsp;       window.addEventListener('load', init);

&nbsp;   </script>

</body>

</html>

